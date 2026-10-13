# tr_geo_phase1 — Durum ve Gece Planı (12→13 Ekim 2026)

Harita: https://ahmetgencozlu.github.io/tr-geo-phase1-map/  ·  Rapor: …/report.html
Repo: github.com/ahmetgencozlu/tr-geo-phase1 (özel) · Harita repo: tr-geo-phase1-map (Pages)

## Mevcut sayılar (12 Ekim 23:50 UTC)
- Varlık: 32.056 (aktif 13.271 / incelemede 18.785)
- Polygonlu: 18.462 (site sınırı 5.380, aday alan 4.247, bina ayak izi 2.053 + 6.782 Overture)
- Adres (kaynaktan): ~3.474 verified; mahalle eşleşen: büyük çoğunluk
- Kategori: konut sitesi 28.096 · plaza 3.160 · toplu konut 614 · teknopark 186 (resmî TGB 115, eşleşen 58)

## Bugün yapılanlar
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
