# KRONOS X

Tek dosyalık (`kronos.html`), çevrimdışı çalışan, bağımlılıksız YKS komuta konsolu.

**KRONOS 9**'un arayüzü ve serbest çalışma katmanı korundu; üzerine **KARARGAH
(index-2)**'ın bütün motorları eklendi. İki uygulama tek sistemde birleşti.

Aç: `kronos.html` dosyasını tarayıcıda aç. Veri `localStorage`'da tutulur
(PDF'ler IndexedDB'de). Kurulum yok, sunucu yok, internet gerekmez.

---

## KRONOS 9, index-2'ye göre neleri taşımıyordu

Aşağıdaki maddelerin hepsi bu sürümde eklendi.

### 1. Müfredat modeli
| index-2 | KRONOS 9 | KRONOS X |
|---|---|---|
| 146 kazanımlık DAG: ön koşul zinciri, faz, zorluk, beklenen soru, puan getirisi | düz metin konu listesi, `kilitli/çalışılıyor/bitti` | **DAG geldi.** Eski düz liste "SERBEST" sekmesinde korunuyor |
| Kilit mantığı: ön koşulu pekişmeyen konu plana giremez | yok | var |
| `kilitAcmaSayisi`, `eksikOnkosul`, `kilitlenenler` | yok | var |

### 2. Puan, sıralama, projeksiyon
- **Puan motoru**: 8 alan × ÖSYM katsayıları + OBP katkısı.
- **Sıralama kalibrasyonu**: puan→sıralama çapa tablosu, logaritmik ara değer;
  SİSTEM ekranından güncellenebilir.
- **Certainty engine**: son 3 denemenin ağırlıklı netleri, 8 denemelik doğrusal
  regresyonla haftalık delta, doyum katsayılı (0,75) sınav günü projeksiyonu.
- **OBP kurtarma ekranı**: diploma notu → OBP → puan açığı; 12. sınıf senaryoları.

KRONOS 9'da bunların hiçbiri yoktu — yalnızca "en iyi net / hedef net" oranı vardı.

### 3. Disiplin motoru
- **Disiplin skoru (DS)**: görev tamamlama (55/80) + soru hedefi (25) + tekrar
  borcu (20), uyku ihlali −8, atlanan görev −2/adet, tam gün +5.
- **Rejim skoru**: EWMA (0,30·DS + 0,70·rejim). Üst barda canlı, alarm şeridi.
- **Alarm**: YEŞİL ≥75 · SARI 60-74 · TURUNCU 45-59 (analiz kilidi) · KIRMIZI <45.
- **Kurtarma rampası**: tek görev → yarım yük → çoğu → tam yük.
- **Borç ve kalıcı kayıp**: mazeret katsayısı (0 / 1,00 / 1,15 / 1,25 / 1,35),
  borç tavanı günün %40'ı, aşan kısım kalıcı kayıp — affedilmez.

### 4. Planlama motoru
KRONOS 9'un "Görev Üret"i konu haritasından sıradaki 3 konuyu alıp sabit 40 dk /
20 soru yazıyordu. Yerine gelen:
- **Kapasite**: faz (KAMP/HİBRİT/OKUL) × gün tipi (hafta içi/sonu) → dakika bütçesi.
- **Aşama kilidi**: 1 TYT Mat → 2 Problemler+TYT Fen+Geometri → 3 AYT Mat → 4 AYT Mat+Fen.
- **Dört blok** (%28/%32/%24/%16) ve bloğa özgü görev şablonları.
- **Mert Hoca akışı** (TYT Mat): her konu 7 adım — 4 video + 3 soru bankası testi;
  konu yarım bırakılıp sıradakine geçilmez. Plan tiki ile kazanım kartı tiki ortak.
- **Soru hızı öğrenme** (EWMA): girdiğin sonuçlardan dk/soru öğrenilir, süreler ona göre.
- **Çözüm günü**: haftanın bir günü konu yok — deneme + test maratonu + kota + kapanış.
- **Soru kotası**: bitirilen her mat konusu 500, fen konusu 250 soru borcu; Blok 2 besler.
- **Elle kurulum**: blok ekle/adlandır/taşı/sil, görev yaz, kazanım bağla, blok içinde sırala.
- **Düzen tablosu**: üretilen görevde yaptığın değişiklik güne kaydedilir; "tazele"
  dediğinde de yerinde durur. Tek görev veya tüm düzenlemeler geri alınabilir.
- **Üretim kapsamı**: tam gün / yarım gün / tek blok.
- **Yarından çek**: yarının konu görevlerini bugüne ek blok olarak alır.

### 5. Tekrar (SRS)
| index-2 | KRONOS 9 |
|---|---|
| Üç kuyruk: hata kaynaklı · planlı · deneme kaynaklı | tek liste |
| Aralıklar 1-3-7-21-45-90, aşama düşme, mezuniyet | `interval × 2` |
| **Konu iadesi**: 2 kez üst üste kaçarsan konu baştan + bağlı üst konular kilitlenir | yok |
| Fotoğraf değil **yönerge** tutma | başlık | 
| Tekrar oturumu (çözdüm/çözemedim akışı) | yok |

### 6. Deneme
KRONOS 9: ders ders D/Y/net tablosu. index-2: 8 alanlı net + puan/sıralama +
yanlış konu kotası. **KRONOS X ikisini birleştirir:**
- Ders tablosunu doldurursun, 8 alanlı net profili otomatik çıkar.
- Anlık puan/sıralama tahmini (diğer oturumun netleri son denemelerinden alınır).
- **Yanlış konuları**: konu + adet + neden (bilgi/işlem/dikkat/süre/okuma/çalışmadım).
  "Henüz çalışmadım" hata sayılmaz. Kota dolunca konu bir kademe geri düşer,
  deneme kaynaklı tekrar açılır; bilgi eksiğinden ikinci kez dolarsa tam konu iadesi.
- **Faz duyarlı okuma**: her alanı haritadaki kapsamınla birlikte okur.
  Çalışılmamış alandaki boşluk başarısızlık sayılmaz; çalışılmış alanda beklenen
  tavan hesaplanır (kapsam %40 ise 40 soruluk testten beklenen tavan 16 net).
  Tavanın üstüne çıkarsan "kapsam eksik işaretli" uyarısı gelir.
- Kaydedince otomatik **deneme raporu** katmanı açılır.

### 7. Analiz
KRONOS 9'un 10 paneline ek olarak: rejim özeti, DS trendi (30 gün çizgi grafik),
blok disiplini (hangi bloğu düşürüyorsun), haftanın günleri, görev tipi davranışı
(izliyorsun ama çözmüyorsun uyarısı), ders bazlı performans tablosu, aşama
ilerlemesi, soru kotası + sınava yetişme hesabı, faz duyarlı son deneme okuması,
30 günlük hata dağılımı, tekrar sağlığı, net trendi + projeksiyon, borç/kalıcı
kayıp + mazeret dağılımı, haftalık rapor. Rejim düşükken ekran kilitlenir
(kilit SİSTEM'den kapatılabilir, "yine de aç" düğmesi var).

### 8. Diğerleri
- **Aylık yol haritası**: bugünden sınava kadar her ay için faz + adımlar, otomatik doldurma.
- **Otomatik ilerleme** görünümü: ders ders bitti/aktif/sırada.
- **TYT Mat denemeleri**: 10 Check Up + 3 Değerlendirme, tik + net + PDF (IndexedDB).
- **Video kaynakları**: derse oynatma listesi (AYT fen için ayrı), kazanıma özel link.
- **Manuel konu sırası**: ▲▼ ile dersin sırasını sen belirlersin, sistem önceliğini ezer.
- **Kılavuz**: 18 maddelik sistem kuralları rehberi.
- **Arşiv & temizlik**: geçmiş günleri özetleyip arşivle, mezun tekrarları sil,
  veri boyutu göstergesi. Kümülatif istatistik silinmez.
- **Bildirim ve zamanlayıcılar**: kalkış, blok 1 gecikmesi (+15/+30 dk), uyku,
  turuncu/kırmızı alarm; gün dönümünde otomatik yenileme.
- **Kurulum (Faz 0)**: ilk açılışta sınav tarihi, hedef sıralama, diploma notu,
  günlük yoğunluk ve çözüm günü sorulur.

---

## KRONOS 9'dan korunanlar

Hiçbiri kaldırılmadı: fosfor CRT teması ve üç faz rengi (kızıl/kehribar/yeşil),
açılış animasyonu, Zen modu, günlük brifing, kanban / Eisenhower matrisi / liste
görünümleri, günün kurbağası, alt görevler ve markdown görev notu, ay-hafta-gün
takvimi ve sürükle-bırak, hedef sayaçları, alışkanlık takibi, markdown not odası,
tüketim arşivi, yedek al/yükle, panel katlama.

## Veri geçişi

- `kronos:db:v3` (KRONOS 9) → otomatik yükselir. Eski basit program blokları yeni
  plan yapısına taşınır, eski konu listesi "SERBEST" sekmesine, eski SRS kayıtları
  planlı tekrar kuyruğuna geçer.
- `karargah_db_v6` (index-2) → aynı tarayıcıda varsa motor katmanı (kazanım
  ilerlemesi, disiplin, sayaçlar, denemeler, tekrar kayıtları, Mert takibi,
  kalibrasyon, ayarlar) olduğu gibi devralınır.
- Yeni anahtar: `kronos:db:v10`.

---

## Spectrum ekranı (Stańczyk Spectrum)

`stanczykspectrum.html`'in tamamı — CSS'i, işaretlemesi, 74 fonksiyonluk betiği ve
CDN kütüphaneleri (JSZip, epub.js, html2pdf, html2canvas, marked) — KRONOS'a
**Spectrum** adıyla ayrı bir ekran olarak eklendi. SİSTEM > Ekranlar listesinden açılır.

İçindekilerin hepsi çalışır durumda: zincir (streak) sayacı, 6 aylık okuma ısı
haritası, kitap kartları (kapak, etiket, kategori, ilerleme çubuğu, tahmini bitiş,
özel işaret, not rozeti), arama + durum/kategori/etiket süzgeçleri + sıralama,
kitap ekleme, oturum kaydı, istediklerim listesi (öncelik, gerekçe, markdown not,
önizleme), günlük kayıtlar, yazar dağılımı, okuma analizi (PDF raporu), kart
oluşturma, yedekleme (dışa/içe aktarma), EPUB/PDF okuyucu ve markdown not dışa
aktarma.

**Nasıl birleştirildi**
- CSS'in tamamı `#view-spectrum` altına hapsedildi (`:root`, `*`, `body`, `header`
  gibi genel seçiciler dahil), böylece KRONOS'un fosfor teması etkilenmiyor ve
  Spectrum kendi Orbitron/Inter tipografisiyle kendi renk değişkenlerini kullanıyor.
- İşaretleme bire bir korundu; 126 ID'nin hiçbiri KRONOS'unkilerle çakışmıyor.
- Betik ayrı bir `<script>` bloğunda, KRONOS'un IIFE'sinden bağımsız global
  kapsamda duruyor — `onclick` bağlarının çalışması için gerekli, çakışma yok.
- Veri ayrı: `cyberread_books` / `cyberread_logs` / `cyberread_wishlist` ve
  kitap dosyaları için `CyberReadFilesDB_v5` (IndexedDB).
- KRONOS'un görünüm giriş animasyonu, `position:fixed` modallara kapsayıcı blok
  yarattığı için bu ekranda kapatıldı; modaller ekranı tam kaplıyor.
- CDN kütüphaneleri çevrimdışıyken yüklenmezse uygulama çalışmaya devam eder;
  yalnızca EPUB/PDF okuma, PDF dışa aktarma ve markdown önizleme devre dışı kalır
  (kaynak dosyadaki davranışın aynısı).
