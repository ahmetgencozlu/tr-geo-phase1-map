# G2 — single_source Profil Analizi

**Tarih:** 2026-10-14  
**Toplam single_source kayıt:** 34.868  

---

## 1. Neden Tek Kanıt?

`single_source` durumu, bir entity'nin yalnızca 1 bağımsız kaynaktan gelmesi anlamına gelir (`array_length(independent_sources,1) = 1`). Bu kayıtlar ikinci bir kaynak tarafından doğrulanmamıştır.

### Kaynak Dağılımı

| Kaynak             | Adet  | Oran  |
|--------------------|-------|-------|
| foursquare         | 29.849 | 85,6% |
| osm                | 4.718  | 13,5% |
| kmaras_kentrehberi | 252    | 0,7%  |
| wikipedia          | 45     | 0,1%  |
| wikidata           | 4      | 0,01% |

**Asıl neden:** Foursquare verisi küçük ya da kapalı siteleri listeler; OSM'de haritalanmış ama Foursquare/Wikidata'da adı geçmeyen yerleşim kompleksleri bulunur. Bu iki kaynak çoğunlukla örtüşmez.

---

## 2. İl/Kategori Dağılımı

### Kategori

| Kategori         | Adet  | Oran  |
|------------------|-------|-------|
| residential_site | 30.421 | 87,3% |
| plaza            | 3.359  | 9,6%  |
| mass_housing     | 1.015  | 2,9%  |
| technopark       | 73     | 0,2%  |

### İller (İlk 10)

| İl       | Adet  |
|----------|-------|
| İstanbul | 3.608 |
| Ankara   | 2.403 |
| İzmir    | 2.218 |
| Antalya  | 1.847 |
| Bursa    | 1.102 |
| Balıkesir| 1.057 |
| Konya    | 1.049 |
| Muğla    | 1.049 |
| Mersin   | 1.046 |
| Kocaeli  | 975   |

---

## 3. Yüksek Güven Segmenti

| Kriter                              | Adet   |
|-------------------------------------|--------|
| name_pattern = true                 | 33.408 |
| polygon IS NOT NULL OR has_footprint | 11.799 |
| **İkisi birden (yüksek güven)**     | **10.650** |
| — kaynaklara göre: foursquare       | 6.883  |
| — kaynaklara göre: osm              | 3.596  |
| — kaynaklara göre: kmaras           | 171    |

### Yorum

- **10.650 kayıt** hem kalıplı isme (name_pattern=true) hem geometriye (polygon/footprint) sahip; bu kayıtlar tutarlı isimlendirme örüntüsüne ve alan sınırına sahip olduğundan aktife terfi edilmeye uygundur.
- OSM single_source'larının büyük çoğunluğu evidence_count=1, member_count=1: çevre verisini henüz kazanamamış gerçek ama izole tespit.
- Foursquare single_source'larının %23,1'i polygon/footprint içeriyor; Foursquare'in geometri verisi sınırlı olduğundan bu oran yüksek sayılır.

---

## 4. G3 için Öneri

`name_pattern = true AND (polygon IS NOT NULL OR has_footprint = true)` koşuluyla 10.650 kayıt `single_source → active` terfiye alınabilir. Bu işlem toplam aktif sayısını **~40.677'ye** çıkaracaktır.

