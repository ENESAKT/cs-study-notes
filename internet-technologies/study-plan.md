# İnternet Teknolojileri - Frontend Çalışma Planı

**Son güncelleme:** 23 Mayıs 2026

Bu plan `yol_haritasi.md`, `defter_notlari.md`, `ders.md`, `pdf/9__HAFTA_DERS_NOTLARI_UPDATED.docx`, `pdf/DERS_NOTLARI.docx`, `pdf/DERS_NOTLARI-NODE_JS.docx` ve `pdf/Bootstrap.pdf` birlikte okunarak güncellendi. Eski planda 15 ve 16. bölümler açık görünüyordu; defter notları ve yol haritası incelendiğinde bu bölümlerin tamamlandığı görüldü. 23 Mayıs 2026'da Node.js / Express.js materyali ve final sınav odaklı routing, form, fonksiyon, console çıktısı ve Bootstrap grid notları ders notlarına eklendi.

---

## Güncel Durum

Tamamlanan ana bölümler:

- 3. Bölüm: Klavye eventleri
- 4. Bölüm: Event parametresi, `this` ve toggle
- 5. Bölüm: Çoklu seçim ve toplu element yönetimi
- 6. Bölüm: Form elemanları
- 7. Bölüm: Validasyon
- 8. Bölüm: `classList`
- 9. Bölüm: Element boyutları
- 10. Bölüm: `getComputedStyle`
- 11. Bölüm: Zamanlayıcılar
- 12. Bölüm: `window` nesnesi
- 13. Bölüm: Fonksiyonlar
- 15. Bölüm: JSON
- 16. Bölüm: Local Storage ve Session Storage

Açık / uzlaştırılması gereken bölümler:

- 1. Bölüm: Event Listener Temelleri
- 2. Bölüm: Fare Eventleri
- 14.1 Senkron ve asenkron çalışma mantığı
- 14.2 Callback Hell problemi
- Final sınav odaklı Node.js / Express routing ve Bootstrap tekrar maddeleri yol haritasına eklendi.

Not: 14.3-14.7 tamamlanmış görünüyor. Bu yüzden 14. bölümde yalnızca temel kavram tekrarı eksik.

---

## Öncelikli Çalışma Sırası

### 1. Hızlı Temel Tamamlama

Amaç: Yol haritasında açık kalan 1 ve 2. bölümleri hızlıca kapatmak.

Çalışılacaklar:

- `addEventListener()` mantığı
- `onclick` ve `addEventListener()` farkı
- `click`, `dblclick`, `mouseover`, `mouseout`, `mousemove`
- Event nesnesinden koordinat okuma

Mini görev:

- Bir butona tıklayınca metin değişsin.
- Bir kutunun üstüne gelince renk değişsin, çıkınca eski haline dönsün.

### 2. Async Temel Boşluğu

Amaç: 14. bölümde açık kalan 14.1 ve 14.2 maddelerini netleştirmek.

Çalışılacaklar:

- Senkron kod neden sırayla çalışır?
- `setTimeout()` neden sonraya kalır?
- Asenkron işlem beklerken JavaScript neden diğer satırlara devam eder?
- Callback Hell neden kodu okunmaz hale getirir?

Mini görev:

- `console.log("1")`, `setTimeout`, `console.log("3")` çıktısını tahmin et.
- İç içe üç `setTimeout` örneğini Promise/async mantığıyla sadeleştir.

### 3. Frontend Pratik Sayfası Tekrarı

Amaç: `pratik.html` üzerinden konuları tek ekranda uygulamak.

Kontrol listesi:

- Event listener çalışıyor mu?
- Form gönderimi boş inputta engelleniyor mu?
- `classList.toggle()` görsel değişiklik yapıyor mu?
- Sayaç birden fazla kez başlatıldığında hızlanıyor mu?
- JSON hatası `try...catch` ile yakalanıyor mu?
- Storage içine nesne doğru biçimde kaydediliyor mu?

### 4. Hata Rehberi Tekrarı

Amaç: Sınav/pratik sırasında en çok bozulan yerleri ezber değil, sebep-sonuç mantığıyla bilmek.

Çalışılacak dosya:

- `hata_rehberi.md`

Özellikle bakılacaklar:

- `querySelectorAll()` listesini tek element sanmak
- `px` eklemeyi unutmak
- `JSON.parse()` yapmadan string veriyi nesne gibi kullanmak
- Storage içinde `[object Object]` oluşması
- `await` kelimesini `async` fonksiyon dışında kullanmak
- Formda `preventDefault()` unutmak

### 5. Final Sınav Odaklı Tekrar

Amaç: Finalde beklenen kod okuma, JavaScript fonksiyon çıktısı, form elemanlarına müdahale, Bootstrap responsive ekran yorumu ve Express routing sorularını hızlıca çözebilecek seviyeye getirmek.

Çalışılacaklar:

- HTML/CSS kodu verildiğinde 800 px ekranda kaç divin yan yana geleceğini yorumlama.
- JavaScript ile input değerini okuyup butona tıklanınca sayfada bir yere yazdırma.
- `submit` eventinde `preventDefault()` ile formun sayfa yenilemesini engelleme.
- Fonksiyonlarda parametre, varsayılan parametre, `return`, `console.log()` çıktısı.
- Arrow function kullanıldığında parametre ve `this` davranışını ayırt etme.
- Bootstrap `sm`, `md`, `lg`, `xl`, `xxl` breakpointlerini 800 px ekran için yorumlama.
- Node.js / Express ile `/`, `/giris`, `/iletisim` gibi route'lar oluşturan `app.js` yazma.
- `Giriş` linkine tıklanınca `giris.html` sayfasının açılmasını routing mantığıyla açıklama.
- Veritabanı kodu beklenmediği için routing kısmına odaklanma.

Mini görev:

- `index.html` içinde `/giris` linki olan bir ana sayfa düşün; `app.js` içinde bu linke karşılık gelen route'u yaz.
- Bir input ve button verildiğinde tıklama sonrası input değerini `div` içine yazdıran JavaScript kodunu kur.
- `col-md-4`, `col-lg-6`, `col-xl-3` gibi Bootstrap sınıflarının 800 px ekranda nasıl davranacağını söyle.

### 6. Node.js ve Express.js Ek Materyali

Amaç: JavaScript'in tarayıcı dışındaki kullanımını, dosya işlemlerini ve basit Express routing mantığını öğrenmek.

Kaynak materyal:

- `pdf/DERS_NOTLARI-NODE_JS.docx`

Çalışılacaklar:

- Node.js nedir, nerelerde kullanılır?
- `node -v`, `npm -v` ve `node app.js` ile kurulum/çalıştırma kontrolü
- JavaScript temel tekrarı: değişken, koşul, döngü, fonksiyon, dizi, obje
- CommonJS modül sistemi: `module.exports` ve `require()`
- `fs` modülü ile dosya yazma, dosyaya ekleme, dosya okuma, varlık kontrolü ve silme
- `writeFileSync()` ile `appendFileSync()` farkı
- JSON dosyasına `JSON.stringify()` ile yazma, `JSON.parse()` ile okuma
- Express.js kurulumu, `app.get()` routing mantığı, `res.send()` ve `res.sendFile()`
- `express.static()` ile CSS gibi statik dosyaları servis etme
- Basit 4 sayfalı Express projesi ve 404 sayfası

Mini görev:

- `math.js` içinde `topla`, `cikar`, `carp` fonksiyonlarını yazıp `app.js` içinde `require()` ile kullan.
- `fs` ile `ogrenciler.txt` dosyasına birkaç öğrenciyi alt alta ekle.
- Express ile `/`, `/iletisim`, `/urunlerimiz`, `/referanslarimiz` route'larını açan küçük bir site kur.

Not: Final kapsamı olarak routing başlıkları yol haritasına eklendi. Veritabanı doğrudan çalışma kapsamına alınmadı.

---

## Kapanış Kriteri

Bu ders şu seviyeye geldiğinde toparlanmış sayılır:

- Açık kalan 1, 2, 14.1 ve 14.2 maddeleri anlatıldı.
- Kullanıcı mini görevleri doğru çözdü veya anladığını net onayladı.
- `yol_haritasi.md` içinde ilgili checkbox'lar işaretlendi.
- `oturum_durumu.md` son durumla güncellendi.
- `pratik.html` üzerinden 1-16 arası konular en az bir kez çalıştırıldı.
- Final routing, form, fonksiyon çıktısı ve Bootstrap grid maddeleri sırayla çalışıldı.
