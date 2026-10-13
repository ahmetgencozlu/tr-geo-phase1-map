# Kaynak Araştırması 4 — Belediye CBS/WFS mahalle sınırları (hedef iller) + kompleks düzeyi yeni açık kaynaklar

Tarih: 2026-10-13 · Durum: **TAMAMLANDI** · Değerlendirilen kaynak: 14 il ucu (A) + 15 kaynak (B)

Kapsam: 2. ve 3. turda değerlendirilen ~40 kaynak tekrarlanmadı. (A) Mahallesiz 4.514 kaydın yoğunlaştığı iller (Mersin, Hatay, Adana, K.Maraş, Afyon, Manisa, Samsun, Eskişehir, Diyarbakır, Erzurum, Malatya, Şanlıurfa, Van, Tekirdağ) belediye açık veri / CBS / ArcGIS REST / GeoServer uçları. (B) Kompleks düzeyi ve önceliklendirme kaynakları.

İlke: yalnızca açık/ücretsiz/yasal, toplu kullanıma izin veren; kişisel veri yok; "humanitarian use only" lisanslılar kullanılmaz; ticari ilan/harita siteleri kapsam dışı.

## Hemen entegre edilebilir (en iyi 5)

| # | Kaynak | Puan | Neden |
|---|---|---|---|
| 1 | **Kahramanmaraş BB ArcGIS REST (MAKS YAPI + Mahalle + POI)** — A1 | **5** | 716 mahalle polygonu (K.Maraş'ın 183 mahallesiz kaydını kapatır) + 298 k bina polygonu, 283 k'sı `siteveyakooperatifadi` dolu → il düzeyinde yüzlerce yeni site/koop; `f=geojson` ile doğrudan `ogr2ogr`. Lisans belirsiz (kamu) → atıf + yazılı teyit. |
| 2 | **Overture base/land_use residential (adlı)** — B1 | 4 | 153.581 TR residential polygonu, 16.516 adlı; ODbL; duckdb ile dakikalar içinde. Mahalle ataması `ST_Within` ile. |
| 3 | **Copernicus Urban Atlas 2018 — 76 TR FUA** — B3 | 3 | Hedef illerin tamamını kapsar (Adana-Mersin, Antakya, K.Maraş, Afyon, Samsun, Eskişehir, Diyarbakır, Erzurum, Malatya, Ş.Urfa, Van); yoğun konut dokusu sınıfları ile önceliklendirme. CLMS hesabı gerekir. |
| 4 | **Meta HRSL Türkiye (HDX, CC BY)** — B4 | 3 | 30 m nüfus; `humanitarian-only` değil; mahallesiz kayıt çevresi yoğunluk skorlaması. |
| 5 | **GHSL BUILT-S/NRES 2023A** — B5 | 3 | Konut-dışı yapı yüzeyi → plaza/sanayi ayrımı; anonim HTTP, AB açık veri. |

Genel sonuç: Hedef 14 ilde **yalnızca Kahramanmaraş** anonim mahalle polygonu servisi veriyor (üstelik MAKS bina + site adıyla). Diyarbakır ve Erzurum'da GeoServer var ama 401; Adana ArcGIS dizini kapalı; Mersin/Samsun/Hatay/Ş.Urfa/Tekirdağ uçlarına erişilemedi. Overture `addresses` TR'yi kapsamıyor. TÜİK CİP mahalle düzeyi göstergeye sahip ama API parametreleri bu oturumda çözülemedi.

## (A) Hedef il belediyeleri — mahalle sınırı uçları

### A1. Kahramanmaraş BB Kent Rehberi — ArcGIS Enterprise 11.5 REST (MAKS + Mahalle + POI) — **puan 5**
- URL: `https://kentrehberi.kahramanmaras.bel.tr/server/rest/services?f=json` (anonim erişim; FeatureServer sorgulanabilir)
- Lisans: **belirsiz** (copyrightText boş; kamu kent rehberi; MAKS verisi NVİ kaynaklı). Önerilen: atıf + kamu kaynağı notu; ticari dağıtımdan önce belediyeye yazılı sorulmalı.
- Erişim: ArcGIS REST `query` (maxRecordCount 2000, `resultOffset` ile sayfalama, `f=geojson` destekli, SR 5256/4326)
- Katmanlar ve kayıt sayıları (ölçüldü):
  - `Mahalle/FeatureServer/0` → **716 mahalle polygonu** (alanlar: `ad, kimlikno, ilceid, belediyeid, ort_katsayisi, ort_dairesa, max_katsayisi, max_dairesa`)
  - `MAKS_Guncel_WGS84/FeatureServer`: 0 NUMARATAJ **606.370** nokta · 1 DIGERYAPI 83.260 · **2 YAPI 298.082 bina polygonu** · 3 YOLORTAHAT 74.500 · 4 MAHALLE · 5 ILCE · 6 IL
  - YAPI alanları: `ad, siteveyakooperatifadi, konutbirimsayi, bagimsizbolumsayi, zeminustukatsayisi, toplamyukseklik, yapiinsaatalan, postakodu, tip, durum, enerjiperformanssinif`
  - **`siteveyakooperatifadi` dolu: 283.655 bina**; `ad LIKE '%SİTESİ%'`: 925 bina; distinct site/koop adı > 2.000 (transfer limiti aşıldı; blok adları da karışık: "D-1 BLOK", "C2-11" → temizlik gerekir)
  - `POI_Ana_Kategori/FeatureServer/0` → 4.174 POI (`poi_adi, bina_adi, alt_kategori` — "konut" alt kategorisi var, `mahalle_adi, yol_adi, kapi_no`)
- Değer: (1) K.Maraş'taki 183 mahallesiz kaydın tamamını `ST_Within` ile doldurur; (2) **kompleks düzeyi**: bina polygonlarını `siteveyakooperatifadi` ile gruplayıp `ST_Union`/centroid → yüzlerce yeni site adayı (il için en kapsamlı kaynak); (3) kat/daire sayısı → tipoloji doğrulama.
- Kanıt:
  ```bash
  B=https://kentrehberi.kahramanmaras.bel.tr/server/rest/services
  curl -sk "$B/Mahalle/FeatureServer/0/query?where=1%3D1&returnCountOnly=true&f=json"            # {"count":716}
  curl -sk "$B/MAKS_Guncel_WGS84/FeatureServer/2/query?where=1%3D1&returnCountOnly=true&f=json"  # {"count":298082}
  curl -sk -G "$B/MAKS_Guncel_WGS84/FeatureServer/2/query" --data-urlencode "where=siteveyakooperatifadi IS NOT NULL AND siteveyakooperatifadi<>''" --data-urlencode returnCountOnly=true --data-urlencode f=json  # 283655
  # indirme: ...&outFields=*&resultOffset=0&resultRecordCount=2000&f=geojson  (döngü)
  ```

### A2. Manisa BB Açık Veri (CKAN) — mahalle tabloları — puan 2
- URL: `https://acikveri.manisa.bel.tr/api/3/action/package_search?q=mahalle` → 107 set
- Lisans: portalda "Manisa Büyükşehir Belediyesi" (lisans alanı boş → **belirsiz**)
- İçerik: ilçe başına `*-mahalle-bilgileri` (XLSX nüfus, XLS "mahalle ve yapı tipine göre yapı sayıları 2025-10", JPEG mahalle krokileri), `mahalle-bazinda-nufus-verisi` XLSX. **Mahalle sınırı GeoJSON/SHP yok** (krokiler JPEG).
- Değer: mahalle listesi/nüfus/yapı tipi sayıları (önceliklendirme); polygon yok.
- Kanıt: `curl -s 'https://acikveri.manisa.bel.tr/api/3/action/package_search?q=mahalle&rows=0' | jq .result.count` → 107

### A3. Diğer hedef il uçları (bu oturumda tarandı: `acikveri.`, `cbs.`, `kentrehberi.` alt alanları; `/api/3/action/package_list`, `/arcgis/rest/services`, `/server/rest/services`, `/geoserver/ows|wfs`)
| İl | Host | Sonuç |
|---|---|---|
| Mersin | acikveri./cbs./gis./kentrehberi./sehirharitasi.mersin.bel.tr | **bağlantı yok (000)** — DNS/erişim yok. (3. turda ULASAV'da yalnızca Yenişehir ilçesi mahalle sınırı vardı.) |
| Adana | cbs.adana.bel.tr | 200; `/arcgis/rest/services?f=json` → **ArcGIS Web Adaptor yardım sayfası (HTML)**, JSON vermiyor → servis dizini kapalı/yetkili. acikveri./kentrehberi. → 000 |
| Hatay | cbs.hatay.bel.tr, kentrehberi.hatay.bel.tr | 200; **Anka Software (ankapano) SPA**; REST/WFS uçları 404; `/rest/services` → `{"success":false,"message":"Page Not Found"}` → API keşfedilemedi |
| Kahramanmaraş | kentrehberi.kahramanmaras.bel.tr | **ArcGIS REST açık → A1** |
| Afyonkarahisar | cbs.afyon.bel.tr | 200 → **"Giriş Yap"** (kimlik gerekli) |
| Manisa | cbs.manisa.bel.tr | SPA (her yol aynı HTML); acikveri → A2 |
| Samsun | cbs.samsun.bel.tr | 404 (IIS); acikveri./kentrehberi. → 000 |
| Eskişehir | acikveri.eskisehir.bel.tr | **403** (3. turla aynı); cbs. → 504 |
| Diyarbakır | cbs.diyarbakir.bel.tr | `/geoserver/ows` → **401 Unauthorized** (GeoServer var, anonim kapalı) |
| Erzurum | cbs.erzurum.bel.tr | `/geoserver/ows` → **401** (GeoServer var, anonim kapalı) |
| Malatya | cbs.malatya.bel.tr | SPA (her yol 200 aynı HTML) |
| Şanlıurfa | cbs./kentrehberi.sanliurfa.bel.tr | 504 |
| Van | cbs.van.bel.tr | 200 IIS varsayılan; REST/WFS 404 |
| Tekirdağ | cbs.tekirdag.bel.tr | **503** |

## (B) Kompleks düzeyi / önceliklendirme kaynakları

### B1. Overture Maps — `base/land_use` residential + `places` (TR) — puan 4
- URL: `s3://overturemaps-us-west-2/release/2026-09-23.1/theme=base/type=land_use/` (anonim S3; DuckDB `httpfs`)
- Lisans: ODbL (OSM türevi land_use) / Overture places CDLA-Permissive-2.0 (Meta+Microsoft kaynaklı)
- Erişim: DuckDB `read_parquet(... hive_partitioning=1)` bbox filtresi; geo env'de duckdb 1.5.6 mevcut
- Kayıt (ölçüldü, TR bbox 25.6–44.9 / 35.8–42.2): **153.581 residential land_use polygonu, 16.516'sı adlı** (`names.primary`); Mersin merkez bbox'ta 168 residential + 24.047 place
- Değer: adlı residential polygonlar = OSM `landuse=residential + name=*` → doğrudan "site" adayı (3. turdaki OSM çıkarımıyla çakışabilir ama polygon+ad birlikte, kolay `ST_Within` ile mahalle atama); adsızlar konut alanı önceliklendirmesi için.
- Kanıt:
  ```python
  duckdb: SELECT count(*), sum(CASE WHEN names.primary IS NOT NULL THEN 1 ELSE 0 END)
          FROM read_parquet('s3://overturemaps-us-west-2/release/2026-09-23.1/theme=base/type=land_use/*', hive_partitioning=1)
          WHERE bbox.xmin>25.6 AND bbox.xmax<44.9 AND bbox.ymin>35.8 AND bbox.ymax<42.2 AND class='residential'  -- (153581, 16516)
  ```

### B2. Overture `addresses` teması — TR kapsamı **YOK** — puan 0
- `docs.overturemaps.org/guides/addresses/`: 41 ülke, 474 M adres (alpha); ülke listesinde Türkiye yok. Doğrulama: aynı release'de TR bbox sorgusu → **0 satır** (21 s). OpenAddresses'ta da `tr/` yok (2. tur). Türkiye için açık adres noktası kaynağı hâlâ yalnızca NVİ/MAKS (belediye REST'leri, bkz. A1).

### B3. Copernicus Urban Atlas 2018 — Türkiye FUA'ları — puan 3
- URL: `https://land.copernicus.eu/en/products/urban-atlas/urban-atlas-2018` (CLMS Plone API `++api++` ile ürün JSON'u; indirme CLMS hesabı + `@datarequest_post` API ister — anonim doğrudan link yok)
- Lisans: Copernicus açık veri politikası (Reg. (EU) 1159/2013) — "full, open and free access", atıf
- İçerik: **76 Türkiye FUA** (TR001 Ankara, TR002 Adana-Mersin, TR007 Diyarbakır, TR009 Erzurum, TR011 Antakya, TR019 Malatya, TR022 Samsun, TR025 Van, TR027 Eskişehir, TR028 Şanlıurfa, TR029 Kahramanmaraş, TR039 Afyonkarahisar …) — 11xxx konut dokusu sınıfları (yoğunluk dereceli), 12100 sanayi/ticari; FlatGeobuf/GPKG, MMU 0,25 ha
- Değer: hedef illerde "sürekli yoğun konut dokusu" polygonları → mahallesiz kayıtların site/toplu konut olma olasılığı ve konut alanı önceliklendirme; kompleks adı yok.
- Kanıt: `curl -s 'https://land.copernicus.eu/++api++/en/products/urban-atlas/urban-atlas-2018' -H 'Accept: application/json' | grep -oE 'TR0[0-9]{2}L1_[A-Z_]+' | sort -u | wc -l` → 76; indirme bu oturumda **denenmedi** (hesap gerekli).

### B4. Meta Data for Good — Türkiye HRSL nüfus yoğunluğu (HDX) — puan 3
- URL: `https://data.humdata.org/dataset/turkey-high-resolution-population-density-maps-demographic-estimates`
- Lisans: **CC BY 4.0** (humanitarian-only değil) · dosyalar: `population_turkey_2020_tif.zip` (57 MB), CSV 135 MB; yaş/cinsiyet katmanları
- Değer: 30 m nüfus rasteri → mahallesiz kayıtların çevresindeki nüfus yoğunluğu (konut/plaza ayrımı, önceliklendirme). Meta "relative-wealth-index" TR de CC BY.
- Kanıt: `curl -s 'https://data.humdata.org/api/3/action/package_show?id=turkey-high-resolution-population-density-maps-demographic-estimates' | jq '.result.license_title'` → "Creative Commons Attribution International (CC BY)"

### B5. GHSL (GHS-BUILT-S / BUILT-V / POP 2023A, 100 m & 10 m) — puan 3
- URL: `https://jeodpp.jrc.ec.europa.eu/ftp/jrc-opendata/GHSL/GHS_BUILT_S_GLOBE_R2023A/.../tiles/*.zip` (anonim FTP/HTTP; tile R5_C21 TR'yi kapsar)
- Lisans: AB açık veri — "Reuse authorised with proper acknowledgment"
- Değer: yapı yüzeyi/hacmi (konut vs konut-dışı ayrımı: `GHS_BUILT_S_NRES`) → plaza/sanayi vs konut önceliklendirme; ad yok.
- Kanıt: `curl -r 0-100 -o /dev/null -w '%{http_code}' '.../GHS_BUILT_S_E2020_GLOBE_R2023A_54009_100_V1_0_R5_C21.zip'` → 206

### B6. WorldPop TR — puan 2
- URL: `https://hub.worldpop.org/rest/data/pop/wpgp?iso3=TUR` → `https://data.worldpop.org/GIS/Population/Global_2000_2020/2020/TUR/tur_ppp_2020.tif`
- Lisans: CC BY 4.0 (`hub.worldpop.org/data/licence.txt`) · HDX kopyası "Other"
- Değer: B4 ile aynı amaç, daha kaba (100 m); B4 yeterliyse gereksiz.

### B7. TÜİK Coğrafi İstatistik Portalı (cip.tuik.gov.tr) — puan 1
- URL: `https://cip.tuik.gov.tr/` (React SPA; `configData.js` → `apiUrl: https://cip.tuik.gov.tr/Home/GetMapData?kaynak=&duzey=&gostergeNo=&kayitSayisi=&period=`; geometri `assets/geometri/nuts{duzey}.json`)
- Lisans: TÜİK veri kullanım koşulları (atıf; ToS'ta toplu indirme hakkında açık ifade yok → belirsiz)
- Bulgu: `uygunDuzeyler: [2,3,4,5]` → düzey 5 **mahalle** göstergeleri var (ADNKS). Ancak `nuts5.json` 404 (mahalle geometrisi istemciye yüklenmiyor; muhtemelen vektör tile/ayrı uç), `GetMapData` tüm denemelerde `500 Object reference not set` (parametre kombinasyonu çözülemedi — `gostergeNo` menüden alınmalı). Mahalle düzeyi **nüfus** var, **konut sayısı** göstergesi doğrulanamadı.
- Kanıt: `curl 'https://cip.tuik.gov.tr/configData.js'` → `uygunDuzeyler [2,3,4,5]`; `curl .../assets/geometri/nuts3.json` → 200 (233 KB il geometrisi); `nuts5.json` → 404
- Durum: **erişilemedi (API keşfi tamamlanamadı)**; tarayıcı ile menü JSON'u izlenirse çözülebilir.

### B8. Kültür ve Turizm Bakanlığı — Bakanlık belgeli tesis istatistikleri — puan 1
- URL: `https://yigm.ktb.gov.tr/TR-201136/turizm-yatirim-ve-isletme-belgeli-tesisler.html` → `/Eklenti/152765,2026-eylul-bakanlik-belgelixlsx.xlsx`
- Lisans: kamu (belirsiz) · İçerik: **il bazında tesis/oda/yatak sayısı** (91 string; yalnızca agregat). Tesis adı/adres listesi bu sayfada yok; `turizm-tesisleri.html` sayfasında indirilebilir liste bulunamadı. Apart/rezidans tespiti için **yetersiz**.
- Kanıt: `curl -skL -o k.xlsx '...152765...'` (17 KB) → sharedStrings: 'YATIRIM BELGELİ','Şehir','Tesis Sayısı','Oda Sayısı'…

### B9. TOBB Sanayi Veri Tabanı — puan 1
- URL: `https://sanayi.org.tr/#/sanayi-veri-tabani` (eski `sanayi.tobb.org.tr` 301) · reCAPTCHA'lı SPA; `api/firma` → 401. Toplu erişim yok; firma düzeyi (işyeri adresi) → plaza için kiracı bilgisi, kompleks değil. Kişisel veri riski düşük ama ToS belirsiz. **Kapsam dışı bırakılması önerilir.**

### B10. MERSİS / ESBİS (Ticaret Bakanlığı) — puan 0
- `https://mersis.ticaret.gov.tr/` 200 ama e-Devlet girişli; toplu sorgu/indirme yok; ticari sicil adresleri kişisel veri içerebilir → **kullanılmaz**.

### B11. Mapillary map features (point features) — puan 1
- `https://graph.mapillary.com/map_features?bbox=…` → token zorunlu (`Invalid OAuth 2.0 Access Token`); lisans CC BY-SA 4.0 (ToS); 2026-01'den beri bbox ≤ 0,01° kısıtı. Nesne sınıfları trafik/sokak mobilyası — site/plaza tanıyan sınıf yok. Değer yok.

### B12. Microsoft "Places" açık seti — puan 0
- `github.com/microsoft/places` = eski Lumia SensorCore örneği; bağımsız açık POI seti yok. Microsoft POI katkısı zaten **Overture places** içinde (B1). Apple/TomTom açık POI seti yok (TomTom Overture üyeliği yalnızca ulaşım teması).

### B13. OpenPOIMap / Geoapify / LocationIQ — puan 1
- OpenPOIMap: Overpass üzerinde OSM görüntüleyici (ODbL, yeni veri yok). Geoapify/LocationIQ ücretsiz katmanları: OSM türevi (ODbL) + kendi ToS'ları (önbellekleme/toplu indirme yasağı) → yeni kaynak değil, kapsam dışı.

### B14. EPDK/TEDAŞ abone yoğunluğu, SGK/İŞKUR işyeri adresleri — puan 0
- EPDK yıllık sektör raporu ve SGK istatistikleri **il düzeyi PDF/XLSX agregat**; adres/koordinat yok. SGK işyeri kayıtları ve İŞKUR işveren verileri kamuya açık değil (kişisel/ticari sır). Kullanılmaz.

### B15. KOSGEB / OSBÜK dışı sanayi: Sanayi Bak. "Sanayi Sicil" — puan 0
- e-Devlet/giriş gerektirir; toplu erişim yok (MEYDİP 2. turda).

## Öneriler (öncelik sırası)
1. A1'i hemen çek: `Mahalle/0` (716) → `ST_Within` ile 183 K.Maraş kaydını doldur; `MAKS_Guncel_WGS84/2` YAPI'yı `resultOffset` döngüsüyle GeoJSON'a al, `siteveyakooperatifadi` normalize et (blok adlarını ayıkla: `^[A-Z]-?\d+ ?BLOK$`, `^[A-Z]\d+-\d+$`), `ST_Union` + centroid → yeni site adayları; mevcut kayıtlarla ad+mesafe eşleştir.
2. Belediyeye (K.Maraş BB Bilgi İşlem/CBS) e-posta ile yeniden kullanım lisansını sor; kaydet.
3. B1 Overture residential adlı polygonları TR geneli için çek (duckdb → GPKG), OSM'den gelen mevcut kayıtlarla `osm_id` üzerinden tekilleştir.
4. Diyarbakır/Erzurum GeoServer (401) ve Adana ArcGIS için belediye CBS birimlerine anonim WFS erişimi talebi; Mersin BB açık veri portalı DNS'siz → MBB web sitesinden güncel adres sorulmalı.
5. TÜİK CİP: tarayıcıda menü isteğini (gösterge listesi JSON) yakalayıp `GetMapData` parametrelerini çöz; mahalle nüfusu/konut sayısı alınabilirse önceliklendirmeye ekle.
