# Kaynak Araştırması 2 — Yeni açık veri kaynakları (site / toplu konut / plaza / teknopark)

Tarih: 2026-10-13 · Yöntem: web araması + her aday için doğrudan `curl` doğrulaması (sudo yok, tarayıcı yok).
Kapsam dışı (ToS scraping yasağı / ticari): Google Maps, Yandex, Sahibinden, Hürriyet Emlak, Zingat, Emlakjet, poidata.io, Apify aktörleri, Kaggle'daki sahibinden/emlakjet türevi setler.

Puan ölçeği (bizim için değer): 5 = binlerce yeni isimli kompleks + koordinat + açık lisans; 1 = erişilemez veya alakasız.

**Genel sonuç:** 19 aday incelendi. Türkiye'de "site adı + konum" veren *toplu* ve *açık lisanslı* yeni bir kaynak **bulunamadı**. Kamu CBS servisleri (TUCBS, TKGM, MEYDİP) kayıt/e-Devlet arkasında; belediye açık veri portalları site düzeyinde veri yayınlamıyor. Gerçekçi kazanım: Wikidata + GeoNames ile **birkaç yüz** bağımsız doğrulama/yeni kayıt; Emlak Konut ile ~50 nitelikli proje; ULASAV/İzmir ile mahalle/bina düzeyi bağlam.

---

## Hemen entegre edilebilir (en iyi 5)

### 1. Wikidata SPARQL — puan 3
- URL: `https://query.wikidata.org/sparql`
- Lisans: **CC0** (toplu kullanım serbest; UA zorunlu, hız sınırı var)
- Erişim: API (SPARQL, CSV/JSON)
- Alanlar: isim (tr/en), lat/lon (P625), il/ilçe (P131), kategori (P31), açılış yılı, kat sayısı (bazıları), OSM ilişkisi (P402/P11693)
- Kapsam/kayıt: TR'de P31 ∈ {residential complex, skyscraper, shopping mall, office building, research park, apartment building, gated community, …} doğrudan sorgu: **3.634 öğe, 3.556'sı koordinatlı** (alt-sınıflar dahil çok daha fazla ama sorgu zaman aşımına düşüyor; sınıf başına bölmek gerek). Doğrudan: residential complex 17, skyscraper 36, AVM 7, teknopark 3.
- Değer: Az ama kaliteli ve **bağımsız 2. kaynak**; plaza/gökdelen/AVM için iyi, konut sitesi için zayıf. Yeni isim sayısı tahmini 100–300.
- Kanıt:
  ```bash
  curl -s -G https://query.wikidata.org/sparql -H 'Accept: text/csv' -A 'yerdex/0.1 (mailto:x@y)' \
    --data-urlencode 'query=SELECT ?i ?iLabel ?c WHERE { ?i wdt:P31 wd:Q11303; wdt:P17 wd:Q43 . OPTIONAL{?i wdt:P625 ?c} SERVICE wikibase:label { bd:serviceParam wikibase:language "tr,en". } } LIMIT 5'
  # → Q166882,Istanbul Sapphire,Point(29.0058 41.085) ...
  ```
- Not: Proje notunda "Wikidata (az)" zaten var; buradaki fark sınıf listesini genişletip (Q41176 alt sınıfları, P131=TR ilçeleri) parça parça çekmek.

### 2. GeoNames TR dump — puan 3
- URL: `https://download.geonames.org/export/dump/TR.zip`
- Lisans: **CC BY 4.0**
- Erişim: dosya (TSV, ~100 k satır, günlük güncel)
- Alanlar: isim + alternatif isimler, lat/lon, feature class/code (P.PPLX = mahalle/semt, L.AREA, S.BLDG…), il kodu (admin1), ilçe (admin2)
- Kapsam/kayıt: **99.886** TR kaydı; adında *sitesi / evleri / konutları / rezidans / plaza / teknokent* geçen **365** kayıt (ör. "Yüz Yıl Sitesi 39.8874 32.7994", "Batı Sitesi"). Çoğu Ankara/İstanbul'da PPLX olarak kodlanmış eski kooperatif siteleri.
- Değer: Düşük hacim ama ücretsiz, açık, bağımsız; özellikle 1970–90 kooperatif siteleri (OSM'de isimsiz olanlar) için faydalı. Koordinat hassasiyeti ~100 m–1 km (mahalle merkezi), eşleştirme eşiği gevşek tutulmalı.
- Kanıt:
  ```bash
  curl -sO https://download.geonames.org/export/dump/TR.zip && unzip -p TR.zip TR.txt | awk -F'\t' 'tolower($2) ~ /sitesi|evleri|plaza/ {print $2"\t"$5"\t"$6"\t"$8}' | head
  ```

### 3. Emlak Konut GYO proje sayfaları — puan 3
- URL: `https://www.emlakkonut.com.tr/tr-TR/proje-hava-gorselleri` (46 proje) · `https://satis.emlakkonut.com.tr/proje/<slug>` · `https://www.emlakkonut.com.tr/tr-TR/projeler`
- Lisans: **belirsiz** (site telif notu; robots engeli yok, ToS'ta scraping maddesi görülmedi). Yalnızca olgu verisi (isim, konum, konut sayısı) alınmalı, görsel/metin alınmamalı.
- Erişim: sayfa (HTML; JSON API yok). Satış alt sitesinde Google Maps embed URL'sinden koordinat düşüyor.
- Alanlar: proje adı, il/ilçe, konut/bağımsız bölüm sayısı, yüklenici, etap; satış sitesinde lat/lon (embed `!2d<lon>!3d<lat>`)
- Kapsam/kayıt: ~50 aktif + geçmiş projeler (toplamda 448 ihale, ama web'de liste olarak ~100)
- Değer: Az kayıt ama hepsi büyük, isimli, konut sayılı kompleks; TOKİ/Emlak Konut projeleri envanterde tek-kaynaklı olanlar için ikinci doğrulama.
- Kanıt:
  ```bash
  curl -s -A 'Mozilla/5.0' https://satis.emlakkonut.com.tr/proje/ebruli-kayasehir | grep -o '!2d[0-9.]*!3d[0-9.]*'
  # → !2d28.767920775741114!3d41.1254281122037   (lon, lat)
  ```
- Aynı yöntem halka açık diğer GYO'lar için denenebilir (Torunlar, Sinpaş, Nef, Ağaoğlu, Kiler GYO site "projeler" sayfaları) — her biri 10–40 proje; lisans aynı şekilde belirsiz.

### 4. ULASAV — Ulusal Akıllı Şehir Açık Veri Platformu (Çevre, Şehircilik ve İklim Değişikliği Bakanlığı) — puan 2
- URL: `https://ulasav.csb.gov.tr/dataset` (CKAN tabanlı)
- Lisans: veri seti bazında; çoğu "İBB/İzBB Açık Veri Lisansı" (atıf ile serbest) veya CC BY
- Erişim: sayfa + dosya (CSV/XLSX/GeoJSON/WMS). **CKAN API (`/api/3/...`) 403 döndürüyor**, HTML liste ve dosya linkleri çalışıyor.
- Alanlar: belediyeye göre değişir; 547 veri seti; ilgili olanlar: `34-mahalle-bazli-bina-analiz-verisi` (mahalle × yapım yılı × kat sayısı), `16-7-bina-metrekare-birim-degerleri`, Ankara ABB katmanları (WMS)
- Kapsam/kayıt: 30+ belediye; site düzeyi kayıt **yok**, mahalle/bina istatistiği var.
- Değer: Doğrudan yeni kompleks vermez; mahalle bazlı "kat sayısı 9+ bina yoğunluğu" gibi türev sinyallerle toplu konut adaylarını önceliklendirmeye yarar.
- Kanıt:
  ```bash
  curl -s -A 'Mozilla/5.0' 'https://ulasav.csb.gov.tr/dataset?q=bina' | grep -o 'href="/dataset/[^"]*"' | sort -u | head
  # → /dataset/34-mahalle-bazli-bina-analiz-verisi, /dataset/16-7-bina-metrekare-birim-degerleri ...
  ```

### 5. İzmir BB Açık Veri — Bina Kimlik Belgesi — puan 2
- URL: `https://acikveri.bizizmir.com/dataset/bina-kimlik-belgesi` · dosya: `https://openfiles.izmir.bel.tr/111661/docs/izbb-bina-kimlik-belgesi.csv` (39 MB) + `bornova_yapi_envanter.csv`
- Lisans: İzmir BB Açık Veri Lisansı (atıf ile ticari dahil serbest)
- Erişim: dosya (CSV, `;` ayraçlı) + CKAN API çalışıyor
- Alanlar: bina kimlik no, il/ilçe/mahalle, sokak/kapı no, **ada/parsel**, kat adedi, kullanım sınıfı (Konut/İşyeri), taşıyıcı sistem, yapı nizamı. **lat/lon yok**, site adı yok.
- Kapsam/kayıt: Bayraklı + Bornova (pilot), on binlerce bina.
- Değer: Site tespiti için dolaylı: aynı ada/parselde ≥3 konut bloğu + yüksek kat → site adayı; adres ile OSM eşleştirme gerek. Yalnızca 2 ilçe.
- Kanıt:
  ```bash
  curl -s -r 0-1500 https://openfiles.izmir.bel.tr/111661/docs/izbb-bina-kimlik-belgesi.csv | head -2
  # → BINA_KIMLIK_NO;IL_ILCE_MAHALLE;SOKAK_CADDE_KAPINO;ADA_PARSEL;...;TOPLAM_KAT_ADEDI;...
  ```

---

## Diğer değerlendirilen kaynaklar

| # | Kaynak | URL | Lisans | Erişim | Sonuç / puan |
|---|---|---|---|---|---|
| 6 | İBB Açık Veri Portalı | data.ibb.gov.tr (CKAN API çalışıyor) | İBB Açık Veri Lisansı | API/dosya | `res_format:GEOJSON` 0 sonuç; "site/konut" aramasında site düzeyi set yok. `ibb-lokasyon-verileri` sadece İBB tesisleri; `mahalle-bazli-bina-analiz-verisi` 2017 mahalle agregatı. **Puan 2** (bağlam, yeni kompleks yok) |
| 7 | MEYDİP – Sanayi Bölgeleri Atlası (OSB parsel/konum) | meydip.sanayi.gov.tr | belirsiz | SPA; `rest/v1/osb` → **401**, e-Devlet SSO gerekli | Toplu erişim yok. **Puan 1**. Mevcut TGB listesi yeterli. |
| 8 | TOKİ illere göre projeler – detay sayfaları | toki.gov.tr/illere-gore-projeler/<id> | belirsiz | sayfa | Detayda harita/koordinat **yok** (metin: il/ilçe/konut sayısı). `/satis-ilanlari`, `/ihale-ilanlari` 404'e yönleniyor. **Puan 1** (zaten entegre). |
| 9 | Kentsel Dönüşüm Başkanlığı (riskli/rezerv alan) | kdb.gov.tr | belirsiz | sayfa; `/Projeler` 404 | Ana sayfada proje kartları (il/ilçe/mahalle, "Rezerv Yapı Alanı") var ama liste/API bulunamadı; Resmî Gazete kararları koordinatsız. **Puan 2** (ileride manuel; ~200–400 alan) |
| 10 | TKGM / TUCBS / Atlas | cbs.tkgm.gov.tr (→ online.tkgm 302), tucbs.gov.tr, atlas.gov.tr (200) | TUCBS "Açık Veri" katmanları kullanıcı girişi ister | WMS/WFS, giriş gerekli | Toplu indirme yok; parsel sorgusu tekil ve ToS belirsiz. **Puan 1** |
| 11 | Microsoft Global ML Building Footprints | minedbuildings…/dataset-links.csv | ODbL | dosya | 119 bölge içinde **Türkiye yok**; zaten Overture'da. **Puan 1** |
| 12 | Google Open Buildings | sites.research.google/open-buildings | CC BY 4.0 | dosya | Kapsam Afrika/G.Asya/GD Asya/LatAm — TR yok (bilinen kapsam; bu oturumda ayrıca test edilmedi). **Puan 0** |
| 13 | OpenAddresses | batch.openaddresses.io/api/data | çeşitli | API | 5.259 kaynak içinde `tr/` **0**. **Puan 0** |
| 14 | KAP (Kamuyu Aydınlatma Platformu) GYO bildirimleri | kap.org.tr | belirsiz (kamuya açık, API resmi değil) | sayfa; `/api/...` 404 / bağlantı reddi | GYO faaliyet raporu PDF'lerinde proje adı/ilçe/bağımsız bölüm var; koordinat yok; manuel. **Puan 2** |
| 15 | Konya BB Açık Veri (CKAN) | acikveri.konya.bel.tr | belirsiz | API çalışıyor, 239 set | bina/site seti yok (e-numarataj başvuru sayıları). **Puan 1** |
| 16 | Bursa BB Açık Veri (CKAN) | acikyesil.bursa.bel.tr | belirsiz | API çalışıyor, 49 set | sadece `mahalle-sinirlari`. **Puan 1** |
| 17 | Ankara BB (seffaf.ankara.bel.tr, cbs.ankara.bel.tr) | — | — | CKAN API 404, ArcGIS REST 404 | Portal statik; ULASAV üzerinden WMS var. **Puan 1** |
| 18 | Marmara Belediyeler Birliği (data.marmara.gov.tr), Kocaeli, Gaziantep, Balıkesir, Antalya, Eskişehir portalları | — | — | API 404 / bağlantı yok / 403 | **Puan 0–1** |
| 19 | ArcGIS Hub / HuggingFace / Kaggle | hub.arcgis.com, huggingface.co/api/datasets | çeşitli | API | ArcGIS Hub "turkey site" → alakasız; HF "turkey" → sadece healthsites; Kaggle TR konut setleri sahibinden/emlakjet kazıması türevi (lisans ve kaynak ToS sorunlu) → **kapsam dışı**. **Puan 1** |

---

## Öneriler (öncelik sırası)
1. **Wikidata**: sınıf başına ayrı sorgu (P31 doğrudan + 1 kademe alt sınıf), P131 ile ilçe; mevcut OSM `wikidata=*` etiketleriyle eşleştir → bağımsız doğrulama bayrağı.
2. **GeoNames**: `TR.txt` → regex (sitesi|evleri|konutları|rezidans|plaza|teknokent|teknopark) → 365 aday; 500 m yarıçapında isim-benzerliği ile OSM/Overture eşle.
3. **Emlak Konut**: 46 proje için satış sitesi embed'inden koordinat; TOKİ kayıtlarıyla eşleştir (ortak proje adları).
4. **ULASAV + İzmir bina kimlik**: site düzeyinde değil, "yüksek yoğunluk mahalle" önceliklendirmesi için.
5. Daha ileri: KDB proje kartlarını ve GYO faaliyet raporlarını (KAP PDF) yarı-manuel çıkarma — lisans belirsiz, yalnızca olgu verisi.

Erişilemeyenler açıkça: ULASAV CKAN API (403), MEYDİP REST (401), TKGM/TUCBS (giriş), KAP API (yok), Marmara/Kocaeli/Gaziantep/Antalya portalları (bağlantı yok).
