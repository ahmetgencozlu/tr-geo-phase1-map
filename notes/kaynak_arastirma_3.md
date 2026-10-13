# Kaynak Araştırması 3 — Mahalle düzeyi idari veri + kompleks düzeyi yeni açık kaynaklar

Tarih: 2026-10-13 · Yöntem: web araması + her aday için `curl`/`ogrinfo` doğrulaması (sudo yok, tarayıcı yok). Önceki turda değerlendirilenler (kaynak_arastirma_2.md: Wikidata, GeoNames, Emlak Konut, ULASAV genel, İzmir Bina Kimlik, İBB, MEYDİP, TOKİ, KDB, TKGM, MS/Google Buildings, OpenAddresses, KAP, Konya/Bursa/Ankara portalları, ArcGIS Hub/HF/Kaggle, AllThePlaces/Foursquare/Overture/OSM) **tekrar edilmedi**.
Kapsam dışı: Google/Yandex/Sahibinden/Emlakjet/Zingat/Hürriyet Emlak.

Puan: 5 = doğrudan büyük hacimli, açık lisanslı, koordinatlı; 1 = erişilemez/alakasız.
Çalışma dosyaları: `/opt/data/cache/scratch/ka3/` (indirilen shp/gpkg/geojson örnekleri).

**Genel sonuç (20 aday):** Mahalle polygonu boşluğu (14.695 kayıt) için **gerçek kazanım var**: belediye açık veri portallarındaki mahalle sınırları ve bunları tek şemada toplayan `ttezer/turkiye-harita-verisi` (11.112 polygon, 25 il, 392 ilçe). Kompleks düzeyinde yeni *toplu* kaynak yine zayıf; en iyisi Wikimedia Commons kategori-koordinatları (birkaç yüz isimli rezidans/plaza) ve Milli Emlak ilan API'si (ada/parsel düzeyi, arsa ağırlıklı). Deprem konutları, PTT posta kodu, NVİ, teknokent firma rehberleri bu oturumda ya erişilemedi ya da toplu veri vermiyor.

---

## Hemen entegre edilebilir (en iyi 5)

### 1. ttezer/turkiye-harita-verisi — il/ilçe/mahalle GeoJSON derlemesi — puan 4
- URL: `https://github.com/ttezer/turkiye-harita-verisi` · ilçe başına dosya: `https://raw.githubusercontent.com/ttezer/turkiye-harita-verisi/master/dist/geojson/mahalle-geometrileri-by-district-v2/TR-D-<il>-<ilçe>.geojson` · kapsam listesi: `.../dist/geojson/mahalle-geometrileri-coverage.json`
- Lisans: kod MIT; **veri kaynağa göre** (DATA-LICENSE.md): il/ilçe = HDX COD-AB-TUR (CC BY-IGO); mahalle = belediye açık veri lisansları (Ankara CC BY 4.0, Kayseri CC BY 4.0, Konya CC BY, Bursa/Denizli/Gaziantep/Muğla/Ordu/Sakarya/Sivas/Trabzon kendi açık veri lisansı — atıf ile serbest) **veya** "kullanıcı tarafından sağlanan KML" (İstanbul, İzmir, Antalya, Aydın, Balıkesir, Kocaeli, Tekirdağ, Kırklareli, Edirne, Çanakkale, Yalova, Bolu, Düzce, Zonguldak → kaynak/lisans **belirsiz**, "Kamu verisi" notu var). Her feature'da `source_name` alanı olduğu için belediye-lisanslı olanları filtrelemek mümkün.
- Erişim: dosya (GitHub raw; 392 ilçe dosyası, toplam ~151 MB). Son push 2026-07.
- Alanlar: `id, level, type (mahalle), parent_id, province_id, district_id, name, source_raw_name, source, source_label, source_name, geometry (MultiPolygon, WGS84)`
- Kapsam/kayıt: **11.112 mahalle polygonu**, 25 il (06, 07, 09, 10, 14, 16, 17, 20, 22, 27, 34, 35, 38, 39, 41, 42, 48, 52, 54, 58, 59, 61, 67, 77, 81), 392 ilçe.
- Değer: Mahallesiz 14.695 kaydın bulunduğu ilçelerle kesişimi hesaplanmalı; kapsam listesinde İstanbul/İzmir/Ankara/Antalya/Kocaeli/Bursa gibi envanterin yoğun olduğu iller var. Belediye-lisanslı iller doğrudan kullanılabilir; "kullanıcı KML" illeri için orijinal belediye portalına gidip (madde 2) doğrudan indirmek daha temiz.
- Kanıt:
  ```bash
  curl -sO https://raw.githubusercontent.com/ttezer/turkiye-harita-verisi/master/dist/geojson/mahalle-geometrileri-by-district-v2/TR-D-34-001.geojson
  ogrinfo -so -al TR-D-34-001.geojson | grep -E 'Feature Count|name'   # Feature Count: 5 (Adalar) — name, source_name=Istanbul Mahalle KML
  curl -s https://raw.githubusercontent.com/ttezer/turkiye-harita-verisi/master/dist/geojson/mahalle-geometrileri-coverage.json | python3 -c "import json,sys;d=json.load(sys.stdin);print(len(d['province_ids']),len(d['district_ids']))"  # 25 392
  ```

### 2. Belediye açık veri portalları — mahalle sınırı setleri (doğrudan) — puan 4
- URL'ler (bu oturumda doğrulananlar):
  - Kayseri BB: `https://acikveri.kayseri.bel.tr/veri-seti/kayseri-mahalle-siniri/1064` → `/uploads/data/2026/4/15/mahalle_siniri-1829771.geojson` (ayrıca CSV, SHP) — **CC BY 4.0**, 711 mahalle, alanlar `CBNO, ILCE_CBNO, ADI, ALAN, CEVRE, NUFUS, POSTAKODU`
  - Trabzon BB: `https://acikveri.trabzon.bel.tr/dataset/adrese-dayali-mahalle-sinirlari` → `.../download/mahalle.geojson` — Trabzon Açık Veri Lisansı, 716 mahalle (`objectid, ad`); ayrıca ilçe bazlı "mahalle bazlı yapı tipleri" XLSX'leri
  - ULASAV (CKAN HTML üzerinden): `/dataset/16-mahalle-sinirlari` (Bursa), `/dataset/34-2-kadikoy-ilcesindeki-mahalle-sinirlari`, `/dataset/58-sivas-mahalle-siniri-haritasi`, `/dataset/akillisehirler-gaziantep-il-ilce-mahalle-snrlar-7efb34bd`, `/dataset/akillisehirler-kayseri-il-ilce-verileri-393d9e74`, `/dataset/karakecili-mahalle-sinirlari`, `/dataset/331-mersin-yenisehir-ilcesi-mahalle-sinirlari`, `/dataset/osmaniye-merkez-mahalle-sinirlari`, `/dataset/gumushane-ili-merkez-mahalle-sinirlari`, `/dataset/61-adrese-dayali-mahalle-sinirlari`
  - Bu oturumda **erişilemeyen** (bağlantı zaman aşımı / 403): Denizli (`acikveri.denizli.bel.tr/dataset/denizli-ili-mahalle-sinirlari`), Sakarya, Ordu, Sivas, Gaziantep, Eskişehir (403), Samsun, Mersin; Ankara `seffaf.ankara.bel.tr/dataset?q=mahalle` 404 (portal yapısı değişmiş). ttezer deposu bunların snapshot'ını içeriyor.
- Lisans: belediye başına (CC BY 4.0 veya kendi "Açık Veri Lisansı"; hepsi atıfla ticari dahil serbest).
- Erişim: dosya (GeoJSON/SHP/KML/CSV).
- Kapsam/kayıt: il başına 300–1.200 polygon; toplamda (ttezer ile birlikte) ~11 k.
- Değer: Mahallesiz kayıtların ilçelerine göre eşleşen belediye seti varsa `ST_Within` ile doğrudan doldurma. Kayseri setinde **posta kodu + nüfus** de var.
- Kanıt:
  ```bash
  curl -sLo kayseri.geojson 'https://acikveri.kayseri.bel.tr/uploads/data/2026/4/15/mahalle_siniri-1829771.geojson'
  ogrinfo -so -al kayseri.geojson | grep -E 'Feature Count|ADI|POSTAKODU'   # 711 · ADI · POSTAKODU
  curl -sLo trabzon.geojson 'https://acikveri.trabzon.bel.tr/dataset/85e19bec-3d71-4531-96ee-0fca5f6a3f87/resource/1957c719-5c32-4bf9-874d-e9769c2d0ef9/download/mahalle.geojson'
  ogrinfo -so -al trabzon.geojson | grep 'Feature Count'   # 716
  ```

### 3. Wikimedia Commons — koordinatlı kategoriler (rezidans/plaza/kule) — puan 3
- URL: `https://commons.wikimedia.org/w/api.php` · `list=geosearch&gsnamespace=14` (kategori koordinatları) ve `list=categorymembers` (`Category:Residential_skyscrapers_in_Istanbul`, `Category:Apartment_buildings_in_Turkey_by_city`, `Category:Housing_estates_in_Turkey`, `Category:Residential_buildings_in_Turkey_by_city`)
- Lisans: kategori adı/koordinat = olgu verisi; Commons yapısal verisi **CC0**; metinler CC BY-SA (alınmıyor). API: UA zorunlu, hız sınırı.
- Erişim: API (JSON).
- Alanlar: kategori adı (= kompleks adı), lat/lon, bağlı Wikidata QID (varsa `prop=pageprops`).
- Kapsam/kayıt: `Residential skyscrapers in Istanbul` 16 alt kategori (Akasya Residence, Anthill Residences, Maçka Residences, Nurol Life, Onaltı Dokuz, Trump Towers…); Ataşehir merkezli 3 km geosearch'te 43 koordinatlı kategori (Metropol İstanbul Tower A/B/C…). Tahmin: TR genelinde **200–500** isimli rezidans/plaza/AVM kategorisi; `Housing estates in Turkey` sadece 1 dosya (konut sitesi için zayıf).
- Değer: Wikidata'yı tamamlayan bağımsız ikinci kaynak; özellikle İstanbul/Ankara/İzmir'deki plaza/rezidans kuleleri için ad+koordinat doğrulaması. Konut sitesi (kooperatif/TOKİ) için neredeyse yok.
- Kanıt:
  ```bash
  curl -s -A 'yerdex/0.1' 'https://commons.wikimedia.org/w/api.php?action=query&list=geosearch&gscoord=40.99|29.12&gsradius=3000&gslimit=500&gsnamespace=14&format=json' | python3 -c "import json,sys;m=json.load(sys.stdin)['query']['geosearch'];print(len(m),[x['title'] for x in m][:4])"
  # → 43 ['Category:Metropol İstanbul Tower C', 'Category:Metropol İstanbul', ...]
  ```
  Toplu çekim: TR'yi 10 km'lik grid ile tarayıp (gsradius max 10000) ad regex (`Residence|Rezidans|Plaza|Tower|Kule|Sitesi|Konutları|Evleri|AVM|Teknopark`) ile filtrele.

### 4. Kontur Boundaries Türkiye (OSM mahalle polygonları + nüfus) — puan 3
- URL: `https://data.humdata.org/dataset/kontur-boundaries-turkey` → `https://geodata-eu-central-1-kontur-public.s3.amazonaws.com/kontur_datasets/kontur_boundaries_TR_20230628.gpkg.gz` (32 MB)
- Lisans: **ODbL** (OSM türevi + Kontur Population; atıf + share-alike)
- Erişim: dosya (GeoPackage).
- Alanlar: `admin_level (Kontur), osm_admin_level, name, name_en, population, hasc, geometry`
- Kapsam/kayıt: 11.682 polygon; admin_level 10 = **10.573 mahalle/köy** (9.712'si adında "Mahalle"), 973 ilçe, 81 il; her birinde **nüfus tahmini** (400 m H3 agregası).
- Değer: OSM türevi olduğu için OSM'de olmayan mahalleleri **doldurmaz**; ama (a) 2023 snapshot'ı — OSM'de sonradan silinmiş/bozulmuş ilişkileri yakalar, (b) mahalle nüfusu ile "yoğun konut mahallesi" önceliklendirme, (c) envanterdeki 14.695 mahallesiz kaydın gerçekten OSM dışı mı olduğunu çapraz kontrol. Hemen ekleme maliyeti düşük (tek gpkg).
- Kanıt:
  ```bash
  curl -sLo k.gpkg.gz https://geodata-eu-central-1-kontur-public.s3.amazonaws.com/kontur_datasets/kontur_boundaries_TR_20230628.gpkg.gz && gunzip k.gpkg.gz
  ogrinfo k.gpkg -dialect SQLite -sql "select admin_level,count(*) from boundaries group by 1" -q   # level 10 → 10573
  ```

### 5. Milli Emlak (MEBİS) ilan API — Hazine taşınmaz satış/kira ilanları — puan 2
- URL: `https://mebis-s-p.csb.gov.tr/api/MileWeb/GetSatisIlanList` (POST JSON `{"pageNumber":1,"pageSize":100}`; max 100/sayfa), `GetKiraIlanList`, `GetIrtifakIlanList`, `GetYatirimTesvikIlanList`, `GetIlanById`; arayüz `https://www.milliemlak.gov.tr`
- Lisans: **belirsiz** (kamu ilanı; API resmi olarak belgelenmemiş, Angular istemcisinin kullandığı uç nokta; giriş gerekmiyor). Olgu verisi (il/ilçe/mahalle/ada/parsel/cins/yüzölçümü) alınmalı; fotoğraflar alınmamalı.
- Erişim: API (JSON, sayfalı; `totalRow` döner).
- Alanlar: `il, ilce, mahalle, mahalle_id, ada, parsel, pafta, tasinmaz_cinsi (Arsa/Arazi/Tarla/…katlı bina…), imar_ozellik, yuzolcumu, ihale_tarihi, toplam_tahmini_bedel`; **koordinat yok** (ada/parsel → TKGM parsel sorgusu gerekir).
- Kapsam/kayıt: satış **709** aktif ilan (örnek 50'de 45 arsa, 3 arazi, 2 tarla); kira/irtifak ayrıca. Bina cinsi (`.. Katlı İdari Hizmet Binası`, lojman vb.) `GetIlanAramaModel` → `tasinmazCinsiList` içinde 1.000+ cins tanımı var ama ilanların çoğu arsa.
- Değer: Kompleks envanteri için düşük (arsa ağırlıklı, koordinatsız); ancak resmi **mahalle_id → mahalle adı** sözlüğü (NVİ/MAKS kimlikleri) ilan akışından toplanabilir; kamu lojmanı/site satışları nadiren çıkar.
- Kanıt:
  ```bash
  curl -s -H 'Content-Type: application/json' -H 'Origin: https://www.milliemlak.gov.tr' -d '{"pageNumber":1,"pageSize":1}' https://mebis-s-p.csb.gov.tr/api/MileWeb/GetSatisIlanList | python3 -c "import json,sys;d=json.load(sys.stdin);print(d['totalRow'],d['response_object'][0]['tasinmazlar'][0]['mahalle'])"
  # → 709 Susuz Köyü
  ```

---

## Diğer değerlendirilen kaynaklar

| # | Kaynak | URL | Lisans | Erişim | Alanlar / kayıt | Sonuç / puan |
|---|---|---|---|---|---|---|
| 6 | HDX "Settlements in Turkey" (HGM yerleşim noktaları adm3/adm4) | `data.humdata.org/dataset/settlements-in-turkey` (`turkey_settlements_3.zip`, `_4.zip` — dikkat: **RAR** paketlenmiş, `bsdtar` açar) | **"Shared by the General Command of Mapping for humanitarian use only"** — ticari/genel kullanım **kapalı** | dosya (SHP point) | adm3: **49.662 nokta** (Mahalle 29.903, Köy 18.281, Kırsal Mahalle 1.085, Belde 392) `adm3_tr, adm2_tr, adm2_code, adm1_tr`; adm4: 43.412 nokta (öznitelik yok) | Türkiye'nin en kapsamlı resmi mahalle **nokta** listesi ama lisans projeye uygun değil; sadece dahili doğrulama/isim normalizasyonu için bakılabilir, yeniden dağıtılamaz. **Puan 2** |
| 7 | HDX COD-AB-TUR (il/ilçe) | `data.humdata.org/dataset/cod-ab-tur` | CC BY-IGO | dosya (GeoJSON/SHP/GDB/XLSX) | 81 il, 973 ilçe, P-code (`TUR001001`) | Mahalle yok; ilçe P-code standardı için faydalı. **Puan 2** |
| 8 | Who's On First — admin TR | `data.geocode.earth/wof/dist/shapefile/whosonfirst-data-admin-tr-latest.zip` (8.9 MB) | CC BY (çoklu kaynak; bazı geometriler ODbL) | dosya (SHP) | county polygon 957; **neighbourhood polygon 778** (774'ü tek parent = İstanbul, Galata vb.); neighbourhood point 960; locality point 51.367; campus 8 | İstanbul dışı mahalle polygonu yok; locality noktaları GeoNames kopyası. **Puan 2** |
| 9 | geoBoundaries TUR | `geoboundaries.org/api/current/gbOpen/TUR/ALL/` | ADM0/1 CC BY-SA 2.0, ADM2 ODbL | API/dosya | ADM2 999 ilçe; **ADM3 yok** | **Puan 1** |
| 10 | HOT OSM Türkiye exports (`hotosm_tur_buildings`, `_destroyed_buildings`) | `data.humdata.org/dataset/hotosm_tur_buildings` (gpkg 488 MB, 2026-05) | ODbL | dosya | OSM bina polygonları; yıkılan binalar (2023 deprem) 169 KB GeoJSON | Zaten OSM; yeni bilgi yok. **Puan 1** |
| 11 | OSBÜK — OSB listesi | `https://www.osbuk.org/view/osb/osbliste.php` | belirsiz (kamu dernek sayfası) | sayfa (HTML tablo) | **~360 OSB**: il, ünvan, fiili durum (İŞLETMEDE / ALTYAPI HAZIRLANIYOR / KAMULAŞTIRMA), e-posta, web, adres; koordinat yok | OSB adı+ilçe doğrulaması için; parsel haritası/WMS yok (MEYDİP kapalı). **Puan 2** |
| 12 | Sanayi ve Teknoloji Bak. — TGB Listesi | `https://teknopark.sanayi.gov.tr/Home/TgbListesi` | belirsiz (kamu) | sayfa (HTML tablo, 115 satır) | bölge adı, e-posta, **adres (mahalle/bulvar)**, il, telefon | Mevcut TGB listesiyle büyük ölçüde örtüşür; adres alanı geocoding için ek. Firma rehberi yok. **Puan 2** |
| 13 | Teknopark İstanbul firma listesi | `https://www.teknoparkistanbul.com.tr/firmalar` | belirsiz | sayfa (HTML, logo `alt`) | **150 firma adı**, sektör; adres/bina yok (`firma_katalogu.pdf` 404) | Bina/kompleks değil, kiracı listesi. **Puan 1**. ODTÜ Teknokent (`/tr/firmalar/` → notfound), Hacettepe/Yıldız/İTÜ Arı `/firmalar` 404, Cyberpark/Ankara Teknopark bağlantı yok, DEPARK 530. |
| 14 | Resmî Gazete — rezerv/riskli alan kararları | `https://www.resmigazete.gov.tr/arama?q=rezerv+yapı+alanı` ; günlük `eskiler/YYYY/MM/YYYYMMDD-N.htm` | kamu (telif yok) | sayfa (arama JS ile dolduruluyor; sonuç listesi curl ile gelmedi) | karar metni: il/ilçe/mahalle + ada/parsel listesi; **koordinat yok**, kroki PDF ekinde raster | Toplu çekim için RG'nin gün bazlı HTML'lerini tarayıp "Rezerv Yapı Alanı" regex'i gerekir; koordinat ada/parselden türetilmeli. **Puan 2** (manuel, ~200–400 alan) |
| 15 | PTT Posta Kodu listesi | `https://postakodu.ptt.gov.tr/` → 302 `https://www.ptt.gov.tr/posta-kodu` → **bağlantı kurulamadı (000)** | belirsiz | dosya (bilinen: il/ilçe/semt/mahalle/PK XLSX, ~50 k satır) | — | Bu oturumda erişilemedi; mahalle adı normalizasyonu için ileride tekrar denenmeli. Kayseri setinde zaten POSTAKODU alanı var. **Puan 2 (doğrulanamadı)** |
| 16 | NVİ Adres Sorgu / TÜİK MEDAS mahalle | `adres.nvi.gov.tr/VatandasIslemleri/AdresGenelSorgu`, `biruni.tuik.gov.tr/medas/?kn=95` | kamu; ToS belirsiz | sayfa (etkileşimli form/captcha; API yok) | mahalle/CSBM/kapı listesi (tekil sorgu) | Toplu erişim yok; kişisel veri riski (AdresKisiSorgu kesinlikle kapsam dışı). **Puan 1** |
| 17 | GitHub mahalle isim listeleri: `ubeydeozdmr/turkiye-api` (MIT, 177★), `metinyildirimnet/turkiye-adresler-json` (lisans yok), `ismetcanbyk/turkey-location-data` (Apache-2.0), `nafizb/Turkey-Geo-Neighborhood-Database` (lisans yok), `kcole93/turkey-closed-neighborhoods` (MIT, yabancılara kapalı mahalleler GeoJSON) | github.com/… | kod lisansı var, **veri kaynağı belirsiz** (çoğu PTT/NVİ kazıması) | dosya (JSON) | il→ilçe→mahalle adları (~50 k), bazılarında CSBM; geometri yok | İsim normalizasyonu/ilçe eşleme için yedek; lisans belirsizliği nedeniyle yeniden dağıtılmamalı. `api.turkiye-api.dev` boş döndü. **Puan 2** |
| 18 | ÇŞB / TOKİ / AFAD deprem konutları listeleri | `csb.gov.tr/deprem-konutlari` 404, `toki.gov.tr/deprem-konutlari` 404, `acikveri.afad.gov.tr` bağlantı yok | — | — | — | Toplu il/ilçe/konut-sayısı listesi bulunamadı (haberler/PDF basın bültenleri dışında). **Puan 1 (erişilemedi)** |
| 19 | HGM ücretsiz veriler | `harita.gov.tr/urunler/indirilebilir-verilerdosyalar/13` | HGM koşulları | dosya | il/ilçe yüzölçümleri, pafta indeksi, 3B test verisi | Mahalle/bina yok. **Puan 1** |
| 20 | OSMnames / Yelp Open Dataset | `osmnames.org/download/` (OSM türevi planet TSV, ODbL) · `yelp.com/dataset` 403, TR kapsamı yok | — | — | — | **Puan 1 / 0** |

---

## Öneriler (öncelik sırası)
1. **Mahalle boşluğu**: envanterdeki 14.695 mahallesiz kaydın ilçe kodlarını `mahalle-geometrileri-coverage.json` ile kesiştir; kapsanan ilçeler için ttezer GeoJSON'larını (belediye-lisanslı `source_name` olanları öncelikle) PostGIS'e yükle → `ST_Within`. Kullanıcı-KML illerinde (İstanbul, İzmir, Antalya, Kocaeli…) mümkünse belediyenin kendi portalından/ULASAV'dan orijinali indir, lisans alanına belediye lisansını yaz.
2. **Kontur gpkg**'yi yükle: hem 2023 OSM mahalle snapshot'ı hem nüfus; mahallesiz kayıtların kaçı Kontur polygonuna düşüyor hızlıca ölç.
3. **Commons geosearch** gridi (10 km, ns=14) → ad regex → Wikidata QID ile birleştir → Wikidata turuyla aynı eşleştirme hattı.
4. OSBÜK + TGB listesi: mevcut OSB/TGB kayıtlarının isim/ilçe/durum doğrulaması (geocoding sadece ilçe düzeyi).
5. Milli Emlak API'sini periyodik tarayıp (`totalRow`, 100/sayfa) `mahalle_id→ad` sözlüğü üret; bina cinsli ilanları (lojman/site) ayrı bayrakla.
6. Tekrar denenecekler: PTT posta kodu XLSX, Denizli/Sakarya/Ordu/Gaziantep/Mersin portalları (zaman aşımı), RG arama (JS; gün bazlı HTML taraması ile).

Erişilemeyenler açıkça: PTT (000), AFAD açık veri (000), csb/toki deprem konutları (404), Denizli/Sakarya/Ordu/Sivas/Gaziantep/Samsun/Mersin portalları (000), Eskişehir (403), Ankara şeffaf portal (404), teknokent firma sayfaları (404/000), MEBİS `GetSatisIlanListCount` (405), `api.turkiye-api.dev` (boş).
