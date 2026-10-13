# G1 — Veri Kalitesi Analizi (SQL, salt okunur)
Tarih: 2026-10-13 gece

## 1. En Sık İsim Kalıpları (rakamlar ayıklanmış)
| İsim Kalıbı | Adet |
|---|---|
| palmiye sitesi | 57 |
| toki konutları | 55 |
| kardelen sitesi | 45 |
| yıldız sitesi | 43 |
| barış sitesi / huzur sitesi / güneş sitesi | 40–35 |

→ Genel isimler (Palmiye, Kardelen, Çamlık, Yıldız, Barış) yaygın; tekrarları ilçe düzeyinde ayırt etmek gerekiyor.

## 2. Yanlış Kategori Adayları

### plaza kategorisinde 'site' içeren: 16 kayıt
- Çoğu "review" statüsünde, tek kaynaklı
- Örnekler: "gulek plaza sitesi", "Vipas plaza sitesi", "Kulesite Plaza"
- **Öneri:** "plaza sitesi" kalıbı → residential_site'a taşı veya elle değerlendir

### residential_site kategorisinde 'plaza' içeren: 1 kayıt
- "Plazakent Sitesi" / Bursa / review — isim site ama plaza da içeriyor, makul

### plaza kategorisinde tekno_isimli: 29 kayıt
- technopark kategorisine taşınması değerlendirilebilir

### mass_housing kategorisinde 'site' içeren ama 'toki' içermeyen: ~32 kayıt
- mass_housing içinde 582/614'ü TOKİ — geri 32 adet gerçekten toplu konut mu kontrol edilmeli

## 3. Çift İsim Tespiti (aynı il+ilçe+isim, >2 kayıt)

| İsim | İl | İlçe | Adet |
|---|---|---|---|
| Site | Balıkesir | Bandırma | **12** |
| Şirinevler Sitesi | Balıkesir | Bandırma | 3 |
| Seğmen Sitesi | Ankara | Çankaya | 3 |
| TOKİ | Mardin | Artuklu | 3 |
| Palmiye Sitesi | Antalya | Muratpaşa | 3 |

→ Bandırma/Balıkesir verisi gürültülü: "Site" adıyla 12 farklı kayıt, muhtemelen OSM aynı alanı birden fazla node/way olarak içeriyor.
→ Mardin/Artuklu'da 3 farklı TOKİ kaydı — farklı etaplar mı? birleştirme/etiketleme gerekebilir.

## 4. Eksik/Boş İsim
- **0 kayıt** — tüm 32.056 kaydın ismi dolu. ✅

## 5. Özet Aksiyon Maddeleri
1. Bandırma "Site" çiftleri (12 adet) → G2'de review profili incelenecek
2. plaza+site çakışması (16 kayıt) → review kuyruğuna alınabilir
3. plaza kategorisinde tekno kelimeli 29 kayıt → technopark kontrolü
4. mass_housing içinde TOKİ olmayan ~32 kayıt → kaynak doğrulaması gerekli
