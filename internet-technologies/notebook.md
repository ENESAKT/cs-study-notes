# 1. Bölüm: Event Listener Temelleri

## addEventListener() Metodu Nedir?
JavaScript’te HTML elemanı üzerinde bir olay (tıklama vs.) gerçekleştiğinde işlem yaptırmak için kullanılır.
**Formül:** `element.addEventListener("olayTipi", calisacakFonksiyon);`
**Örnek:**
```javascript
document.getElementById("btn").addEventListener("click", function() {
    alert("Tıklandı!");
});
```

## onclick ile addEventListener() Arasındaki Fark
*   **onclick dezavantajı:** Bir elemana sadece tek olay atanabilir, ikincisi yazılırsa ilkini siler.
*   **addEventListener avantajı:** Aynı eleman üzerinde çok sayıda farklı veya aynı tipte olayı ezilmeden çalıştırmayı sağlar.

---

# 2. Bölüm: Fare (Mouse) Eventleri

## Temel Tıklamalar
*   **click:** Tek tıklama.
*   **dblclick:** Çift tıklama.

## Hover (Bölge) Etkileşimleri
*   **mouseover:** Fare elemanın üstüne geldiğinde.
*   **mouseout:** Fare elemanın üstünden ayrıldığında.

## Basılı Tutma ve Hareket
*   **mousedown:** Farenin tuşuna tıklandığı ilk an.
*   **mouseup:** Basılı tutulan tuş bırakıldığında.
*   **mousemove:** Fare elemanın üzerinde hareket ettikçe anlık koordinat okumalarını sağlar.

---

# 3. Bölüm: Klavye (Keyboard) Eventleri

Klavye olayları kullanıcı tuşlara bastığında çalışır. Genellikle input (metin girişi) alanlarında tercih edilir.

## Temel Olaylar
*   **keydown:** Herhangi bir tuşa *basıldığı ilk anda* (tuş henüz aşağıdayken) tetiklenir.
*   **keyup:** Basılı tutulan tuş *bırakıldığı an* tetiklenir. Verinin anlık olarak kontrolü (şifre zorluğu vs.) için kullanılır.

**Örnek (Yazılan değeri okuma):**
```javascript
document.getElementById("metinGiris").addEventListener("keyup", function() {
    console.log(this.value); // Input içindeki anlık değeri yazdır
});
```

---

# 4. Bölüm: Event Parametresi, Domain (`this`) ve Toggle

## 1. Event Nesnesi (e)
Olay dinleyicisi içindeki fonksiyona otomatik gönderilen, gerçekleşen olay hakkındaki tüm detayları barındıran nesnedir (genelde `e` veya `event` olarak yazılır).
*   **e.key:** Klavyede tam olarak hangi tuşa basıldığını verir.
*   **e.clientX / e.clientY:** Tıklamanın fare koordinatını verir.

**Örnek:**
```javascript
document.addEventListener("keydown", function(e) {
    if(e.key === "Enter") {
        console.log("Enter tuşuna basıldı!");
    }
});
```

## 2. `this` Anahtar Kelimesi (Domain)
Event dinleyicilerinde `this`, o an tetiklenen elemanın kendisini temsil eder.

**Örnek:**
```javascript
document.getElementById("btn").addEventListener("click", function() {
    this.style.backgroundColor = "red"; // Tıklanan butonu kırmızı yap
});
```

## 3. Toggle (Aç/Kapat Mantığı)
Bir durumu tersine çevirme mantığıdır (Aktif/Pasif). CSS `classList.toggle()` veya Boolean bir değişken kullanılarak yapılır.

---

# 5. Bölüm: Çoklu Sınıf Seçimi ve Element Toplu Yönetimi

## 1. `getElementsByClassName()` ve `querySelectorAll()`
Aynı sınıfa (class) sahip birden fazla HTML elemanını seçmek için kullanılırlar. Tek bir eleman yerine **bir liste (HTMLCollection veya NodeList)** döndürürler. Bu listelere tek bir elemanmış gibi doğrudan `.addEventListener()` veya `.style` eklenemez.

*   `document.getElementsByClassName("kutu")` -> "kutu" sınıfına sahip tüm elemanları yakalar.
*   `document.querySelectorAll(".kutu")` -> Modern ve daha esnek bir seçicidir, CSS seçicileriyle (nokta, diyez vb.) çalışır.

## 2. Toplu Yönetim (Döngü Kullanımı)
Liste halinde dönen bu elemanların hepsine bir olay eklemek veya özelliklerini değiştirmek için **döngü (for)** kurmak zorundayız.

**Örnek:** Sayfadaki tüm ".kutu" sınıfına sahip div'lerin rengini aynı anda kırmızı yapma:
```javascript
const kutular = document.querySelectorAll(".kutu");

for(let i = 0; i < kutular.length; i++) {
    kutular[i].style.backgroundColor = "red";
}
```

**Örnek 2:** Tüm kutulara aynı anda Tıklama (click) özelliği kazandırma:
```javascript
const kutular = document.querySelectorAll(".kutu");

for(let i = 0; i < kutular.length; i++) {
    kutular[i].addEventListener("click", function() {
        // Hangi kutuya tıklandıysa, sadece "o" (this) kutuyu gizle
        this.style.display = "none";
    });
}
```

---

# 6. Bölüm: Form Elemanları ve Dinamik Değer Okuma

HTML Form elemanları, kullanıcıdan veri aldığımız en kritik yapılardır. JavaScript ile bu elemanların değerlerini (`value`), seçili olma durumlarını (`checked`) okur ve anlık tepkiler veririz.

## 1. Metin ve Uzunluk Kontrolü (`input type="text"`, `textarea`)
Kullanıcının yazdığı metni okumak için `.value` kullanırız. Girilen metnin kaç karakterden oluştuğunu bulmak için ise `.value.length` özelliğinden yararlanırız (Twitter karakter sayacı gibi).

**Örnek:**
```javascript
const mesaj = document.getElementById("mesajAlani");
console.log("Kullanıcı şunu yazdı: " + mesaj.value);
console.log("Karakter uzunluğu: " + mesaj.value.length);
```

## 2. Onay Kutuları (`checkbox`) ve `checked` Özelliği
Checkbox'lar (veya Toggle Switch'ler) açık/kapalı mantığıyla çalışır. Bunların değerini okumak için `.value` yerine `.checked` (True/False döner) kullanılır.

**Örnek:**
```javascript
const onayKutusu = document.getElementById("sozlesmeOnay");
if (onayKutusu.checked === true) {
    console.log("Sözleşme onaylandı!");
}
```

## 3. Aralık Seçici (`range` / Slider) ve Yaş/Fiyat Okuma
Kullanıcıya belli bir aralıkta seçim yaptıran çubuklardır. Ses açma/kısma, fiyat aralığı seçme işlemlerinde kullanılır.
Bunun o anki durumu her kaydırıldığında `input` veya `change` olayı ile `.value` üzerinden sayısal olarak okunur.

**Örnek:**
```javascript
const sesAyar = document.getElementById("sesCubugu");
sesAyar.addEventListener("input", function() {
    console.log("Anlık ses seviyesi: " + this.value);
});
```

---

# 7. Bölüm: Validasyon (Doğrulama ve Form Kontrolü)

Kullanıcının forma girdiği verilerin kurallara uyup uymadığını (boş mu, yeterince uzun mu, checkbox işaretli mi) arka planda sunucuya (backend) göndermeden önce JavaScript ile tarayıcıda kontrol etme işlemidir.

## 1. Boş Geçilemez Kontrolü (Text)
Eğer kullanıcının girdiği değerin içi boşsa (`""`) veya `length` sıfırsa hata verdirtip form gönderimini engelleriz.
```javascript
const adInput = document.getElementById("ad");
const gonderBtn = document.getElementById("gonder");

gonderBtn.addEventListener("click", function(e) {
    // trim() metodu baştaki ve sondaki boşlukları siler, sadece boşluk girilmesini engeller
    if (adInput.value.trim() === "") {
        e.preventDefault(); // Formun gitmesini engelle
        alert("Lütfen adınızı giriniz!");
    }
});
```

## 2. Checkbox Zorunluluk Kontrolü
"Kullanıcı sözleşmesini okudum" gibi kutuların işaretlenmeden geçilmemesi gerekir.
```javascript
const sozlesme = document.getElementById("sozlesme");

if (!sozlesme.checked) { // checked false ise (işaretlenmemişse)
    alert("Devam etmek için sözleşmeyi onaylamalısınız.");
}
```

## 3. Dinamik Buton Pasifleştirme (`disabled`)
Sözleşme kutusu işaretlenene kadar Gönder butonunu tamamen tıklanamaz (`disabled = true`) hale getirebiliriz. Bu, hata mesajı çıkartmaktan çok daha profesyonel bir yöntemdir.
```javascript
const btn = document.getElementById("btnKayit");
// Butonu pasif (tıklanamaz) yap:
btn.disabled = true; 

// Butonu tekrar aktif yap:
btn.disabled = false;
```

---

# 8. Bölüm: CSS Sınıf (Class) İşlemleri (`classList`)

JavaScript üzerinden elementlerin görünümünü değiştirmek için doğrudan `style.renk = "..."` yazmak yerine, elementin **class**'larını (sınıflarını) manipüle etmek her zaman daha profesyonel ve yönetilebilir bir yaklaşımdır. CSS tarafında önceden hazırlanmış sınıfları elemente dinamik olarak ekleyip çıkarırız.

## 1. `classList.add()` ve `classList.remove()`
Bir elemana yeni bir sınıf ekler veya var olan sınıfı siler.

**Örnek:**
```javascript
const kutu = document.getElementById("kutu1");

// "aktif" sınıfını ekle (CSS'te .aktif { background: green; } gibi tanımlanmış olabilir)
kutu.classList.add("aktif");

// "gizli" sınıfını sil
kutu.classList.remove("gizli");
```

## 2. Aktif Sekme (Menü) Mantığı
Birden fazla menü sekmemiz varsa, tıklanan sekmeye "aktif" sınıfını verip, diğer tüm sekmelerden bu sınıfı silmemiz gerekir. Bunu yapmak için önce tüm sekmeleri seçip bir `for` döngüsü ile "aktif" sınıfını temizleriz, ardından sadece tıklanan (`this`) elemana ekleriz.

**Örnek Algoritma:**
```javascript
const sekmeler = document.querySelectorAll(".sekme");

for(let i = 0; i < sekmeler.length; i++) {
    sekmeler[i].addEventListener("click", function() {
        // 1. Önce Hepsinden "aktif" sınıfını temizle
        for(let j = 0; j < sekmeler.length; j++) {
            sekmeler[j].classList.remove("aktif");
        }
        // 2. Sadece tıklanan sekmeye (this) "aktif" sınıfını ekle
        this.classList.add("aktif");
    });
}
```

---

# 9. Bölüm: Elementlerin Gerçek Boyutlarını Yönetme

Sayfadaki elemanların genişliğini veya yüksekliğini bulmak ya da değiştirmek istediğimizde, CSS özellikleri ile gerçek boyutlar arasındaki farkı bilmemiz gerekir.

## 1. Sadece Okunabilen Boyutlar (`offsetWidth` ve `clientWidth`)
Bu özellikler elementin ekranda tam olarak kaç piksel yer kapladığını okur. Birimi yoktur, doğrudan "Sayı" (Örn: `150`) dönerler. **Değiştirilemezler**, sadece boyut ölçmek için kullanılırlar.

*   **`offsetWidth` / `offsetHeight`:** Kutunun genişliğini + padding (iç boşluk) + border (kenarlık) ile beraber tam dış boyutunu verir.
*   **`clientWidth` / `clientHeight`:** Kutunun sadece genişliğini + padding'ini verir. Border'ı dahil etmez.

```javascript
const kutu = document.getElementById("kutu");
console.log("Kutunun tam genişliği: " + kutu.offsetWidth + " piksel");
```

## 2. Değiştirilebilen Boyutlar (`style.width`)
Eğer bir elementin boyutunu JavaScript ile değiştirmek istiyorsak, `offsetWidth` gibi salt okunur değerler yerine `style.width` veya `style.height` kullanırız. 
Dikkat edilmesi gereken en önemli nokta, `style.width` kullanılırken mutlaka birimin (`px`, `%`) "string" olarak eklenmesi zorunluluğudur.

```javascript
// Kutunun genişliğini dinamik olarak 250px yap
kutu.style.width = "250px"; 

// Hata! Piksel (px) belirtilmediği için çalışmaz
// kutu.style.width = 250; 
```

---

# 10. Bölüm: Computed Style (Hesaplanmış/CSS'den Gelen Özellikler)

JavaScript'te `element.style.width` dediğimizde, sadece HTML içinde **inline (satır içi)** olarak yazılmış (`<div style="width:100px;">`) özellikleri okuyabiliriz. Eğer genişlik harici bir CSS dosyasında (`style.css`) verildiyse `element.style.width` **boş** döner.

Bunu çözmek için tarayıcının hesapladığı son gerçek stili okumamız gerekir: `getComputedStyle()`.

## 1. CSS Dosyasından Gelen Değeri Okuma
```javascript
const kutu = document.getElementById("kutu");

// Computed style nesnesini alırız
const stiller = window.getComputedStyle(kutu);

// CSS'ten gelen genişliği okuruz (Örn: "100px" şeklinde String döner)
console.log("Kutu genişliği: " + stiller.width); 
```

## 2. Okunan Değeri Sayıya Çevirme (`parseInt`) ve Artırma
`getComputedStyle` bize `"100px"` şeklinde bir **Metin (String)** verir. Biz bu kutuyu 20px daha büyütmek istersek `"100px" + 20` diyemeyiz (sonuç "100px20" olur). 
Önce içindeki sayıyı çekip almamız gerekir. Bunun için `parseInt()` kullanırız.

**Örnek: Kutunun genişliğini alıp 50px artırma:**
```javascript
const halaKutu = document.getElementById("kutu");
const hesaplanmisStil = window.getComputedStyle(halaKutu);

// 1. "100px" metninden sadece 100 sayısını al
let mevcutGenislik = parseInt(hesaplanmisStil.width); 

// 2. Yeni boyutu hesapla ve "px" ekleyerek geri ata
halaKutu.style.width = (mevcutGenislik + 50) + "px";
```

---

# 11. Bölüm: Zamanlayıcılar (Timers) ile Programlama

Bazen işlemlerin hemen değil, belirli bir süre sonra gerçekleşmesini veya belirli aralıklarla sürekli tekrarlanmasını isteriz (Örn: Geri sayımlar, bildirimlerin 3 saniye sonra kaybolması, animasyonlar). JS'de 2 ana zamanlayıcı vardır. Zaman ölçüsü **milisaniye (ms)** cinsindendir (1000ms = 1 Saniye).

## 1. Gecikmeli İşlem: `setTimeout()`
Belirttiğiniz fonksiyonu, belirttiğiniz süre kadar **bekledikten sonra sadece 1 kez** çalıştırır.

```javascript
// 3 saniye (3000ms) bekledikten sonra uyarı ver
setTimeout(function() {
    alert("3 saniye doldu!");
}, 3000);
```

## 2. Sürekli Tekrar: `setInterval()`
Belirttiğiniz fonksiyonu, belirttiğiniz süre **arayalıklarıyla sürekli olarak (dur diyene kadar)** çalıştırır. Geri sayımlar ve sayaçlar için biçilmiş kaftandır.

```javascript
let sayac = 0;

// Her 1 saniyede (1000ms) bir sayacı 1 artır ve ekrana yaz
setInterval(function() {
    sayac++;
    console.log("Geçen süre: " + sayac + " saniye");
}, 1000);
```

## 3. Zamanlayıcıları İptal Etme (`clearTimeout` / `clearInterval`)
Bir zamanlayıcıyı sonradan durdurmak için onu önce bir **değişkene** atamalısınız.

```javascript
let iptalEdilecek = setTimeout(function() {
    console.log("Bu mesaj hiç görünmeyecek");
}, 5000);

// Süre dolmadan zamanlayıcıyı iptal et
clearTimeout(iptalEdilecek);
```

---

# 12. Bölüm: Window Nesnesi, Sekme Kontrolü ve Yüzey Koordinatları

JavaScript'in en tepesindeki baba nesne `window` (tarayıcı penceresinin kendisi) nesnesidir. Bizim kullandığımız `document`, `alert()`, `setTimeout()` gibi tüm yapılar aslında `window`'un bir parçasıdır.

## 1. Tarayıcı (Pencere) Olayları
Bir elemana değil, doğrudan sayfanın kendisine olay atadığımızda `window` kullanırız.
*   **`load`:** Sayfadaki her şey (resimler, videolar dahil) tamamen yüklendiğinde tetiklenir.
*   **`resize`:** Kullanıcı tarayıcı penceresinin boyutunu büyütüp küçülttüğünde tetiklenir. Ekran genişliğine (`window.innerWidth`) göre tasarım değiştirmek için kullanılır.
*   **`scroll`:** Sayfa aşağı/yukarı kaydırıldığında tetiklenir. Ne kadar kaydırıldığını `window.scrollY` ile okuruz (Örn: Menüyü sabitlemek için).

```javascript
window.addEventListener("scroll", function() {
    console.log("Yukarıdan kaydırma miktarı: " + window.scrollY + "px");
});
```

## 2. Koordinat ve Yüzey Okumaları
Fare (mouse) ile tıklanan noktanın ekranın neresinde olduğunu bulmak için olay nesnesini (`e`) kullanırız.
*   **`e.clientX` / `e.clientY`:** Tıklanan noktanın sadece aktif web sayfası içindeki (sol ve üst) koordinatlarıdır.
*   **`e.screenX` / `e.screenY`:** Tıklanan noktanın kullanıcının fiziksel monitörünün tamamına göre (Windows/Mac ekranı) koordinatlarıdır.

```javascript
document.addEventListener("click", function(e) {
    console.log("X ekseni (Yatay): " + e.clientX);
    console.log("Y ekseni (Dikey): " + e.clientY);
});
```

---

## 27 Nisan 2026 - 13. Bölüm: JavaScript Fonksiyonlar

**Kısa Tanım:** Fonksiyon, belirli bir işi yapan ve tekrar kullanılabilen kod bloğudur.

**Anahtar Kavramlar:**
- Function Declaration: `function selamVer() { ... }` şeklinde klasik tanımdır.
- Parametre: Fonksiyona dışarıdan gönderilen değerdir.
- `return`: Fonksiyonu bitirir ve dışarı değer döndürür.
- Function Expression: Fonksiyonun değişkene atanmış halidir.
- Arrow Function: ES6 ile gelen kısa fonksiyon yazımıdır.
- Callback: Başka bir fonksiyona parametre olarak verilen fonksiyondur.
- Rest `...args`: Sınırsız sayıda parametre alır.
- Spread `...`: Dizi elemanlarını ayrı ayrı gönderir.
- Hoisting: Function declaration yukarı taşınır; function expression aynı şekilde çalışmaz.

**Örnek / Komut:**
```javascript
function topla(a, b) {
    return a + b;
}

const kare = x => x * x;
```

**Hatırla:** `return` sonucu dışarı verir; `console.log()` sadece ekrana yazar. Function declaration çağrıdan önce yazılmasa da çalışabilir, function expression çalışmaz.

---

## 27 Nisan 2026 - 14. Bölüm: JavaScript Async / Await

**Kısa Tanım:** Asenkron işlemler zaman alan işlemlerdir; `async/await`, Promise tabanlı asenkron kodu daha okunabilir yazmayı sağlar.

**Anahtar Kavramlar:**
- Senkron: Kodlar sırayla çalışır.
- Asenkron: Zaman alan işlem beklerken diğer kodlar çalışmaya devam eder.
- Callback Hell: İç içe callback yazımıyla kodun karmaşık hale gelmesidir.
- Promise: Asenkron işlemin başarılı (`resolve`) veya hatalı (`reject`) sonucunu temsil eder.
- `async`: Fonksiyonun Promise döndürmesini sağlar.
- `await`: Promise sonucunu bekler; sadece `async` fonksiyon içinde kullanılır.
- `try...catch`: Asenkron hataları yakalamak için kullanılır.

**Örnek / Komut:**
```javascript
async function veriGetir() {
    try {
        let res = await fetch("ogrenciler.json");
        let data = await res.json();
        console.log(data);
    } catch (hata) {
        console.log("Hata oluştu:", hata);
    }
}
```

**Hatırla:** `await` tek başına kullanılmaz; mutlaka `async` fonksiyon içinde olmalıdır.

---

## 29 Nisan 2026 - 15. Bölüm: JSON

**Kısa Tanım:** JSON, verileri saklamak ve taşımak için kullanılan metin tabanlı bir veri formatıdır.

**Anahtar Kavramlar:**
- JSON: JavaScript Object Notation kısaltmasıdır.
- Anahtar-değer: Veriler `"anahtar": değer` biçiminde tutulur.
- JavaScript nesnesi farkı: JSON'da anahtarlar çift tırnak içinde yazılır.
- Desteklenen türler: string, number, object, array, boolean ve null.

**Örnek / Komut:**
```json
{
  "ad": "Ali",
  "yas": 20,
  "sehir": "Ankara"
}
```

**Hatırla:** JSON bir veri formatıdır; fonksiyon, `undefined`, yorum satırı ve tek tırnaklı yapı içermez.

---

## 30 Nisan 2026 - 15.5 JSON.stringify ile Biçimli Yazdırma ve Replacer

**Kısa Tanım:** `JSON.stringify()` metodunun üçüncü parametresi olan `space`, JSON çıktısını okunabilir şekilde girintili yazdırır; `replacer` ise hangi alanların çıktıya dahil edileceğini seçmeye yarar.

**Anahtar Kavramlar:**
- `JSON.stringify(deger, replacer, space)`: JavaScript verisini JSON metnine çevirir.
- `space`: Çıktıda kaç boşluk girinti kullanılacağını belirler.
- `replacer`: Dizi olarak verilirse sadece seçilen anahtarlar JSON çıktısına alınır.
- Hassas alanları dışarıda bırakma: Örneğin `sifre` gibi alanlar JSON çıktısına eklenmeyebilir.

**Örnek / Komut:**
```javascript
let kullanici = {
    ad: "Ahmet",
    sifre: "12345",
    rol: "admin"
};

let sonuc = JSON.stringify(kullanici, ["ad", "rol"], 2);
console.log(sonuc);
```

**Hatırla:** `space` sadece görüntüyü güzelleştirir; verinin anlamını değiştirmez. `replacer` ise hangi alanların yazılacağını gerçekten filtreler.

---

## 30 Nisan 2026 - 15.6 JSON İçinde Diziler ve İç İçe Nesneler

**Kısa Tanım:** JSON içinde diziler ve dizilerin içinde nesneler tutulabilir; bu yapı özellikle API'lerden gelen çoklu kayıtları temsil etmek için kullanılır.

**Anahtar Kavramlar:**
- JSON dizisi: Birden fazla değeri veya nesneyi `[]` içinde tutar.
- İç içe nesne: Bir nesnenin içinde başka nesne veya dizi bulunabilir.
- `JSON.parse()`: JSON metnini JavaScript nesnesine/dizisine çevirir.
- `for...of`: Dizi içindeki nesneleri sırayla gezmek için kullanılır.

**Örnek / Komut:**
```javascript
let veri = `{
  "urunler": [
    {"ad": "Defter", "fiyat": 50},
    {"ad": "Kalem", "fiyat": 15}
  ]
}`;

let nesne = JSON.parse(veri);

for (let urun of nesne.urunler) {
    console.log(urun.ad, urun.fiyat);
}
```

**Hatırla:** JSON metni doğrudan JavaScript nesnesi gibi gezilmez; önce `JSON.parse()` ile dönüştürülmelidir.

---

## 30 Nisan 2026 - 15.7 JSON Dosyaları ve Fetch ile Veri Çekme

**Kısa Tanım:** JSON verileri `.json` uzantılı dosyalarda saklanabilir; JavaScript tarafında `fetch()` ile bu dosya okunur ve `response.json()` ile kullanılabilir JavaScript verisine dönüştürülür.

**Anahtar Kavramlar:**
- `.json` dosyası: JSON formatında veri saklayan dosyadır.
- `fetch("dosya.json")`: Dosyaya veya API adresine istek gönderir.
- `response.json()`: Gelen JSON cevabını JavaScript nesnesine/dizisine çevirir.
- `then()`: Promise başarılı olduğunda sıradaki işlemi çalıştırır.

**Örnek / Komut:**
```javascript
fetch("ogrenciler.json")
    .then(response => response.json())
    .then(veri => {
        console.log(veri);
    });
```

**Hatırla:** `fetch()` sonucu doğrudan veri değildir; önce cevap gelir, sonra `response.json()` ile veri okunur.

---

## 1 Mayıs 2026 - 15.8 JSON.parse Hata Durumları ve try...catch

**Kısa Tanım:** `JSON.parse()` hatalı JSON metniyle karşılaşırsa hata üretir; programın tamamen durmaması için riskli dönüşüm işlemi `try...catch` içine alınır.

**Anahtar Kavramlar:**
- `JSON.parse()`: JSON metnini JavaScript nesnesine veya dizisine çevirir.
- Hatalı JSON: Tırnaksız anahtar, tek tırnak, fazla virgül gibi yazım sorunları hata oluşturur.
- `try`: Hata çıkarma ihtimali olan kodun yazıldığı bölümdür.
- `catch`: Hata oluşursa çalışan güvenli yakalama bölümüdür.

**Örnek / Komut:**
```javascript
let veri = "{ad:'Ali'}";

try {
    let kisi = JSON.parse(veri);
    console.log(kisi.ad);
} catch (hata) {
    console.log("JSON verisi hatalı");
}
```

**Hatırla:** Dışarıdan gelen JSON verisi her zaman doğru olmayabilir; bu yüzden `JSON.parse()` güvenli kullanılmalıdır.

---

## 1 Mayıs 2026 - 15.9 JSON Dosyasından Veri Çekme Uygulaması

**Kısa Tanım:** Bir `.json` dosyasından gelen öğrenci listesi `fetch()` ile çekilebilir, `response.json()` ile JavaScript dizisine çevrilebilir ve döngü/filtreleme/ortalama işlemlerinde kullanılabilir.

**Anahtar Kavramlar:**
- `fetch("ogrenciler.json")`: JSON dosyasını okuma isteği gönderir.
- `response.json()`: Gelen cevabı JavaScript verisine dönüştürür.
- `for...of`: Öğrenci dizisini sırayla gezmek için kullanılır.
- `filter()`: Şarta uyan öğrencileri seçmek için kullanılabilir.
- Ortalama: Notların toplamı öğrenci sayısına bölünerek hesaplanır.

**Örnek / Komut:**
```javascript
async function ogrencileriYukle() {
    let response = await fetch("ogrenciler.json");
    let ogrenciler = await response.json();
    let toplam = 0;

    for (let ogrenci of ogrenciler) {
        console.log(ogrenci.ad, ogrenci.not);
        toplam += ogrenci.not;
    }

    console.log("Ortalama:", toplam / ogrenciler.length);
}
```

**Hatırla:** JSON dosyasından gelen veri dizi ise `for...of`, `filter()` ve benzeri dizi metotlarıyla işlenebilir.

---

## 1 Mayıs 2026 - 16. Bölüm: Local Storage ve Session Storage

**Kısa Tanım:** Web Storage API, tarayıcıda veri saklamayı sağlar; `localStorage` uzun süreli, `sessionStorage` ise sekme/oturum süresince veri tutar.

**Anahtar Kavramlar:**
- `localStorage`: Veri tarayıcı kapansa bile kalır.
- `sessionStorage`: Veri sadece açık sekme boyunca kalır.
- `setItem()`: Veri kaydeder.
- `getItem()`: Veri okur.
- `removeItem()`: Belirli veriyi siler.
- `clear()`: Tüm storage verilerini siler.
- `key()`: Sıradaki anahtar adını verir.
- String saklama: Storage içindeki her veri metin olarak tutulur.
- `JSON.stringify()`: Nesne/diziyi saklamadan önce JSON metnine çevirir.
- `JSON.parse()`: Saklanan JSON metnini geri JavaScript verisine çevirir.

**Örnek / Komut:**
```javascript
let kullanici = {
    ad: "Ayşe",
    yas: 22
};

localStorage.setItem("kullanici", JSON.stringify(kullanici));

let veri = localStorage.getItem("kullanici");
let nesne = JSON.parse(veri);

console.log(nesne.ad);
```

**Hatırla:** Storage güvenli veri deposu değildir; şifre, token ve hassas bilgiler burada tutulmamalıdır. Nesne doğrudan kaydedilirse `[object Object]` hatalı sonucu oluşur.

---

## 14 Mayıs 2026 - Bootstrap 5

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap, hazır CSS sınıflarıyla hızlı ve responsive web sayfası tasarlamayı sağlayan front-end framework'tür.

**Anahtar Kavramlar:**
- **CDN kurulumu:** Bootstrap CSS ve JS dosyaları link/script ile sayfaya eklenir.
- **Viewport:** Mobil uyum için `<meta name="viewport" content="width=device-width, initial-scale=1">` kullanılır.
- **Container:** `.container` içeriği ortalar ve belirli genişliklerde tutar; `.container-fluid` tüm ekran genişliğini kullanır.
- **Grid sistemi:** Bootstrap 12 kolon mantığıyla çalışır. `6 + 6 = 12` iki eşit sütun, `4 + 4 + 4 = 12` üç eşit sütun demektir.
- **Temel sıra:** Önce `.container`, içinde `.row`, onun içinde `.col-*` kullanılır.
- **Breakpoint:** Sınıflar ekran genişliğine göre aktif olur: `col-` her ekranda, `col-sm-` 576px+, `col-md-` 768px+, `col-lg-` 992px+, `col-xl-` 1200px+, `col-xxl-` 1400px+.

**Örnek / Komut:**
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

<div class="container">
  <div class="row">
    <div class="col-12 col-md-6 col-lg-4">A</div>
    <div class="col-12 col-md-6 col-lg-4">B</div>
    <div class="col-12 col-md-6 col-lg-4">C</div>
  </div>
</div>
```

**Hatırla:** Mobilde `.col-12` kutuyu tam satır yapar. 800px ekranda `md` aktif olduğu için `.col-md-6` çalışır; A ve B aynı satırda yarım yarım durur, C bir alt satıra geçer. `lg` ise 992px ve üstünde çalışır.

---

## 23 Mayıs 2026 - Bootstrap 5 Renk Sınıfları

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap'te metin ve arka plan renkleri hazır sınıflarla verilir; böylece ayrı CSS yazmadan renk düzeni kurulabilir.

**Anahtar Kavramlar:**
- **Metin rengi:** `.text-primary`, `.text-success`, `.text-danger`, `.text-warning`, `.text-dark`, `.text-white` gibi sınıflarla yazı rengi değiştirilir.
- **Arka plan rengi:** `.bg-primary`, `.bg-success`, `.bg-danger`, `.bg-warning`, `.bg-dark`, `.bg-light` gibi sınıflarla kutunun arka planı değiştirilir.
- **Birlikte kullanım:** `.bg-*` sadece arka planı değiştirir; yazı rengi okunmuyorsa ayrıca `.text-white` veya `.text-dark` eklenir.

**Örnek / Komut:**
```html
<p class="text-danger">Kırmızı uyarı yazısı</p>

<div class="bg-danger text-white">
  Hata mesajı
</div>
```

**Hatırla:** `bg-danger` arka planı kırmızı yapar; `text-white` yazıyı beyaz yapar.

---

## 23 Mayıs 2026 - Bootstrap 5 Tablolar

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap tablo sınıfları, HTML tablolarına hazır görünüm, çizgi, hover efekti, koyu tema ve responsive kaydırma desteği verir.

**Anahtar Kavramlar:**
- **Temel tablo:** `.table` sınıfı tabloya Bootstrap'in temel tablo stilini verir.
- **Şeritli tablo:** `.table-striped` satırları zebra düzeninde renklendirir.
- **Çerçeveli tablo:** `.table-bordered` hücrelere kenarlık verir.
- **Hover efekti:** `.table-hover` satırın üzerine gelince görsel vurgu oluşturur.
- **Koyu tablo:** `.table-dark` tabloyu koyu temaya çevirir.
- **Responsive tablo:** `.table-responsive` tabloyu küçük ekranda yatay kaydırılabilir yapar.

**Örnek / Komut:**
```html
<div class="table-responsive">
  <table class="table table-striped table-bordered table-hover">
    <tr>
      <th>Ad</th>
      <th>Not</th>
    </tr>
    <tr>
      <td>Ali</td>
      <td>80</td>
    </tr>
  </table>
</div>
```

**Hatırla:** `.table` temel sınıftır; diğer tablo sınıfları genelde onun yanına eklenir.

---

## 23 Mayıs 2026 - Bootstrap 5 Görseller

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap görsel sınıfları, resimlere hazır köşe yuvarlama, daire görünümü, küçük çerçeve ve hizalama davranışı verir.

**Anahtar Kavramlar:**
- **Köşe yuvarlama:** `.rounded` resmin köşelerini yuvarlatır.
- **Daire görsel:** `.rounded-circle` özellikle kare görselleri daire şeklinde gösterir.
- **Küçük çerçeve:** `.img-thumbnail` görsele ince çerçeveli küçük ön izleme görünümü verir.
- **Sola hizalama:** `.float-start` görseli sola yaslar.
- **Sağa hizalama:** `.float-end` görseli sağa yaslar.
- **Ortaya hizalama:** `.mx-auto d-block` birlikte kullanıldığında görseli yatayda ortalar.

**Örnek / Komut:**
```html
<img src="profil.jpg" class="rounded-circle" alt="Profil">
<img src="manzara.jpg" class="img-thumbnail" alt="Manzara">
<img src="logo.jpg" class="mx-auto d-block" alt="Logo">
```

**Hatırla:** `.mx-auto` tek başına her zaman yetmez; görseli ortalamak için çoğunlukla yanında `.d-block` kullanılır.

---

## 23 Mayıs 2026 - Bootstrap 5 Butonlar

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap buton sınıfları, HTML butonlarına hazır görünüm ve renk varyantları verir.

**Anahtar Kavramlar:**
- **Temel buton:** `.btn` sınıfı butona Bootstrap'in temel buton görünümünü verir.
- **Renk varyantı:** `.btn-primary`, `.btn-secondary`, `.btn-success`, `.btn-warning`, `.btn-danger`, `.btn-dark`, `.btn-light` gibi sınıflar butonun rengini belirler.
- **Link görünümü:** `.btn-link` butonu link gibi gösterir.
- **Birlikte kullanım:** Genelde `.btn` ve renk sınıfı birlikte yazılır; örnek: `btn btn-primary`.

**Örnek / Komut:**
```html
<button class="btn btn-primary">Kaydet</button>
<button class="btn btn-danger">Sil</button>
<button class="btn btn-success">Onayla</button>
```

**Hatırla:** Sadece `btn-primary` yazmak yeterli değildir; temel buton görünümü için yanında `.btn` de olmalıdır.

---

## 23 Mayıs 2026 - Bootstrap 5 İlerleme Çubukları

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap progress bar, bir işlemin yüzde kaç tamamlandığını görsel çubukla göstermek için kullanılır.

**Anahtar Kavramlar:**
- **Dış kapsayıcı:** `.progress` ilerleme çubuğunun ana kutusudur.
- **İç çubuk:** `.progress-bar` dolan kısmı gösterir.
- **Genişlik:** `style="width:70%"` gibi değerle çubuğun ne kadar dolu görüneceği belirlenir.
- **Renk:** İç çubuğa `.bg-success`, `.bg-warning`, `.bg-danger` gibi arka plan sınıfları eklenebilir.
- **Çoklu segment:** Aynı `.progress` içinde birden fazla `.progress-bar` kullanılarak parçalı ilerleme çubuğu yapılabilir.

**Örnek / Komut:**
```html
<div class="progress">
  <div class="progress-bar bg-success" style="width:70%">70%</div>
</div>
```

**Hatırla:** `.progress` dış kutu, `.progress-bar` dolan iç kısımdır; yüzde görünümü `width` ile ayarlanır.

---

## 23 Mayıs 2026 - Bootstrap 5 Padding/Margin Yardımcıları

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap spacing yardımcıları, ayrı CSS yazmadan elemanlara padding ve margin boşluğu vermeyi sağlar.

**Anahtar Kavramlar:**
- **Padding:** Elemanın iç boşluğudur. `p-5` her taraftan padding verir.
- **Margin:** Elemanın dış boşluğudur. `m-5` her taraftan margin verir.
- **Yön kısaltmaları:** `t` üst, `b` alt, `s` sol/başlangıç, `e` sağ/bitiş, `x` sağ-sol, `y` üst-alt yönünü temsil eder.
- **Örnek kullanım:** `pt-5` üst padding, `my-5` üst-alt margin, `mx-auto` sağ-sol otomatik margin anlamına gelir.

**Örnek / Komut:**
```html
<div class="container p-5 my-5 bg-primary text-white">
  Bootstrap boşluk örneği
</div>
```

**Hatırla:** `p` iç boşluk, `m` dış boşluk demektir; `x` yatay, `y` dikey ekseni ifade eder.

---

## 23 Mayıs 2026 - Bootstrap 5 Bütünleştirme Pratiği

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9; `ders.md`, Bootstrap 5 bölümü

**Kısa Tanım:** Bootstrap pratiklerinde `container`, `row`, `col-*`, renk, buton ve spacing sınıfları birlikte kullanılarak responsive sayfa düzeni kurulur.

**Anahtar Kavramlar:**
- **Responsive kart ızgarası:** `col-12 col-md-4` ile mobilde alt alta, md ve üstünde üç kolonlu yapı kurulabilir.
- **Kart görünümü:** Basit kutular için `p-3`, `border`, `bg-light` gibi sınıflar birlikte kullanılabilir.
- **Buton seti:** `btn btn-primary`, `btn btn-success`, `btn btn-danger` gibi sınıflarla farklı anlamlı butonlar hazırlanır.
- **Boşluk düzeni:** `my-4`, `p-3`, `gap` veya margin yardımcılarıyla elemanlar sıkışmadan yerleştirilir.

**Örnek / Komut:**
```html
<div class="container my-5">
  <div class="row">
    <div class="col-12 col-md-4">
      <div class="p-3 border bg-light">
        <h5>Kart 1</h5>
        <button class="btn btn-primary">Detay</button>
      </div>
    </div>
  </div>
</div>
```

**Hatırla:** Bootstrap pratiğinde önce yerleşimi (`container-row-col`), sonra görünümü (`bg-*`, `border`, `p-*`, `btn`) kur.

---

## 23 Mayıs 2026 - 18. Bölüm: Final Odaklı Web ve Routing Tekrarı (Bölüm 1)

**Kaynak:** `ders.md`
**Kısa Tanım:** Final sınavına yönelik olarak, HTML/CSS kod çıktısını koda bakarak yorumlama ve JS ile form elemanlarından veri okuyup sayfaya yazdırma konularının tekrarıdır.

**Anahtar Kavramlar:**
- **HTML Çıktı Okuma:** Hangi elementlerin (div, p) blok seviyesinde (alt alta) ve hangilerinin (span, a, button) satır içi (yan yana) olduğunu koda bakarak analiz etmek.
- **Form Değeri Okuma:** `input` veya `textarea` elemanlarına girilen metni `.value` ile okumak.
- **Sayfaya Yazdırma:** Okunan metni ekrandaki bir `div` veya `p` içine `.innerText` veya `.innerHTML` ile aktarmak.
- **Form Yenilenmesini Önleme:** Submit butonlarına tıklandığında sayfanın yenilenmesini durdurmak için `e.preventDefault()` fonksiyonunu kullanmak.

**Örnek / Komut:**
```javascript
const formBtn = document.getElementById("gonderBtn");
const isimKutusu = document.getElementById("isimInput");
const sonucYeri = document.getElementById("sonuc");

formBtn.addEventListener("click", function(e) {
    e.preventDefault(); // Sayfanın yenilenmesini engeller
    let girilenDeger = isimKutusu.value; // Formdan veriyi okur
    sonucYeri.innerText = "Hoş geldin, " + girilenDeger; // Ekrana yazar
});
```

**Hatırla:** `.value` inputlar için, `.innerText` ise div/p/span gibi standart HTML elemanlarının içindeki yazılar için kullanılır. Form işlemlerinde sayfa sıfırlanıyorsa sorun genelde `e.preventDefault()` eksikliğidir.

---

## 23 Mayıs 2026 - 18.1 Node.js Nedir ve Modül Sistemi

**Kaynak:** `ders.md`
**Kısa Tanım:** Node.js, JavaScript kodlarını tarayıcı dışında (sunucuda) çalıştırmayı sağlayan asenkron ve olay güdümlü çalışma ortamıdır.

**Anahtar Kavramlar:**
- **Kullanım Alanları:** REST API'ler, web sunucuları, gerçek zamanlı chat ve e-ticaret uygulamaları.
- **Asenkron Yapı:** İşlemlerin birbirini beklemeden arka planda hızlıca çalışabilmesi.
- **Modül Sistemi:** Node.js'de her dosya bir modüldür. Diğer dosyalara açılacak kodlar `module.exports` ile dışa aktarılır.
- **require():** Dışa aktarılmış bir modülü veya paketi içeri aktarmak için kullanılır.

**Örnek / Komut:**
```javascript
// math.js (Dışa Aktarma)
function topla(a, b) { return a + b; }
module.exports = topla;

// app.js (İçeri Aktarma)
const matematik = require('./math');
console.log(matematik(5, 10)); // Çıktı: 15
```

**Hatırla:** JavaScript tarayıcıda DOM'u (`window`, `document`) yönetirken, Node.js sunucu işlemlerini ve dosya sistemini (`fs`, `http`) yönetir. Node.js içinde DOM elementleri bulunmaz.

---

## 23 Mayıs 2026 - 18.2 ve 18.3 FS Modülü: Dosyaya Yazma ve Ekleme

**Kaynak:** `ders.md`
**Kısa Tanım:** `fs` (File System) modülü, Node.js'in bilgisayarımızdaki (veya sunucudaki) dosyaları yönetmesini (yazma, okuma, silme) sağlar.

**Anahtar Kavramlar:**
- **fs'i Dahil Etme:** Node.js çekirdek modülü olduğu için `const fs = require('fs');` ile projeye çekilir.
- **`writeFileSync(dosyaAdi, veri)`:** Dosyaya sıfırdan veri yazar. Eğer dosya zaten varsa içindeki **eski veriyi tamamen siler** ve üzerine yazar.
- **`appendFileSync(dosyaAdi, veri)`:** Dosyadaki eski verileri koruyarak yeni veriyi dosyanın **sonuna** ekler.
- **Alt Satıra Geçiş:** Dosyada her veriyi yeni bir satıra yazdırmak için metnin sonuna `\n` (newline) karakteri konur.

**Örnek / Komut:**
```javascript
const fs = require('fs');

// 1. Sıfırdan dosya oluştur ve yaz
fs.writeFileSync('not.txt', 'İlk kayıt.\n');

// 2. Döngü ile veriyi kaybetmeden sonuna ekle
for (let i = 1; i <= 3; i++) {
    fs.appendFileSync('not.txt', `Kayıt ${i}\n`);
}
```

**Hatırla:** Döngü içinde `writeFileSync` kullanırsan, döngünün her adımında dosya silinip baştan yazılacağı için sadece en son değer kalır. Döngüyle çoklu veri yazarken mutlaka `appendFileSync` kullanmalısın!

---

## 24 Mayıs 2026 - 18.4 ve 18.5 Dosya Okuma ve JSON Dosyası Yönetimi

**Kaynak:** `ders.md`
**Kısa Tanım:** Node.js'te `.txt` veya `.json` gibi dosyalardaki verileri okumak (`readFileSync`) ve JavaScript objelerini metin olarak dosyaya kaydedip sonradan tekrar objeye çevirmek (`JSON.stringify` ve `JSON.parse`) için kullanılan komutlardır.

**Anahtar Kavramlar:**
- **`readFileSync(dosyaAdi, karakterSeti)`:** Dosyadaki tüm metni tek seferde okur. Türkçe karakter sorunu olmaması için karakter seti her zaman `'utf8'` olarak verilmelidir.
- **Satırlara Bölme (`split('\n')`):** Dosyadan okunan tek parça dev metni alt alta satırlardan kopararak bir JavaScript Dizisine (Array) çevirir.
- **`JSON.stringify(obje)`:** Bir JavaScript nesnesini veya dizisini bilgisayardaki bir dosyaya yazabilmek için önce düz bir metne çevirir. (Dosyaya doğrudan obje kaydedilemez, edilirse dosyada yazısı çıkar).
- **`JSON.parse(metin)`:** JSON dosyasından okunan ve henüz bir "metin" olan yapıyı tekrar içindeki verilere erişebileceğimiz gerçek bir JS objesine veya dizisine çevirir.

**Örnek / Komut:**
```javascript
const fs = require('fs');

// 1. Text Dosyası Okuma ve Satırlara Ayırma
const data = fs.readFileSync('sayilar.txt', 'utf8');
const satirlar = data.split('\n'); // Örn: ["1", "2", "3"]

// 2. JS Objesini JSON Olarak Dosyaya Kaydetme
let ogrenci = { ad: "Ali", not: 90 };
fs.writeFileSync('veri.json', JSON.stringify(ogrenci));

// 3. JSON Dosyasından Veri Okuyup Objeye Geri Çevirme
const jsonMetni = fs.readFileSync('veri.json', 'utf8');
const okunanOgrenci = JSON.parse(jsonMetni);
console.log(okunanOgrenci.ad); // Konsola "Ali" yazar
```

**Hatırla:** Bir JS nesnesini dosyaya yazmadan önce **kesinlikle** `JSON.stringify()` yap; bir JSON dosyasından okuduğun verinin içindeki `ad` veya `yas` özelliklerine erişmek için ise o veriyi **kesinlikle önce** `JSON.parse()` ile çevir!

---

## 24 Mayıs 2026 - 18.6 ve 18.7 Express.js ve Routing Temelleri

**Kaynak:** `ders.md`
**Kısa Tanım:** Express.js, Node.js üzerinde çalışan, web sunucusu (web sitesi) kurmamızı ve farklı sayfalara (`/`, `/iletisim` vb.) yönlendirmeleri (routing) çok kolaylaştıran bir eklentidir (framework).

**Anahtar Kavramlar:**
- **Kurulum:** Terminalde `npm install express` yazılarak projeye indirilir.
- **Routing (Yönlendirme):** `app.get('/adres', ...)` yapısıyla hangi URL'ye (linke) girildiğinde ne cevap verileceği belirlenir.
- **`res.send()`:** Tarayıcıya doğrudan düz metin göndermek için kullanılır.
- **`res.sendFile()`:** Daha önceden hazırlanmış tam bir HTML dosyasını (örneğin `views/index.html`) kullanıcıya göndermek için kullanılır.
- **`express.static('public')`:** HTML'in içindeki CSS dosyalarının veya resimlerin çalışabilmesi için bir klasörü (genelde adı `public` olur) dışarıya (internete) açık hale getirme işlemidir.

**Örnek / Komut:**
```javascript
const express = require('express');
const app = express();

// CSS ve resimlerin çalışması için public klasörünü dışarıya açıyoruz
app.use(express.static('public'));

// Ana sayfa ('/') açıldığında çalışacak kod
app.get('/', (req, res) => {
    // res.send("Hoş geldiniz!"); // Sadece metin olsaydı
    res.sendFile(__dirname + '/views/index.html'); // Tüm HTML'i gönderir
});

// Sunucuyu başlatıyoruz
app.listen(3000, () => {
    console.log("Sunucu http://localhost:3000 adresinde çalışıyor");
});
```

**Hatırla:** Express.js projelerinde standart bir düzen vardır: HTML dosyaları `views` klasöründe tutulur, stil (CSS) ve resim dosyaları ise `public` klasöründe tutulur.

---

## 24 Mayıs 2026 - 19.4 ve 19.5 Final Tekrarı: Fonksiyonlar ve 'this' Davranışı

**Kaynak:** `ders.md`
**Kısa Tanım:** Final sınavında sıkça sorulan; normal fonksiyonlar ile Arrow (Ok) fonksiyonlarının farkları ve `return` mantığının hızlı tekrarıdır.

**Anahtar Kavramlar:**
- **`return` vs `console.log`:** `console.log` sadece ekrana bilgi basar, hesaplanan veriyi dışarıya vermez. `return` ise fonksiyonun ürettiği sonucu başka bir yerde (örneğin bir değişkene atayarak) kullanabilmemiz için dışarı aktarır.
- **Varsayılan Parametre (Default Parameter):** Fonksiyon çağrılırken değer gönderilmezse hata çıkmasını önlemek için baştan atanan değerdir. (Örn: `function selamla(isim = "Ziyaretçi")`)
- **Event Listener İçinde `this`:** Bir tıklama (click) olayında Normal Fonksiyon kullanırsan, `this` direkt olarak **tıklanan butonu** temsil eder. Ancak **Arrow Function (`=>`) kullanırsan `this` butonu bulamaz** (Window nesnesini gösterir) ve kod patlar.

**Örnek / Komut:**
```javascript
const buton = document.getElementById("btnGonder");

// DOĞRU KULLANIM (Normal Fonksiyon)
buton.addEventListener("click", function() {
    this.style.backgroundColor = "red"; // 'this' butonu işaret eder.
});

// HATALI KULLANIM (Arrow Function)
buton.addEventListener("click", () => {
    this.style.backgroundColor = "red"; // 'this' butonu GÖREMEZ, kod çalışmaz!
});
```

**Hatırla:** HTML olaylarında (`addEventListener` içinde) tıklanan elementin rengini/boyutunu `this` ile değiştireceksen **kesinlikle** Normal `function()` kullanmalısın, Arrow Function kullanmaktan kaçınmalısın!

---

## 24 Mayıs 2026 - 19.6 Final Tekrarı: Bootstrap Grid Breakpointleri

**Kaynak:** `ders.md`
**Kısa Tanım:** Bootstrap ızgara (Grid) sisteminde, kutuların farklı ekran genişliklerinde (telefon, tablet, bilgisayar) kaçar sütun yer kaplayacağını belirleyen kırılma noktalarıdır.

**Anahtar Kavramlar:**
- **Breakpoint (Kırılma Noktası) Sınırları:** 
  - `col-`: Tüm ekranlar için geçerlidir.
  - `col-sm-`: **576px** ve üstü ekranlarda aktifleşir (Büyük telefonlar).
  - `col-md-`: **768px** ve üstü ekranlarda aktifleşir (Tabletler ve küçük laptoplar).
  - `col-lg-`: **992px** ve üstü ekranlarda aktifleşir (Büyük laptop, masaüstü).
- **Temel Davranış:** Bir sütuna küçük ekran için özel bir sınıf yazılmamışsa, Bootstrap onu mobil cihazlarda otomatik olarak tam genişlikte (`col-12` gibi) gösterir ve kutular **alt alta** düşer.
- **Yorumlama Örneği:** Bir elemanda `<div class="col-12 col-md-4">` yazıyorsa; bu kutu 768px'ten küçük tüm ekranlarda (mobilde) satırı tam kaplar (12 birim). Ekran genişliği 768px'i geçtiği anda (`md` aktif olur) satırın 3'te 1'ini (4 birim) kaplar ve yan yana dizilmeye başlarlar.
