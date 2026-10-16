# Yerdex Veri Kaynakları Güvenilirlik Matrisi

> Son güncelleme: 2026-10-16  
> Bu belge, `geo_phase1.source_quality` tablosunun insan-okunabilir karşılığıdır.

---

## Kullanılan Kaynaklar (Aktif)

| Kaynak | Etiket | Kalite | Lisans | Koordinat Türü | Not |
|--------|--------|--------|--------|----------------|-----|
| `osm` | OpenStreetMap | **0.92** | ODbL | surveyed | Gönüllü haritalama; TR için iyi kapsam. Birincil polygon kaynağı. |
| `wikidata` | Wikidata | **0.93** | CC0 | surveyed | Yapılandırılmış, özgür lisans. AVM ve teknopark doğrulaması için kullanılır. |
| `sanayi_gov_tr` | sanayi.gov.tr TGB | **0.95** | Kamu | unknown | Resmi bakanlık verisi; koordinat yok, adres bazlı geocoding gerekir. |
| `overture_buildings` | Overture Buildings | **0.90** | ODbL | surveyed | Bina footprint; yüksek geometrik doğruluk. 7.4M TR bina. |
| `overture` | Overture Places | **0.88** | ODbL | gps | Meta+OSM+Foursquare derlemesi; conf≥0.55 eşiği uygulandı. |
| `meta` | Meta Spatial Data | **0.91** | CC BY 4.0 | gps | Doğrulanmış ticari veri. |
| `wikipedia` | Wikipedia | **0.90** | CC BY-SA | geocoded | Koordinat genellikle ana giriş noktası; teknopark kuruluş yılı için kullanıldı. |
| `geonames` | GeoNames | **0.85** | CC BY 4.0 | geocoded | Geniş kapsam; bazı yerleşim hatası mümkün. |
| `foursquare` | Foursquare Places | **0.83** | CC BY 4.0 | gps | Kullanıcı katkılı; büyük hacim (53.921 kayıt). |
| `antalya_bb` | Antalya Büyükşehir | **0.82** | CC BY | surveyed | Belediye CBS verisi; koordinat kalitesi yüksek. |
| `emlakkonut` | Emlak Konut GYO (çoklu) | **0.85** | Faktüel | gps | Birden fazla kaynakla doğrulanmış kayıtlar. |
| `kiptas` | KİPTAŞ | **0.82** | Kamu | gps | İBB iştiraki; güvenilir. |
| `toki_gov_tr` | toki.gov.tr | **0.80** | Kamu | centroid | Resmi TOKİ; scraping ile alındı. |
| `alltheplaces` | AllThePlaces | **0.78** | CC0 | gps | Zincir mağaza verisi. |
| `kmaras_kentrehberi` | K.Maraş Kent Rehberi | **0.65** | Faktüel | geocoded | Şehir rehberi; tek kaynak kayıtları excluded yapıldı (19 kayıt). |

---

## Dışlanan Kaynaklar

| Kaynak | Etiket | Kalite | Neden Dışlandı | Tarih |
|--------|--------|--------|----------------|-------|
| `ayd` | AYD AVM Listesi | **0.40** | JavaScript değişkeninden çıkarıldı; koordinat doğrulaması yok; tümü tek kaynak; 224 kayıt excluded_entity | 2026-10-16 |
| `emlak_konut` | Emlak Konut GYO (tek) | **0.45** | Tek kaynak, polygon yok, çapraz doğrulama yapılamadı | 2026-10-16 |
| `toki_talep` | talep.toki.gov.tr | **0.55** | Aktif satış projesi = geçici inşaat konumu; 203/205 OSM/Overture ile doğrulanamadı | 2026-10-16 |

---

## Kalite Kriterleri

Bir kaynağın **aktif kayıt** üretebilmesi için en az ikisini karşılaması gerekir:

1. **Koordinat kaynağı**: `surveyed` veya `gps` (geocoded kabul edilir, kalite düşer)
2. **Bağımsız doğrulama**: En az 1 farklı bağımsız kaynak ile 300m içinde eşleşme
3. **Lisans uyumu**: ODbL, CC BY, CC0 veya Kamu (kapalı kaynak kabul edilmez)
4. **Kaynak kalite skoru** ≥ 0.65

---

## Araştırılan Ama Entegre Edilemeyen Kaynaklar

| Kaynak | Engel |
|--------|-------|
| ULASAV (ulasav.csb.gov.tr) | AVM shapefile mevcut ama indirme engeli (browser gerekli) |
| OSBÜK (osbuk.org) | 420 OSB, koordinat yok |
| Ankara Şeffaf Portal | SPA, kimlik doğrulama gerekiyor |
| Kayseri CBS | ASP.NET/Telerik, API erişilemiyor |
| Malatya CBS | 404 |

---

## Güncelleme Politikası

- OSM/Overture: Yeni PBF/Parquet yayınlandığında güncelle
- Wikidata: Her 3 ayda bir yeniden sorgula
- Belediye verileri: Yıllık kontrol
- Bu belge: Her kaynak eklendiğinde/çıkarıldığında güncellenecek
