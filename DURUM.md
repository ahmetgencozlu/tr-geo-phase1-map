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
