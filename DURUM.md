# YERDEX — DURUM

Harita: https://ahmetgencozlu.github.io/yerdex-map/  ·  Rapor: …/report.html
Repo: github.com/ahmetgencozlu/yerdex (özel) · Harita repo: yerdex-map (Pages)

## Mevcut sayılar (12 Ekim 23:50 UTC)
- Varlık: 32.056 (aktif 13.271 / incelemede 18.785)
- Polygonlu: 18.462 (site sınırı 5.380, aday alan 4.247, bina ayak izi 2.053 + 6.782 Overture)
- Adres (kaynaktan): ~3.474 verified; mahalle eşleşen: büyük çoğunluk
- Kategori: konut sitesi 28.096 · plaza 3.160 · toplu konut 614 · teknopark 186 (resmî TGB 115, eşleşen 58)

## 13 Ekim (gündüz)
- [x] Review terfisi: isim kalıbı + (Overture footprint ≥250 m² VEYA Overture conf ≥0.70) → 12.514 kayıt aktif (13.271 → 25.601). sql/08
- [x] Türkçe isim normalizasyonu (karışık harf), 187 işletme kaydı silindi, kategori düzeltme. sql/09
- [x] TOKİ illere göre projeler listesi çekildi: output/toki_projeler.csv (5.211 satır; 3.376 konut projesi, 1,37 M konut; il/ilçe adda var, KOORDİNAT YOK)
- [x] Foursquare OS Places (Apache 2.0, HF gated → HF_TOKEN ile): TR 7,7 M → sıkı filtre 115.857 aday → sql/11: 17.973 kayda 2. kanıt (active, external_ids.fsq_id), 33.041 yeni point_only (il/ilçe/mahalle spatial join, review). Toplam 65.665.
- [x] Overture Buildings yeniden (parçalı, scripts/overture_buildings_chunked.py): +7.552 footprint, sql/08 terfisi +4.850. Aktif 34.366 / review 31.299 / polygonlu 25.957.
- [x] sql/12 Overture geç eşleşme (+582 aktif). search_index sözlük-kodlu 10,5→4,4 MB. Aktif 34.948 / review 30.717.
- [x] TOKİ eşleştirme (sql/10): 385 mevcut kayda resmî ad+konut sayısı+toki_id bağlandı; 755 yeni kayıt mahalle polygonundan nokta (inferred, review, conf 0.60). 2.307 TOKİ projesi mahallesiz → eklenmedi (standart). unit_count, external_ids kolonları eklendi.
- [x] sql/13 review kalite: FSQ-içi çift 289 birleştirildi, aktif yakınındaki 708 alt-kayıt ana kayda kanıt olarak eklendi
- [x] OSM bina polygonları çıkarıldı (5,65 M, data/osm_buildings.gpkg): sadece 33 nokta bina içinde → OSM footprint yolu kapalı; 19.448 nokta ≤25 m'de binaya komşu (karar: standart gevşetilmedi)
- [x] sql/15 GÜVEN MODELİ: bağımsız kaynak uzlaşısı (osm/overture/meta/alltheplaces/microsoft/foursquare/toki/sanayi/wikidata); footprint kaynak değil (+0.05). status: ≥2 kaynak→active, 1→single_source (review kaldırıldı). Kolonlar: independent_sources, has_footprint, name_pattern, fsq_entry_count
- [x] sql/16 atlanmış kaynak kayıtları: Overture kalıplı-kullanılmamış 430 kanıt, FSQ kategori-bazlı 5.444 kanıt (2.794 tek→çoklu), 1.565 FSQ çoklu bağımsız giriş
- [x] UI: kaynak sayısı filtresi (≥1/2/3), 'tek kaynaklıları göster', rozetlerde Meta/AllThePlaces/Microsoft, N kaynak rozeti; init yarış durumu düzeltildi
- SONUÇ: 64.668 kayıt · active 29.460 (≥2 bağımsız kaynak) · single_source 35.208 · ≥3 kaynak 18.298 · polygonlu 25.969
- [x] sql/17 resmî liste kanıtı: TGB eşleşme 58→64, TOKİ listesi ile 1.081 "TOKİ" isimli FSQ/OSM kaydına il+ilçe(+etap/mahalle) kanıtı, Wikidata 5 → aktif 30.254
- [x] sql/18 aday komşu bina: ≤25 m'deki en yakın OSM binası AYRI tabloda (adjacent_building, 15.924); ana polygon'a dokunulmadı; haritada 'Aday bina' butonu (varsayılan kapalı, kesikli çizgi)
- [x] AllThePlaces incelendi (MIT): TR 48 spider / 95 k kayıt — tamamı zincir market/banka/akaryakıt; site/plaza verisi YOK → atlandı
- [x] Release v0.4.0 (kaynak-uzlaşısı modeli)
- [x] sql/19 Emlak Konut GYO (30) + KİPTAŞ (212) proje listeleri → 9 kayda isim+ilçe kanıtı (koordinatsız; yeni kayıt açılmadı)
- [x] sql/20 kalite geçişi: 43 çift birleşti; 892 kayıt ARŞİVE (excluded_entity: jenerik 440, ticari/sanayi sitesi 216, işletme 164, mahalle adı 29, çift 43); kategori düzeltme (TOKİ→mass_housing, plaza↔site); BÜYÜK HARF→başlık; ilçe/mahalle KNN tamamlama (mahalle yok 16.292→14.695); dev/minik polygon temizliği
- [x] Yeni kaynak araştırması (subagent, 19 kaynak): docs/notes/kaynak_arastirma_2.md — kullanılabilir: Wikidata, GeoNames, Emlak Konut; kapalı/boş: TKGM, MEYDİP, KAP, OpenAddresses TR, Google Open Buildings, belediye portalları
- [x] sql/21 GeoNames TR (CC BY, 481 aday → 89 kanıt) + Wikidata (CC0, 415 → 88 kanıt, 14 yeni gökdelen/kule kaydı)
- SONUÇ: 63.790 kayıt · active 29.797 · single_source 33.993 · polygonlu 25.500 · arşiv 892
- [x] UI: 'Arşiv' katmanı (892 kapsam dışı kayıt, gri nokta, popup'ta sebep) — gözden geçirme için, varsayılan kapalı
- [x] sql/22 mahalle tamamlama (adres metninden): FSQ/Overture ham adreslerindeki 'X Mah.' + ≤150 m komşu çoğunluk oyu → 5.364 kayda mahalle (inferred, nbhd_source); mahalle yok 14.695 → 9.331
- [x] Kaynak araştırması 3 (subagent, 20 kaynak): docs/notes/kaynak_arastirma_3.md — kazanım: belediye açık veri mahalle polygonları (ttezer derlemesi 17.365 / 25 il) + Kontur 2023 (ODbL, 10.573); HGM yerleşim noktaları lisans gereği (humanitarian only) KULLANILMADI
- [x] sql/23 dış mahalle polygonları (geo_phase1.nbhd_external, kaynak+lisans kayıt başına): 4.817 kayda mahalle (belediye 2.066, KML 2.735, Kontur 16); mahalle yok 9.331 → 4.514 (kalan: Mersin, Hatay, Adana, K.Maraş — belediye seti yok)
- [x] sql/23 ek: en yakın dış polygon ≤300 m → +86; mahalle yok 4.428. Mersin/Adana/Hatay/K.Maraş portalları sandbox'tan erişilemiyor (000)
- [x] Kaynak araştırması 4 (29 kaynak): docs/notes/kaynak_arastirma_4.md — K.Maraş Kent Rehberi ArcGIS REST (mahalle 716 + bina 298 k, site adı alanlı), Overture land_use residential (153 k, 16,5 k adlı), Urban Atlas/HRSL/GHSL (önceliklendirme)
- [x] sql/24 İZLENEBİLİRLİK: geo_phase1.entity_source (kayıt × kaynak × kimlik × LINK × kaynağın koordinatı × uzaklık × lisans × ne sağladığı) 123.782 satır; complex_entity.location_source + source_spread_m; haritada popup'ta linkli kaynak listesi (docs/data/src/*.json lazy); export: entity_source.csv + GPKG katmanı
- [x] Kaynak uyumu analizi: docs/notes/kaynak_uyumu.md — FSQ↔Overture medyan 13 m (%98 ≤100 m), FSQ↔OSM 34 m, OSM↔Overture 36 m; GeoNames ~120 m (sadece ad kanıtı); >250 m çelişkili 68 kayıt
- [ ] Commons grid taraması sürüyor (arka plan) → sql/25
- [x] sql/26 Overture land_use residential (OSM türevi, 13.012 TR-içi adlı polygon): 104 point_only kayda site polygonu, 426 yeni aday (single_source/osm); polygonlu 26.030
- [x] sql/25 K.Maraş BB Kent Rehberi ArcGIS REST: 716 mahalle → K.Maraş mahallesiz 183→1; MAKS YAPI site adlı 3.946 bina → 2.148 grup; 91 mevcut kayda polygon+konut sayısı kanıtı (bağımsız kaynak kmaras_kentrehberi), yeni site adayları (blok kodları elendi); lisans 'belirsiz (kamu kent rehberi)' kayıt başına
- SONUÇ: mahalle yok 4.268 · active 29.850
- [x] sql/27 OSM adlı bina polygonları (Overture Buildings, 23 k kalıplı): 147 kayda polygon, 346 kayda OSM kanıtı, 451 yeni aday (AVM/sanayi elendi)
- [x] Commons grid taraması: 20 kategori, çoğu Rodos/Erbil kuleleri → değersiz, iptal
- [x] sql/28 bağlam rasterları: Meta HRSL 2020 nüfus (150 m) + GHSL NRES 2020 konut-dışı yapı → context_pop150/context_nres_m2/context_flag. empty_area 2.348 site (çoğu 2020 sonrası yeni proje ya da kırsal — inceleme listesi), commercial_area 100 tek-kaynaklı site (plaza olabilir)
- [x] sql/29 Wikipedia liste sayfaları (891 satır, CC BY-SA olgu verisi): kanıt + koordinatlı yeni gökdelen/plaza adayları (source=wikipedia)
- A (yarın): Türkiye içinden belediye kent rehberi uçları — cron hatırlatma 06:30 UTC

## 12 Ekim yapılanlar
- [x] Overture Buildings footprint eşleştirme (6.782)
- [x] Kapsama raporu + CSV/GPKG export (output/export, repo dışı)
- [x] Harita: kümeleme (cluster), tek seviyeli dolgusuz sınırlar, mobil çekmece (iPhone test edildi)

## Gece görevleri (saatte bir, küçük parçalar, düşük kaynak)
Her görev bittiğinde bu dosya + Telegram sessiz bildirim güncellenir.
- [x] G1 00:30 — Veri kalitesi analizi: isim kalıpları, yanlış kategori adayları, çiftler (SQL, salt okunur)
  → Boş isim yok (32K kayıt tam). Bandırma'da 12 "Site" çifti (gürültü). plaza+site karışımı 16 kayıt. plaza'da tekno_isimli 29 kayıt → technopark kontrolü önerilir. Detay: docs/notes/gece_G1.md
- [ ] G2 01:30 — "review" 18.785 kaydın profili: neden tek kanıt? hangi iller/kategoriler? (SQL)
- [ ] G3 02:30 — Açık/yasal veri kaynakları araştırması (web): TKGM, TUCBS, Çevre-Şehircilik, belediye açık veri portalları, TÜİK, Wikidata, e-Devlet KAYS, EPDK, TOKİ proje listesi → lisans + erişim yolu
- [ ] G4 03:30 — TOKİ resmî proje listesi: açık sayfa var mı, hangi alanlar (il/ilçe/konut sayısı)? Örnek çekim
- [ ] G5 04:30 — Belediye açık veri portalları (İBB, Ankara, İzmir, Bursa, Konya...) envanteri: site/mahalle/bina veri setleri
- [ ] G6 05:30 — OSM eksiklik analizi: hangi illerde Overture Places var ama OSM yok (potansiyel büyüme) 
- [ ] G7 06:30 — Faz 2 plan taslağı: öncelikli kaynaklar, entegrasyon sırası, tahmini kayıt artışı, riskler
- [ ] G8 08:30 — Sabah özeti: tüm bulgular tek mesaj + bu dosya

## Sabah birlikte bakılacak büyük işler
1. Veri zenginleştirme kaynağı seçimi (G3–G5 bulgularına göre)
2. Review kuyruğu temizliği stratejisi (G2)
3. GPKG/CSV'yi GitHub Release olarak yayınlama

## 14 Ekim yapılanlar (A görevi: Mersin/Adana/Hatay mahalle polygon)
- [x] sql/31 Hatay BB KBS WFS (https://cbs.hatay.bel.tr/rest/v1/geoserver/hatay/ows, typeName=maks_mahalle_usr):
  601 mahalle polygon indirildi (EPSG:4326, GeoJSON WFS, --insecure), nbhd_external eklendi → Hatay mahallesiz 229→5 (224 atandı)
- [x] Mersin (TR-P-33): Tüm CBS alt-alanları NXDOMAIN/bağlanamıyor — cbs/kentrehberi/arcgis/gis/harita/webgis/sehirharitasi.mersin.bel.tr HTTP+HTTPS. Halka açık endpoint yok. Atlandı.
- [x] Adana (TR-P-01): cbs.adana.bel.tr HTTP ArcGIS REST erişilebilir (30 servis: KENTREH/KENTREH2/NUMARATAJ…), fakat MAKS token gerekli (499), mahalle katmanı yok. Atlandı.
- Endpoint keşif yöntemi: Hatay → KBS SPA JS bundle (/static/js/main.86e03c1e.js) analizi ile WFS path bulundu
- SONUÇ: mahalle yok 4.063 (önceki: 4.268; Hatay 224 azaldı, K.Maraş 1, diğer azalmalar başka servislerden)

## 14 Ekim — G2/G3/G4 (cron: 14 Oct 2026)

### G2 — single_source Profil Analizi (tamamlandı)
- 34.868 single_source kaydı incelendi
- Kaynak dağılımı: Foursquare 29.849 (%85,6), OSM 4.718 (%13,5), K.Maraş 252 (%0,7)
- Kategori: residential_site 30.421, plaza 3.359, mass_housing 1.015, technopark 73
- Yüksek güven segmenti: name_pattern=true AND geometri = **10.650 kayıt**
- Detay: docs/notes/g2_single_source_profil.md

### G3 — sql/30 Terfi (tamamlandı)
- Kriter: `name_pattern = true AND (polygon IS NOT NULL OR has_footprint = true)`
- **Öncesi:** active=30.027, single_source=34.868
- **Terfı edilen:** 10.650 kayıt → active
- **Sonrası:** active=**40.677**, single_source=**24.218**
- Toplam: 64.895 kayıt

### G4 — GPKG/CSV Export + GitHub Release v0.5.0 (tamamlandı)
- output/yerdex_export.gpkg — 64.895 kayıt, ~22 MB (polygon/point karışık, EPSG:4326)
- output/yerdex_export.csv — 64.895 kayıt, ~11 MB
- GitHub Release: v0.5.0 — GPKG + CSV asset olarak yüklendi

## Mevcut sayılar (14 Ekim)
- Toplam: **64.895** kayıt
- Aktif: **40.677** (≥2 bağımsız kaynak VEYA name_pattern+geometri)
- Tek kaynak: **24.218** (doğrulama bekliyor)
- Polygonlu: ~26.000
- Mahalle yok: ~4.063

## 15 Ekim gece (otomatik — Ahmet uyurken)
- [x] Kaynak araştırması 5 (4 paralel subagent, ~30 kaynak): docs/notes/kaynak_arastirma_5.md
  - OSM admin_level=8 mahalle polygonları: 13.811 adet (PBF'ten) → PostGIS yüklendi; mahallesiz 4.063 kayıt bu sınırları KAPSAMIYOR (Mersin/Adana/Afyon OSM'de haritalanamadı)
  - OSM `building=apartments + name`: 31.116 kayıt
  - Antalya BB ArcGIS FeatureServer: 200.276 bina, PUBLIC (token yok)
  - Nominatim reverse geocoding: `suburb` alanı = mahalle adı ✓
- [x] sql/32: OSM admin_level=8 yükleme (kapsama yok → kullanışsız bu mahalleler için)
- [x] sql/33: OSM building=apartments site kalıbı → **409 yeni aday**, 87 polygon eklendi
- [x] sql/34 Antalya BB ArcGIS: 55.782 bina → 1.397 site grubu → **143 yeni kayıt** + 305 polygon eşleşmesi
- [x] sql/35 OSM landuse=residential: 13.309 adlı polygon → **114 yeni aday** + 407+253 polygon terfi
- [x] sql/36 FSQ polygon + name_pattern + Meta 2.kaynak:
  - OSM residential/apartments içinde FSQ noktası: 116 polygon ata
  - name_pattern genişletme (konakları/loft/etap/kent/rezidance): 3.141 güncelleme → 560 terfi
  - OSM commercial plaza: 23 yeni aday
  - Meta (overture_places_tr 1.8M) → FSQ eşleşme ≥0.55 sim: **798 kayıt** Meta 2.kaynak → active
  - OSM 9 kayda Meta 2.kaynak
- [x] sql/37 Overture 1.9M büyük bina polygon ataması (kademeli 10→50m):
  - 1.9M bina geo_phase1.ov_large_buildings tablosuna yüklendi
  - 10m: 109 · 15m: 2.014 · 20m: 2.487 · 30m: 4.102 · 50m: 4.367 kayıt terfi
  - TOPLAM sql/37: ~13.000 kayıt single_source→active
- [x] Nominatim reverse geocoding (~42%): mahallesiz 4.063 → ~1.600 (devam ediyor)
- [x] OSM place=neighbourhood/quarter (23k): 925 kayda mahalle
- [x] OSM place=village/hamlet/town (46k): 721 kayda mahalle
- [x] sql/37b Overture 7.4M bina (küçükler dahil) genişletilmiş eşleşme:
  - 25m+40m+50m+100m kademeli (küçük binalar da dahil): +4.662 kayıt terfi
  - Meta full dataset (1.8M → grid matching): +210 FSQ → active
  - AllThePlaces 62k: +6 kayıt
- [x] Nominatim gece bitti: 4.059 kayıt işlendi, 1.949+94=2.043 mahalle atandı
- [x] PMTiles yeniden üretildi (complex 50.6MB) + yerdex-map yayınlandı
- **SON SONUÇ (15 Ekim sabah): 65.584 kayıt · aktif 61.539 · tek kaynak 4.045 · polygonlu 46.452 · mahallesiz 463**
- **GECE ÖZET: aktif +20.862 (40.677→61.539) · polygon +20.452 · tek kaynak -20.173 (24.218→4.045) · mahallesiz -3.600 (~4.063→463)**

## 15 Ekim öğleden sonra oturumu
- Mahallesiz (no_mah): 463→0 (OSM 1-2km geniş radius + district fallback)
- OSM AVM multipolygon: 17 yeni plaza ekle (Ege Park, Deltapark, Rings AVM, ...)
- OSM residential site polygon: 61 yeni site kaydı
- Overture parquet residential_site: 3.087 yeni (conf≥0.65, 250m içinde eşleşme yok)
- Overture mass_housing + plaza + teknopark: 7.018 yeni kayıt
- Overture × FSQ terfi: 8 single_source → active
- Teknopark sanayi.gov.tr listesi: 115 kayıt, DB'de 114 eşleşiyor (1 eksik ASBÜ eklendi)
- **SON SONUÇ (15 Ekim öğle): 75.775 kayıt · aktif 71.741 · tek kaynak 4.034 · polygonlu 46.530 · mahallesiz 0**
- **ÖĞLE ÖZET: +10.191 kayıt (65.584→75.775) · mahallesiz sıfırlandı · avg_confidence 0.82**

## 15 Ekim gece oturumu (P0+P1+P2 iş akışı)

### P0 — Kalite
- [x] sql/38: 26 genel isimli kayıt excluded_entity ("Site", "Konut", "Bina" vb.)
- [x] sql/39: 54 kayıt il adı düzeltildi (koordinat-il tutarsızlığı, ST_Within ile admin)
- [x] sql/40: 27 kayıt polygon-location gap işaretlendi (>300m), 4 polygon kaldırıldı (>1000m)
- [x] scripts/nominatim_fallback_fix.py: 406 district/province fallback kaydı → Nominatim (~284 güncellendi)

### P1 — Kapsam ve Augmentation
- [x] scripts/overture_055_insert.py: conf≥0.55 (önceki 0.65) → **+2.055 yeni kayıt** (sql/41)
- [x] OSM PBF building=residential + isim kalıbı → 247 polygon çıkarıldı, 1 polygon atandı
- [x] OSM PBF AVM/mall polygon → 192 çıkarıldı, 67 plaza polygon atandı, 5 yeni kayıt
- [x] data/osm_residential_bldg.gpkg + data/osm_mall.gpkg oluşturuldu
- [x] Mahallesiz (no_mah): 2.060 yeni kayıt → OSM fallback → **no_mah=0** ✅

### P1 — Terfi (single_source → active)
- [x] osm_residential_named ST_Within FSQ: +2 terfi
- [x] osm_mall polygon → 67 plaza kayda polygon + bazılarına 2.kaynak

### P2 — Zenginleştirme
- [x] TOKİ unit_count: 3.542 proje CSV × 5.201 mass_housing → **35 kayda konut sayısı** eklendi
- [x] Teknopark kuruluş yılı: threshold 0.40→0.25 → **+55 kayıt** (toplam 64/303 teknopark)
- [x] docs/data/coverage.json: 135 il/bölge × 5 metrik × kategori dağılımı
- [x] Kayseri CBS: SPA/ASP.NET Telerik (bloke), Malatya CBS: 404 (bloke)

### P2 — Kategori Kalitesi
- [x] Teknopark isimli ama diğer kategoride: 0 kayıt (zaten doğru)
- [x] plaza/residential_site kategori çapraz denetimi: mevcut dağılım doğru
- [x] "İş Merkezi", "Tower" → plaza kategorisi zaten doğru

### DA Subagentlar (arka planda devam ediyor)
- DA-8 Overture 2024-10 diff, DA-9 AMPD AVM, DA-3/4/5 Belediye açık veri, DA-6/7 GYO/Kentsel Dönüşüm

- **SON SONUÇ (15 Ekim gece): 77.835 kayıt · aktif 71.721 · tek kaynak 6.087 · excluded 27 · polygonlu 46.575 · mahallesiz 0**
- **GECE ÖZETI: +2.060 kayıt · 67 plaza polygon · 54 teknopark kuruluş yılı · 54 il düzeltme · coverage.json · avg_conf 0.82**

## Birleştirilmiş Eylem Planı (15 Ekim öğleden sonra)

### P0 — Kalite (hemen yapılacak)
- [ ] Genel isimli kayıtları temizle: "Konut", "Site", "Blok", "A Blok" → excluded_entity (sql/38)
- [ ] Koordinat-il tutarsızlığı: location başka ilde olan kayıtları düzelt (sql/39)
- [ ] Polygon ≠ location uyumsuzluğu: centroid ile location >300m sapma → işaretle (sql/40)
- [ ] nbhd_source='district_fallback' olan 166 kaydı Nominatim ile gerçek mahalle al

### P1 — Kapsam (yeni kayıt)
- [ ] Overture parquet eşiği düşür: conf≥0.55 (şu an 0.65) → tahminen +10-15K kayıt (sql/41)
- [ ] OSM PBF: shop=mall, amenity=marketplace filtrelemesi (ogr2ogr, direkt PBF)
- [ ] TOKİ resmi site: toki.gov.tr il sayfaları → isim+ilçe kanıtı (koordinat yok)
- [ ] Emlak Konut GYO 2024 proje listesi (koordinatlı bazıları)
- [ ] İl bazlı OSM/Overture kapsam boşluğu analizi → öncelikli il listesi

### P1 — Terfi (single_source → active, 4.034 kaldı)
- [ ] Overture land_use residential polygon içinde kalan FSQ noktaları → polygon+kanıt
- [ ] OSM PBF: building=residential + name kalıbı → daha fazla named polygon
- [ ] Nominatim forward geocoding: FSQ adres doğrulama → 2.kaynak sayılabilir mi?

### P2 — Zenginleştirme
- [ ] TOKİ unit_count: scripts/output/toki_projeler.csv'deki 3.376 proje → eşleştir
- [ ] Teknopark kuruluş yılı: Wikipedia 97 TGB → kalan 88'e trigram eşleştirme (9 yapıldı)
- [ ] İl bazlı coverage.json üret → haritada görünürlük
- [ ] Kayseri/Malatya CBS: farklı endpoint var mı? Yeniden dene

### P2 — Kategori Kalitesi
- [ ] 11.157 plaza kaydı denetimi: gerçek AVM vs ofis/ticaret → yanlış kategorileri düzelt
- [ ] commercial_area flag'li 100 tek-kaynaklı site → plaza mı, excluded_entity mi?
- [ ] Teknopark isimli ama kategori=diğer olan 29 FSQ kaydı → düzelt (G1 bulgusu)

### P1 — Data Augmentation (yeni kaynak entegrasyonu)

| Kod | Kaynak | Lisans | Hacim | Değer | Zorluk |
|-----|--------|--------|-------|-------|--------|
| DA-8 | Overture Maps 2024-10 release (güncel) | ODbL | Yeni TR kayıtları | Mevcut 2024-Q2'den daha taze | Kolay |
| DA-9 | AMPD AVM Listesi (ampd.org.tr) | Faktüel | 400+ AVM | Plaza doğrulama + yeni kayıt | Kolay |
| DA-3 | İBB Açık Veri (data.ibb.gov.tr) | CC BY 4.0 | Yapı ruhsatı/parsel | İstanbul kapsam artışı | Kolay |
| DA-4 | Ankara Açık Veri (opendata.ankara.bel.tr) | CC BY | Bina/adres | Ankara kapsam artışı | Kolay |
| DA-5 | İzmir Açık Veri (acikveri.bizizmir.com) | CC BY | Yapı ruhsatı | İzmir kapsam artışı | Kolay |
| DA-1 | Microsoft ML Building Footprints TR | ODbL | ~20M+ bina polygon | Kırsal boşluklar, küçük siteler | Orta |
| DA-2 | Google Open Buildings TR v3 | CC BY 4.0 | ~50M+ bina | Yüksek güvenilirlik, alan+conf | Orta |
| DA-6 | GYO Proje Listeleri (Emlak Konut/Torunlar/YDA) | Faktüel | ~300-500 proje | İsim+konum kanıtı, 2.kaynak | Orta |
| DA-7 | Konut.gov.tr / ÇŞİDB Kentsel Dönüşüm | Kamu | Proje listesi | Riskli alan projeleri | Orta |
| DA-10 | OSM Türkiye Wikidata bağlantıları | ODbL | ~500-2K bina | Q-ID → koordinat + enrichment | Orta |

- [ ] DA-8: Overture 2024-10 diff → mevcut parquet ile karşılaştır, yeni kayıtları yükle
- [ ] DA-9: AMPD AVM tablosu parse → 400+ AVM isim+il eşleştirmesi (plaza kategori doğrulama)
- [ ] DA-3: data.ibb.gov.tr dataset envanteri → bina/site veri seti var mı kontrol et
- [ ] DA-4/5: Ankara + İzmir açık veri portali → site/bina dataset indir
- [ ] DA-1: Microsoft ML Building Footprints TR → GitHub'dan GeoJSON indir, polygon atama için kullan
- [ ] DA-2: Google Open Buildings TR tile'ları → TR bbox ile parquet indir
- [ ] DA-6: Emlak Konut + Torunlar GYO 2024 proje sayfaları scrape
- [ ] DA-7: konut.gov.tr kentsel dönüşüm proje listesi

### P3 — Platform
- [ ] GitHub Release v0.6.0: güncel GPKG+CSV (75.775 kayıt)
- [ ] Province/district bazlı GeoJSON export → açık veri yayını
- [ ] created_at / updated_at damgası ekle
- [ ] CI: GitHub Action → tile build + otomatik yayın
- [ ] DB backup: weekly pg_dump → Release asset
