JAVASCRIPT DERS NOTU
1. addEventListener() Metodu
1.1. addEventListener() Nedir?
JavaScript’te bir HTML elemanı üzerinde bir olay gerçekleştiğinde bir işlem yaptırmak için addEventListener() metodu kullanılır.
Bu metot sayesinde:
bir butona tıklanınca işlem yapılabilir, 
fare elemanın üzerine gelince stil değiştirilebilir, 
klavyeden bir tuşa basılınca tepki verilebilir, 
pencere boyutu değişince sayfa güncellenebilir. 
Genel kullanım:
element.addEventListener("event", fonksiyon);
Örnek:
<button id="btn">Tıkla</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    alert("Butona tıklandı");
});
</script>
Bu örnekte kullanıcı butona tıklayınca ekranda bir uyarı kutusu açılır.

onclick ile addEventListener() Arasındaki Fark
onclick da olay eklemek için kullanılabilir, ancak addEventListener() daha güçlüdür.
onclick kullanımı:
btn.onclick = function() {
    alert("Tıklandı");
};
addEventListener() kullanımı:
btn.addEventListener("click", function() {
    alert("Tıklandı");
});
addEventListener() neden daha iyidir?
Aynı elemana birden fazla olay eklenebilir. 
Aynı elemana aynı türden birden fazla işleyici eklenebilir. 
Kod daha düzenli olur. 
Modern JavaScript’te daha çok tercih edilir. 

Farklı Fare Eventleri ile Örnekler
Fare olayları kullanıcı fare ile işlem yaptığında tetiklenir.

click
Kullanıcı elemana tıkladığında çalışır.
<button id="btn1">Tıkla</button>
<p id="sonuc1"></p>

<script>
document.getElementById("btn1").addEventListener("click", function() {
    document.getElementById("sonuc1").innerText = "Butona tıklandı.";
});
</script>

dblclick
Kullanıcı çift tıklama yaptığında çalışır.
<button id="btn2">Çift Tıkla</button>
<p id="sonuc2"></p>

<script>
document.getElementById("btn2").addEventListener("dblclick", function() {
    document.getElementById("sonuc2").innerText = "Butona çift tıklandı.";
});
</script>

mouseover
Fare elemanın üzerine gelince çalışır.
<div id="kutu1" style="width:200px; height:100px; background-color:lightblue;">
    Fareyi buraya getir
</div>

<script>
document.getElementById("kutu1").addEventListener("mouseover", function() {
    this.style.backgroundColor = "orange";
});
</script>

mouseout
Fare elemanın üzerinden ayrılınca çalışır.
<div id="kutu2" style="width:200px; height:100px; background-color:lightgreen;">
    Fareyi getir ve çek
</div>

<script>
let kutu2 = document.getElementById("kutu2");

kutu2.addEventListener("mouseover", function() {
    kutu2.style.backgroundColor = "red";
});

kutu2.addEventListener("mouseout", function() {
    kutu2.style.backgroundColor = "lightgreen";
});
</script>

mousedown
Fare tuşuna basıldığı anda çalışır.
<button id="btn3">Basılı tut</button>

<script>
document.getElementById("btn3").addEventListener("mousedown", function() {
    this.innerText = "Fare tuşuna basıldı";
});
</script>

mouseup
Fare tuşu bırakıldığında çalışır.
<button id="btn4">Bana bas</button>

<script>
let btn4 = document.getElementById("btn4");

btn4.addEventListener("mousedown", function() {
    btn4.style.backgroundColor = "red";
});

btn4.addEventListener("mouseup", function() {
    btn4.style.backgroundColor = "green";
});
</script>

mousemove
Fare eleman üzerinde hareket ettikçe çalışır.
<div id="alan" style="width:300px; height:150px; border:1px solid black;"></div>
<p id="koordinat"></p>

<script>
document.getElementById("alan").addEventListener("mousemove", function(event) {
    document.getElementById("koordinat").innerText =
        "X: " + event.offsetX + " Y: " + event.offsetY;
});
</script>
Bu örnek fare koordinatlarını gösterir.

Farklı Klavye Eventleri ile Örnekler
Klavye olayları kullanıcı klavyeden tuşlara bastığında çalışır.

keydown
Bir tuşa basıldığı anda çalışır.
<input type="text" id="metin1" placeholder="Bir tuşa bas">
<p id="sonuc3"></p>

<script>
document.getElementById("metin1").addEventListener("keydown", function(event) {
    document.getElementById("sonuc3").innerText = "Basılan tuş: " + event.key;
});
</script>

keyup
Basılan tuş bırakıldığında çalışır.
<input type="text" id="metin2" placeholder="Yazı yaz">
<p id="sonuc4"></p>

<script>
document.getElementById("metin2").addEventListener("keyup", function() {
    document.getElementById("sonuc4").innerText = this.value;
});
</script>
Bu örnekte kullanıcı yazdıkça metin aşağıda gösterilir.

Basit Click Event
<button id="btn">Tıkla</button>

<script>
function tikla(){
    alert("Butona tıklandı");
}

document.getElementById("btn").addEventListener("click", tikla);
</script>
👉 Dikkat:
tikla   // DOĞRU
tikla() // YANLIŞ (hemen çalıştırır)

Arka Plan Değiştirme
<button id="btn">Renk Değiştir</button>

<script>
function renkDegistir(){
    document.body.style.background = "yellow";
}

document.getElementById("btn").addEventListener("click", renkDegistir);
</script>

Metin Değiştirme
<p id="yazi">Eski yazı</p>
<button id="btn">Değiştir</button>

<script>
function yaziyiDegistir(){
    document.getElementById("yazi").innerText = "Yeni yazı";
}

document.getElementById("btn").addEventListener("click", yaziyiDegistir);
</script>

Mouseover / Mouseout
<div id="kutu" style="width:200px;height:100px;background:lightblue;"></div>

<script>
let kutu = document.getElementById("kutu");

function ustuneGel(){
    kutu.style.background = "red";
}

function ustundenCik(){
    kutu.style.background = "lightblue";
}

kutu.addEventListener("mouseover", ustuneGel);
kutu.addEventListener("mouseout", ustundenCik);
</script>

Klavye Event
<input type="text" id="giris">
<p id="sonuc"></p>

<script>
function tusYaz(event){
    document.getElementById("sonuc").innerText = event.key;
}

document.getElementById("giris").addEventListener("keydown", tusYaz);
</script>

Input Kontrol
<input type="text" id="input">
<p id="mesaj"></p>

<script>
function kontrol(){
    let deger = document.getElementById("input").value;

    if(deger.length < 5){
        document.getElementById("mesaj").innerText = "En az 5 karakter giriniz";
    }
else
        document.getElementById("mesaj").innerText = "Tamam";
}

document.getElementById("input").addEventListener("keyup", kontrol);
</script>

EVENT PARAMETRESİ KULLANIMI

Event Nesnesi Kullanımı
<div id="alan" style="width:300px;height:150px;border:1px solid black;"></div>
<p id="koordinat"></p>

<script>
function koordinatGoster(e){
    document.getElementById("koordinat").innerText =
        "X: " + e.clientX + " Y: " + e.clientY;
}

document.getElementById("alan").addEventListener("mousemove", koordinatGoster);
</script>

Aynı Fonksiyon Birden Fazla Event’te
<button id="btn">Tıkla</button>

<script>
function mesajVer(){
    console.log("Event çalıştı");
}

let btn = document.getElementById("btn");

btn.addEventListener("click", mesajVer);
btn.addEventListener("mouseover", mesajVer);
</script>


this Kullanımı
<button id="btn">Tıkla</button>

<script>
function renkDegistir(){
    this.style.background = "green";
}

document.getElementById("btn").addEventListener("click", renkDegistir);
</script>
👉 this = event’i tetikleyen eleman

Parametreli Fonksiyon (Wrapper ile)
Doğrudan parametre gönderemeyiz, dolaylı kullanırız:
<button id="btn">Tıkla</button>

<script>
function mesaj(yazi){
    console.log(yazi);
}

document.getElementById("btn").addEventListener("click", function(){
    mesaj("Merhaba Dünya");
});
</script>

Toggle Mantığı
<button id="btn">Başlat / Durdur</button>

<script>
let aktif = false;

function degistir(){
    aktif = !aktif;

    if(aktif){
        document.body.style.background = "black";
    }else{
        document.body.style.background = "white";
    }
}

document.getElementById("btn").addEventListener("click", degistir);
</script>

Profesyonel Kullanım (Modüler)
<button id="btn">Tıkla</button>

<script>
function init(){

    let btn = document.getElementById("btn");

    btn.addEventListener("click", tikla);

}

function tikla(){
    alert("Başlatıldı");
}

init();
</script>




getElementsByClassName() Metodu
Belirli bir class adına sahip tüm elemanları seçmek için kullanılır.
Kullanımı:
document.getElementsByClassName("classAdi")
Bu metot bir dizi benzeri koleksiyon döndürür. Bu yüzden genellikle döngü ile kullanılır.

Temel Örnek
<p class="yazi">Birinci paragraf</p>
<p class="yazi">İkinci paragraf</p>
<p class="yazi">Üçüncü paragraf</p>

<script>
let elemanlar = document.getElementsByClassName("yazi");

for(let i = 0; i < elemanlar.length; i++) {
    elemanlar[i].style.color = "blue";
}
</script>
Bu örnekte yazi sınıfına sahip tüm paragrafların yazı rengi mavi olur.
Belirli Bir Elemanı Seçme
<div class="kutu">Kutu 1</div>
<div class="kutu">Kutu 2</div>
<div class="kutu">Kutu 3</div>

<script>
let kutular = document.getElementsByClassName("kutu");
kutular[1].style.backgroundColor = "yellow";
</script>
Burada ikinci kutunun arka planı sarı olur.

Butona Tıklayınca Tüm Class Elemanlarını Değiştirme
<p class="renkli">Metin 1</p>
<p class="renkli">Metin 2</p>
<p class="renkli">Metin 3</p>
<button id="degistir">Renk Değiştir</button>

<script>
document.getElementById("degistir").addEventListener("click", function() {
    let metinler = document.getElementsByClassName("renkli");

    for(let i = 0; i < metinler.length; i++) {
        metinler[i].style.color = "red";
        metinler[i].style.fontSize = "24px";
    }
});
</script>

Aynı Öğeye Birden Fazla Event İşleyici Ekleme
addEventListener() metodunun en güçlü yönlerinden biri budur.
<button id="btn5">Tıkla</button>

<script>
let btn5 = document.getElementById("btn5");

btn5.addEventListener("click", function() {
    alert("Birinci işleyici çalıştı");
});

btn5.addEventListener("click", function() {
    console.log("İkinci işleyici çalıştı");
});

btn5.addEventListener("click", function() {
    document.body.style.backgroundColor = "lightgray";
});
</script>
Bu örnekte tek bir tıklamada üç farklı işlem yapılır.

Aynı Öğeye Farklı Türde Eventler Ekleme
Bir elemana hem tıklama, hem fare ile üzerine gelme, hem de fare ayrılma olayları eklenebilir.
<div id="kutu3" style="width:200px; height:100px; background-color:skyblue;">
    Kutunun üzerine gel ve tıkla
</div>

<script>
let kutu3 = document.getElementById("kutu3");

kutu3.addEventListener("mouseover", function() {
    kutu3.style.backgroundColor = "orange";
});

kutu3.addEventListener("mouseout", function() {
    kutu3.style.backgroundColor = "skyblue";
});

kutu3.addEventListener("click", function() {
    kutu3.innerText = "Kutuya tıklandı";
});
</script>


Sık Kullanılan Form Elemanları
TEXT (Metin Kutusu)

Değer Alma
<input type="text" id="ad">
<button onclick="goster()">Göster</button>

<script>
function goster(){
    let deger = document.getElementById("ad").value;
    alert(deger);
}
</script>

Yazarken Canlı Gösterme
<input type="text" id="ad">
<p id="sonuc"></p>

<script>
document.getElementById("ad").addEventListener("keyup", function(){
    document.getElementById("sonuc").innerText = this.value;
});
</script>
Uzunluk Kontrolü
if(ad.value.length < 3){
    ad.style.border = "2px solid red";
}

TEXTAREA (Çok Satırlı Metin)
<textarea id="mesaj"></textarea>
<button onclick="oku()">Oku</button>

<script>
function oku(){
    let veri = document.getElementById("mesaj").value;
    console.log(veri);
}
</script>

Karakter Sayacı
<textarea id="mesaj"></textarea>
<p id="sayac"></p>

<script>
mesaj.addEventListener("input", function(){
    sayac.innerText = "Karakter: " + mesaj.value.length;
});
</script>

CHECKBOX
Seçili mi?
<input type="checkbox" id="onay"> Onaylıyorum

<script>
onay.addEventListener("change", function(){
    console.log(onay.checked);
});
</script>

Birden Fazla Checkbox
<input type="checkbox" class="secim" value="HTML"> HTML
<input type="checkbox" class="secim" value="CSS"> CSS
<input type="checkbox" class="secim" value="JS"> JS

<button onclick="goster()">Göster</button>

<script>
function goster(){
    let secimler = document.querySelectorAll(".secim:checked");

    secimler.forEach(s => console.log(s.value));
}
</script>

RADIO BUTTON
Seçilen değeri alma
<input type="radio" name="cinsiyet" value="Erkek"> Erkek
<input type="radio" name="cinsiyet" value="Kadın"> Kadın

<button onclick="sec()">Seç</button>

<script>
function sec(){
    let secilen = document.querySelector("input[name='cinsiyet']:checked");

    if(secilen)
        alert(secilen.value);
}
</script>

COLOR INPUT
Renk seçimi
<input type="color" id="renk">

<script>
renk.addEventListener("input", function(){
    document.body.style.background = renk.value;
});
</script>

Metin rengini değiştirme
yazi.style.color = renk.value;

DATE INPUT
Tarih alma
<input type="date" id="tarih">

<script>
tarih.addEventListener("change", function(){
    console.log(tarih.value);
});
</script>

Yaş Hesaplama
let secilen = new Date(tarih.value);
let bugun = new Date();

let yas = bugun.getFullYear() - secilen.getFullYear();

console.log(yas);

TIME INPUT

Saat alma
<input type="time" id="saat">

<script>
saat.addEventListener("change", function(){
    console.log(saat.value);
});
</script>

Saat karşılaştırma
if(saat.value < "12:00"){
    console.log("Sabah");
}

RANGE (Slider)
<input type="range" id="range" min="0" max="100">
<p id="deger"></p>

<script>
range.addEventListener("input", function(){
    deger.innerText = range.value;
});
</script>

Font büyüklüğü kontrolü
range.addEventListener("input", function(){
    yazi.style.fontSize = range.value + "px";
});

Mini Form Uygulaması
<input type="text" id="ad" placeholder="Ad">
<textarea id="mesaj"></textarea>

<input type="checkbox" id="onay"> Onay

<input type="color" id="renk">
<input type="range" id="range">

<button id="gonder">Gönder</button>

<script>
document.getElementById("gonder").addEventListener("click", function(){

    let ad = document.getElementById("ad").value;
    let mesaj = document.getElementById("mesaj").value;
    let onay = document.getElementById("onay").checked;
    let renk = document.getElementById("renk").value;
    let boyut = document.getElementById("range").value;

    console.log(ad, mesaj, onay);

    document.body.style.background = renk;
    document.body.style.fontSize = boyut + "px";

});
</script>

VALIDATION (Doğrulama)
Basit Form Kontrolü
if(ad.value == ""){
    alert("Ad boş olamaz");
}
Checkbox zorunlu
if(!onay.checked){
    alert("Onaylamalısınız");
}

Focus örneği
input.addEventListener("focus", function(){
    input.style.border = "3px solid blue";
});

Hepsini Seç / Kaldır
<input type="checkbox" id="tumunuSec"> Tümünü Seç <br>

<input type="checkbox" class="ders" value="JS"> JS
<input type="checkbox" class="ders" value="CSS"> CSS
<input type="checkbox" class="ders" value="HTML"> HTML

<script>
document.getElementById("tumunuSec").addEventListener("change", function(){
    let dersler = document.querySelectorAll(".ders");

    dersler.forEach(d => d.checked = this.checked);
});
</script>

Seçilenleri Listeleme
<button onclick="listele()">Seçilenleri Göster</button>
<p id="sonuc"></p>

<script>
function listele(){
    let secilen = document.querySelectorAll(".ders:checked");

    let liste = [];

    secilen.forEach(s => liste.push(s.value));

    document.getElementById("sonuc").innerText = liste.join(", ");
}
</script>

En az 2 seçim zorunlu
function kontrol(){
    let secilen = document.querySelectorAll(".ders:checked");

    if(secilen.length < 2){
        alert("En az 2 seçim yapmalısınız");
    }
}

Checkbox ile tema değiştir
<input type="checkbox" id="tema"> Dark Mode

<script>
tema.addEventListener("change", function(){
    document.body.style.background = tema.checked ? "black" : "white";
    document.body.style.color = tema.checked ? "white" : "black";
});
</script>

Checkbox ile buton aktif/pasif
<input type="checkbox" id="onay"> Kabul ediyorum
<button id="gonder" disabled>Gönder</button>

<script>
onay.addEventListener("change", function(){
    gonder.disabled = !onay.checked;
});
</script>

Checkbox ile fiyat hesaplama
<input type="checkbox" class="urun" value="100"> Ürün 1
<input type="checkbox" class="urun" value="200"> Ürün 2

<p id="toplam"></p>

<script>
document.querySelectorAll(".urun").forEach(u=>{
    u.addEventListener("change", hesapla);
});

function hesapla(){
    let toplam = 0;

    document.querySelectorAll(".urun:checked").forEach(u=>{
        toplam += Number(u.value);
    });

    toplam.innerText = "Toplam: " + toplam;
}
</script>

Seçilen değeri anında göster
<input type="radio" name="cinsiyet" value="Erkek"> Erkek
<input type="radio" name="cinsiyet" value="Kadın"> Kadın

<p id="sonuc"></p>

<script>
document.querySelectorAll("input[name='cinsiyet']").forEach(r=>{
    r.addEventListener("change", function(){
        sonuc.innerText = this.value;
    });
});
</script>

 Radio ile fiyat belirleme
<input type="radio" name="paket" value="100"> Temel
<input type="radio" name="paket" value="200"> Orta
<input type="radio" name="paket" value="300"> Premium

<p id="fiyat"></p>

<script>
document.querySelectorAll("input[name='paket']").forEach(r=>{
    r.addEventListener("change", function(){
        fiyat.innerText = "Fiyat: " + this.value;
    });
});
</script>
Seçim zorunlu kontrol
let secilen = document.querySelector("input[name='paket']:checked");

if(!secilen){
    alert("Seçim yapmalısınız");
}

Radio ile arka plan değiştir
<input type="radio" name="renk" value="red"> Kırmızı
<input type="radio" name="renk" value="blue"> Mavi

<script>
document.querySelectorAll("input[name='renk']").forEach(r=>{
    r.addEventListener("change", function(){
        document.body.style.background = this.value;
    });
});
</script>

Radio ile görünüm değiştirme
if(secilen.value === "dark"){
    document.body.classList.add("dark");
}


window Nesnesi İçin Event Listener
window, tarayıcı penceresini temsil eder. Bu nesneye de event listener eklenebilir.

load
Sayfa tamamen yüklendiğinde çalışır.
<script>
window.addEventListener("load", function() {
    alert("Sayfa tamamen yüklendi");
});
</script>

resize
Pencere boyutu değiştiğinde çalışır.
<p id="boyut"></p>

<script>
window.addEventListener("resize", function() {
    document.getElementById("boyut").innerText =
        "Genişlik: " + window.innerWidth + " Yükseklik: " + window.innerHeight;
});
</script>

scroll
Sayfa kaydırıldığında çalışır.
<p id="kaydirma">Aşağı kaydır</p>

<script>
window.addEventListener("scroll", function() {
    document.getElementById("kaydirma").innerText =
        "Sayfa kaydırıldı. scrollY = " + window.scrollY;
});
</script>

Class İşlemleri
JavaScript ile HTML elemanlarının sınıfları üzerinde işlem yapmak için classList kullanılır.

Add Class
Bir elemana yeni bir class eklemek için kullanılır.
<style>
.kirmizi {
    color: red;
}
</style>

<p id="metinA">Merhaba Dünya</p>
<button id="ekle">Class Ekle</button>

<script>
document.getElementById("ekle").addEventListener("click", function() {
    document.getElementById("metinA").classList.add("kirmizi");
});
</script>

Remove Class
Bir elemandan class silmek için kullanılır.
<style>
.buyuk {
    font-size: 30px;
}
</style>

<p id="metinB" class="buyuk">Büyük Yazı</p>
<button id="sil">Class Sil</button>

<script>
document.getElementById("sil").addEventListener("click", function() {
    document.getElementById("metinB").classList.remove("buyuk");
});
</script>

Change Class
Bir class’ı değiştirmenin birkaç yolu vardır. En yaygın yöntemlerden biri eski class’ı silip yeni class eklemektir.
<style>
.mavi {
    color: blue;
}
.yesil {
    color: green;
}
</style>

<p id="metinC" class="mavi">Renk değiştirecek</p>
<button id="degis">Class Değiştir</button>

<script>
document.getElementById("degis").addEventListener("click", function() {
    let eleman = document.getElementById("metinC");
    eleman.classList.remove("mavi");
    eleman.classList.add("yesil");
});
</script>

Mevcut Öğeye Aktif Sınıf Ekleme
Çok kullanılan bir uygulamadır. Özellikle menülerde seçili öğeyi göstermek için kullanılır.
<style>
.menuElemani {
    padding: 10px;
    display: inline-block;
    background-color: lightgray;
    margin-right: 5px;
    cursor: pointer;
}
.aktif {
    background-color: darkblue;
    color: white;
}
</style>

<div class="menuElemani">Ana Sayfa</div>
<div class="menuElemani">Hakkımızda</div>
<div class="menuElemani">İletişim</div>

<script>
let menuler = document.getElementsByClassName("menuElemani");

for(let i = 0; i < menuler.length; i++) {
    menuler[i].addEventListener("click", function() {
        for(let j = 0; j < menuler.length; j++) {
            menuler[j].classList.remove("aktif");
        }
        this.classList.add("aktif");
    });
}
</script>
Bu örnekte hangi menü öğesine tıklanırsa sadece o öğe aktif olur.

HTML Elemanlarının Genişlik ve Yükseklik Özelliklerini Öğrenme
JavaScript ile image, button, div gibi elemanların genişlik ve yükseklik bilgileri alınabilir.

offsetWidth ve offsetHeight
Elemanın görünen genişlik ve yüksekliğini verir.
<img id="resim1" src="https://via.placeholder.com/150" alt="örnek resim">

<p id="bilgi"></p>

<script>
let resim1 = document.getElementById("resim1");
let bilgi = document.getElementById("bilgi");

bilgi.innerText = "Genişlik: " + resim1.offsetWidth + 
                  " Yükseklik: " + resim1.offsetHeight;
</script>
clientWidth ve clientHeight
Padding dahil, border hariç iç boyutu verir.
<div id="kutu4" style="width:200px; height:100px; padding:20px; border:5px solid black;">
    Örnek kutu
</div>

<script>
let kutu4 = document.getElementById("kutu4");
console.log("clientWidth:", kutu4.clientWidth);
console.log("clientHeight:", kutu4.clientHeight);
</script>
style.width ve style.height
CSS ile atanmış genişlik ve yüksekliği almak veya değiştirmek için kullanılır.
<button id="btnGenislik" style="width:120px; height:50px;">Buton</button>

<script>
let btnGenislik = document.getElementById("btnGenislik");

console.log(btnGenislik.style.width);
console.log(btnGenislik.style.height);
</script>
HTML Elemanlarının Genişlik ve Yüksekliğini Değiştirme

Resim Boyutunu Değiştirme
<img id="resim2" src="https://via.placeholder.com/100" alt="resim">
<button id="buyutResim">Resmi Büyüt</button>

<script>
document.getElementById("buyutResim").addEventListener("click", function() {
    let resim2 = document.getElementById("resim2");
    resim2.style.width = "300px";
    resim2.style.height = "200px";
});
</script>

Buton Boyutunu Değiştirme
<button id="btnBoyut">Boyut Değiştir</button>

<script>
document.getElementById("btnBoyut").addEventListener("click", function() {
    this.style.width = "200px";
    this.style.height = "80px";
    this.style.fontSize = "24px";
});
</script>

Div Genişliği ve Yüksekliğini Artırma
<div id="kutu5" style="width:100px; height:100px; background-color:coral;"></div>
<button id="arttir">Kutuyu Büyüt</button>

<script>
document.getElementById("arttir").addEventListener("click", function() {
    let kutu5 = document.getElementById("kutu5");

    kutu5.style.width = "250px";
    kutu5.style.height = "150px";
});
</script>
Uygulama: Resim Üzerine Gelince Büyüsün
<img id="resim3" src="https://via.placeholder.com/150" alt="örnek">

<script>
let resim3 = document.getElementById("resim3");

resim3.addEventListener("mouseover", function() {
    resim3.style.width = "250px";
    resim3.style.height = "250px";
});

resim3.addEventListener("mouseout", function() {
    resim3.style.width = "150px";
    resim3.style.height = "150px";
});
</script>

Uygulama: Klavyeden Tuşa Basınca Kutunun Boyutu Değişsin
<div id="kutu6" style="width:100px; height:100px; background-color:purple;"></div>

<script>
document.addEventListener("keydown", function(event) {
    let kutu6 = document.getElementById("kutu6");

    if(event.key === "ArrowUp") {
        kutu6.style.height = "150px";
    }

    if(event.key === "ArrowRight") {
        kutu6.style.width = "150px";
    }
});
</script>

Uygulama: getElementsByClassName() ile Tüm Butonların Boyutunu Değiştirme
<button class="btnSinif">Buton 1</button>
<button class="btnSinif">Buton 2</button>
<button class="btnSinif">Buton 3</button>
<button id="hepsiniBuyut">Hepsini Büyüt</button>

<script>
document.getElementById("hepsiniBuyut").addEventListener("click", function() {
    let butonlar = document.getElementsByClassName("btnSinif");

    for(let i = 0; i < butonlar.length; i++) {
        butonlar[i].style.width = "180px";
        butonlar[i].style.height = "60px";
        butonlar[i].style.fontSize = "20px";
    }
});
</script>


 getComputedStyle() Nedir?
getComputedStyle() metodu, bir HTML elementine uygulanan tüm CSS kurallarının birleşmiş (hesaplanmış) son halini döndürür.
Bir elementin stili şu kaynaklardan gelebilir:
Inline CSS (style="") 
<style> etiketi 
Harici CSS dosyası 
Tarayıcı varsayılanları
Tanım
getComputedStyle() → “Elementin şu anda ekranda görünen gerçek stil değerlerini verir.”
Kullanım Şekli
getComputedStyle(element)
veya:
const stil = getComputedStyle(element);

Temel Örnek
<div id="kutu" style="width:150px; height:50px;"></div>
const kutu = document.getElementById("kutu");

const stil = getComputedStyle(kutu);

console.log(stil.width);   // "150px"
console.log(stil.height);  // "50px"

style ile farkı
kutu.style.width
getComputedStyle(kutu).width


Örnek
#kutu {
  width: 200px;
}
console.log(kutu.style.width);           // ""
console.log(getComputedStyle(kutu).width); // "200px"
Dönen Veri Türü
getComputedStyle() bir nesne döndürür:
CSSStyleDeclaration
İçinden şu şekilde veri alınır:
const stil = getComputedStyle(element);

stil.width
stil.margin
stil.padding
stil.color
En Çok Kullanılan Özellikler
stil.width
stil.height
stil.margin
stil.padding
stil.display
stil.position
stil.color
stil.backgroundColor
stil.fontSize
Önemli Kullanım Senaryoları
Element gizli mi?
if (getComputedStyle(element).display === "none") {
    console.log("Element gizli");
}

Dinamik genişlik değiştirme
const kutu = document.getElementById("kutu");

let genislik = parseInt(getComputedStyle(kutu).width);

kutu.style.width = (genislik + 50) + "px";
 Margin/Padding hesaplama
const stil = getComputedStyle(element);

console.log(stil.marginLeft);
console.log(stil.paddingTop);
Responsive kontrol
if (getComputedStyle(element).display === "block") {
    console.log("Mobil görünüm aktif");
}
String Değer Problemi
getComputedStyle() değerleri string olarak döndürür
"100px"
👉 Sayıya çevirmek için:
parseInt(stil.width)

Gelişmiş Örnek
<div id="kutu"></div>
#kutu {
  width: 100px;
  height: 100px;
  background-color: red;
}
const kutu = document.getElementById("kutu");

function buyut() {
    let width = parseInt(getComputedStyle(kutu).width);
    kutu.style.width = (width + 20) + "px";
}
Kısa Alıştırmalar
Aşağıdaki uygulamaları kendin yapmaya çalış:
Bir butona tıklanınca paragraf rengini değiştir. 
Fare bir resmin üzerine gelince resmi büyüt. 
Klavyede Enter tuşuna basılınca ekrana mesaj yazdır. 
getElementsByClassName() ile tüm kutuların arka planını değiştir. 
Menüde tıklanan elemanı aktif yap. 
Bir butona tıklanınca tüm butonların genişliğini artır. 
Pencere boyutu değişince yeni genişliği ekrana yazdır. 

JavaScript DOM ve Event — 30 Uygulama Sorusu ve Çözümleri

1. Butona tıklanınca paragrafın metnini değiştiriniz.
Çözüm
<p id="yazi">Eski metin</p>
<button id="btn">Değiştir</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    document.getElementById("yazi").innerText = "Yeni metin";
});
</script>

2. Butona tıklanınca sayfanın arka plan rengini sarı yapınız.
Çözüm
<button id="btn">Arka Planı Değiştir</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    document.body.style.backgroundColor = "yellow";
});
</script>

3. Bir kutunun üzerine fare gelince kutunun rengini kırmızı yapınız.
Çözüm
<div id="kutu" style="width:200px; height:100px; background-color:lightblue;"></div>

<script>
document.getElementById("kutu").addEventListener("mouseover", function() {
    this.style.backgroundColor = "red";
});
</script>

4. Fare kutudan çıkınca eski rengine dönsün.
Çözüm
<div id="kutu" style="width:200px; height:100px; background-color:lightblue;"></div>

<script>
let kutu = document.getElementById("kutu");

kutu.addEventListener("mouseover", function() {
    kutu.style.backgroundColor = "red";
});

kutu.addEventListener("mouseout", function() {
    kutu.style.backgroundColor = "lightblue";
});
</script>

5. Butona tıklanınca resmin genişliğini 300px yapınız.
Çözüm
<img id="resim" src="https://via.placeholder.com/100" alt="örnek">
<button id="btn">Resmi Büyüt</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    document.getElementById("resim").style.width = "300px";
});
</script>

6. Butona tıklanınca resmin genişliği ve yüksekliği değişsin.
Çözüm
<img id="resim" src="https://via.placeholder.com/100" alt="örnek">
<button id="btn">Boyut Değiştir</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    let resim = document.getElementById("resim");
    resim.style.width = "250px";
    resim.style.height = "180px";
});
</script>

7. Bir input alanına yazı yazıldıkça yazılan metni altta gösteriniz.
Çözüm
<input type="text" id="metinKutusu">
<p id="sonuc"></p>

<script>
document.getElementById("metinKutusu").addEventListener("keyup", function() {
    document.getElementById("sonuc").innerText = this.value;
});
</script>

8. Bir butona çift tıklanınca ekrana uyarı veriniz.
Çözüm
<button id="btn">Çift Tıkla</button>

<script>
document.getElementById("btn").addEventListener("dblclick", function() {
    alert("Butona çift tıklandı");
});
</script>

9. Butona basılı tutulduğunda rengi kırmızı, bırakıldığında yeşil olsun.
Çözüm
<button id="btn">Basılı Tut</button>

<script>
let btn = document.getElementById("btn");

btn.addEventListener("mousedown", function() {
    btn.style.backgroundColor = "red";
});

btn.addEventListener("mouseup", function() {
    btn.style.backgroundColor = "green";
});
</script>

10. Fare bir alan içinde hareket ettikçe X ve Y koordinatlarını gösteriniz.
Çözüm
<div id="alan" style="width:300px; height:150px; border:1px solid black;"></div>
<p id="koordinat"></p>

<script>
document.getElementById("alan").addEventListener("mousemove", function(event) {
    document.getElementById("koordinat").innerText =
        "X: " + event.offsetX + " Y: " + event.offsetY;
});
</script>

11. Klavyede basılan tuşu ekranda gösteriniz.
Çözüm
<input type="text" id="giris">
<p id="sonuc"></p>

<script>
document.getElementById("giris").addEventListener("keydown", function(event) {
    document.getElementById("sonuc").innerText = "Basılan tuş: " + event.key;
});
</script>

12. Enter tuşuna basılınca “Form gönderildi” yazdırınız.
Çözüm
<input type="text" id="giris">
<p id="sonuc"></p>

<script>
document.getElementById("giris").addEventListener("keydown", function(event) {
    if (event.key === "Enter") {
        document.getElementById("sonuc").innerText = "Form gönderildi";
    }
});
</script>

13. getElementsByClassName() kullanarak tüm paragrafların rengini mavi yapınız.
Çözüm
<p class="ortak">Paragraf 1</p>
<p class="ortak">Paragraf 2</p>
<p class="ortak">Paragraf 3</p>

<script>
let paragraflar = document.getElementsByClassName("ortak");

for (let i = 0; i < paragraflar.length; i++) {
    paragraflar[i].style.color = "blue";
}
</script>

14. Butona tıklanınca aynı class’a sahip tüm kutuların arka planını değiştiriniz.
Çözüm
<div class="kutu" style="width:100px;height:50px;background:gray;margin:5px;"></div>
<div class="kutu" style="width:100px;height:50px;background:gray;margin:5px;"></div>
<div class="kutu" style="width:100px;height:50px;background:gray;margin:5px;"></div>
<button id="btn">Hepsini Değiştir</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    let kutular = document.getElementsByClassName("kutu");

    for (let i = 0; i < kutular.length; i++) {
        kutular[i].style.backgroundColor = "orange";
    }
});
</script>

15. Aynı butona iki farklı click event’i ekleyiniz.
Çözüm
<button id="btn">Tıkla</button>

<script>
let btn = document.getElementById("btn");

btn.addEventListener("click", function() {
    alert("Birinci işlem");
});

btn.addEventListener("click", function() {
    console.log("İkinci işlem");
});
</script>

16. Aynı elemana hem mouseover hem click event’i ekleyiniz.
Çözüm
<div id="kutu" style="width:200px;height:100px;background:lightgreen;">Kutu</div>

<script>
let kutu = document.getElementById("kutu");

kutu.addEventListener("mouseover", function() {
    kutu.style.backgroundColor = "pink";
});

kutu.addEventListener("click", function() {
    kutu.innerText = "Kutunun içine tıklandı";
});
</script>

17. Pencere yeniden boyutlandırıldığında genişlik ve yüksekliği ekrana yazdırınız.
Çözüm
<p id="boyut"></p>

<script>
window.addEventListener("resize", function() {
    document.getElementById("boyut").innerText =
        "Genişlik: " + window.innerWidth + " Yükseklik: " + window.innerHeight;
});
</script>

18. Sayfa yüklendiğinde uyarı veren bir uygulama yapınız.
Çözüm
<script>
window.addEventListener("load", function() {
    alert("Sayfa yüklendi");
});
</script>

19. Sayfa kaydırıldığında scroll miktarını gösteriniz.
Çözüm
<p id="sonuc">Kaydırma miktarı</p>
<div style="height:1500px;"></div>

<script>
window.addEventListener("scroll", function() {
    document.getElementById("sonuc").innerText = "scrollY: " + window.scrollY;
});
</script>

20. Butona tıklanınca bir elemana aktif class’ı ekleyiniz.
Çözüm
<style>
.aktif {
    background-color: darkblue;
    color: white;
}
</style>

<p id="metin">Merhaba</p>
<button id="btn">Aktif Yap</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    document.getElementById("metin").classList.add("aktif");
});
</script>

21. Butona tıklanınca bir elemandan class siliniz.
Çözüm
<style>
.buyuk {
    font-size: 30px;
}
</style>

<p id="metin" class="buyuk">Büyük yazı</p>
<button id="btn">Class Sil</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    document.getElementById("metin").classList.remove("buyuk");
});
</script>

22. Bir class’ı başka bir class ile değiştiriniz.
Çözüm
<style>
.kirmizi { color: red; }
.yesil { color: green; }
</style>

<p id="metin" class="kirmizi">Renk değişecek</p>
<button id="btn">Değiştir</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    let metin = document.getElementById("metin");
    metin.classList.remove("kirmizi");
    metin.classList.add("yesil");
});
</script>

23. Menüde tıklanan öğeye aktif class ekleyip diğerlerinden kaldırınız.
Çözüm
<style>
.menu {
    display: inline-block;
    padding: 10px;
    background: lightgray;
    margin-right: 5px;
    cursor: pointer;
}
.aktif {
    background: black;
    color: white;
}
</style>

<div class="menu">Ana Sayfa</div>
<div class="menu">Hakkımızda</div>
<div class="menu">İletişim</div>

<script>
let menuler = document.getElementsByClassName("menu");

for (let i = 0; i < menuler.length; i++) {
    menuler[i].addEventListener("click", function() {
        for (let j = 0; j < menuler.length; j++) {
            menuler[j].classList.remove("aktif");
        }
        this.classList.add("aktif");
    });
}
</script>

24. Bir butona tıklanınca bir kutunun mevcut genişliğini ekrana yazdırınız.
Çözüm
<div id="kutu" style="width:250px;height:100px;background:gray;"></div>
<button id="btn">Genişliği Öğren</button>
<p id="sonuc"></p>

<script>
document.getElementById("btn").addEventListener("click", function() {
    let kutu = document.getElementById("kutu");
    document.getElementById("sonuc").innerText = "Genişlik: " + kutu.offsetWidth + " px";
});
</script>

25. Bir butona tıklanınca bir butonun yüksekliğini değiştiriniz.
Çözüm
<button id="btn1" style="height:40px;">Normal Buton</button>
<button id="btn2">Yüksekliği Değiştir</button>

<script>
document.getElementById("btn2").addEventListener("click", function() {
    document.getElementById("btn1").style.height = "80px";
});
</script>

26. Fare resmin üzerine gelince resmi büyütüp ayrılınca eski boyuta getiriniz.
Çözüm
<img id="resim" src="https://via.placeholder.com/150" alt="örnek">

<script>
let resim = document.getElementById("resim");

resim.addEventListener("mouseover", function() {
    resim.style.width = "250px";
    resim.style.height = "250px";
});

resim.addEventListener("mouseout", function() {
    resim.style.width = "150px";
    resim.style.height = "150px";
});
</script>

27. Klavyede sağ ok tuşuna basıldığında kutunun genişliğini artırınız.
Çözüm
<div id="kutu" style="width:100px;height:100px;background:purple;"></div>

<script>
document.addEventListener("keydown", function(event) {
    let kutu = document.getElementById("kutu");

    if (event.key === "ArrowRight") {
        kutu.style.width = "200px";
    }
});
</script>

28. Klavyede yukarı ok tuşuna basıldığında kutunun yüksekliğini artırınız.
Çözüm
<div id="kutu" style="width:100px;height:100px;background:teal;"></div>

<script>
document.addEventListener("keydown", function(event) {
    let kutu = document.getElementById("kutu");

    if (event.key === "ArrowUp") {
        kutu.style.height = "200px";
    }
});
</script>

29. Butona tıklanınca yeni bir paragraf oluşturup sayfaya ekleyiniz.
Çözüm
<button id="btn">Paragraf Ekle</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    let p = document.createElement("p");
    p.innerText = "Yeni oluşturulan paragraf";
    document.body.appendChild(p);
});
</script>

30. Butona tıklanınca aynı class’a sahip tüm butonların genişlik ve yüksekliğini değiştiriniz.
Çözüm
<button class="ortakBtn">Buton 1</button>
<button class="ortakBtn">Buton 2</button>
<button class="ortakBtn">Buton 3</button>
<button id="btn">Hepsini Büyüt</button>

<script>
document.getElementById("btn").addEventListener("click", function() {
    let butonlar = document.getElementsByClassName("ortakBtn");

    for (let i = 0; i < butonlar.length; i++) {
        butonlar[i].style.width = "180px";
        butonlar[i].style.height = "60px";
        butonlar[i].style.fontSize = "20px";
    }
});
</script>


Zamanlayıcılar, Window Nesnesi ve Fare Koordinatları
setTimeout() Method
setTimeout(), belirli bir süre sonra bir defa çalışacak fonksiyon tanımlamak için kullanılır.
setTimeout(fonksiyon, milisaniye);

setTimeout(function(){
    alert("3 saniye sonra çalıştı");
}, 3000);

 Fonksiyon Referansı ile Kullanım
function mesaj(){
    console.log("Merhaba");
}

setTimeout(mesaj, 2000);

 Parametre Gönderme
function yaz(metin){
    console.log(metin);
}

setTimeout(yaz, 2000, "Selam");

Timeout İptali
let id = setTimeout(function(){
    alert("Bu mesaj iptal edildi");
}, 3000);

clearTimeout(id);

Uygulama
document.getElementById("btn").addEventListener("click", function(){

    setTimeout(function(){
        document.body.style.background="yellow";
    }, 2000);

});

setInterval() Method
Belirli aralıklarla sürekli çalışan fonksiyon oluşturur.
setInterval(fonksiyon, milisaniye);
Örnek
setInterval(function(){
    console.log("Her 1 saniyede çalışır");
}, 1000);
Sayaç Uygulaması
let sayi = 0;

setInterval(function(){
    sayi++;
    console.log(sayi);
}, 1000);

 Animasyon Örneği
let pos = 0;
let kutu = document.getElementById("kutu");

setInterval(function(){
    pos += 5;
    kutu.style.left = pos + "px";
}, 50);
 clearInterval()
setInterval() ile başlatılan işlemi durdurur.
Örnek
let sayac = 0;

let id = setInterval(function(){
    sayac++;
    console.log(sayac);

    if(sayac == 5){
        clearInterval(id);
    }

}, 1000);
Buton ile Durdurma
let id = setInterval(function(){
    console.log("Çalışıyor...");
}, 1000);

document.getElementById("dur").onclick = function(){
    clearInterval(id);
}

Window Nesnesi Nedir?
JavaScript’te window nesnesi, tarayıcı penceresini temsil eder.
Tarayıcıda çalışan tüm JavaScript kodları aslında window içinde çalışır.
Örnek:
console.log(window);

window.open() Metodu
Tanım
Yeni bir tarayıcı penceresi veya sekmesi açar.
 Kullanım
window.open(url, name, specs);

 Örnek 1 – Basit kullanım
window.open("https://www.google.com");
Örnek 2 – Özellikli pencere
window.open(
  "https://www.google.com",
  "_blank",
  "width=400,height=300"
);
Örnek 3 – Değişkene atama
let yeniPencere = window.open("", "", "width=300,height=200");

yeniPencere.document.write("Merhaba yeni pencere!");

⚠️ ÖNEMLİ
Çoğu tarayıcı popup engelleyici kullanır 
Genelde buton tıklaması gibi kullanıcı etkileşimi gerekir 

window.close() Metodu
Tanım
Açılmış olan pencereyi kapatır.

Kullanım
window.close();

 Örnek
let pencere = window.open("", "", "width=300,height=200");

setTimeout(() => {
    pencere.close();
}, 3000);
👉 3 saniye sonra pencere kapanır

⚠️ ÖNEMLİ
Sadece JavaScript ile açılmış pencereler kapatılabilir 
Ana tarayıcı sekmesi kapatılamaz 

window.moveTo() Metodu
Tanım
Pencereyi ekran üzerinde belirli bir konuma taşır.

Kullanım
window.moveTo(x, y);


Örnek
let pencere = window.open("", "", "width=300,height=200");

pencere.moveTo(100, 100);
👉 Pencere ekranın (100,100) noktasına gider

⚠️ Not
Modern tarayıcılar güvenlik nedeniyle bazen çalıştırmaz 
Kullanıcı izni gerekebilir 

window.resizeTo() Metodu
Tanım
Pencerenin boyutunu değiştirir.

Kullanım
window.resizeTo(width, height);

Örnek
let pencere = window.open("", "", "width=300,height=200");

pencere.resizeTo(600, 400);
👉 Pencere boyutu değişir

Tüm Metotların Birlikte Kullanımı
<button onclick="pencereAc()">Pencere Aç</button>
<button onclick="pencereKapat()">Pencere Kapat</button>
<button onclick="tasima()">Taşı</button>
<button onclick="boyut()">Boyutlandır</button>

<script>
let pencere;

function pencereAc() {
    pencere = window.open("", "", "width=300,height=200");
}

function pencereKapat() {
    pencere.close();
}

function tasima() {
    pencere.moveTo(200, 200);
}

function boyut() {
    pencere.resizeTo(500, 500);
}
</script>

Özellik Parametreleri (specs)
window.open(
  "sayfa.html",
  "_blank",
  "width=400,height=300,left=100,top=100"
);
Kullanılabilen özellikler:
width 
height 
left 
top 
resizable 
scrollbars 

Güvenlik ve Kısıtlamalar
Modern tarayıcılar:
Popup engelleyici kullanır 
moveTo ve resizeTo kısıtlıdır 
close sadece script ile açılan pencereyi kapatır 

En Sık Kullanım Senaryoları
✔ Popup pencere açma
✔ Login ekranı açma
✔ Yardım penceresi
✔ Reklam / bildirim
✔ Özel boyutlu sayfa açma

Fare Koordinatları

Mouse Event ile Koordinat Alma
document.addEventListener("mousemove", function(e){

    console.log("ClientX:", e.clientX);
    console.log("ClientY:", e.clientY);

});

ClientX ve ClientY
Tanım
Fare imlecinin görünen pencere içindeki konumu

Özellik
Scroll’dan etkilenir 
Tarayıcıya göre ölçülür 

12. ScreenX ve ScreenY
Fare imlecinin ekran üzerindeki konumu
Örnek
document.addEventListener("mousemove", function(e){

    console.log("ScreenX:", e.screenX);
    console.log("ScreenY:", e.screenY);

});

Uygulama
<p id="bilgi"></p>

<script>
document.addEventListener("mousemove", function(e){

document.getElementById("bilgi").innerText =
"ClientX: " + e.clientX +
" ScreenX: " + e.screenX;

});
</script>

Uygulama — Mouse Takip
<div id="top" style="width:20px;height:20px;background:red;position:absolute;"></div>

<script>
document.addEventListener("mousemove", function(e){

let top = document.getElementById("top");

top.style.left = e.clientX + "px";
top.style.top = e.clientY + "px";

});
</script>


JAVASCRIPT FONKSİYONLAR

1. Fonksiyon Nedir?
Fonksiyon, belirli bir işi gerçekleştirmek için yazılmış, tekrar kullanılabilir kod bloklarıdır.
Kod tekrarını önler
Programı daha düzenli hale getirir
Modüler programlama sağlar

2. Fonksiyon Tanımlama (Function Declaration)
function selamVer() {
    console.log("Merhaba Dünya");
}

Fonksiyonu çağırma:
selamVer();

3. Parametreli Fonksiyonlar
Fonksiyonlara dışarıdan veri gönderilebilir.
function selamVer(isim) {
    console.log("Merhaba " + isim);
}

selamVer("Ahmet");

4. Return Kullanımı (Değer Döndürme)
function topla(a, b) {
    return a + b;
}

let sonuc = topla(5, 3);
console.log(sonuc); // 8

return:
Fonksiyonu sonlandırır
Değer geri döndürür

5. Varsayılan Parametreler (Default Parameters)
function selam(isim = "Misafir") {
    console.log("Merhaba " + isim);
}

selam(); // Merhaba Misafir

6. Function Expression (Fonksiyon İfadesi)
Fonksiyon bir değişkene atanabilir.
let topla = function(a, b) {
    return a + b;
};

console.log(topla(4, 6));

7. Arrow Function (Ok Fonksiyonu)
ES6 ile gelen kısa yazım şeklidir.
const topla = (a, b) => {
    return a + b;
};

Kısa kullanım:
const topla = (a, b) => a + b;

Tek parametre:
const kare = x => x * x;

8. Anonim Fonksiyon (Anonymous Function)
İsimsiz fonksiyonlardır.
setTimeout(function() {
    console.log("3 saniye geçti");
}, 3000);

9. Immediately Invoked Function Expression (IIFE)
Tanımlandığı anda çalışan fonksiyon:
(function() {
    console.log("Hemen çalıştı!");
})();

10. Recursive Fonksiyon (Özyineleme)
Fonksiyonun kendini çağırmasıdır.
function faktoriyel(n) {
    if (n === 1) return 1;
    return n * faktoriyel(n - 1);
}

console.log(faktoriyel(5)); // 120

11. Callback Fonksiyon
Bir fonksiyona parametre olarak verilen fonksiyondur.
function islemYap(a, b, callback) {
    return callback(a, b);
}

function topla(a, b) {
    return a + b;
}

console.log(islemYap(3, 4, topla));

12. Rest Parametre (...args)
Sınırsız sayıda parametre almak için kullanılır.
function topla(...sayilar) {
    let toplam = 0;
    for (let s of sayilar) {
        toplam += s;
    }
    return toplam;
}

console.log(topla(1,2,3,4));

Spread Operatörü (...)
Dizi elemanlarını ayırarak gönderir.
let sayilar = [1,2,3];
console.log(topla(...sayilar));

13. Hoisting (Yukarı Taşıma)
Fonksiyonlar tanımlanmadan önce çağrılabilir:
selam();

function selam() {
    console.log("Merhaba");
}

❗ Ama function expression için geçerli değildir.
selam();  // ❌ HATA

let selam = function() {
    console.log("Merhaba");
};

❌ Bu kod hata verir!
NEDEN HATA VERİR?
Çünkü burada:
let selam = function() { ... }
➡️ Bu bir fonksiyon tanımı değil,
➡️ Bu bir değişkene fonksiyon atamadır.

JavaScript bunu şöyle yorumlar:
let selam;   // hoisting olur (ama değeri yok)
selam();     // ❌ undefined çağrılıyor → hata
selam = function() {
    console.log("Merhaba");
};

Yani:
sadece değişken tanımı yukarı taşınır
fonksiyon içeriği taşınmaz

14. this Anahtar Kelimesi
Bulunduğu bağlama göre değişir.
let kisi = {
    isim: "Ali",
    selam: function() {
        console.log(this.isim);
    }
};

kisi.selam();

15. arguments Nesnesi
Fonksiyona gelen tüm parametreleri tutar.
function yazdir() {
    console.log(arguments);
}

yazdir(1,2,3);


JAVASCRIPT ASYNC / AWAIT

ASENKRON (ASYNC) NEDİR?
JavaScript'te bazı işlemler zaman alır:
API'den veri çekme
dosya okuma
zamanlayıcılar (setTimeout)
veritabanı işlemleri
Bu tür işlemlere asenkron işlemler denir.

Senkron vs Asenkron
Senkron (sırayla çalışır)
console.log("1");
console.log("2");
console.log("3");

Çıktı:
1 2 3

Asenkron örnek
console.log("1");

setTimeout(() => {
    console.log("2");
}, 2000);

console.log("3");

Çıktı:
1 3 2
👉 Çünkü setTimeout bekler

PROBLEM: CALLBACK HELL
setTimeout(() => {
    console.log("1");

    setTimeout(() => {
        console.log("2");

        setTimeout(() => {
            console.log("3");
        }, 1000);

    }, 1000);

}, 1000);

😵 Kod karmaşık hale gelir → callback hell

ÇÖZÜM: PROMISE
Promise, asenkron işlemleri daha düzenli yönetmek için kullanılır.
let soz = new Promise(function(resolve, reject) {
    resolve("Başarılı");
});

Promise kullanımı
soz.then(function(sonuc) {
    console.log(sonuc);
});

ASYNC / AWAIT NEDİR?
👉 Promise'leri daha okunabilir ve basit hale getiren yapıdır.
👉 Kod senkron gibi görünür ama asenkron çalışır

ASYNC NEDİR?
Bir fonksiyonun başına async yazarsak:
async function test() {
    return "Merhaba";
}

Bu fonksiyon aslında Promise döndürür.

Örnek
async function test() {
    return "Merhaba";
}

test().then(console.log);
Çıktı: Merhaba

AWAIT NEDİR?
👉 await, Promise'in sonucunu bekler

Örnek
function bekle() {
    return new Promise(resolve => {
        setTimeout(() => {
            resolve("Bitti");
        }, 2000);
    });
}

await ile kullanım
async function calistir() {
    let sonuc = await bekle();
    console.log(sonuc);
}

calistir();

ÖNEMLİ KURAL
👉 await sadece async fonksiyon içinde kullanılır
❌ Hatalı:
let sonuc = await bekle(); // ❌
✔ Doğru:
async function test() {
    let sonuc = await bekle();
}

FETCH + ASYNC / AWAIT

Promise ile
fetch("ogrenciler.json")
    .then(res => res.json())
    .then(data => {
        console.log(data);
    });

async / await ile
async function veriGetir() {
    let res = await fetch("ogrenciler.json");
    let data = await res.json();

    console.log(data);
}

veriGetir();
👉 Daha sade ve okunabilir ✔

ADIM ADIM FETCH AÇIKLAMASI
let res = await fetch("ogrenciler.json");
➡️ dosya çekilir

let data = await res.json();
➡️ JSON → JS object dönüşür

console.log(data);
➡️ veri kullanılır

HATA YAKALAMA (TRY...CATCH)
Asenkron işlemlerde hata olabilir.

Örnek
async function veriGetir() {
    try {
        let res = await fetch("ogrenciler.json");
        let data = await res.json();

        console.log(data);
    } catch (hata) {
        console.log("Hata oluştu:", hata);
    }
}

veriGetir();

GERÇEK HAYAT ÖRNEĞİ
async function kullaniciGetir() {
    try {
        let res = await fetch("https://jsonplaceholder.typicode.com/users");
        let data = await res.json();

        console.log(data);
    } catch (err) {
        console.log("Hata:", err);
    }
}

kullaniciGetir();


JSON NEDİR?
JSON, İngilizce açılımıyla JavaScript Object Notation ifadesinin kısaltmasıdır.
Türkçe olarak söyleyecek olursak JSON, veri saklamak ve verileri taşımak için kullanılan metin tabanlı bir formattır.

JSON'un en önemli amacı şudur:
Verileri düzenli bir biçimde tutmak
Bir yerden başka bir yere veri göndermek
Özellikle web uygulamalarında sunucu ile tarayıcı arasında veri alışverişi yapmak

JSON, adı JavaScript ile başlasa da sadece JavaScript'te değil; Python, Java, C#, PHP ve daha birçok programlama dilinde kullanılabilir.

JSON NEDEN KULLANILIR?
Bir web sitesi düşünelim. Kullanıcı giriş yapıyor. Sunucudan şu bilgiler gelsin:
ad, soyad, yaş, şehir
Bu bilgiler bir düzende gönderilmelidir. İşte JSON bu düzeni sağlar.

Örneğin bir kullanıcı verisi şöyle taşınabilir:
{
  "ad": "Ali",
  "soyad": "Yılmaz",
  "yas": 20,
  "sehir": "Ankara"
}

Bu yapı hem insanlar tarafından okunabilir hem de programlar tarafından kolayca işlenebilir.

JSON İLE JAVASCRIPT NESNESİ AYNI ŞEY Mİ?
Kısa cevap: Hayır, birebir aynı şey değildir.

JavaScript Nesnesi
let ogrenci = {
    ad: "Ayşe",
    soyad: "Kara",
    yas: 21
};

Dikkat edilirse:
anahtarlar tırnaksız yazılmıştır
bu yapı JavaScript içinde kullanılabilir

JSON Yapısı
{
  "ad": "Ayşe",
  "soyad": "Kara",
  "yas": 21
}

Dikkat edilirse:
anahtarlar mutlaka çift tırnak içinde yazılır
JSON aslında bir metin veri formatıdır

JSON'UN TEMEL YAPISI
JSON, anahtar-değer (key-value) mantığıyla çalışır.
{
  "isim": "Mehmet",
  "yas": 25
}
Burada:
"isim" → anahtar
"Mehmet" → değer

JSON YAZIM KURALLARI
Anahtarlar çift tırnak içinde olmalıdır
String ifadeler çift tırnak içinde yazılır
Sayılar tırnaksız yazılır
true, false ve null kullanılabilir
Son elemandan sonra virgül konulmaz
JSON içinde yorum satırı olmaz

JSON HANGİ VERİ TÜRLERİNİ DESTEKLER?
String, Number, Object, Array, Boolean, null

JSON İÇİNDE KULLANILAMAYAN ŞEYLER
fonksiyonlar
undefined
yorum satırları
tek tırnaklı yapı
Date nesnesi doğrudan özel biçimiyle

JSON.STRINGIFY() NEDİR?
JSON.stringify() metodu, bir JavaScript nesnesini JSON metnine çevirir.
JavaScript object → JSON string

Basit örnek
let ogrenci = {
    ad: "Ali",
    yas: 20,
    bolum: "Bilgisayar Mühendisliği"
};

let jsonVeri = JSON.stringify(ogrenci);

console.log(jsonVeri);
console.log(typeof jsonVeri);

Çıktı:
{"ad":"Ali","yas":20,"bolum":"Bilgisayar Mühendisliği"}
string

Neden stringify kullanılır?
Çünkü çoğu zaman:
veriyi dosyaya yazmak için
localStorage'a kaydetmek için
sunucuya göndermek için
veriyi string haline getirmek gerekir.

JSON.PARSE() NEDİR?
JSON.parse() metodu, JSON formatındaki bir metni alır ve JavaScript nesnesine dönüştürür.
JSON string → JavaScript object

Örnek
let jsonMetin = '{"ad":"Mehmet","yas":23}';

let nesne = JSON.parse(jsonMetin);

console.log(nesne);
console.log(typeof nesne);
console.log(nesne.ad);

Çıktı:
{ad: "Mehmet", yas: 23}
object
Mehmet

STRINGIFY VE PARSE MANTIĞI
Başlangıçta:
let kisi = {
    ad: "Ali",
    yas: 30
};
Bu bir JavaScript nesnesidir.

String'e dönüştürme:
let metin = JSON.stringify(kisi);
Artık metin, JSON biçiminde bir yazıdır.

Tekrar nesneye çevirme:
let yeniKisi = JSON.parse(metin);
Böylece tekrar JavaScript nesnesi elde edilir.

JSON İÇİNDE DİZİLER
Nesne içinde dizi
{
  "renkler": ["kırmızı", "mavi", "yeşil"]
}

Dizinin içindeki nesneler
[
  {
    "ad": "Ali",
    "yas": 20
  },
  {
    "ad": "Ayşe",
    "yas": 21
  }
]
Bu yapı özellikle API'lerde çok görülür.

JSON DOSYASI NEDİR?
JSON verileri bazen .json uzantılı dosyalarda saklanır.
Örnek bir ogrenci.json dosyası:
{
  "ad": "Can",
  "soyad": "Demir",
  "yas": 22,
  "dersler": ["Matematik", "Programlama", "Veri Tabanı"]
}

JSON.STRINGIFY() İLE BİÇİMLİ YAZDIRMA
JSON.stringify(deger, replacer, space)
deger → dönüştürülecek veri
replacer → hangi alanların dahil edileceği
space → girinti miktarı

Güzel formatlı çıktı
let kisi = {
    ad: "Ali",
    yas: 25,
    sehir: "İstanbul"
};

let sonuc = JSON.stringify(kisi, null, 4);
console.log(sonuc);

REPLACER KULLANIMI
let kullanici = {
    ad: "Ahmet",
    sifre: "12345",
    rol: "admin"
};

let sonuc = JSON.stringify(kullanici, ["ad", "rol"], 2);
console.log(sonuc);
Burada sifre alanı dışarıda bırakılmıştır.

JSON.PARSE() HATA VEREBİLİR Mİ?
Evet. Eğer JSON metni hatalıysa JSON.parse() hata verir.

try...catch ile güvenli kullanım
let veri = '{"ad":"Ali","yas":20}';

try {
    let kisi = JSON.parse(veri);
    console.log(kisi.ad);
} catch (hata) {
    console.log("JSON verisi hatalı");
}

API'LERDE JSON KULLANIMI
Modern web uygulamalarında API'lerden gelen verilerin büyük kısmı JSON formatındadır.

Temel mantık:
fetch("ornek-url")
    .then(response => response.json())
    .then(veri => {
        console.log(veri);
    });
Buradaki response.json() işlemi, gelen JSON verisini JavaScript nesnesine çevirir.

JSON ÜZERİNDE DÖNGÜ İLE GEZME
let veri = `{
  "urunler": [
    {"ad":"Defter","fiyat":50},
    {"ad":"Kalem","fiyat":15},
    {"ad":"Silgi","fiyat":10}
  ]
}`;

let nesne = JSON.parse(veri);

for (let urun of nesne.urunler) {
    console.log(urun.ad, urun.fiyat);
}

UYGULAMA ÖRNEĞİ: KULLANICI BİLGİSİNİ SAKLAMA
let kullanici = {
    ad: "Selin",
    sifre: "1234",
    email: "selin@mail.com"
};

localStorage.setItem("kullanici", JSON.stringify(kullanici));

let gelenVeri = localStorage.getItem("kullanici");
let nesne = JSON.parse(gelenVeri);

console.log(nesne.ad);
console.log(nesne.email);


JAVASCRIPT'TE LOCAL STORAGE VE SESSION STORAGE

Web Storage Nedir?
JavaScript'te bazen kullanıcıyla ilgili bazı bilgileri tarayıcıda saklamak isteriz.
Örneğin:
kullanıcının tema seçimi
formda yazdığı geçici bilgiler
giriş yapmış kullanıcı adı
sepete eklenen ürünler
dil tercihi
son ziyaret bilgisi

Eskiden bu tür işlemler çoğunlukla cookie ile yapılırdı. Daha sonra tarayıcılar, veri saklamak için daha kolay bir yapı sundu:
localStorage
sessionStorage

Bu iki yapı birlikte genel olarak Web Storage API olarak adlandırılır.

Local Storage ve Session Storage Arasındaki Temel Fark
En önemli fark, verinin ne kadar süre saklandığıdır.

Local Storage
veri uzun süre saklanır
tarayıcı kapatılsa bile silinmez
kullanıcı veya kod silene kadar durur

Session Storage
veri sadece açık sekme/oturum boyunca saklanır
sekme kapatılınca silinir
tarayıcıda yeni sekme açılırsa genelde yeni bir oturum olur

Nasıl Kullanılır?
En temel metodlar:
setItem() → veri kaydetmek
getItem() → veri okumak
removeItem() → belirli bir veriyi silmek
clear() → tüm verileri silmek
key() → belirli sıradaki anahtarı almak

Local Storage Kullanımı

Veri kaydetme
localStorage.setItem("isim", "Ali");

Veri okuma
let isim = localStorage.getItem("isim");
console.log(isim);

Veri silme
localStorage.removeItem("isim");

Tüm verileri silme
localStorage.clear();

Session Storage Kullanımı

Veri kaydetme
sessionStorage.setItem("sehir", "Ankara");

Veri okuma
let sehir = sessionStorage.getItem("sehir");
console.log(sehir);

Veri silme
sessionStorage.removeItem("sehir");

Tüm session verilerini silme
sessionStorage.clear();

Çok Önemli Nokta: Veriler String Olarak Saklanır
Hem localStorage hem sessionStorage verileri string (metin) olarak saklar.

Örnek:
localStorage.setItem("yas", 25);
console.log(localStorage.getItem("yas"));
console.log(typeof localStorage.getItem("yas"));
Çıktı:
25
string
Yani sayı bile kaydedilse, geri alınırken string gelir.

Sayısal Veri Saklama
localStorage.setItem("puan", 90);

let puan = Number(localStorage.getItem("puan"));
console.log(puan + 10);

Boolean Veri Saklama
localStorage.setItem("girisYapti", true);

let durum = localStorage.getItem("girisYapti");
Kontrol ederken dikkat etmek gerekir:
if (localStorage.getItem("girisYapti") === "true") {
    console.log("Kullanıcı giriş yapmış");
}

Nesne ve Dizi Saklama
Storage içinde doğrudan nesne veya dizi saklanmaz. Çünkü bunlar string olarak tutulur.
Bu yüzden JSON.stringify() ve JSON.parse() kullanılır.

Nesne kaydetme
let kullanici = {
    ad: "Ayşe",
    yas: 22,
    sehir: "İzmir"
};

localStorage.setItem("kullanici", JSON.stringify(kullanici));

Nesneyi geri alma
let veri = localStorage.getItem("kullanici");
let kullanici = JSON.parse(veri);

console.log(kullanici.ad);
console.log(kullanici.sehir);

Dizi kaydetme
let renkler = ["kırmızı", "mavi", "yeşil"];
localStorage.setItem("renkler", JSON.stringify(renkler));

Diziyi geri alma
let renkler = JSON.parse(localStorage.getItem("renkler"));
console.log(renkler[1]);

Nesne Güncelleme Nasıl Yapılır?
Storage içindeki veri doğrudan değiştirilemez. Önce alınır, sonra değiştirilir, sonra tekrar kaydedilir.
let kullanici = JSON.parse(localStorage.getItem("kullanici"));
kullanici.sehir = "Bursa";
localStorage.setItem("kullanici", JSON.stringify(kullanici));

Veri Var mı Yok mu Kontrolü
let veri = localStorage.getItem("tema");

if (veri !== null) {
    console.log("Veri bulundu:", veri);
} else {
    console.log("Veri yok");
}
Bu kontrol çok önemlidir. Çünkü getItem() veri yoksa null döndürür.

Tüm Anahtarları Görme
for (let i = 0; i < localStorage.length; i++) {
    let anahtar = localStorage.key(i);
    console.log(anahtar, localStorage.getItem(anahtar));
}

Cookie ile Farkı Nedir?
Cookie: her istekle sunucuya gönderilebilir, küçük boyutludur
localStorage / sessionStorage: yalnızca tarayıcı tarafında tutulur, sunucuya otomatik gönderilmez, daha büyük veri saklayabilir

Tema Kaydetme Uygulaması

HTML
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>Tema Seçimi</title>
</head>
<body>
    <h1>Tema Seçimi Uygulaması</h1>
    <button onclick="temaDegistir('light')">Açık Tema</button>
    <button onclick="temaDegistir('dark')">Koyu Tema</button>
    <script src="script.js"></script>
</body>
</html>

JavaScript
function temaDegistir(tema) {
    document.body.className = tema;
    localStorage.setItem("tema", tema);
}

window.onload = function() {
    let kayitliTema = localStorage.getItem("tema");
    if (kayitliTema) {
        document.body.className = kayitliTema;
    } else {
        document.body.className = "light";
    }
};

Ziyaret Sayısı Tutma
let sayi = localStorage.getItem("ziyaret");

if (sayi === null) {
    sayi = 1;
} else {
    sayi = Number(sayi) + 1;
}

localStorage.setItem("ziyaret", sayi);
console.log("Siteyi ziyaret sayınız:", sayi);

Formu Otomatik Kaydetme
let kutu = document.getElementById("mesaj");

kutu.addEventListener("input", function() {
    localStorage.setItem("mesaj", kutu.value);
});

window.onload = function() {
    let kayitliMesaj = localStorage.getItem("mesaj");
    if (kayitliMesaj) {
        kutu.value = kayitliMesaj;
    }
};

Çok Sık Yapılan Hatalar

Hata 1: Nesneyi doğrudan kaydetmek
let kisi = { ad: "Ali" };
localStorage.setItem("kisi", kisi);
Bu yanlış sonuç verir. Çünkü [object Object] olarak kaydedilir.
Doğrusu:
localStorage.setItem("kisi", JSON.stringify(kisi));

Hata 2: JSON.parse unutmak
let veri = localStorage.getItem("kisi");
console.log(veri.ad);
Bu çalışmaz çünkü veri string'dir.
Doğrusu:
let veri = JSON.parse(localStorage.getItem("kisi"));
console.log(veri.ad);

Hata 3: Sayıyı doğrudan sayı sanmak
let puan = localStorage.getItem("puan");
console.log(puan + 10);
Eğer puan = "90" ise sonuç "9010" olabilir.
Doğrusu:
let puan = Number(localStorage.getItem("puan"));
console.log(puan + 10);

Güvenlik Uyarısı
localStorage ve sessionStorage güvenli veri deposu değildir.
Şunlar burada tutulmamalıdır:
kullanıcı şifresi
çok hassas kişisel veriler
kritik güvenlik bilgileri
Çünkü tarayıcıdaki JavaScript kodu bunlara erişebilir.

Kısa Karar Rehberi
"Bu veri uzun süre mi kalsın, sadece bu sekmede mi yaşasın?"
Uzun süre kalsın → localStorage
Sadece oturum boyunca kalsın → sessionStorage

---

# Bootstrap 5 — Front-end Framework

**Kaynak:** `pdf/Bootstrap.pdf`, s. 1-9

## 1. Bootstrap Nedir?

Bootstrap, daha hızlı ve daha kolay web geliştirme için ücretsiz bir ön uç (front-end) çerçevesidir. Form, düğme, tablo, gezinme menüsü, modal, görüntü karuseli gibi pek çok bileşen için HTML/CSS tabanlı tasarım şablonları ve isteğe bağlı JavaScript eklentileri içerir. Responsive (duyarlı) tasarımı kolaylaştırır.

## 2. CDN ile Kurulum

Bootstrap 5'i kendin barındırmak istemiyorsan jsDelivr CDN üzerinden ekleyebilirsin:

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5/dist/js/bootstrap.bundle.min.js"></script>
```

CSS başlık (`<head>`) içine, JS bundle ise sayfanın en altına yerleştirilir.

## 3. HTML5 Doctype ve Viewport

Bootstrap 5, HTML5 belge türünü ve mobil önceliklilik gerektirir.

```html
<!DOCTYPE html>
<html lang="tr">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Bootstrap Sayfa</title>
  </head>
</html>
```

- `width=device-width`: sayfa genişliğini cihaz ekran genişliğine eşitler.
- `initial-scale=1`: ilk yakınlaştırma seviyesini ayarlar.

## 4. Konteynerler

Bootstrap içeriği bir kapsayıcıyla sarmalı bekler:

- `.container`: duyarlı, sabit max-width'li kapsayıcı.
- `.container-fluid`: görünümün tüm genişliğini kaplayan kapsayıcı.

Responsive (breakpoint'e göre devreye giren) varyantlar:

```html
<div class="container-sm">.container-sm</div>
<div class="container-md">.container-md</div>
<div class="container-lg">.container-lg</div>
<div class="container-xl">.container-xl</div>
<div class="container-xxl">.container-xxl</div>
```

Padding/margin yardımcıları sıkça birlikte kullanılır: `p-5` (her tarafa padding), `my-5` (üst/alt margin), `pt-5` (üst padding), vb.

```html
<div class="container p-5 my-5 border"></div>
<div class="container p-5 my-5 bg-dark text-white"></div>
<div class="container p-5 my-5 bg-primary text-white"></div>
```

## 5. Grid Sistem (12 Sütun)

Bootstrap'in ızgara sistemi flexbox tabanlıdır ve her satır 12 sütuna kadar bölünebilir. Sınıflar ekran genişliğine göre devreye girer:

| Sınıf       | Devreye girdiği genişlik       |
| ----------- | ------------------------------ |
| `.col-`     | tüm cihazlar (en küçük)        |
| `.col-sm-`  | ≥ 576 piksel (küçük)           |
| `.col-md-`  | ≥ 768 piksel (orta)            |
| `.col-lg-`  | ≥ 992 piksel (büyük)           |
| `.col-xl-`  | ≥ 1200 piksel (xlarge)         |
| `.col-xxl-` | ≥ 1400 piksel (xxlarge)        |

Üç eşit sütun:

```html
<div class="row">
  <div class="col">.col</div>
  <div class="col">.col</div>
  <div class="col">.col</div>
</div>
```

Tabletten itibaren dört eşit sütun (mobilde üst üste yığılır):

```html
<div class="row">
  <div class="col-sm-3">.col-sm-3</div>
  <div class="col-sm-3">.col-sm-3</div>
  <div class="col-sm-3">.col-sm-3</div>
  <div class="col-sm-3">.col-sm-3</div>
</div>
```

İki eşit olmayan sütun:

```html
<div class="row">
  <div class="col-sm-4">.col-sm-4</div>
  <div class="col-sm-8">.col-sm-8</div>
</div>
```

## 6. Renkler

Metin renkleri için bağlamsal sınıflar:

```text
.text-muted, .text-primary, .text-success, .text-info, .text-warning,
.text-danger, .text-secondary, .text-white, .text-dark, .text-body, .text-light
```

Arka plan renkleri:

```text
.bg-primary, .bg-success, .bg-info, .bg-warning, .bg-danger,
.bg-secondary, .bg-dark, .bg-light
```

`.bg-*` metin rengini ayarlamaz; gerektiğinde `.text-white` veya `.text-dark` ile birlikte kullan.

## 7. Tablolar

Temel sınıflar tabloya farklı stiller verir:

- `.table`: temel stil.
- `.table-striped`: zebra şeritleri.
- `.table-bordered`: tüm tarafa kenarlık.
- `.table-hover`: satır üzerine gelince vurgu.
- `.table-dark`: koyu arka plan.
- `.table-borderless`: kenarlıkları kaldırır.

Birleştirilebilir: `.table .table-dark .table-striped`.

Responsive tablo:

```html
<div class="table-responsive">
  <table class="table">…</table>
</div>
```

Belirli breakpoint'lerde scroll çubuğu istemek için: `.table-responsive-sm/md/lg/xl/xxl`.

## 8. Görseller

Şekil sınıfları:

- `.rounded`: köşeleri yuvarlat.
- `.rounded-circle`: tamamen daire (kare görseller için ideal).
- `.img-thumbnail`: küçük çerçeveli minik görsel.

Hizalama:

```html
<img src="paris.jpg" class="float-start">  <!-- sola -->
<img src="paris.jpg" class="float-end">    <!-- sağa -->
<img src="paris.jpg" class="mx-auto d-block">  <!-- ortala -->
```

`.mx-auto` `margin: auto`, `.d-block` `display: block` etkisi verir.

## 9. Butonlar

Tek bir `.btn` sınıfı butonun temel görünümünü verir; renk varyantları yanına eklenir:

```html
<button class="btn btn-primary">Primary</button>
<button class="btn btn-secondary">Secondary</button>
<button class="btn btn-success">Success</button>
<button class="btn btn-info">Info</button>
<button class="btn btn-warning">Warning</button>
<button class="btn btn-danger">Danger</button>
<button class="btn btn-dark">Dark</button>
<button class="btn btn-light">Light</button>
<button class="btn btn-link">Link</button>
```

## 10. İlerleme Çubukları (Progress Bar)

```html
<div class="progress">
  <div class="progress-bar" style="width:70%">70%</div>
</div>
```

Çoklu segment:

```html
<div class="progress">
  <div class="progress-bar bg-success" style="width:40%">Free</div>
  <div class="progress-bar bg-warning" style="width:10%">Warn</div>
  <div class="progress-bar bg-danger"  style="width:20%">Danger</div>
</div>
```

## 11. Pratik Hatırlatmalar

- Container → Row → Col hiyerarşisini bozma; `.col` mutlaka bir `.row` içinde olmalı.
- 12 sütun toplamı: bir satırdaki `col-*` değerleri 12'yi aştığında yeni satıra düşer.
- `.text-*` ve `.bg-*` semantik anlam taşır; "primary/success/danger" gibi isimler markanın ana paletine bağlanır.
- Mobil öncelik: önce `.col-` (mobil), sonra büyük breakpoint'lere doğru ekleme yap (`col-12 col-md-6 col-lg-4` gibi).

---

# Node.js ve Express.js Ders Notları

**Kaynak:** `pdf/DERS_NOTLARI-NODE_JS.docx`

Bu bölüm, JavaScript'in tarayıcı dışındaki kullanımını, Node.js ile dosya işlemlerini ve Express.js ile temel web sunucusu/routing mantığını özetler.

## 1. Node.js Nedir?

Node.js, JavaScript kodlarının tarayıcı dışında çalışmasını sağlayan bir çalışma ortamıdır. Frontend tarafında JavaScript tarayıcı içinde DOM, event ve arayüz işlemleriyle ilgilenirken; Node.js tarafında aynı dil backend geliştirme, API oluşturma, dosya işlemleri ve sunucu yazma için kullanılabilir.

Node.js'in temel özellikleri:

- **Event driven yapı:** İşler olaylara göre tetiklenir.
- **Asenkron çalışma:** Dosya okuma, ağ isteği veya zaman alan işlemler beklenirken program tamamen durmak zorunda kalmaz.
- **Hızlı ve pratik geliştirme:** JavaScript bilgisiyle backend tarafına geçiş yapılabilir.

Kullanım alanları:

- REST API geliştirme
- Web sunucusu oluşturma
- Gerçek zamanlı uygulamalar
- Chat uygulamaları
- Blog, e-ticaret ve panel sistemleri

## 2. Kurulum ve İlk Program

Node.js kurulumu resmi siteden yapılır:

```text
https://nodejs.org
```

Kurulumdan sonra terminalde sürüm kontrolü:

```bash
node -v
npm -v
```

İlk program için `app.js` dosyası:

```js
console.log("Merhaba Node.js");
```

Çalıştırma:

```bash
node app.js
```

## 3. JavaScript Temel Tekrarı

Node.js kullanırken tarayıcıdaki JavaScript temelleri yine geçerlidir.

Değişkenler:

```js
let ad = "Ali";
const yas = 20;
```

Temel veri tipleri:

- `String`
- `Number`
- `Boolean`
- Dizi
- Obje

Koşul örneği:

```js
let notu = 70;

if (notu >= 50) {
  console.log("Geçti");
} else {
  console.log("Kaldı");
}
```

Döngü:

```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

Fonksiyon:

```js
function topla(a, b) {
  return a + b;
}
```

Arrow function:

```js
const kare = x => x * x;
```

Dizi ve obje:

```js
let sayilar = [10, 20, 30];

let ogrenci = {
  ad: "Ali",
  yas: 20
};
```

## 4. CommonJS Modül Sistemi

Node.js'te kodları ayrı dosyalara bölmek için modül sistemi kullanılır. Bir dosyadaki fonksiyon başka dosyada kullanılacaksa `module.exports` ile dışa aktarılır, `require()` ile içe alınır.

Tek fonksiyon dışa aktarma:

```js
// math.js
function topla(a, b) {
  return a + b;
}

module.exports = topla;
```

```js
// app.js
const topla = require("./math");

console.log(topla(3, 4));
```

Birden fazla fonksiyon dışa aktarma:

```js
// math.js
function topla(a, b) {
  return a + b;
}

function cikar(a, b) {
  return a - b;
}

function carp(a, b) {
  return a * b;
}

module.exports = {
  topla,
  cikar,
  carp
};
```

```js
// app.js
const math = require("./math");

console.log(math.topla(10, 5));
console.log(math.cikar(10, 5));
console.log(math.carp(10, 5));
```

## 5. `fs` Modülü ile Dosya İşlemleri

`fs`, Node.js'in dosya sistemi modülüdür. Açılımı **File System**'dır. Dosya oluşturma, okuma, yazma, veri ekleme ve silme işlemleri için kullanılır.

Modülü projeye dahil etme:

```js
const fs = require("fs");
```

### 5.1 Dosyaya Veri Yazma

`writeFileSync()` bir dosyaya veri yazar. Dosya yoksa oluşturur; dosya varsa eski içeriği silip yeni veriyi yazar.

```js
const fs = require("fs");

fs.writeFileSync("not.txt", "Merhaba Dünya");
```

Değişken yazdırma:

```js
let isim = "Ahmet";

fs.writeFileSync("isim.txt", isim);
```

Sayısal veri yazdırırken metne çevirmek güvenlidir:

```js
let sayi = 125;

fs.writeFileSync("sayi.txt", sayi.toString());
```

Birden fazla satır yazdırma:

```js
let ad = "Ali";
let yas = 20;

fs.writeFileSync("ogrenci.txt", `Ad: ${ad}\nYaş: ${yas}`);
```

`\n` alt satıra geçmeyi sağlar.

### 5.2 Döngü İçinde Yazma Hatası

Şu kullanım hatalıdır:

```js
for (let i = 1; i <= 5; i++) {
  fs.writeFileSync("sayilar.txt", i.toString());
}
```

Çünkü `writeFileSync()` her turda dosyayı baştan yazar. Sonuçta dosyada yalnızca son değer kalır.

### 5.3 Dosyaya Veri Ekleme

`appendFileSync()` dosyanın sonuna veri ekler.

```js
const fs = require("fs");

for (let i = 1; i <= 5; i++) {
  fs.appendFileSync("sayilar.txt", i + "\n");
}
```

Çarpım tablosu örneği:

```js
const fs = require("fs");

for (let i = 1; i <= 10; i++) {
  fs.appendFileSync("carpim.txt", `5 x ${i} = ${5 * i}\n`);
}
```

Dizi elemanlarını dosyaya yazma:

```js
const fs = require("fs");

let isimler = ["Ali", "Ayşe", "Mehmet"];

for (let i = 0; i < isimler.length; i++) {
  fs.appendFileSync("isimler.txt", isimler[i] + "\n");
}
```

### 5.4 Dosya Okuma

`readFileSync()` dosya içeriğini okur. Türkçe karakter sorunu yaşamamak için genelde `utf8` kullanılır.

```js
const fs = require("fs");

const data = fs.readFileSync("not.txt", "utf8");
console.log(data);
```

Satır satır ayırma:

```js
const data = fs.readFileSync("sayilar.txt", "utf8");
const satirlar = data.split("\n");

console.log(satirlar);
```

### 5.5 Dosya Var mı Kontrolü ve Silme

```js
const fs = require("fs");

if (fs.existsSync("not.txt")) {
  console.log("Dosya var");
} else {
  console.log("Dosya yok");
}
```

Dosya silme:

```js
fs.unlinkSync("not.txt");
```

## 6. JSON Dosyasına Yazma ve Okuma

JavaScript objesi doğrudan dosyaya yazılırsa düzgün bir JSON metni oluşmaz. Bu yüzden `JSON.stringify()` ile metne çevrilir.

```js
const fs = require("fs");

let user = {
  ad: "Ali",
  yas: 20
};

fs.writeFileSync("user.json", JSON.stringify(user));
```

JSON dosyasını okurken önce metin okunur, sonra `JSON.parse()` ile objeye dönüştürülür.

```js
const fs = require("fs");

const data = fs.readFileSync("user.json", "utf8");
const user = JSON.parse(data);

console.log(user.ad);
```

Öğrenci bilgilerini dosyaya ekleme:

```js
const fs = require("fs");

let ogrenciler = [
  { ad: "Ali", not: 90 },
  { ad: "Ayşe", not: 85 }
];

for (let i = 0; i < ogrenciler.length; i++) {
  fs.appendFileSync(
    "ogrenciler.txt",
    `Ad: ${ogrenciler[i].ad}\nNot: ${ogrenciler[i].not}\n`
  );
}
```

## 7. Express.js Nedir?

Express.js, Node.js üzerinde web sunucusu ve route yönetimi kurmayı kolaylaştıran bir framework'tür.

Kurulum:

```bash
npm install express
```

İlk Express uygulaması:

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Ana Sayfa");
});

app.get("/iletisim", (req, res) => {
  res.send("İletişim");
});

app.listen(3000, () => {
  console.log("Sunucu çalıştı");
});
```

Bu örnekte:

- `express()` uygulama nesnesini oluşturur.
- `app.get()` belirli bir adrese gelen GET isteğini yakalar.
- `req` istek bilgisini, `res` cevap gönderme işlemlerini temsil eder.
- `app.listen(3000)` sunucuyu 3000 portunda başlatır.

## 8. Express Routing ve Statik Dosyalar

Örnek proje yapısı:

```text
proje/
├── app.js
├── package.json
├── public/
│   └── style.css
└── views/
    ├── index.html
    ├── iletisim.html
    ├── urunler.html
    └── referanslar.html
```

`app.js`:

```js
const express = require("express");
const path = require("path");
const app = express();

app.use(express.static("public"));

app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "views", "index.html"));
});

app.get("/iletisim", (req, res) => {
  res.sendFile(path.join(__dirname, "views", "iletisim.html"));
});

app.get("/urunlerimiz", (req, res) => {
  res.sendFile(path.join(__dirname, "views", "urunler.html"));
});

app.get("/referanslarimiz", (req, res) => {
  res.sendFile(path.join(__dirname, "views", "referanslar.html"));
});

app.use((req, res) => {
  res.status(404).send("<h1>404 Sayfa Bulunamadı</h1>");
});

app.listen(3000, () => {
  console.log("Sunucu çalıştı");
});
```

`public/style.css` örneği:

```css
body {
  margin: 0;
  font-family: Arial;
  background: #f4f4f4;
}

header {
  background: #222;
  padding: 20px;
}

nav {
  text-align: center;
}

nav a {
  color: white;
  text-decoration: none;
  margin: 20px;
  font-size: 20px;
}

nav a:hover {
  color: orange;
}

.container {
  width: 80%;
  margin: auto;
  background: white;
  padding: 30px;
  margin-top: 30px;
  border-radius: 10px;
}
```

HTML sayfaları `views/` altında tutulur. `sendFile()` ile ilgili HTML dosyası kullanıcıya gönderilir. CSS gibi ortak dosyalar `public/` klasöründen servis edilir.

## 9. Node.js Bölümü İçin Pratik Kontrol Listesi

- Terminalde `node -v` ve `npm -v` çıktısı alınabiliyor mu?
- `app.js` dosyası `node app.js` ile çalışıyor mu?
- `module.exports` ve `require()` ile ayrı dosyadaki fonksiyon kullanılabiliyor mu?
- `writeFileSync()` ile dosya yazmanın eski içeriği sildiği biliniyor mu?
- Döngü içinde veri biriktirmek için `appendFileSync()` kullanılıyor mu?
- `readFileSync(..., "utf8")` ile Türkçe karakterler doğru okunuyor mu?
- JSON dosyası yazarken `JSON.stringify()`, okurken `JSON.parse()` kullanılıyor mu?
- Express sunucusu `localhost:3000` üzerinden açılıyor mu?
- Route bulunamadığında 404 cevabı dönüyor mu?

---

# Final Sınav Odaklı Notlar: Routing, Form, Fonksiyon ve Bootstrap

**Kaynak:** `pdf/9__HAFTA_DERS_NOTLARI_UPDATED.docx`; `pdf/DERS_NOTLARI.docx`; `pdf/DERS_NOTLARI-NODE_JS.docx`; `pdf/Bootstrap.pdf`, s. 1-9

Bu bölüm finalde çıkması beklenen başlıkları hızlı tekrar ve kod yazma mantığıyla toparlar. Ağırlık JavaScript fonksiyonları, web form elemanlarına müdahale, Bootstrap responsive grid ve Node.js / Express routing üzerindedir. Veritabanı doğrudan beklenmiyor; routing mantığını bilmek yeterlidir.

## 1. HTML ve CSS Çıktısı Okuma

HTML/CSS sorularında genellikle verilen kodun ekranda nasıl görüneceği sorulur. Beklenen şey kod yazmaktan çok yapıyı okuyabilmektir.

Dikkat edilecekler:

- Kaç tane `div`, `button`, `input`, `form`, `p` var?
- Elemanlar blok mu, inline mı?
- CSS'te `display: flex`, `grid`, `width`, `height`, `margin`, `padding` verilmiş mi?
- Ekran genişliği 800 px ise medya sorgusu veya Bootstrap breakpoint hangi düzende çalışır?
- Elemanlar yan yana mı, alt alta mı dizilir?

Örnek:

```html
<div class="kutu">1</div>
<div class="kutu">2</div>
<div class="kutu">3</div>
```

```css
.kutu {
  width: 200px;
  height: 100px;
  float: left;
}
```

800 px ekranda her kutu 200 px olduğundan 3 kutu yan yana sığar. Toplam genişlik 600 px olur.

## 2. JavaScript ile Form Elemanlarına Müdahale

Finalde önemli kısım: butona tıklandığında formdaki değeri alıp sayfada bir yere yazdırmak.

Örnek:

```html
<input type="text" id="ad" placeholder="Adınızı girin">
<button id="btn">Gönder</button>
<p id="sonuc"></p>
```

```js
const adInput = document.getElementById("ad");
const btn = document.getElementById("btn");
const sonuc = document.getElementById("sonuc");

btn.addEventListener("click", function () {
  sonuc.innerText = "Girilen ad: " + adInput.value;
});
```

Çalışma sırası:

1. `adInput.value` input içindeki değeri alır.
2. Butona tıklanınca `click` eventi çalışır.
3. `sonuc.innerText` ile değer sayfaya yazdırılır.

Form submit varsa sayfanın yenilenmesini engellemek için `preventDefault()` kullanılır:

```html
<form id="kayitForm">
  <input type="text" id="ad">
  <button type="submit">Kaydet</button>
</form>
<div id="liste"></div>
```

```js
const form = document.getElementById("kayitForm");
const ad = document.getElementById("ad");
const liste = document.getElementById("liste");

form.addEventListener("submit", function (e) {
  e.preventDefault();
  liste.innerText = "Kayıt alındı: " + ad.value;
  ad.value = "";
});
```

## 3. Fonksiyonlar ve Console Çıktısı

Fonksiyon sorularında en çok şu üç şey sorulur:

- Fonksiyon çağrılınca `console.log()` ne yazar?
- Parametre gönderilirse sonuç nasıl değişir?
- Arrow function ile normal function arasında ne fark vardır?

Örnek:

```js
function topla(a, b) {
  console.log(a + b);
}

topla(3, 5);
```

Console çıktısı:

```text
8
```

Parametre eksik gönderilirse:

```js
function carp(a, b) {
  console.log(a * b);
}

carp(4);
```

Console çıktısı:

```text
NaN
```

Çünkü `b` değeri `undefined` olur. `4 * undefined` sonucu `NaN` verir.

Varsayılan parametre:

```js
function carp(a, b = 1) {
  console.log(a * b);
}

carp(4);
```

Console çıktısı:

```text
4
```

Arrow function:

```js
const bol = (a, b) => {
  return a / b;
};

console.log(bol(10, 2));
```

Console çıktısı:

```text
5
```

Tek satır arrow function:

```js
const kare = sayi => sayi * sayi;
console.log(kare(6));
```

Console çıktısı:

```text
36
```

Önemli fark: Normal `function` kendi `this` değerini oluşturabilir. Arrow function kendi `this` değerini oluşturmaz; dış kapsamın `this` değerini kullanır. Bu yüzden event içinde tıklanan butonu `this` ile yakalamak gerekiyorsa normal function daha güvenlidir.

```js
btn.addEventListener("click", function () {
  console.log(this.innerText);
});
```

Bu örnekte `this`, tıklanan butondur.

```js
btn.addEventListener("click", () => {
  console.log(this);
});
```

Bu örnekte `this` butonu temsil etmez; dış kapsamdaki `this` neyse onu kullanır.

## 4. Bootstrap Responsive Grid ve 800 px Ekran Yorumu

Bootstrap grid sistemi 12 kolonludur. Sınıflar ekran genişliğine göre çalışır.

Temel breakpointler:

- `col-sm-*`: 576 px ve üzeri
- `col-md-*`: 768 px ve üzeri
- `col-lg-*`: 992 px ve üzeri
- `col-xl-*`: 1200 px ve üzeri
- `col-xxl-*`: 1400 px ve üzeri

800 px ekran genişliği:

- `sm` çalışır.
- `md` çalışır.
- `lg`, `xl`, `xxl` çalışmaz.

Örnek:

```html
<div class="row">
  <div class="col-md-4">A</div>
  <div class="col-md-4">B</div>
  <div class="col-md-4">C</div>
</div>
```

800 px ekranda `md` aktif olduğu için her div 4 kolon kaplar. Toplam 12 kolon eder, yani 3 div yan yana görünür.

Örnek:

```html
<div class="row">
  <div class="col-lg-6">A</div>
  <div class="col-lg-6">B</div>
</div>
```

800 px ekranda `lg` aktif değildir. Bu yüzden `col-lg-6` kuralı uygulanmaz; divler varsayılan olarak alt alta görünebilir. 992 px ve üzerinde iki div yan yana gelir.

## 5. Node.js / Express Routing: Sayfalar Arasında Gezinme

Routing, kullanıcı hangi adrese giderse hangi sayfanın açılacağını belirleme işlemidir.

Örnek proje:

```text
proje/
├── app.js
├── public/
│   └── style.css
└── views/
    ├── index.html
    ├── giris.html
    ├── hakkimizda.html
    └── iletisim.html
```

`app.js`:

```js
const express = require("express");
const path = require("path");

const app = express();
const PORT = 3000;

app.use(express.static("public"));

app.get("/", function (req, res) {
  res.sendFile(path.join(__dirname, "views", "index.html"));
});

app.get("/giris", function (req, res) {
  res.sendFile(path.join(__dirname, "views", "giris.html"));
});

app.get("/hakkimizda", function (req, res) {
  res.sendFile(path.join(__dirname, "views", "hakkimizda.html"));
});

app.get("/iletisim", function (req, res) {
  res.sendFile(path.join(__dirname, "views", "iletisim.html"));
});

app.use(function (req, res) {
  res.status(404).send("<h1>404 - Sayfa bulunamadı</h1>");
});

app.listen(PORT, function () {
  console.log("Sunucu http://localhost:" + PORT + " adresinde çalışıyor");
});
```

Ana sayfada giriş linki:

```html
<a href="/giris">Giriş</a>
```

Kullanıcı `Giriş` linkine tıklayınca tarayıcı `/giris` adresine gider. Express içindeki şu route çalışır:

```js
app.get("/giris", function (req, res) {
  res.sendFile(path.join(__dirname, "views", "giris.html"));
});
```

Sonuç: `views/giris.html` dosyası açılır.

Sınavda beklenen temel mantık:

- `app.get("/", ...)` ana sayfadır.
- `app.get("/giris", ...)` giriş sayfasıdır.
- `res.sendFile()` HTML dosyasını gönderir.
- `express.static("public")` CSS, görsel ve JS dosyalarını açar.
- Bilinmeyen adreste 404 route çalışır.

# Node.js ve Express.js (Kapsamlı)

## Node.js Nedir?
Node.js, JavaScript kodlarının tarayıcı dışında çalışmasını sağlayan bir çalışma ortamıdır. Backend geliştirme, API oluşturma, dosya işlemleri ve sunucu geliştirme için kullanılır.

**Node.js Özellikleri:**
- Event Driven (Olay Güdümlü) yapı
- Asenkron çalışma sistemi
- Hızlı performans

**Node.js Kullanım Alanları:**
- REST API
- Web Sunucuları
- Gerçek zamanlı uygulamalar (Chat uygulamaları vb.)
- Blog sistemleri ve E-ticaret uygulamaları

**Kurulum ve İlk Program:**
Resmi site: https://nodejs.org
Kurulum kontrolü: `node -v` ve `npm -v`

`app.js`:
```javascript
console.log("Merhaba Node.js");
```
Çalıştırma: `node app.js`

## Modül Sistemi
`math.js`:
```javascript
function topla(a,b) { return a+b; }
function cikar(a,b) { return a-b; }
function carp(a,b) { return a*b; }

module.exports = { topla, cikar, carp };
```

`app.js`:
```javascript
const math = require('./math');
console.log(math.topla(5, 3));
```

## FS (File System) Modülü
fs modülü: Dosya oluşturma, okuma, silme, yazma ve ekleme işlemleri için kullanılır.
Projeye dahil etme: `const fs = require('fs');`

**1. Dosyaya Yazma (`writeFileSync`)**
Eski veri tamamen silinir, yerine yeni veri yazılır.
```javascript
const fs = require('fs');
fs.writeFileSync('not.txt', 'Merhaba Dünya');

let ad = "Ayşe";
let notu = 90;
fs.writeFileSync('bilgi.txt', `Ad: ${ad}\nNot: ${notu}`);
```

**2. Dosyanın Sonuna Ekleme (`appendFileSync`)**
Döngülerde vs. mevcut veriyi silmeden eklemek için kullanılır.
```javascript
const fs = require('fs');
for(let i=1; i<=5; i++){
    fs.appendFileSync('sayilar.txt', i + "\n");
}
```

**3. Dosya Okuma (`readFileSync`)**
`utf8` parametresi Türkçe karakter sorunlarını önler.
```javascript
const fs = require('fs');
const data = fs.readFileSync('not.txt', 'utf8');
const satirlar = data.split('\n'); // Satır satır ayırma
```

**4. Dosya Var mı Kontrolü (`existsSync`) ve Silme (`unlinkSync`)**
```javascript
if(fs.existsSync('not.txt')) {
    fs.unlinkSync('not.txt'); // Dosyayı siler
}
```

**5. JSON Dosyası İşlemleri**
```javascript
const fs = require('fs');
let user = { ad: "Ali", yas: 20 };

// JSON yazma
fs.writeFileSync('user.json', JSON.stringify(user));

// JSON okuma
const data = fs.readFileSync('user.json', 'utf8');
const okunanUser = JSON.parse(data);
console.log(okunanUser.ad);
```

## Express.js ve Routing Projesi
Node.js framework'üdür. Kurulum: `npm install express`

**Temel Express Uygulaması:**
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send("Ana Sayfa");
});

app.listen(3000);
```

**Kapsamlı Proje Yapısı (Routing ve HTML Gösterimi):**
`proje/`
├── `app.js`
├── `package.json`
├── `public/style.css`
└── `views/`
    ├── `index.html`
    ├── `iletisim.html`
    ├── `urunler.html`
    └── `referanslar.html`

`app.js` içinde `res.sendFile()` ile views klasöründeki HTML'ler çağrılır ve `express.static('public')` ile stil dosyaları aktif edilir.
