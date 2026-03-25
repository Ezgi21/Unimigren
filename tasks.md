# UniMigren Görev Listesi

Bu doküman, `prd.md`’deki vizyona göre uygulamayı adım adım geliştirmek için çıkarılmış görev listesidir.

## Hedef
Öğrenci kullanıcının günlük rutinlerini girip (uyku/kafein/stres), hava durumu ve akademik takvim bilgisini kullanarak Gemini üzerinden “Migren Risk Skoru (%)” ve kısa önleyici tavsiyeyi görmesini sağlayan çalışan MVP.

## Task Formatı
- Her görev başlangıçta `pending` varsayılır (dilerseniz daha sonra durum ekleyebiliriz).
- Kabul kriterleri, görevin “bitti” sayılması için gereken minimum işlevi belirtir.

## Phase 1 — MVP Kurulum ve Temel UI

1. [ ] Proje iskeletini oluştur (static site başlangıç)
   - Kabul kriterleri: `index.html`, `styles.css` (veya Tailwind kullanımı) ve `app.js` dosyaları kökte bulunur; sayfa tarayıcıda hatasız açılır.

2. [ ] Tailwind CSS yaklaşımını netleştir ve temel tema oluştur
   - Kabul kriterleri: Dashboard görünümü okunabilir renk paletiyle gelir; “Risk” alanı ve “Tavsiye” alanı için placeholder UI yerleşir.

3. [ ] Ana Ekran UI: Risk Göstergesi + AI Tavsiye Kutusu
   - Kabul kriterleri: Risk alanı “0-100 arası yüzde” olarak görsel gösterilir (renk/gradients dahil); tavsiye kutusu metin basabilir.

4. [ ] Akıllı Veri Giriş Paneli UI (Günlük Rutinler)
   - Kabul kriterleri: Form en az şu alanları içerir: `uyku_suresi`, `kafein_miktari`, `stres_seviyesi`; “Kaydet” ve “Bugün Analiz Et” butonları vardır.

5. [ ] Akademik Takvim UI (yüksek stresli dönem tanımı)
   - Kabul kriterleri: En az “Vize/Final Haftası” gibi bir dönem türünü gün bazında işaretlemeye yarayan basit bir arayüz olur (ör. tarih girişi veya mevcut tarih aralığı).

6. [ ] Model/Şema tasarımı (front-end veri formatı)
   - Kabul kriterleri: Uygulama içinde tek bir `state`/`payload` formatı tanımlanır: günlük rutin + takvim flag + lokasyon/konum (hava için) + tarih.

## Phase 2 — Veri Alma: Hava Durumu Entegrasyonu (anahtar gerektirmeyen başlangıç)

7. [ ] Hava durumu provider seç ve entegrasyon iskeletini kur
   - Kabul kriterleri: “Basınç” ve “nem” alanlarını döndürebilen bir servis (tercihen API key’siz, ör. Open-Meteo) kullanılır; hata durumlarında kullanıcıya mesaj gösterilir.

8. [ ] Konum alma stratejisi
   - Kabul kriterleri: Basit yaklaşım olarak ya (a) kullanıcıdan şehir/alantıp girişi alınır ya da (b) tarayıcı konum izniyle enlem/boylam toplanır; ikisinden hangisi seçildiyse çalışır.

## Phase 3 — Gemini Tahmin Motoru (server-side + güvenli anahtar yönetimi)

9. [ ] Backend yaklaşımını seç (Netlify/Lovable uyumlu)
   - Kabul kriterleri: Gemini API anahtarı istemcide görünmez; minimum bir server endpoint’i (ör. `/api/predict`) hazırdır.

10. [ ] Gemini “Risk Skor + Tavsiye” için prompt tasarımı
   - Kabul kriterleri: Prompt; giriş verilerinden yararlanarak hem `risk_score_percent` (0-100 sayısı) hem de `recommendations` (kısa metin) üretir; çıktı formatı (tercihen JSON) belirlenmiştir.

11. [ ] AI cevabını doğrulama ve parse etme
   - Kabul kriterleri: Gemini yanıtı JSON beklenen şemaya uymadığında güvenli fallback yapılır (ör. risk skoru yoksa “hesaplanamadı” durumu).

12. [ ] API isteği akışını bağla (front-end -> backend -> Gemini)
   - Kabul kriterleri: Kullanıcı “Bugün Analiz Et”e bastığında risk skoru ve tavsiye kutusu güncellenir; yükleme (loading) ve hata durumları UI’da görünür.

## Phase 4 — Günlük Akış, Kalıcılık ve Tarihçeleme (MVP kalite artışı)

13. [ ] Gün bazında sonuç saklama (localStorage veya backend DB)
   - Kabul kriterleri: Aynı gün için veri girildiyse tekrar analiz edilmeden önce “kaydedilmiş sonuç” gösterilebilir.

14. [ ] “Migren Risk Skoru” tarihçesi (mini grafik veya liste)
   - Kabul kriterleri: Son 7/14 gün risk skorları ekranda görüntülenir (basit bir bar/list yeterli).

15. [ ] Girdi validasyonu
   - Kabul kriterleri: Uyku/kafein/stres alanları için aralık doğrulaması vardır; eksik/hatali girişte form kullanıcıyı uyarır.

16. [ ] Ekran süresi ve ek değişkenler (opsiyonel, PRD’ye uyumlu genişletme)
   - Kabul kriterleri: Eğer eklenecekse, UI + payload + prompt tarafına aktarılır; ek değişkenler opsiyonel kalır (boşsa çalışır).

## Phase 5 — Test, Güvenlik ve Deploy

17. [ ] End-to-end manuel test senaryoları oluştur
   - Kabul kriterleri: En az şu senaryolar dokümante edilir ve denenir: boş form, normal form, aşırı değerler, hava servis hatası, Gemini servis hatası.

18. [ ] Unit test (parse/validation) (varsa test runner ekle)
   - Kabul kriterleri: Gemini JSON parse ve risk skoru sınırlandırma test edilir (ör. 0-100 clamp).

19. [ ] Güvenlik kontrol listesi
   - Kabul kriterleri: API anahtarı sadece backend’te; istemciye sızmıyor; kullanıcı girdi verileri basic sanitize/validate ediliyor.

20. [ ] Deploy
   - Kabul kriterleri: Tercih edilen platforma (Netlify veya Lovable) statik site + backend fonksiyonu publish edilir; canlı endpoint çalışır.

## Phase 6 — Beta İyileştirmeler (isterseniz)

21. [ ] Onaylı kalibrasyon: geçmiş atak örüntüsü (baseline)
   - Kabul kriterleri: Kullanıcı geçmiş atak bilgisi (varsa) girer; risk skoru kişiselleştirmeye alınır (en azından prompt’ta kullanılacak şekilde).

22. [ ] Uyarı/hatırlatma sistemi (opsiyonel)
   - Kabul kriterleri: Belirlenen saat aralığında (ör. akşam 18:00) kullanıcının “bugün risk yüksek olabilir” uyarısı gösterilir (push veya mail; platforma göre).

23. [ ] Accessibility (erişilebilirlik) iyileştirmeleri
   - Kabul kriterleri: Renk körlüğüne uygun risk renkleri + ekran okuyucu için etiketler eklenir.

## Açık Sorular (netleşsin diye)
1. Kullanıcı konum/veri girişi: şehir mi, konum izni mi?
2. Takvim modeli: sadece “vize/final haftası” mı, yoksa kullanıcı kişisel dönem de ekleyebilecek mi?
3. Gemini’den risk skoru: tamamen LLM üretimi mi, yoksa deterministik bir baseline + LLM tavsiye mi?

