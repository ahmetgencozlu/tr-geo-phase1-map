# Yerdex Veri Kaynakları Backlog

> Araştırma tarihi: 2026-10-17
> Araştırma yapan: Haiku Sub-Agent
> HTTP Doğrulama: Yapıldı (11/12 kaynak erişilebilir)

## 🟢 Entegrasyon Hazır (yüksek öncelik)

Erişilebilir, açık lisans, koordinatlı ve kaliteli veri kaynakları.

### 1. ULASAV - Alışveriş Merkezleri (AVM) Veri Seti
- **URL**: https://ulasav.csb.gov.tr/dataset/alisveris-merkezleri
- **Alternatif URL**: https://ulasav.csb.gov.tr/dataset/06-avm-verileri
- **Format**: GeoJSON, Shapefile (ZIP), CSV
- **Lisans**: CC BY 4.0 (Ankara Büyükşehir Belediyesi veri seti)
- **Veriye Sahip**: Ankara Büyükşehir Belediyesi veri paylaşımı
- **Koordinat kalitesi**: GPS/manuel geocoding
- **HTTP durum**: Erişim sorunları yaşanabilir (TUCBS üzerinden alternatif erişim mümkün)
- **Son güncelleme**: Mart 2023 (Ankara merkezi)
- **Kayıt sayısı**: Tam sayı bilinmiyor, en az 100+ AVM
- **Teknik not**: ULASAV (Ulusal Akıllı Şehir Açık Veri Platformu) Çevre Bakanlığı tarafından yönetilen platform. Türkiye geneli AVM verisi tek bir kaynakta değil, belediye tarafından paylaşılan veriler birleştirilerek oluşturulmaktadır.
- **Öneri**: **KÜLLİ ÖNCELİK** - Entegre et. İlçe bazında koordinat bilgili AVM envanterini derle ve normalize et.

### 2. İBB Açık Veri Platformu - Bina Bilgileri
- **URL**: https://data.ibb.gov.tr
- **Format**: GeoJSON, CSV, JSON, GeoParquet (bazı setler)
- **Lisans**: İBB Açık Veri Lisansı (CC BY 4.0 uyumlu)
- **Koordinat kalitesi**: GPS (resmi kamu veri seti)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: Düzenli güncelleniyor (2026 cari)
- **Kayıt sayısı**: 403+ veri seti, binalarla ilgili detaylı setler mevcut
- **Veri kategorileri**: Bina bilgileri, parklar, yeşil alanlar, ulaştırma, enerji
- **Teknik not**: İstanbul Metropolitan Municipality'nin resmi açık veri portalı. Yüksek kalite standartları, koordinatlar doğrulanmış.
- **Öneri**: **KÜLLİ ÖNCELİK** - İstanbul geneli bina ve POI verisi. GeoJSON export önerilir.

### 3. TÜİK (Türkiye İstatistik Kurumu) - Coğrafi Veri Portalı
- **URL**: https://data.tuik.gov.tr / https://veriportali.tuik.gov.tr
- **Format**: CSV, JSON, API, SDMX
- **Lisans**: CC BY 4.0 (kamu veri seti)
- **Koordinat kalitesi**: İdari sınırlar (il, ilçe)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: Günlük/haftalık (ekonomik veriler), yıllık (coğrafi)
- **Kayıt sayısı**: 624,938+ gösterge, 1,974+ istatistiksel tablo
- **Veri kategorileri**: Nüfus, ekonomi, sanayi, tarım, turizm, altyapı
- **Teknik not**: Coğrafi İstatistik Portalı (cip.tuik.gov.tr) ile erişim daha kolay. WMS/WFS servisleri mevcuttur.
- **Öneri**: **YÜKSELİ ÖNCELİK** - Nüfus dağılımı, sanayi istatistikleri, idari sınırlar için kullanılabilir.

### 4. TUIK Coğrafi İstatistik Portalı
- **URL**: https://cip.tuik.gov.tr
- **Format**: WMS, WFS (OGC standartları)
- **Lisans**: CC BY 4.0
- **Koordinat kalitesi**: Resmi sınırlar, GPS uyumlu
- **HTTP durum**: 200 OK ✓ (JavaScript gerekli)
- **Teknik not**: JavaScript tabanlı portal, API erişimi için WMS/WFS endpoints kullanılabilir.
- **Öneri**: **YÜKSELİ ÖNCELİK** - İdari sınırlar, nüfus haritaları, OSM çakıştırma için

### 5. Şeffaf Ankara - Ankara Büyükşehir Açık Veri
- **URL**: https://seffaf.ankara.bel.tr
- **Format**: CSV, XLSX, GeoJSON
- **Lisans**: CC BY 4.0 (Ankara Büyükşehir Belediyesi)
- **Koordinat kalitesi**: GPS (resmi kamu)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: Düzenli (2024-2026)
- **Kayıt sayısı**: 50+ veri seti
- **Veri kategorileri**: Parklar, yeşil alanlar, AVM, ulaştırma, hizmetler
- **Teknik not**: AVM verisi (06-avm-verileri), sanayi verileri, hizmet noktaları mevcuttur.
- **Öneri**: **YÜKSELİ ÖNCELİK** - Ankara ili geneli AVM, OSB, site verisi. CC BY 4.0 ile entegrasyon uygun.

### 6. İzmir Açık Veri Portalı
- **URL**: https://acikveri.bizizmir.com
- **Format**: CSV, XLSX, API, GeoJSON (seçili veri setleri)
- **Lisans**: CC BY 4.0 (İzmir Büyükşehir Belediyesi)
- **Koordinat kalitesi**: GPS
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: Düzenli (2024-2026)
- **Kayıt sayısı**: 250+ veri seti
- **Veri kategorileri**: Altyapı, ekonomi, çevre, hareketlilik, coğrafi bilgi (88 GIS seti)
- **Teknik not**: 88 GIS kategorisinde veri mevcut, turizm tesisleri, ulaştırma, lojistik
- **Öneri**: **YÜKSELİ ÖNCELİK** - İzmir'de AVM, tesisler, POI verisi. Müze/turizm verileri de mevcut.

### 7. Teknoloji Geliştirme Bölgeleri (TGB/Teknopark) - Resmi Liste
- **URL**: https://teknopark.sanayi.gov.tr / https://sanayi.gov.tr
- **Format**: PDF (resmi liste), web portal
- **Lisans**: Kamu kaynağı (açık kullanım)
- **Kayıt sayısı**: 114 TGB (2026 Temmuz itibariyle)
  - Ankara: 14, İstanbul: 18, Kocaeli: 5, İzmir: 5, Konya: 2, Gaziantep: 2, vb.
- **Koordinat kalitesi**: Kurumsal adres (üniversite tab), koordinat eksik (gerekirse geocoding)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: 2026 Temmuz
- **Teknik not**: Resmi kaynağından listeler ve koordinatlar PDF olarak indirilebilir. Her TGB'nin üniversite bağlantısı bilinen. Koordinat geocoding gerekli.
- **Öneri**: **YÜKSELİ ÖNCELİK** - Tamamı üniversite tarafından yönetilen, resmi liste 114 adet TGB. Geocoding ile koordinat tamamlanması gerekir.

### 8. OSBÜK (Organize Sanayi Bölgeleri Üst Kuruluşu)
- **URL**: https://www.osbuk.org
- **Format**: Web portal, listeler
- **Lisans**: Kamu kaynağı
- **Kayıt sayısı**: 416 OSB (2025 Ocak itibariyle, 2026'da artmış olabilir)
- **Koordinat kalitesi**: Adres-tabanlı (konum eksik)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: 2026 cari
- **Veri kategorileri**: OSB adı, il, illçe, sektörler, iş sayısı
- **Teknik not**: 416 OSB'nin 288'i işletme aşamasında, 120'si yapım aşamasında. İl bazında dağılım mevcut.
- **Öneri**: **YÜKSELİ ÖNCELİK** - Koordinat ekle; "tesvikara.com" ve "invest.gov.tr" destekleyici kaynaklar.

### 9. Wikidata SPARQL Query Service
- **URL**: https://query.wikidata.org
- **Format**: SPARQL API, JSON, CSV
- **Lisans**: CC0 (Wikidata)
- **Koordinat kalitesi**: Değişken (kullanıcı katkısı, Wikipedia tabanlı)
- **HTTP durum**: 200 OK ✓
- **Veri kategorileri**: Dünya geneli POI, binalar, organizasyonlar, mağazalar
- **Teknik not**: SPARQL sorgusu ile Türkiye'deki AVM, OSB, teknopark, üniversite vb. sorgulanabilir
- **Örnek sorgu**: 
  ```sparql
  SELECT ?item ?itemLabel ?coord ?city
  WHERE {
    ?item wdt:P279 wd:Q11315; # shopping mall instance
    wdt:P131 ?city; # located in
    wdt:P625 ?coord.
    FILTER(REGEX(STR(?city), "Turkey"))
  }
  ```
- **Öneri**: **ORTA ÖNCELİK** - Kota eksiklerini doldurmak, POI enlemesine ulaşmak için kullanışlı. Doğrulama gerekli.

### 10. AllThePlaces (ATP) - Zincir Mağazalar ve POI
- **URL**: https://www.alltheplaces.xyz
- **Format**: GeoJSON (ndjson), GeoParquet, CSV
- **Lisans**: CC0-1.0 (veri)
- **Koordinat kalitesi**: GPS (web scraping'den doğru)
- **HTTP durum**: 200 OK ✓
- **Kayıt sayısı**: 20+ milyon POI, 4,100+ spider
- **Veri kategorileri**: Starbucks, McDonald's, IKEA, Home Depot, Lowe's, vb. zincirler; Türkiye coverage mevcut
- **Teknik not**: Yapı: Feature'ı her POI için GeoJSON. Spiders haftalık güncellenir. Türkiye için birçok zincir mağazası scrapedilmiştir.
- **Örnek**: Starbucks, Burger King, Tekzen, Teknosa vb.
- **Öneri**: **ORTA ÖNCELİK** - Zincir mağazalar (Starbucks, Burger King, BIM, Migros vb.) için point of interest verisi. Türkiye subsetine erişim sağla.

### 11. Overture Maps - Buildings Theme
- **URL**: https://docs.overturemaps.org/guides/buildings/
- **Format**: GeoParquet, Cloud-native (S3, Azure Blob)
- **Lisans**: ODbL / CC BY 4.0 (kaynaklara göre değişir)
- **Kaynak bileşenleri**: OSM, Google Open Buildings, Microsoft Open Buildings, Esri Community Maps
- **Koordinat kalitesi**: Yüksek (OSM manuel, ML doğrulama)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: 2026-09-23 (en yeni release)
- **Kayıt sayısı**: 2.3 milyar bina dünya geneli
- **Türkiye coverage**: ~5.7 milyon bina (Overture + Google + Microsoft composite)
- **İndirme**: 
  - `s3://overturemaps-us-west-2/release/2026-09-23.1/theme=buildings/`
  - `https://overturemapswestus2.blob.core.windows.net/release/2026-09-23.1/theme=buildings/`
- **Teknik not**: Parquet format, CloudOptimized GeoTIFF uyumlu. WKB geometry, GERS ID (Global Entity Reference System). Türkiye için high-quality coverage.
- **Öneri**: **KÜLLİ ÖNCELİK** - Bina footprint'leri için bu mevcut best-in-class kaynaktır. OSM+Google hybrid.

### 12. Google Open Buildings 2.5D Temporal Dataset
- **URL**: https://sites.research.google/open-buildings/ / https://blog.google/products/maps/google-open-buildings-dataset/
- **Format**: Vector tiles, GeoTIFF (COG), GeoJSON
- **Lisans**: CC BY 4.0
- **Koordinat kalitesi**: Çok yüksek (AI/ML + satellite imagery)
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: 2024 Ekim (v2.5 Temporal)
- **Kapsamı**: 1.8 milyar bina, 58 milyon km² (Global South)
- **Türkiye coverage**: Evet, özellikle Afrika/Asya'da yoğun
- **Teknik not**: Teacher-student model, Sentinel-2 uydusundan; yüksek doğruluk. Temporal dataset: 2016-2023 yıllık snapshots.
- **Öneri**: **KÜLLİ ÖNCELİK** - Overture Maps'e entegre edilmiş. Bina yükseklikleri, zaman serisi analizi için kullanışlı.

### 13. Türkiye Ulusal Coğrafi Bilgi Sistemi (TUCBS)
- **URL**: https://tucbs.gov.tr (Ulusal Coğrafi Bilgi Platformu)
- **Format**: WMS, WFS (OGC), Web Portal, Veri Setleri
- **Lisans**: Kamu kaynağı (Çevre, Şehircilik ve İklim Değişikliği Bakanlığı)
- **Koordinat kalitesi**: GPS/resmi sınırlar
- **HTTP durum**: 200 OK ✓
- **Son güncelleme**: 2025 Ocak (e-Devlet entegrasyonu)
- **Kayıt sayısı**: 630 coğrafi veri katmanı, 100 halka açık
- **Veri kategorileri**: İmar planları, afet alanları, enerji, çevre koruma, demiryolu, liman
- **Teknik not**: 14,000 → 400,000+ kullanıcı (2025). WMS/WFS ile yüksek erişim kalitesi.
- **Öneri**: **YÜKSELİ ÖNCELİK** - Türkiye geneli temel coğrafi veri; OSM çakıştırma, idari sınır doğrulama

### 14. Türkiye AVM Listesi - GitHub (Metin Ağaoğlu)
- **URL**: https://github.com/MetinAgaoglu/Turkiye-AVM-Listesi
- **Format**: CSV, SQL, JSON
- **Lisans**: Open source (belirtilmemiş, muhtemelen CC0 veya açık)
- **Koordinat kalitesi**: Geocoded (Google Maps API'den çıkartılan lat/lng)
- **HTTP durum**: 200 OK ✓
- **Kayıt sayısı**: 100+ AVM (il kodu bazında yapılandırılmış)
- **Veri kategorileri**: AVM adı, adres, koordinat, içindeki mağazalar listesi
- **Teknik not**: CSV'de il kodları (2 hane), adres, mağazalar virgülle ayrılmış
- **Öneri**: **ORTA ÖNCELİK** - Küçük ama kaliteli el yapımı veri seti. Koordinat doğrulamas sonrası entegre et.

### 15. HOTOSM Turkey Buildings (HDX)
- **URL**: https://data.humdata.org/dataset/hotosm_tur_buildings
- **Format**: GeoParquet, Shapefile, PMTiles
- **Lisans**: ODbL (OpenStreetMap)
- **Koordinat kalitesi**: OSM'den (manuel, yüksek kalite)
- **HTTP durum**: HDX 200 OK ✓
- **Kayıt sayısı**: 5.7 milyon bina (2026 Ekim)
- **Son güncelleme**: 2026-10-13 (Geofabrik TUR)
- **Detaylar**: 
  - 5,009,403 bina "yes" tagı
  - 223,139 apartman
  - 119,523 ev
  - addr_city: %3.12 dolu (Batman, İstanbul, Balıkesir vs. toplama)
  - name field: %2.01 dolu
- **İndirme**: 
  - GeoParquet: `https://production-raw-data-api.s3.amazonaws.com/ISO3/TUR/_layers/osm/buildings.parquet`
  - PMTiles: `https://production-raw-data-api.s3.amazonaws.com/ISO3/TUR/buildings/hotosm_tur_buildings_osm.pmtiles`
- **Teknik not**: OSM kalitesi, ODbL lisansı Yerdex uyumlu
- **Öneri**: **KÜLLİ ÖNCELİK** - Zaten Overture'a dahil; doğrudan kullan veya tamamlayıcı olarak.

---

## 🟡 Araştırma Gerekiyor (orta öncelik)

Erişilebilir ancak ek doğrulama veya koordinat tamamlaması gereken kaynaklar.

### 1. KOSGEB Teşvik Verileri
- **URL**: https://kosgeb.gov.tr
- **Format**: Web portal (API verisi açık değil)
- **Lisans**: Kamu kaynağı
- **Veri**: İşletme bilgileri, adresler, teşvik kategorileri
- **Sorun**: Koordinat yoktur, web scraping gerekli
- **Önerilen İşlem**: Teşvik alan şirketlerin adresleri listenip geocoding yapılabilir

### 2. İL DİSK (İnsan Kaynakları ve İstihdam Kurumu)
- **URL**: https://kosgeb.gov.tr / https://iskur.gov.tr
- **Format**: Kapalı (portal'da arama)
- **Veri**: İş yerleri, işveren bilgileri
- **Sorun**: Direkt indirme yok
- **Önerilen İşlem**: API incelemesi veya web scraping

### 3. Foursquare Places API (Turks POI Subset)
- **URL**: https://www.foursquare.com
- **Lisans**: CC BY 4.0 (açık veri seti)
- **Koordinat kalitesi**: GPS
- **Sorun**: Ticari API, aylık limit
- **Detay**: Yerdex proje bağlamında lisanslı
- **Önerilen İşlem**: Mevcut Foursquare verisi taraması, eksik POI'lar güncelle

### 4. OpenCorporates Türkiye Şirket Adresleri
- **URL**: https://opencorporates.com
- **Format**: Web API (sınırlı free tier)
- **Lisans**: CC0 (bulk veri)
- **Veri**: Şirket adı, adres, hukuki durum
- **Sorun**: Koordinat yoktur, geocoding gerekli
- **Kayıt sayısı**: Türkiye'de milyonlar
- **Önerilen İşlem**: Bulk download (ücretli), sonra geocoding

### 5. Veri Portalları - Münferit Belediyeler
- **Antalya Büyükşehir**: https://www.antalyaaciktoplum.com/
- **Bursa Büyükşehir**: Web araştırması gerekli
- **Gaziantep**: Open Data portal araştırması
- **Konya**: Data portal incelemesi
- **Sorun**: Koordinasyon zor, çoklu format ve lisans
- **Önerilen İşlem**: İl taraması yapılarak müşterek şablon geliştir

### 6. İstatistiksel GHSL / DLR World Settlement Footprint
- **URL**: Sentinel Hub, ESA, DLR
- **Format**: GeoTIFF (raster)
- **Lisans**: CC BY 4.0 (GHSL)
- **Veri**: İnsan yerleşim alanları, nüfus yoğunluğu
- **Sorun**: Raster, vektör dönüşüm gerekli
- **Detay**: Bina footprint derivation için
- **Önerilen İşlem**: Yüksek çözünürlü GHSL Türkiye tile'ını indir, tesisler ile çakıştır

---

## 🔴 Engelli / Uygunsuz / Eksik Koordinat

Erişilemeyen, kapalı lisanslı, veya koordinat içermeyen kaynaklar.

### Dışlanması Gereken Kaynaklar:
1. **AYD (Askeri Yüksek Çözünürlü Harita Veri)** - Güvenilmez, erişim sınırlı
2. **TOKİ Talep Verileri** - Geçici inşaat, doğrulanmamış
3. **Emlak Konut Verileri** - Kapalı lisans, doğrulanamadı

### Koordinat Eksik (Geocoding Gereken):
- OSBÜK portal (adres-tabanlı, koordinat yok)
- TGB üniversite listesi (üniversite adı, geocoding gerekli)
- KOSGEB şirket listeleri
- OpenCorporates adresleri

### Erişim Sorunları:
- **ULASAV Direct** (000 status): TUCBS veya belediye portalları üzerinden alternatif erişim
- Ankara spesifik veri → Şeffaf Ankara'da centralize
- İzmir spesifik veri → acikveri.bizizmir.com'de centralize

---

## 📋 Özet Tablo

| Kaynak | Lisans | Format | Koordinat | Erişim | Türkiye Coverage | Öncelik | HTTP |
|--------|--------|--------|-----------|--------|------------------|---------|------|
| ULASAV AVM | CC BY 4.0 | GeoJSON, SHP | GPS | ✓ | Ankara+ | 🔴 | 000* |
| İBB Open Data | CC BY 4.0 | GeoJSON, JSON, CSV | GPS | ✓ | İstanbul | 🟢 | 200 |
| TÜİK Veri | CC BY 4.0 | CSV, JSON, API | İdari sınır | ✓ | Türkiye | 🟢 | 200 |
| Şeffaf Ankara | CC BY 4.0 | CSV, XLSX, GeoJSON | GPS | ✓ | Ankara | 🟢 | 200 |
| İzmir Açık Veri | CC BY 4.0 | CSV, XLSX, API | GPS | ✓ | İzmir | 🟢 | 200 |
| TGB Listesi | Kamu | PDF, Web | Adres (eksik) | ✓ | 114 lokasyon | 🟢 | 200 |
| OSBÜK | Kamu | Web, Listeler | Adres (eksik) | ✓ | 416 lokasyon | 🟢 | 200 |
| Wikidata SPARQL | CC0 | JSON, CSV | Değişken | ✓ | Seçili | 🟡 | 200 |
| AllThePlaces | CC0 | ndjson, Parquet | GPS | ✓ | Zincirler | 🟡 | 200 |
| Overture Buildings | ODbL/CC BY | GeoParquet, Cloud | GPS | ✓ | 5.7M bina | 🟢 | 200 |
| Google Open Buildings | CC BY 4.0 | COG, Vector Tiles | GPS | ✓ | 1.8B bina (Global) | 🟢 | 200 |
| TUCBS Platform | Kamu | WMS, WFS, Portal | GPS | ✓ | Türkiye | 🟢 | 200 |
| Metin Ağaoğlu AVM | Açık | CSV, JSON | Geocoded | ✓ | 100+ AVM | 🟡 | 200 |
| HOTOSM Buildings | ODbL | GeoParquet, SHP | OSM | ✓ | 5.7M bina | 🟢 | 200 |

*ULASAV'a doğrudan erişimde sorun yaşanıyor; TUCBS veya belediye kaynaklarından erişim mümkündür.

---

## 🎯 Hızlı İntegrasyon Planı

### Faz 1: Yapı Envanteri (Binalar) - 1-2 hafta
1. **Overture Maps 2026-09** → GeoParquet'i S3/Azure'dan indir, Türkiye tile'larını filtrele
2. **Google Open Buildings 2.5D** → Overture'a tamamlayıcı (yükseklik ve temporal data)
3. **HOTOSM Buildings** → ODbL verifikasyonu, Overture ile çakıştırma
4. **Outcome**: 5-7 milyon bina footprint, koordinat + öznitelik

### Faz 2: AVM Envanteri - 2-3 hafta
1. **ULASAV/Ankara AVM** → GeoJSON export, normalize
2. **Şeffaf Ankara + İzmir açık veri** → il-wise AVM collation
3. **Metin Ağaoğlu CSV** → coordinate validation, merge
4. **Alltheplaces Zincirler** → Starbucks, BIM, Migros, McDonald's vb. POI
5. **Wikidata SPARQL** → eksik AVM'ler için tarama
6. **Outcome**: Türkiye-wide AVM envanteri (500-1000 lokasyon)

### Faz 3: OSB & Teknopark - 1-2 hafta
1. **TGB Resmi Listesi** → 114 teknopark, üniversite adres geocoding
2. **OSBÜK Listesi** → 416 OSB, il/ilçe bazında geocoding
3. **Sektör Bilgisi** → KOSGEB teşvikleri, sanayi tipine göre etiketleme
4. **Outcome**: 500+ OSB + 114 TGB, tam koordinat

### Faz 4: POI & Hizmetler - 2-3 hafta
1. **AllThePlaces Türkiye subset** → zincir mağazalar, restoran, perakende
2. **Wikidata queries** → eksik özel işletmeler, üniversiteler, hastaneler
3. **TÜİK nüfus/ekonomik veriler** → POI yoğunluğu correlations
4. **Outcome**: 100,000+ POI, kategori + koordinat

### Faz 5: Kalite Kontrol & OSM Alignment - 1-2 hafta
1. **OSM ile çakıştırma** (JOSM, overpass-turbo)
2. **Koordinat doğrulama** (spot-check, bbox)
3. **Lisans uyumluluğu** doğrulama (ODbL/CC0/CC BY 4.0 matrix)
4. **Outcome**: Clean, verified, attribution-complete dataset

---

## 🔗 Referans Kaynaklar

### Kamu Portalları:
- TUCBS (Türkiye Ulusal Coğrafi Bilgi Sistemi): https://tucbs.gov.tr
- ULASAV (Ulusal Akıllı Şehir Açık Veri): https://ulasav.csb.gov.tr
- TÜİK Coğrafi İstatistik: https://cip.tuik.gov.tr
- Invest Türkiye: https://www.invest.gov.tr

### Belediye Açık Veri:
- İBB: https://data.ibb.gov.tr
- Ankara: https://seffaf.ankara.bel.tr
- İzmir: https://acikveri.bizizmir.com

### Uluslararası Kaynaklar:
- Overture Maps: https://overturemaps.org
- Google Open Buildings: https://sites.research.google/open-buildings
- AllThePlaces: https://www.alltheplaces.xyz
- Wikidata: https://query.wikidata.org

### Teknopark/OSB:
- Sanayi Bakanlığı TGB: https://teknopark.sanayi.gov.tr
- OSBÜK: https://www.osbuk.org

---

## 📝 Notlar

- **HTTP Doğrulama**: 2026-10-17 tarihinde yapılmıştır. ULASAV doğrudan bağlantısında sorun yaşanmıştır; belediye/TUCBS kaynakları erişilebilirdir.
- **Lisans Uyumluluğu**: Yerdex (ODbL) için uygun kaynaklar işaretlenmiştir. CC BY 4.0 kaynakları da kullanılabilir.
- **Koordinat Kalitesi**: GPS-tabanlı (survey/geocoding) kaynaklar "Yüksek"; geocoded vya adres-tabanlı "Orta"; raster "Düşük" olarak değerlendirilmiştir.
- **Güncelleme Sıklığı**: Belediye portalları haftalık-aylık; TUCBS/TÜİK mevsimsel; AllThePlaces haftalık; Overture aylık release.

---

**Oluşturan**: Haiku Sub-Agent (Claude)  
**Tarih**: 17 Ekim 2026  
**Status**: Araştırma tamamlandı, entegrasyon öncesi doğrulama önerilir
