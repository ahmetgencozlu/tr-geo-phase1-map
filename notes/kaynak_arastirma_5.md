# Kaynak Araştırması 5 — OSM Genişletilmiş + Belediye ArcGIS/CKAN + Geocoding Alternatifleri

Tarih: 2026-10-14 · Durum: **TAMAMLANDI** · 4 paralel subagent · Değerlendirilen kaynak: ~25

## Hemen entegre edilebilir (öncelik sırası)

| # | Kaynak | Puan | Hedef |
|---|--------|------|-------|
| 1 | **OSM admin_level=8 Mahalle polygonları** | **5** | 4.268 mahallesiz kayıt → spatial join (13.902 polygon mevcut) |
| 2 | **OSM `building=apartments + name`** | **5** | 31.795 adlı apartman → yeni aday (hiç çekilmemiş tag) |
| 3 | **Antalya BB ArcGIS FeatureServer (public)** | **5** | Bina polygon + kat sayısı; token yok, direkt sorgu |
| 4 | **Konya BB CKAN — Mahalle GeoJSON** | **4** | Konya mahalle polygonları (GeoJSON, açık) |
| 5 | **Photon geocoder (Komoot)** | **4** | Spatial join sonrası kalan kayıtlar için reverse geocoding |
| 6 | **OSM `landuse=residential + name`** (genişletilmiş) | **3** | 16.804 kayıt; bir kısmı Overture ile örtüşür ama OSM ilişkileri yeni |

---

## Erişilemeyen kaynaklar (DNS/güvenlik duvarı sorunu — sandbox kısıtı)

| Kaynak | Durum | Not |
|--------|-------|-----|
| Ankara BB (`data.ankara.bel.tr`, `sehir.ankara.bel.tr`) | 000 DNS | Tarayıcıdan denenebilir |
| İzmir BB (`acikveri.izmir.bel.tr`, `opendata.izmir.bel.tr`) | 000 DNS | — |
| data.gov.tr | 000 DNS | Ulusal portal — tarayıcıdan denenebilir |
| Bursa BB CKAN | 000 DNS | CBS endpointi de kapalı |
| Gaziantep BB CKAN | 000 DNS | ArcGIS: login zorunlu |
| CSB Web API (`cbswebapi.csb.gov.tr`) | 000 DNS | — |
| Ulusal Geoportal CSB | 000 DNS | — |

---

## Ticaret Bakanlığı AVM Listesi
- `ticaret.gov.tr/ic-ticaret/avm` → HTTP 200 ama içerik: `Internal Server Error`
- Alternatif: **AYD (Alışveriş Merkezleri ve Yatırımcıları Derneği)** — `ayd.org.tr`

---

## Detay Bulgular

### OSM `building=apartments + name` — **31.795 kayıt**
- Overpass sorgusu: `building=apartments` + `name` tag'i olan node+way (TR bbox)
- Way sayısı: 31.755, node: 40
- Lisans: ODbL
- Bu tag kombinasyonu daha önce hiç çekilmemiş; sql/30'daki OSM bina sorgusundan farklı
- OSM adlı bina polygon → centroid + ad → mevcut kayıtlarla eşleştir veya yeni aday aç
- **Aksiyon:** `sql/33` — Overpass ile çek, osm_id bazında tekilleştir, mevcut kayıtlarla eşleştir

### OSM `admin_level=8` Mahalle Polygon — **13.902 relation**
- Türkiye'de mahalle = `boundary=administrative + admin_level=8` (NOT 10!)
- `admin_level=10`: sadece 68 relation (neredeyse hiç kullanılmıyor)
- Önceki mahalle araştırmalarında `admin_level=8` denenmiş mi belirsiz; mevcut mahalle setlerini tamamlar
- Overpass ile TR geneli mahalle relation'ları indirilebilir (ya da Geofabrik PBF'den extract)
- **Aksiyon:** `sql/32` — mahallesiz 4.268 kayıt için spatial join

### Antalya BB ArcGIS Building FeatureServer — **PUBLIC, token yok**
- URL: `https://cbs.antalya.bel.tr/server/rest/services/Antalya_MM_Building/FeatureServer/0/query`
- Alanlar: `ZEMINUSTUKATSAYISI` (kat), `ZEMINALTIKATSAYISI`, `PARSELNO`, `ADANO`, `TIP`, `Shape_Area`
- 2000+ feature pagination ile tüm il taranabilir
- IMAR klasöründe 20 plan servisi de var
- Lisans: kamu (belirsiz; K.Maraş ile aynı yaklaşım: atıf + kayıt)
- **Aksiyon:** `sql/34` — pagination ile çek, `ZEMINUSTUKATSAYISI>2` filtresi, centroid → new candidates

### Konya BB CKAN Mahalle GeoJSON
- URL: `https://acikveri.konya.bel.tr/api/3/action/package_search?q=mahalle`
- `Konya Mahalleleri` → GeoJSON x2 (mahalle + ilçe sınırları)
- 239 toplam paket, açık CKAN
- **Aksiyon:** GeoJSON indir → `geo_phase1.nbhd_external` tablosuna ekle → Konya mahallesizleri kapat

### Photon (Komoot) Reverse Geocoding
- `https://photon.komoot.io/reverse?lon=...&lat=...`
- `district` alanı → mahalle adı (Türkiye testleri başarılı)
- Rate limit belirsiz (public API); self-host seçeneği var (Java 21+, 64 GB RAM)
- Nominatim public'den daha hızlı olabilir (rate limit yok)
- **Aksiyon:** spatial join sonrası kalan kayıtlar için Photon API — sql/36

### Nominatim Reverse Geocoding
- `suburb` alanı → Türkiye'de mahalle adını döndürüyor (tutarlı)
- Public: 1 req/sn → 4.268 kayıt ≈ 71 dk (tek seferlik kabul edilebilir)
- Self-host için: `turkey-261013.osm.pbf` (650 MB, 13 Ekim güncel)
- Docker: `mediagis/nominatim:5.3`
- Sandbox'ta Docker yok; public API kullanımı 1 req/sn ile uygun

### Wikidata SPARQL Genişletilmiş
- Q3257686 (konut kompleksi) + Q174814 (housing estate) + Q1497375 (residential area) = **16 kayıt** (temiz)
- Q44539 (gated community): antik tapınak kayıtları → **KULLANILMAZ**
- Q2020153 (science park): akademik konferanslar → **KULLANILMAZ**
- Teknopark için Wikidata yetersiz; BTGM/TÜBİTAK resmî listesi daha iyi

### İBB Açık Veri (kısmi)
- `Istanbul Building Density Map` — WMS katmanı (geometri mevcut)
- Konut istatistikleri CSV/XLSX (site bazlı değil, istatistiksel)
- Doğrudan site/kompleks verisi yok → Yerdex için düşük öncelik

### data.gov.tr, geoBoundaries, OpenCage
- data.gov.tr: DNS yok (sandbox kısıtı)
- geoBoundaries TUR: sadece ADM1+ADM2 (il+ilçe), mahalle yok
- OpenCage: 2.500/gün ücretsiz tier; "test only, not production" koşulu → uygunsuz

---

## Aksiyon Planı

```
sql/32 — OSM admin_level=8 mahalle polygonları → 4.268 mahallesiz kayıt spatial join
sql/33 — OSM building=apartments adlı (31.795) → yeni aday + mevcut kanıt
sql/34 — Antalya BB ArcGIS bina → Antalya coverage + kat/tip bilgisi
sql/35 — Konya CKAN mahalle GeoJSON → nbhd_external + Konya mahallesiz kapat
sql/36 — Photon/Nominatim reverse geocoding → kalan mahallesiz kayıtlar
```
