# UniMigren — Kullanıcı Akışı (User Flow)

## Adım 1: Karşılama ve Hızlı Veri Girişi

Kullanıcı uygulamayı açtığında karşısına temiz ve yormayan bir arayüz çıkar.

- **Gördüğü:** "Bugün Nasıl Hissediyorsun?" başlığı ve altında 3-4 adet hızlı seçim alanı.

- **Yaptığı:**
  - Uyku süresini seçer (örn: 4-6 saat).
  - İçtiği kahve/kafein miktarını işaretler.
  - O anki stres seviyesini 1-10 arası bir kaydırıcı (slider) ile belirler.
  - (Eğer o gün sınavı varsa) "Sınav Günü" kutucuğunu işaretler.

---

## Adım 2: AI Analiz Süreci

Kullanıcı verilerini girdikten sonra "Riskimi Hesapla" butonuna basar.

- **Arka Planda Ne Olur?:**
  - Uygulama, kullanıcının verilerini alır.
  - Canlı hava durumu API'sinden o anki basınç ve nem verilerini çeker.
  - Tüm bu verileri (Kişisel Rutin + Akademik Takvim + Hava Durumu) Gemini API'ye gönderir.

---

## Adım 3: Sonuç ve Tahmin Ekranı (Dashboard)

Analiz saniyeler içinde tamamlanır ve kullanıcı ana ekrana (Dashboard) yönlendirilir.

- **Gördüğü:**
  - **Risk Skoru:** "%75 Yüksek Risk" gibi belirgin bir yüzde ve renkli bir uyarı (Kırmızı / Sarı / Yeşil).
  - **AI Tavsiyesi:** Gemini tarafından o ana özel üretilmiş bir mesaj: "Ezgi, düşük basınç ve sınav stresi birleşiyor. Lütfen şimdi 2 bardak su iç ve ekran ışığını düşür."

---

## Adım 4: Takip ve Kayıt

- **Sonuç:** Kullanıcı, atağın gelme ihtimalini bilerek önlemini alır.
- **Geçmiş:** Bu veriler "Geçmiş" sekmesine kaydedilir; böylece kullanıcı hangi günlerde neden atak yaşadığını geriye dönük inceleyebilir.
