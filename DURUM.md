# tr_geo_phase1 — Durum ve Gece Planı (12→13 Ekim 2026)

Harita: https://ahmetgencozlu.github.io/tr-geo-phase1-map/  ·  Rapor: …/report.html
Repo: github.com/ahmetgencozlu/tr-geo-phase1 (özel) · Harita repo: tr-geo-phase1-map (Pages)

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
