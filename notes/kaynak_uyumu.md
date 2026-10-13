# Kaynaklar birbirini ne kadar tutuyor? (2026-10-13)

Tablo: `geo_phase1.entity_source` (kayıt × kaynak × kaynağın kendi koordinatı × ana konuma uzaklık × link × lisans). 123.782 satır, 63.790 kaydın tamamı kapsanıyor. Kayıt alanı `source_spread_m` = o kaydın kaynakları arasındaki en büyük konum farkı.

## Konum uyumu (aynı kayıtta iki koordinatlı kaynak)
| Kaynak çifti | Kayıt | Medyan | Ortalama | ≤25 m | ≤100 m | >250 m |
|---|---|---|---|---|---|---|
| Foursquare ↔ Overture | 16.866 | 13 m | 23 m | %65 | %98 | 18 |
| Foursquare ↔ OSM | 5.502 | 34 m | 39 m | %34 | %100 | 0 |
| OSM ↔ Overture | 3.388 | 36 m | 55 m | %36 | %83 | 64 |
| Foursquare ↔ GeoNames | 74 | 122 m | 162 m | %3 | %43 | 16 |
| GeoNames ↔ Overture | 57 | 118 m | 168 m | %9 | %46 | 13 |

Yorum: FSQ/Overture/OSM üçlüsü birbirini güçlü tutuyor (medyan 13–36 m; OSM site polygonunun merkezi ile POI girişi arasındaki fark doğal). GeoNames mahalle-merkezi hassasiyetinde (~120 m) — sadece ad kanıtı, konum için kullanılmıyor.

## Kayıt başına yayılım (≥2 koordinatlı kaynak, 19.631 kayıt)
≤10 m 7.691 · 10–25 m 3.726 · 25–50 m 4.126 · 50–100 m 3.577 · 100–250 m 443 · **>250 m 68** (çelişki adayı, incelenecek)

## İsim uyumu (trigram benzerliği)
| Çift | Kayıt | Ort. | ≥0,8 |
|---|---|---|---|
| Foursquare ↔ Overture | 16.866 | 0,97 | %91 |
| Foursquare ↔ OSM | 5.502 | 0,93 | %81 |
| OSM ↔ Overture | 3.388 | 0,91 | %72 |
| GeoNames ↔ FSQ/Overture/OSM | 152 | 0,82–0,85 | ~%50 |
| TOKİ listesi ↔ herhangi | 554 | 0,10 | %0 |
| Bakanlık TGB ↔ herhangi | 98 | 0,40 | ~%6 |

Yorum: TOKİ ve TGB'de düşük benzerlik beklenen; resmî ad ("Ankara Mamak 3. Etap 1250 Konut") ile halk adı ("Toki Konutları") farklı — eşleşme il+ilçe+etap ile yapıldı, `official_name` ayrı tutuluyor.

## Çelişki örnekleri (>250 m)
Veliefendi Konutları (266 m), Göztepe Konutları (256 m), Yüreğir Afet Evleri (393 m) — büyük sitelerde kaynakların farklı kapıları göstermesi; polygon olanlarda sorun değil, point_only olanlar gözden geçirilmeli.
