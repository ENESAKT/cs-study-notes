# Yazılım Mühendisliği — Defter Notları

Bu dosya, interaktif derslerde işlenen konuların kısa, öz defter notlarını barındırır.
Yeni notlar her ders sonrasında dosyanın sonuna eklenir (append). Eski notlar asla silinmez.

> **Başlangıç:** 5 Haziran 2026 — Sıfırdan öğretim

---

*Notlar, dersler başladıkça buraya eklenecektir.*

---

## 6 Haziran 2026 — Yazılım Mühendisliğine Giriş

**Kaynak:** `pdf/Ders1.pdf`, s. 1-23

**Kısa Tanım:** Yazılım mühendisliği, spesifikasyondan bakıma kadar yazılım üretiminin tüm yönleriyle ilgilenen mühendislik disiplinidir.

**Temel Kavramlar:**
- **Profesyonel vs Bireysel:** Profesyonel yazılım → başkası kullanır, takım geliştirir, bakım gerektirir
- **8 Uygulama Türü:** Stand-alone, Transaction-based, Embedded, Batch, Entertainment, Modelleme/Simülasyon, Veri Toplama, Systems of Systems
- **Etik 4 Eksen:** Gizlilik, Yeterlilik, Fikri Mülkiyet, Kötü Kullanım
- **4 Durum Çalışması:** İnsülin pompası (gömülü/emniyet kritik), ZihinSaS (bilgi sistemi/gizlilik kritik), Kır hava istasyonu (veri toplama), Sayısal öğrenme ortamı (servis tabanlı)

**Hatırla:** Yazılım müh. yöntemleri kullanmamanın maliyeti → test, kalite güvence ve uzun dönem bakım maliyetleri patlar!

---

## 6 Haziran 2026 — Yazılım Süreçleri (Bölüm 1)

**Kaynak:** `pdf/Ders2_YazilimSurecleri.pdf`, s. 1-58

**Kısa Tanım:** Yazılım süreci, geliştirmede izlenen yapılandırılmış aktiviteler bütünüdür. Her süreç 4 temel aktiviteyi içerir.

**Temel Kavramlar:**
- **4 Temel Aktivite:** Spesifikasyon (s. 8), Tasarım/Gerçekleştirim (s. 9), Doğrulama (s. 10), Evrim (s. 11)
- **Geri bildirim (feedback)** her projede zorunlu (s. 17)
- **Code and Fix (s. 30):** Plansız, küçük proje → tamam; büyük proje → kaos
- **Şelale/Waterfall (s. 33-38):** Doğrusal, geri dönüş pahalı, belgeleme güçlü. Gereksinim sabitse ideal
- **V Modeli (s. 39-46):** Şelalenin V hali — her tasarım aşamasının karşısında bir test aşaması var
- **Evrimsel Geliştirme (s. 48-54):** İç içe süreç. Keşifsel (müşteriyle keşfet) vs Atılır prototip (anla ve at)
- **Prototipleme (s. 56-58):** Döngüsel — hızlı prototip → geri bildirim → tekrar. Quick & dirty prototip asla ürün olmamalı!

**Hatırla:** Şelale'de geri dönüş PAHALIDIR çünkü aşamalar sıralı akar! Sınavda kesin sorulacak.


---

## 6 Haziran 2026 — Factory Method Tasarım Deseni

**Kaynak:** `pdf/Ders4_Kod.pdf`, s. 1-8

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Factory Method, nesne oluşturma sorumluluğunu alt sınıflara devreden, kod içindeki bağımlılıkları azaltan (Loose Coupling) ve sisteme yeni nesneler eklerken mevcut kodu bozmamayı sağlayan (OCP - Open/Closed Principle) yaratımsal bir tasarım desenidir.
- **Ne Değildir:** Hazır bir kütüphane veya fonksiyon değildir; bir mimari şablondur. `new` anahtar kelimesinden tamamen kurtulmak demek değildir, `new` işlemini sadece Fabrika sınıflarının içine hapsetmek demektir.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Ortak Arayüz (Product)
public interface ITasima
{
    void TeslimEt();
}

// 2. Somut Sınıflar (Concrete Products)
public class Kamyon : ITasima {
    public void TeslimEt() => Console.WriteLine("Kamyon ile teslimat");
}
public class Gemi : ITasima {
    public void TeslimEt() => Console.WriteLine("Gemi ile teslimat");
}

// 3. Soyut Fabrika Sınıfı (Creator)
public abstract class LojistikFabrika
{
    // Sınavda boş bırakılabilecek kritik nokta: abstract dönüş tipi
    public abstract ITasima TasimaOlustur();
    
    public void Planla()
    {
        ITasima arac = TasimaOlustur(); // Polimorfizm devrede
        arac.TeslimEt();
    }
}

// 4. Somut Fabrika Sınıfları (Concrete Creators)
public class KaraLojistik : LojistikFabrika
{
    // Sınavda boş bırakılabilecek kritik nokta: override
    public override ITasima TasimaOlustur() => new Kamyon();
}
public class DenizLojistik : LojistikFabrika
{
    public override ITasima TasimaOlustur() => new Gemi();
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **`interface ITasima` veya `abstract class ITasima`**: Fabrikanın üreteceği tüm nesnelerin ortak noktasıdır. Hoca `Kamyon` ve `Gemi` sınıflarını verip ortak arayüzü eksik bırakırsa buraya `ITasima` tipi gelmelidir.
- **`abstract ITasima TasimaOlustur()`**: Soyut fabrika sınıfında bulunur. Gövdesi yoktur. Hoca bu metodun imzasını (dönüş tipi ve abstract kelimesi) boş bırakabilir.
- **`override`**: Somut fabrikalarda (`KaraLojistik`), soyut fabrikadaki metodu ezmek zorundayız. Hoca `public ___ ITasima TasimaOlustur()` şeklinde boşluk bırakırsa, boşluğa `override` kelimesi gelmelidir.
- **`new Kamyon()` / `new Gemi()`**: Somut fabrikaların içindeki oluşturma işlemidir. Hoca nesnenin nerede türetileceğini sorarsa, bu işlemin her zaman "ilgili somut fabrika sınıfının içinde" yapıldığını bilmelisin.

---

## 7 Haziran 2026 — Abstract Factory Tasarım Deseni

**Kaynak:** `pdf/Ders4_Kod.pdf`, s. 10-15

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Abstract Factory, birbiriyle ilişkili veya uyumlu nesne "ailelerini" (Örneğin; `IDbConnection` ve `IDbCommand` gibi) somut sınıflara bağımlı olmadan üretmeyi sağlayan yaratımsal bir tasarım desenidir.
- **Ne Değildir:** Sadece tek bir ürün üreten Factory Method ile karıştırılmamalıdır. Factory Method tek bir ürün üretirken, Abstract Factory birbiriyle uyumlu çalışması gereken **birden fazla ürünü (bir aileyi)** birlikte üretir.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Soyut Fabrika (Abstract Factory)
public interface IDatabaseFactory
{
    // Sınavda buradaki dönüş tipleri (Abstract Product) sorulabilir
    IDbConnection CreateConnection();
    IDbCommand CreateCommand();
}

// 2. Somut Fabrika (Concrete Factory - MySQL Ailesi)
public class MySqlFactory : IDatabaseFactory
{
    public IDbConnection CreateConnection() => new MySqlConnection();
    public IDbCommand CreateCommand() => new MySqlCommand();
}

// 3. Kullanım (Client)
class Application
{
    private IDbConnection connection;
    private IDbCommand command;

    // Sınavda parametre olarak Abstract Factory alınmasına dikkat et!
    public Application(IDatabaseFactory factory)
    {
        connection = factory.CreateConnection();
        command = factory.CreateCommand();
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Nesne Ailesi (Family of objects)**: Hoca soruda "birbiriyle uyumlu/ilişkili nesneler" veya "ürün ailesi" kelimelerini geçirirse cevap kesinlikle Abstract Factory'dir.
- **Factory Method vs Abstract Factory**: "Tek ürün" üretiliyorsa Factory Method, "ürün ailesi" (Connection + Command gibi birden fazla ilişkili ürün) üretiliyorsa Abstract Factory'dir. Sınavda bu ayrım klasik veya boşluk doldurma olarak sorulabilir.
- **Soyut Parametre (Abstract Dependency)**: İstemci (Client/Application) sınıfı, yapıcı metodunda (constructor) somut bir fabrika (`MySqlFactory`) değil, soyut bir fabrika (`IDatabaseFactory`) alır. Hoca burayı boş bırakıp "Buraya hangi tip referans gelmeli?" diye sorarsa cevap abstract class veya interface (soyut yapı) olmalıdır.

---
## 7 Haziran 2026 — Builder Tasarım Deseni

**Kaynak:** `pdf/Ders5.pdf`, s. 1-9

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Builder, çok fazla parametre alan karmaşık nesnelerin (örneğin 10 farklı özelliği olan bir `Araba`) adım adım ve okunaklı bir şekilde oluşturulmasını sağlayan yaratımsal bir tasarım desenidir. "Constructor patlamasını" (aşırı parametre yüklü yapıcı metotlar) önler.
- **Ne Değildir:** Singleton gibi tek bir nesne üretmez; `Build()` metodu her çağrıldığında yepyeni bir nesne oluşturur. Nesnenin yarı tamamlanmış, eksik halde kullanılmasını engeller.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Üretilecek Nesne (Product)
public class Araba
{
    public string Renk { get; set; }
    public int Motor { get; set; }
}

// 2. İnşa Edici Sınıf (Builder)
public class ArabaBuilder
{
    private Araba _araba = new Araba();

    // Zincirleme kullanım (Fluent API) için Builder'ın kendisi dönmeli (return this)
    public ArabaBuilder SetRenk(string renk)
    {
        _araba.Renk = renk;
        return this; // Sınavda burası sorulabilir
    }

    public ArabaBuilder SetMotor(int hp)
    {
        _araba.Motor = hp;
        return this; // Sınavda burası sorulabilir
    }

    // Nihai nesneyi döndüren metot
    public Araba Build()
    {
        return _araba; // Sınavda burası sorulabilir
    }
}

// 3. Kullanım (Client)
// Sınavda zincirleme kullanım yapısı sorulabilir
Araba arabam = new ArabaBuilder()
                .SetRenk("Kırmızı")
                .SetMotor(200)
                .Build();
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Constructor Patlaması (Telescoping Constructor)**: Sınavda "çok fazla parametre alan kurucu metotların yarattığı karmaşayı ne çözer?" diye sorulursa cevap Builder'dır.
- **`return this;`**: Metotların arka arkaya (zincirleme / Fluent API) çağrılabilmesi için builder metotlarının dönüş tipi kendi sınıfı (`ArabaBuilder`) olmalı ve içeride `return this;` demelidir. Hoca kodu verip "zincirleme çalışması için buraya ne gelecek?" diyebilir.
- **`Build()` metodu**: Nesne inşasının bittiğini belirtir. Hoca "Builder'da nesneyi son olarak hangi metot teslim eder?" diye sorarsa cevap `Build()`'dir. Aynı zamanda doğrulama (validation) işlemleri de bu metodun içinde yapılır.

---

## 7 Haziran 2026 — Prototype Tasarım Deseni

**Kaynak:** `pdf/Ders5.pdf`, s. 9-13

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Prototype, sıfırdan oluşturulması pahalı (maliyetli, uzun süren) nesneleri `new` ile tekrar tekrar üretmek yerine, eldeki mevcut bir nesnenin "kopyasını" (klonunu) çıkararak yeni nesne üretmemizi sağlayan yaratımsal bir tasarım desenidir.
- **Ne Değildir:** Nesne referansını başka bir değişkene atamak ( `A = B` ) kopyalamak değildir; bu sadece aynı bellek adresini işaret etmektir. Prototype, gerçekten bellekte yeni bir nesne (kopya) yaratmayı hedefler.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Kopyalanabilir Sınıf (ICloneable arayüzünü uygular)
public class Araba : ICloneable
{
    public string Marka { get; set; }
    public int Yil { get; set; }

    public Araba(string marka, int yil)
    {
        // Uzun süren pahalı işlemler burada olabilir...
        Marka = marka;
        Yil = yil;
    }

    // 2. Klonlama Metodu
    public object Clone()
    {
        // Constructor ÇALIŞTIRILMADAN hızlıca yüzeysel kopya oluşturur
        return this.MemberwiseClone(); // Sınavda burası sorulabilir
    }
}

// 3. Kullanım
class Program
{
    static void Main()
    {
        // Önce asıl nesne bir kere üretilir
        Araba ilkAraba = new Araba("Togg", 2025);

        // Sonraki ihtiyaçlarda new denmez, klonlanır!
        Araba kopyaAraba = (Araba)ilkAraba.Clone(); // Sınavda cast işlemi sorulabilir
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **`MemberwiseClone()`**: C# içerisinde yüzeysel kopya (Shallow Copy) oluşturmayı sağlayan yerleşik metottur. Sınavda "nesnenin sığ kopyasını döndüren metot hangisidir?" diye sorulursa cevap budur. Hoca kodu verip return kısmını boş bırakırsa buraya yazılır.
- **Shallow Copy (Sığ Kopya) vs Deep Copy (Derin Kopya)**: Sınavda klasik soru olarak gelebilir. Sığ kopyalamada nesnenin içindeki "referans tipleri" (örneğin başka bir sınıf nesnesi) kopyalanmaz, sadece adresleri tutulur. Derin kopyalamada ise nesnenin içindeki her şey sıfırdan tamamen yeni baştan kopyalanır.
- **`ICloneable`**: Bir sınıfın kopyalanabilir olduğunu belirten .NET arayüzüdür. Hoca sınıf tanımlamasını `public class Araba : ______` şeklinde bırakırsa cevap `ICloneable` olmalıdır.

---

## 7 Haziran 2026 — Generic (Jenerik) Yapılar

**Kaynak:** `pdf/Ders6.pdf`, s. 1-9

### 1. Yapı Nedir ve Ne Değildir?
- **Nedir:** Generic'ler, aynı kodun farklı veri tipleriyle (int, string, class vb.) bozulmadan ve güvenli bir şekilde çalışmasını sağlayan yapılardır. Tip genellikle `<T>` (Type) harfiyle temsil edilir. Kod tekrarını önler (Reusability).
- **Ne Değildir:** C#'taki `object` veya `var` tipiyle karıştırılmamalıdır. `object` her şeyi kabul eder ama tipi bellekte dönüştürürken (Boxing/Unboxing işlemi yapar) performansı düşürür ve tip güvenliği (Type Safety) sağlamaz. Generic'ler ise derleme zamanında (Compile-time) tam uyumlu çalışarak hata yapmanı engeller ve performansı artırır.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Generic Sınıf (T: Type)
// Kısıtlama (Constraint) eklendi: T mutlaka 'class' (referans tipi) olmalı
public class GenericDepo<T> where T : class
{
    private T _data;

    // Sınavda parametre tipi (T) boş bırakılabilir
    public void Ekle(T data)
    {
        _data = data;
    }

    public T Getir()
    {
        return _data;
    }
}

// 2. Generic Metot (Sınıf generic değil, sadece metot generic)
public class Yardimci
{
    // Sınavda metodun yanındaki <T> kısmı sorulabilir
    public void Yazdir<T>(T deger)
    {
        Console.WriteLine(deger);
    }
}

// 3. Kullanım
class Program
{
    static void Main()
    {
        // String için depo (T yerine string geldi)
        GenericDepo<string> metinDeposu = new GenericDepo<string>();
        metinDeposu.Ekle("Sınav sorusu");

        Yardimci y = new Yardimci();
        y.Yazdir<int>(100); // T yerine int geldi
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Tip Güvenliği (Type Safety) ve Performans**: Sınavda "Generic yapıların `object` kullanımına göre en büyük avantajı nedir?" diye sorulursa cevap **Tip Güvenliği** ve **Performans (Boxing/Unboxing yapmaması)**'dır. Hataları program çalışmadan (derleme - compile anında) yakalar.
- **Kısıtlamalar (`where T : ...`)**: Hoca kodda `public class Depo<T> _______` şeklinde bir boşluk bırakıp "Sadece referans tiplerinin gelmesini nasıl sağlarsınız?" derse cevap `where T : class` olmalıdır. (Değer tipleri için `where T : struct`, parametresiz kurucu metot isteyenler için `where T : new()` yazılır).
- **`T` Parametresi**: Tip (Type) kelimesinin kısaltmasıdır. Sınavda `public class Depo<__>` gibi bir boşlukta `T` (veya herhangi bir jenerik isim) kullanıldığını bilmek gerekir.

---

## 7 Haziran 2026 — Singleton Tasarım Deseni

**Kaynak:** `pdf/Ders6.pdf`, s. 10-11

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Singleton, bir sınıftan uygulama boyunca yalnızca **bir nesne (instance)** oluşturulmasını garanti eden ve o nesneye global bir erişim noktası sağlayan yaratımsal bir tasarım desenidir.
- **Ne Değildir:** Statik (static) bir sınıf değildir; statik sınıflar arayüz (interface) uygulayamazken, Singleton uygulayabilir. Her yerde rastgele kullanılacak global bir değişken deposu değildir; asıl amacı kaynak yönetimini (örneğin veritabanı bağlantısı) tek elden yapmaktır.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
public class Logger
{
    // 1. Static Instance (Kritik nokta: private ve static)
    private static Logger _instance;
    
    // Thread-safety (iş parçacığı güvenliği) için kilit objesi
    private static readonly object _lockObject = new object();

    // 2. Private Constructor (Kritik nokta: Dışarıdan nesne üretimini engeller)
    private Logger() 
    { 
        // Kurucu metot sadece sınıfın kendi içinden çağrılabilir
    }

    // 3. Global Erişim Noktası (Kritik nokta: public ve static)
    public static Logger Instance
    {
        get
        {
            // Double-Check Locking (Çift Kontrollü Kilitleme)
            if (_instance == null) // İlk kontrol
            {
                lock (_lockObject) // Kilitleme (Thread güvenliği için)
                {
                    if (_instance == null) // İkinci kontrol
                    {
                        _instance = new Logger(); // Sadece ilk çağrıda oluşturulur
                    }
                }
            }
            return _instance;
        }
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Private Constructor (Özel Kurucu Metot)**: Sınavda "Dışarıdan nesne oluşturulmasını (new anahtar kelimesini) nasıl engelleriz?" diye sorulursa cevap `private` constructor kullanmaktır.
- **Double-Check Locking**: Hoca "Singleton yapısının çoklu iş parçacıklarında (multi-threading) aynı anda nesne üretip patlamamasını sağlayan güvenli yöntem nedir?" derse veya `lock` keyword'ünü boş bırakırsa cevap "Double-Check Locking" (veya `lock` anahtar kelimesi) olmalıdır.
- **Kullanım Alanları**: Veritabanı bağlantısı, loglama (logging), konfigürasyon (config) dosyası okuma gibi "sistemde sadece bir tane olması gereken" nesnelerde kullanılır.

---

## 7 Haziran 2026 — Adapter Tasarım Deseni

**Kaynak:** `pdf/Ders7.pdf`, s. 1-8

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Adapter (Bağdaştırıcı), arayüzleri (interface) birbiriyle uyumsuz olan iki farklı sınıfın birlikte çalışmasını sağlayan **yapısal (structural)** bir tasarım desenidir. Gerçek hayattaki elektrik adaptörü mantığıyla çalışır (eski tip fişi, yeni tip prize uydurmak).
- **Ne Değildir:** Var olan sınıfların (eski sistemlerin) kendi kaynak kodunu doğrudan değiştirmek demek değildir. Aksine, o kodlara hiç dokunmadan araya "çevirmen" bir sınıf (Adapter) koyarak sistemi dışarıdan uydurmak demektir.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Target (Sistemimizin beklediği standart/yeni arayüz)
public interface IOdemeIslemi
{
    void Ode(decimal miktar);
}

// 2. Adaptee (Uyumsuz olan, değiştiremeyeceğimiz eski sistem veya dış servis sınıfı)
public class EskiOdemeSistemi 
{
    // Metot adı farklı, parametre tipi farklı
    public void OdemeYap(double value) 
    {
        Console.WriteLine($"Eski sistemden ödeme: {value}");
    }
}

// 3. Adapter (Arayı bulan, çevirmen sınıf)
// Sınavda sorulur: Adapter sınıfı her zaman Target arayüzünü uygular (implement eder)!
public class OdemeYontemiAdapter : IOdemeIslemi
{
    // İçinde Adaptee (eski sistem) nesnesini gizlice barındırır
    private readonly EskiOdemeSistemi _eskiSistem;

    public OdemeYontemiAdapter(EskiOdemeSistemi eskiSistem)
    {
        _eskiSistem = eskiSistem;
    }

    // Target'ın metodunu uygulayıp içerde Adaptee'nin metoduna dönüştürür
    public void Ode(decimal miktar)
    {
        // Tür dönüşümü (decimal -> double) ve metot isim çevirisi burada yapılır
        _eskiSistem.OdemeYap((double)miktar);
    }
}

// 4. Kullanım
class Program
{
    static void Main()
    {
        // Sistem sadece yeni arayüzü (IOdemeIslemi) biliyor, adaptör sayesinde eski kod tıkır tıkır çalışıyor.
        IOdemeIslemi odeme = new OdemeYontemiAdapter(new EskiOdemeSistemi());
        odeme.Ode(100);
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Target ve Adaptee**: Sınavda boşluk doldurmada sıklıkla sorulur. İstemcinin/sistemin beklediği uyumlu arayüze **Target**, elimizde olan ancak uyumsuz olan eski/dış koda **Adaptee** denir.
- **`public class Adapter : [Boşluk]`**: Hoca Adapter sınıfının tanımını verip interface kısmını boş bırakırsa, Adapter her zaman **Target** arayüzünü (örneğin `IOdemeIslemi`) uygular. Buraya Adaptee (`EskiOdemeSistemi`) KESİNLİKLE YAZILMAZ.
- **Legacy (Eski) Kod ve 3rd Party**: Sınavda "Üçüncü parti bir kütüphaneyi (örn. İyzico, PayTR) kendi sistemimize entegre ederken" veya "Kaynak kodunu değiştiremediğimiz eski bir sistemi kullanırken" ifadeleri geçerse cevap kesinlikle **Adapter** desenidir.

---

## 7 Haziran 2026 — Decorator (Dekoratör) Tasarım Deseni

**Kaynak:** `pdf/Ders8.pdf`, s. 1-8

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Decorator, bir nesnenin davranışını çalışma zamanında (runtime) nesnenin orjinal kodunu değiştirmeden "sarmalayarak" (wrap ederek) genişletmeyi (yeni özellik eklemeyi) sağlayan **yapısal (structural)** bir tasarım desenidir.
- **Ne Değildir:** Kalıtım (Inheritance) DEĞİLDİR! Kalıtım kullansaydık "SütlüŞekerliKahve", "SadeŞekerliKahve" diye onlarca alt sınıf yazmamız gerekirdi (Sınıf patlaması). Decorator, kalıtım yerine kompozisyon (iç içe sarmalama) kullanarak bu karmaşayı önler. Adapter gibi sistemin arayüzünü değiştirmez, sadece mevcut yetenekleri artırır.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Component (Ortak Arayüz)
public interface IKahve
{
    string AciklamaIste();
    decimal FiyatIste();
}

// 2. ConcreteComponent (Ana/Temel Nesne)
public class SadeKahve : IKahve
{
    public string AciklamaIste() => "Sade Kahve";
    public decimal FiyatIste() => 50;
}

// 3. Decorator (Soyut Sarmalayıcı Sınıf)
// Kritik: Hem IKahve'yi uygular, hem de İÇİNDE bir IKahve tutar.
public abstract class KahveDekorator : IKahve
{
    protected readonly IKahve _kahve; // Sarmalanan asıl nesne

    protected KahveDekorator(IKahve kahve)
    {
        _kahve = kahve;
    }

    public virtual string AciklamaIste() => _kahve.AciklamaIste();
    public virtual decimal FiyatIste() => _kahve.FiyatIste();
}

// 4. ConcreteDecorator (Gerçek özelliği ekleyen sınıf)
public class SutDekorator : KahveDekorator
{
    // base() ile ana nesneyi üst sınıfa yollarız
    public SutDekorator(IKahve kahve) : base(kahve) { }

    public override string AciklamaIste() => _kahve.AciklamaIste() + ", Süt";
    public override decimal FiyatIste() => _kahve.FiyatIste() + 30;
}

// 5. Kullanım (İç İçe Sarmalama)
class Program
{
    static void Main()
    {
        // Sade kahveyi al, süt sınıfıyla sar.
        IKahve sutluKahve = new SutDekorator(new SadeKahve());
        Console.WriteLine(sutluKahve.FiyatIste()); // 50 + 30 = 80
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Sarmalama (Wrapping)**: Sınavda "Bir nesneye çalışma zamanında (runtime) yeni özellikler katmak için onu sarmalayan desen hangisidir?" diye sorulursa cevap **Decorator**'dır.
- **Kalıtımın Alternatifi**: "Sınıf patlamasını önlemek için kalıtım (inheritance) yerine sarmalama kullanan desen nedir?" sorusunun cevabı Decorator'dır.
- **Hem uygular hem barındırır**: `KahveDekorator` sınıfının sırrı (ve hocanın soracağı yer), bu sınıfın hem `IKahve` interface'ini **implemente etmesi** hem de yapıcı metodunda (constructor) içine parametre olarak `IKahve` **almasıdır** (Çünkü nesneyi sarmalayabilmek için o nesneye ihtiyaç duyar).

---

## 7 Haziran 2026 — Facade (Cephe) Tasarım Deseni

**Kaynak:** `pdf/Ders8.pdf`, s. 9-11

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Facade (Cephe), arkada çalışan birbirine bağlı ve karmaşık birçok alt sistemi (subsystem) tek ve basit bir arayüz (Facade sınıfı) üzerinden kullanıcıya (client) sunan **yapısal (structural)** bir tasarım desenidir.
- **Ne Değildir:** Sistemi tamamen kısıtlayıp erişilemez hale getirmek demek değildir. İsteyen deneyimli kullanıcılar karmaşık alt sistemleri tek tek kendileri de çağırıp kullanabilir. Facade sadece "Benim için tüm o karışık detayları sen hallet, bana tek tuş ver" diyenler için bir kolaylaştırıcı (wrapper) katmandır.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Karmaşık Alt Sistemler (Subsystems)
class DvdPlayer {
    public void On() => Console.WriteLine("DVD Açıldı");
    public void Play() => Console.WriteLine("Film Oynatılıyor");
}
class SesSistemi {
    public void On() => Console.WriteLine("Ses Sistemi Açıldı");
    public void SesSeviyesi(int level) => Console.WriteLine($"Ses: {level}");
}
class Projektor {
    public void On() => Console.WriteLine("Projektör Açıldı");
}

// 2. Facade (Cephe) Sınıfı - Karmaşıklığı gizleyen yapı
public class EvSinemaSistemiFacade
{
    private DvdPlayer _dvd;
    private SesSistemi _ses;
    private Projektor _projektor;

    // Alt sistemler Facade içine toplanır
    public EvSinemaSistemiFacade(DvdPlayer dvd, SesSistemi ses, Projektor projektor)
    {
        _dvd = dvd;
        _ses = ses;
        _projektor = projektor;
    }

    // Sınav noktası: Kullanıcıya sunulan basit TEK METOT (arka planda işleri sıraya sokar)
    public void FilmIzle()
    {
        Console.WriteLine("Film Başlıyor...");
        _projektor.On();
        _ses.On();
        _ses.SesSeviyesi(20);
        _dvd.On();
        _dvd.Play();
    }
}

// 3. Kullanım
class Program
{
    static void Main()
    {
        // Alt sistemler yaratılır
        var facade = new EvSinemaSistemiFacade(new DvdPlayer(), new SesSistemi(), new Projektor());
        
        // Müşteri (Client) sadece tek bir metodu bilir ve çağırır! (Karmaşıklık gizlendi)
        facade.FilmIzle();
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Karmaşık Alt Sistemleri Gizleme (Subsystems)**: Sınavda "Birden fazla sınıfın veya karmaşık işlemlerin oluşturduğu sistemi tek bir giriş noktası (arayüz) arkasına gizleyen desen nedir?" diye sorulursa cevap **Facade**'dır.
- **Decorator vs Facade Farkı**: Sınavda bu klasik/test sorusu gelebilir. Decorator nesneye **yeni özellik (davranış) ekler**. Facade ise sisteme yeni davranış eklemez, zaten var olan karmaşık metotları **tek bir basit kullanım arayüzü (API) arkasında toplayarak basitleştirir**.
- **Tek Tuş (Tek Metot) Prensibi**: Hoca "Müşterinin (Client) sipariş sürecinde Banka, Stok ve Kargo sınıflarını ayrı ayrı çağırması yerine, tüm bunları sırasıyla çalıştıran sadece `SiparisVer()` metodunu çağırması hangi tasarımdır?" derse cevap yine Facade olmalıdır.

---

## 7 Haziran 2026 — Proxy (Vekil) Tasarım Deseni

**Kaynak:** `pdf/Ders9.pdf`, s. 1-5

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Proxy (Vekil), asıl nesneye (Real Subject) doğrudan erişmek yerine araya bir "vekil" nesne koyarak o nesneye erişimi kontrol altına almamızı sağlayan **yapısal (structural)** bir tasarım desenidir.
- **Ne Değildir:** Adapter gibi arayüz değiştirmez veya Decorator gibi nesneye yeni bir özellik/davranış eklemez. Zaten var olan bir hedefe "ulaşımı" denetler (Kapıdaki güvenlik veya sekreter gibi çalışır).

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Ortak Arayüz (Subject)
public interface IImage
{
    void Display();
}

// 2. Asıl Nesne (Real Subject - Üretilmesi pahalı/ağır nesne)
public class RealImage : IImage
{
    private string _fileName;
    public RealImage(string fileName)
    {
        _fileName = fileName;
        LoadFromDisk(); // Ağır işlem: Diskten okuma
    }
    private void LoadFromDisk() => Console.WriteLine($"{_fileName} diskten yükleniyor...");
    public void Display() => Console.WriteLine($"Gösteriliyor: {_fileName}");
}

// 3. Vekil Nesne (Proxy) - Arayı bulucu / Kontrolcü
public class ProxyImage : IImage
{
    private RealImage _realImage; // Sınavda sorulabilir: Asıl nesneyi içinde tutar
    private string _fileName;

    public ProxyImage(string fileName)
    {
        _fileName = fileName; // Henüz resmi diskten yüklemez (Lazy Loading)
    }

    public void Display()
    {
        // Gecikmeli yükleme (Sadece ihtiyaç olduğunda asıl nesneyi new'le)
        if (_realImage == null)
        {
            _realImage = new RealImage(_fileName); // Sınavda bu kısıma dikkat!
        }
        _realImage.Display();
    }
}

// 4. Kullanım
class Program
{
    static void Main()
    {
        // Resim henüz yüklenmedi, RAM'de yer kaplamıyor, sadece vekil oluşturuldu.
        IImage image = new ProxyImage("photo.jpg"); 
        
        // İlk çağrıda diskten yüklenir ve gösterilir.
        image.Display(); 
        
        // İkinci çağrıda diskten yüklenmez, RAM'de hazır olan gösterilir (Performans artışı).
        image.Display(); 
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Erişim Kontrolü (Access Control)**: Sınavda "Bir nesneye yetkisi olmayanların erişimini engellemek için araya konulan yapısal desen nedir?" diye sorulursa cevap **Proxy**'dir. (Buna Protection Proxy denir).
- **Gecikmeli Yükleme (Lazy Loading)**: Hoca "Pahalı nesnelerin belleğe hemen yüklenmesi yerine, sadece ekranda gösterileceği (ihtiyaç duyulduğu) anda yüklenmesini (lazy loading) sağlayan desen hangisidir?" diye sorarsa cevap **Proxy** olmalıdır.
- **Proxy vs Decorator Farkı**: Sınavda klasik sorudur. Decorator, asıl nesneye **yeni bir yetenek/davranış katar** (Genişletir). Proxy ise asıl nesnenin davranışını değiştirmez, sadece ona **kimin, ne zaman ulaşacağını kontrol eder**.

---

## 7 Haziran 2026 — Composite (Bileşik) Tasarım Deseni

**Kaynak:** `pdf/Ders9.pdf`, s. 6-8

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Composite (Bileşik), nesneleri ağaç (tree) yapısında organize etmeyi ve "tekil nesneler" (Leaf) ile "grup nesnelerini" (Composite) aynı kodla (ortak bir arayüz üzerinden) yönetebilmeyi sağlayan **yapısal (structural)** bir tasarım desenidir.
- **Ne Değildir:** Sadece nesneleri bir listeye koymak demek değildir. Amaç hiyerarşik (iç içe geçen) yapılarda döngüleri (recursion) otomatikleştirerek kullanıcının kodunu basitleştirmektir (Örn: Klasör -> Dosya -> Alt Klasör). İstemci (Müşteri) bir gruba mı yoksa tek bir elemana mı işlem yaptığını bilmek zorunda kalmaz.

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Component (Ortak Arayüz)
// Hem tekil nesne hem de grup nesnesi bu arayüzü uygulayacak
public interface ICalisan
{
    decimal MaasHesapla();
}

// 2. Leaf (Yaprak - Alt elemanı olmayan tekil nesne)
public class Calisan : ICalisan
{
    private decimal _maas;

    public Calisan(decimal maas)
    {
        _maas = maas;
    }

    public decimal MaasHesapla()
    {
        return _maas;
    }
}

// 3. Composite (Bileşik - İçinde başka nesneleri tutan nesne)
public class Departman : ICalisan
{
    // Kilit Nokta: Kendi içinde alt elemanları (ICalisan türünde) liste olarak tutar
    private List<ICalisan> _uyeler = new List<ICalisan>();

    public void Ekle(ICalisan calisan)
    {
        _uyeler.Add(calisan); // Departmana ister tek çalışan ekle, ister başka bir departman!
    }

    // Rekürsif (kendi kendini çağıran) hesaplama
    public decimal MaasHesapla()
    {
        decimal toplam = 0;
        foreach (var uye in _uyeler)
        {
            // Eğer uye bir 'Calisan' ise kendi maaşını verir.
            // Eğer uye bir 'Departman' ise altındaki tüm çalışanların maaşını toplayıp verir.
            toplam += uye.MaasHesapla(); 
        }
        return toplam;
    }
}

// 4. Kullanım
class Program
{
    static void Main()
    {
        // Tekil nesneler
        ICalisan isci1 = new Calisan(5000);
        ICalisan isci2 = new Calisan(6000);

        // Grup nesnesi
        Departman itDepartmani = new Departman();
        itDepartmani.Ekle(isci1);
        itDepartmani.Ekle(isci2);

        // İster tek kişinin maaşını iste, ister tüm departmanın. Metot hep aynı!
        Console.WriteLine(itDepartmani.MaasHesapla()); // 11000
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Ağaç Yapısı (Tree Structure) / Parça-Bütün İlişkisi**: Sınavda "Nesneleri ağaç yapısı şeklinde organize eden ve hiyerarşik yapı kuran tasarım deseni nedir?" diye sorulursa cevap **Composite**'tir.
- **Tekil ile Grubu Ayırt Etmemek**: Hoca "İstemcinin (Client) tek bir nesne ile nesne gruplarını aynı şekilde işleyebilmesini (kod farklılığı olmamasını) sağlayan desen hangisidir?" derse cevap yine Composite olmalıdır.
- **Rekürsif (Özyinelemeli) İşlem**: Composite sınıfı içindeki `foreach` döngüsünde alt elemanların metotlarının çağrılması işlemi rekürsiftir. Ağacın en dibine kadar inerek kendi kendine çalışır.

---

## 7 Haziran 2026 — Observer (Gözlemci) Tasarım Deseni

**Kaynak:** `pdf/Ders10.pdf`, s. 1-6

### 1. Desen Nedir ve Ne Değildir?
- **Nedir:** Observer (Gözlemci), bir nesnenin (Subject) durumunda değişiklik olduğunda, o nesneyi takip eden diğer tüm nesnelere (Observers) bu değişikliğin otomatik olarak bildirilmesini sağlayan **davranışsal (behavioral)** bir tasarım desenidir. "Yayıncı-Abone" (Publisher-Subscriber) mantığıyla çalışır. Gerçek hayattaki YouTube kanalı (Subject) ve aboneleri (Observers) buna en iyi örnektir.
- **Ne Değildir:** Nesnelerin sürekli "Değiştin mi? Değiştin mi?" diye anlık olarak sorması (Polling döngüsü) DEĞİLDİR. Olay gerçekleştiğinde ana nesne herkese "Ben değiştim, alın bu da yeni durum" diyerek kendisi haber verir (Push mantığı).

### 2. Uygulama Kodu (Sınavda Çıkabilecek Şekilde)
```csharp
// 1. Observer Arayüzü (Abonelerin uyması gereken sözleşme)
public interface IObserver
{
    void Guncelle(string mesaj); // Sınavda sorulur: Update/Guncelle metodu buradadır
}

// 2. Subject Arayüzü (Yayıncının uyması gereken sözleşme)
public interface ISubject
{
    void AboneEkle(IObserver observer);
    void AboneCikar(IObserver observer);
    void Bildir(); // Abonelere haber veren metot
}

// 3. Concrete Subject (Gerçek Konu/Yayıncı)
public class YouTubeKanali : ISubject
{
    // Kilit Nokta: Aboneleri liste halinde tutar
    private List<IObserver> _aboneler = new List<IObserver>();
    private string _sonVideo;

    public void AboneEkle(IObserver observer) => _aboneler.Add(observer);
    public void AboneCikar(IObserver observer) => _aboneler.Remove(observer);

    public void VideoYukle(string videoAdi)
    {
        _sonVideo = videoAdi;
        Console.WriteLine($"Kanal: Yeni video yüklendi -> {_sonVideo}");
        Bildir(); // Videoyu yükler yüklemez herkese haber verir
    }

    public void Bildir()
    {
        // Listedeki herkesin Guncelle metodunu tetikler
        foreach (var abone in _aboneler)
        {
            abone.Guncelle(_sonVideo);
        }
    }
}

// 4. Concrete Observers (Gerçek Aboneler)
public class EmailAbonesi : IObserver
{
    public void Guncelle(string mesaj) => Console.WriteLine($"Email'e gelen bildirim: {mesaj}");
}
public class SmsAbonesi : IObserver
{
    public void Guncelle(string mesaj) => Console.WriteLine($"SMS'e gelen bildirim: {mesaj}");
}

// 5. Kullanım
class Program
{
    static void Main()
    {
        YouTubeKanali kanal = new YouTubeKanali();
        
        IObserver ahmet = new EmailAbonesi();
        IObserver mehmet = new SmsAbonesi();

        kanal.AboneEkle(ahmet);
        kanal.AboneEkle(mehmet);

        // Kanal video yüklediği an, Ahmet ve Mehmet otomatik olarak bilgilendirilir.
        kanal.VideoYukle("C# Tasarım Desenleri - 1");
    }
}
```

### 3. Anahtar Kelimeler ve Boşluk Doldurma İpuçları
- **Subject ve Observer Rolleri**: Sınavda "Durumu değişen ana nesneye ne denir?" (Cevap: **Subject**). "Onu takip edip durum değişince güncellenen nesneye ne denir?" (Cevap: **Observer**).
- **Loose Coupling (Gevşek Bağlılık)**: Sınavda "Neden iki tane interface (ISubject ve IObserver) kullanıyoruz?" diye sorulursa cevap: "Gevşek bağlılık (Loose Coupling) sağlamak için. Subject, abonelerin (Observer) içeride ne iş yaptığını bilmek zorunda değildir, sadece onların `Guncelle()` metoduna sahip olduğunu bilir."
- **Otomatik Bildirim Sistemi**: Hoca "Borsa uygulamasında altın fiyatı değiştiğinde, onu ekrana yazdıran arayüzün, e-posta atan servisin ve log tutan servisin otomatik olarak çalışmasını sağlayan desen hangisidir?" diye sorarsa cevap kesinlikle **Observer**'dır.
