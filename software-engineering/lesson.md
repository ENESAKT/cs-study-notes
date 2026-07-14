# Yazılım Mühendisliği — Ders Notları

Bu dosya, tüm PDF kaynaklarından derlenen kapsamlı ders notlarını içerir.
PDF verildikçe ilgili konunun notları buraya eklenir.

---

## Ders 1 — Yazılım Mühendisliğine Giriş

Kaynak: `pdf/Ders1.pdf` (23 sayfa) — s. 1-23

### Yazılım Mühendisliğinin Tanımı

Yazılım mühendisliği, sistem spesifikasyonunun ilk aşamalarından sistemin kullanıma verildikten sonraki bakımına kadar yazılım üretiminin tüm yönleriyle ilgilenen bir mühendislik disiplinidir. İki anahtar kavram vardır: **mühendislik disiplini** ve **yazılım üretiminin tüm yönleri**.

İki önemli neden:

1. Bireyler ve toplum gittikçe daha karmaşık yazılım sistemlerine güvenir; emniyetli ve güvenilir sistemleri ekonomik biçimde üretmek gerekir.
2. Uzun vadede profesyonel sistemler için yazılım mühendisliği yöntemlerini kullanmak, "sadece program yazmaktan" daha ucuza gelir; aksi durumda sınama, kalite güvence ve bakım maliyetleri patlar.

### Profesyonel Yazılım Geliştirme

Profesyonel yazılımın amatör (kişisel) programlamadan iki temel farkı vardır:

1. Yazılım, geliştiren kişiden ayrı biri tarafından kullanılır.
2. Bireyler yerine takımlar tarafından geliştirilir ve tüm yaşam döngüsü boyunca bakım gerektirir.

### Yazılım Mühendisliği Çeşitliliği

Hangi yöntemin uygun olduğu üretilen uygulamanın türüne bağlıdır. Sık görülen sekiz tür:

1. Bağımsız uygulamalar (stand-alone)
2. Etkileşimli işlem tabanlı uygulamalar (transaction-based)
3. Gömülü kontrol sistemleri (embedded)
4. Destek işleme sistemleri (batch processing)
5. Eğlence sistemleri (entertainment)
6. Modelleme ve simülasyon için sistemler
7. Veri toplama ve analiz sistemleri
8. Sistemlerin sistemleri (systems of systems)

### İnternet Yazılım Mühendisliği

Web 2000'lerden sonra bilgi deposundan yazılım platformuna dönüştü. Tarayıcı tabanlı sistemler, reklamla desteklenen modeller, mikro-ödeme yerine SaaS gibi yeni dağıtım kalıpları doğdu.

### Yazılım Mühendisliği Etiği

Teknik beceri tek başına yetmez; mesleki sorumluluk dört eksen üzerinde durur:

- **Gizlilik:** Kullanıcı/müşteri verilerini koruma.
- **Yeterlilik:** Yapamayacağın işi kabul etmemek.
- **Fikri mülkiyet hakları:** Patent, telif, lisanslara saygı.
- **Bilgisayarın kötü kullanımı:** Virüs/zararlı yazılım/illegal kullanıma karşı tutum.

### Durum Çalışmaları (Sommerville)

- **İnsülin pompası kontrol sistemi:** Kandaki şekeri sensörle ölçüp uygun dozda insülin pompalayan gömülü sistem; emniyet kritik.
- **ZihinSaS — Ruh sağlığı hasta bilgi sistemi:** Dağıtık merkezi DB + dizüstüde çevrimdışı çalışabilen klinik kullanım; güvenlik ve gizlilik kritik.
- **Kır hava istasyonu:** Sensörlerden veri toplayıp merkezi hava bilgi sistemine ileten veri toplama sistemi.
- **Okullar için sayısal öğrenme ortamı:** Yardımcı, uygulama ve konfigürasyon servislerinden oluşan servis tabanlı bir destek ortamı.

---

## Ders 2 — Yazılım Süreçleri

Kaynak: `pdf/Ders2_YazilimSurecleri.pdf` (92 sayfa) — s. 1-92

> **Sayfa Haritası:**
> - s. 1-3: Giriş
> - s. 4-17: Süreç tanımı, 4 temel aktivite (Spesifikasyon s.8, Tasarım s.9, Doğrulama s.10, Evrim s.11), geri bildirim
> - s. 18-28: Genel süreç adımları (Tasarım s.23, Implementasyon s.24, Entegrasyon s.25, Test s.26, Teslim s.27, Bakım s.28)
> - s. 29-31: Code and Fix (s.30)
> - s. 33-38: Waterfall / Şelale Modeli (Royce, 1970) (s.33)
> - s. 39-46: V Modeli (s.39, 43-46), Verification vs Validation (s.45)
> - s. 48-54: Evrimsel Geliştirme (s.48-50), Exploratory ve Throw-away prototipleme
> - s. 55-58: Prototipleme (s.56 — süreç döngüsü diyagramı)
> - s. 59-63: Spiral Model (Boehm) (s.59-61)
> - s. 64-65: Formal System Development (s.64)
> - s. 66-74: Artımlı Geliştirme / Incremental (s.67-70), Bileşen Tabanlı (s.66)
> - s. 75-81: Unified Process (s.77-81) — iterasyon, use case, UML
> - s. 82-88: Agile Manifesto (s.82), Agile ilkeler, XP, SCRUM, ASD, Crystal (s.88)
> - s. 89-92: Model karşılaştırma tablosu (s.89-90)

### Süreç Nedir?

Yazılım süreci, bir yazılım sisteminin geliştirilmesinde izlenen aktivitelerin yapılandırılmış toplamıdır. Tüm süreçlerde ortak dört temel aktivite bulunur:

1. **Yazılım spesifikasyonu (Software Specification / Requirements Engineering):** Sistemin ne yapacağı ve kısıtları tanımlanır.
2. **Yazılım tasarımı ve gerçekleştirimi (Design and Implementation):** Spesifikasyonun çalışan sisteme dönüştürülmesi.
3. **Yazılım doğrulaması (Software Validation):** Sistemin müşteri isteklerini karşıladığının kontrolü.
4. **Yazılım evrimi (Software Evolution):** Değişen müşteri ihtiyaçlarına uyum sağlanması; bakım ve geliştirme.

Her projeye geri bildirim (feedback) gerekir; resmi olmayan (informal) gereksinimler resmî gereksinimlere dönüştürülürken müşteri ile sürekli iletişim şarttır.

### Yazılım Süreç Modelleri

#### 1. Code and Fix
Plansız yaklaşımdır; küçük projeler için olabilir ama orta/büyük projelerde sürdürülemez.

#### 2. Şelale Modeli (Waterfall — Royce 1970)
Gereksinim → Analiz → Tasarım → Kod → Test → Bakım aşamaları doğrusal akar. Aşama bitmeden bir sonrakine geçilmez; geri dönüş pahalıdır.

#### 3. V Modeli
Şelalenin V şeklinde gösterimi; sol kol spesifikasyon/tasarım, sağ kol birim/sistem/kabul testleri. Her tasarım aşamasının karşısında bir doğrulama (verification) aşaması vardır.

#### 4. Evrimsel Geliştirme (Evolutionary Development)
Spesifikasyon, geliştirme ve doğrulama iç içe yapılır; anahat (outline) sürümden başlayarak ara sürümlerle son sürüme ulaşılır.

- **Keşifsel (Exploratory) geliştirme:** Müşteriyle birlikte sistem keşfedilir.
- **Atılır prototipleme (Throw-away prototyping):** Hızlı prototip üretilip ardından atılır.

#### 5. Prototipleme
Sistem gereksinimlerini anlamak için hızlı bir prototip üretmek; iletişim → hızlı plan → modelleme → hızlı tasarım → kurulum/teslim/geri bildirim adımları döner.

#### 6. Spiral Model (Boehm)
Risk odaklı yinelemeli model; planlama, risk analizi, mühendislik ve değerlendirme dilimleriyle her döngüde sistem büyür.

#### 7. Bileşen Tabanlı (Reuse-based) Geliştirme
Hazır bileşenlerin entegrasyonuyla yeni sistem üretilir; analiz, bileşen arama, gereksinim değiştirme, sistem tasarımı, geliştirme ve entegrasyon adımları.

#### 8. Artımlı Geliştirme (Incremental)
Sistem küçük artımlar halinde teslim edilir; her artım çalışan bir parçayı tamamlar.

#### 9. Birleşik Süreç (Unified Process)
Yinelemeli, artımlı, use-case güdümlü ve mimari merkezli süreç. Dört fazı vardır:
- **Inception (Başlangıç):** Projenin kapsamı ve yapılabilirliği belirlenir.
- **Elaboration (Ayrıntılandırma):** Mimari tasarlanır, riskler analiz edilir.
- **Construction (İnşa):** Sistem parça parça kodlanır.
- **Transition (Geçiş):** Sistem kullanıcıya teslim edilir.

#### 10. Formal System Development
Matematiksel sistem modeli kullanan, kritik sistemlerde tercih edilen yaklaşım.

### Verification vs Validation

- **Verification (doğrulama):** "Sistemi doğru mu inşa ediyoruz?" — Spesifikasyona uygunluk.
- **Validation (geçerleme):** "Doğru sistemi mi inşa ediyoruz?" — Müşteri ihtiyacını karşılayıp karşılamadığı.

Sistemin teslim sonrası da bakım (maintenance), iyileştirici ve düzeltici etkinliklerle yaşam döngüsü devam eder.

---

## Ders 3 — OOP Temelleri ile Kalıtım

Kaynak: `pdf/Ders3_Kod.pdf` (5 sayfa) — s. 1-5

### Klasik Senaryo: Kisi → Ogrenci / Ogretmen

Bir `Kisi` sınıfı `ad` ve `soyad` alanlarını tutar; `Ogrenci` öğrenci numarasını, `Ogretmen` ise branş bilgisini ekler. Her sınıf `virtual BilgiYaz` metodunu override eder.

```csharp
public class Kisi
{
    protected string ad;
    protected string soyad;

    public Kisi(string ad, string soyad)
    {
        this.ad = ad;
        this.soyad = soyad;
    }

    public virtual void BilgiYaz()
    {
        Console.WriteLine($"Ad: {ad}, Soyad: {soyad}");
    }
}

public class Ogrenci : Kisi
{
    private string ogrenciNo;

    public Ogrenci(string ad, string soyad, string ogrenciNo) : base(ad, soyad)
    {
        this.ogrenciNo = ogrenciNo;
    }

    public override void BilgiYaz()
    {
        base.BilgiYaz();
        Console.WriteLine($"Öğrenci No: {ogrenciNo}");
    }
}

public class Ogretmen : Kisi
{
    private string brans;

    public Ogretmen(string ad, string soyad, string brans) : base(ad, soyad)
    {
        this.brans = brans;
    }

    public override void BilgiYaz()
    {
        base.BilgiYaz();
        Console.WriteLine($"Branş: {brans}");
    }
}
```

Bu örnek aşağıdaki temel OOP kavramlarını canlandırır:

- **Inheritance (Kalıtım):** Kisi → Ogrenci, Kisi → Ogretmen
- **`protected` görünürlük:** Alt sınıfların erişebilmesi için
- **`virtual` / `override`:** Çalışma zamanında doğru metodun çağrılması (polimorfizm)
- **Constructor zinciri `base(...)`:** Alt sınıfın üst sınıfın constructor'ını çağırması

---

## UML — Birleşik Modelleme Dili

Kaynak: `pdf/UML.pdf` (21 sayfa) — s. 1-21

> **Sayfa Haritası:**
> - s. 1-4: UML nedir, tarihçe, faydaları
> - s. 5-6: Diyagram türleri genel bakış
> - s. 7-12: Yapısal diyagramlar (Sınıf s.8-9, Nesne s.10, Bileşen s.11, Dağıtım s.12, Paket s.12, Profil s.12)
> - s. 13-15: Davranışsal diyagramlar (Etkinlik s.13, Use Case s.13, Zamanlama s.14, Durum Makinesi s.15, Sıralı s.15, İletişim s.15)
> - s. 16-18: UML İlişkileri — sembol tablosu (s.16-17)
> - s. 19-20: İlişkilerin detaylı açıklamaları ve karşılaştırma tablosu (s.20)
> - s. 21: Çokluk (Multiplicity) gösterimleri

### UML Nedir, Neden Kullanılır?

UML; Booch, Jacobson ve Rumbaugh'un 1990'larda standartlaştırdığı, sistemin yapı ve davranışını görsel olarak gösteren bir gösterimdir. Karmaşık fikirleri basitleştirir, kodu görselleştirir, ekipler arası ortak dil kurar ve teknik olmayan paydaşlarla iletişimi kolaylaştırır.

### Diyagram Türleri

UML diyagramları iki ana gruba ayrılır:

**1. Yapısal diyagramlar (statik yapı):**

- **Sınıf diyagramı:** En yaygın diyagram. Her sınıf üç bölmeli kutu: ad, alanlar, metotlar.
- **Nesne diyagramı:** Sınıf diyagramının "bir an" gerçeklenmiş hali; doğruluk kontrolü için.
- **Bileşen diyagramı:** Modüller ve ilişkileri.
- **Bileşik yapı diyagramı:** Sınıfın iç yapısını ayrıntılandırır.
- **Dağıtım diyagramı:** Donanım düğümleri ve üzerlerinde çalışan yazılım bileşenleri.
- **Paket diyagramı:** Modeli oluşturan paketler arası bağımlılıklar.
- **Profil diyagramı:** UML'i özel platforma uyarlayan stereotip/etiket setleri.

**2. Davranışsal diyagramlar (dinamik):**

- **Etkinlik diyagramı:** Adım adım iş akışı/süreç.
- **Kullanım durumu (Use Case):** Aktörler ve sistemle olan etkileşimleri; işlevsel gereksinimleri görselleştirir.
- **Etkileşim özeti:** Etkinlik + etkileşim diyagramı birleşimi.
- **Zamanlama diyagramı:** Yaşam çizgileri ve zaman kısıtları üzerinden nesne durumlarının zaman içindeki değişimi.
- **Durum makinesi (Statechart):** Bir nesnenin iç/dış olaylara göre değişen davranışı.
- **Sıralı diyagram (Sequence):** Aktör/nesneler arasındaki ileti dizisi kronolojik olarak.
- **İletişim/işbirliği diyagramı:** Sıralı diyagrama benzer ama nesne organizasyonunu öne çıkarır.

### UML İlişkileri

| İlişki | Güç | Yaşam döngüsü | Örnek | Sembol |
| --- | --- | --- | --- | --- |
| Association | Zayıf | Yok | Öğrenci–Ders | Düz çizgi |
| Aggregation | Orta | Bağımsız | Takım–Oyuncu | Boş elmas (◇) |
| Composition | Güçlü | Bağımlı | Ev–Oda | Dolu elmas (◆) |
| Inheritance | Kalıtımsal | Üst sınıfa bağlı | Kedi–Hayvan | Boş üçgen (△) |
| Dependency | Çok zayıf | Geçici | Sürücü–Navigasyon | Kesik çizgi + açık ok |
| Realization | Sözleşme | Interface'e bağlı | Sınıf–Arayüz | Kesik çizgi + boş üçgen |

### Çokluk (Multiplicity)

- `1`: Tam olarak bir tane (TC kimlik no).
- `0..1`: Olabilir veya olmayabilir (kullanıcının opsiyonel sosyal hesabı).
- `*` veya `0..*`: Sınırsız (öğrenci hiç ders alabilir veya çok sayıda).
- `1..*`: En az bir, üst sınır yok.
- `n..m`: Belirli aralık (takım `11..11` tam oyuncu, sınav `20..50` soru).

---

## Ders 4 — Tasarım Desenleri Giriş + Factory Method

Kaynaklar: `pdf/Ders4_Sunum.pdf` (22 sayfa) — s. 1-22, `pdf/Ders4_Kod.pdf` (8 sayfa) — s. 1-8

### Tasarım Deseni Nedir?

Tasarım desenleri, yazılım tasarımında sıkça karşılaşılan problemlere yönelik tipik çözümlerdir. Hazır fonksiyon veya kütüphane değildir; çözüm için **yüksek seviye mimari plan** sağlar. Aynı desen iki farklı projede farklı kodlar üretebilir.

Bir desen genellikle şunlardan oluşur: amaç (intent), motivasyon, sınıf yapısı, kod örneği, uygulanabilirlik (applicability), uygulama adımları ve diğer desenlerle ilişkiler.

### Hiyerarşi

- **Idiom (Deyim):** En düşük seviyeli, dile özgü çözümler.
- **Tasarım Deseni:** Orta seviye; sınıf/nesne ilişkilerini kapsar.
- **Mimari Desen (Architectural Pattern):** En yüksek seviye; uygulamanın tüm mimarisini şekillendirir.

### Üç Ana Grup

- **Creational (Yaratımsal):** Nesne oluşturma mekanizmaları; esneklik ve yeniden kullanım sağlar. (Factory Method, Builder, Singleton, Abstract Factory, Prototype.)
- **Structural (Yapısal):** Nesne ve sınıfların büyük yapılarda birleşmesini düzenler. (Adapter, Decorator, Facade, Proxy, Composite, Bridge, Flyweight.)
- **Behavioral (Davranışsal):** Nesneler arası iletişim ve sorumluluk dağılımı. (Strategy, Observer, Command, Iterator, Template Method, Chain of Responsibility, State, Visitor, Mediator, Memento, Interpreter.)

### Tuzaklar

- **Verimsiz/dogmatik kullanım:** Deseni bağlama uyarlamadan birebir kopyalamak.
- **Gereksiz kullanım:** "Elinizde sadece çekiç varsa her şey çivi" tuzağı; basit kod yetiyorsa desen şart değildir.

### Factory Method

Factory Method, yaratımsal bir desendir. Amaç, nesne oluşturmayı `new` ile doğrudan yapmak yerine bir metoda devretmektir; hangi nesnenin oluşturulacağına alt sınıflar karar verir.

**Sağladıkları:**

- Nesne oluşturma mantığı tek yerde toplanır.
- Kod somut sınıflara bağımlı olmaz.
- Genişletilebilirlik artar.

**Avantajlar:** Loose Coupling, Open/Closed Principle uyumu, kod tekrarının azalması, runtime'da seçim esnekliği.

**Dezavantajlar:** Her ürün için ayrı factory sınıfı gerektiği için sınıf sayısı artar; küçük projelerde gereksiz olabilir.

**Kullanım alanları:** Lojistik sistemler, Payment Gateway, DB sağlayıcı seçimi, Notification servisleri.

**C# Kod Örneği (Lojistik):**

```csharp
// PRODUCT (Ürün Arayüzü)
public interface ITasima
{
    void TeslimEt();
}

// CONCRETE PRODUCTS (Somut Ürünler)
public class Kamyon : ITasima
{
    public void TeslimEt() => Console.WriteLine("Karayolu - Kamyon ile teslim edildi.");
}

public class Gemi : ITasima
{
    public void TeslimEt() => Console.WriteLine("Denizyolu - Gemi ile teslim edildi.");
}

public class Ucak : ITasima
{
    public void TeslimEt() => Console.WriteLine("Havayolu - Uçak ile teslim edildi.");
}

// CREATOR (Soyut Fabrika)
public abstract class LojistikFabrika
{
    // Factory Method — alt sınıflar bu metodu override ederek
    // hangi ürünün üretileceğini belirler.
    public abstract ITasima TasimaOlustur();

    // İş mantığı — hangi ürün olursa olsun aynı akış çalışır.
    public void Planla()
    {
        ITasima arac = TasimaOlustur();
        arac.TeslimEt();
    }
}

// CONCRETE CREATORS (Somut Fabrikalar)
public class KaraLojistik : LojistikFabrika
{
    public override ITasima TasimaOlustur() => new Kamyon();
}

public class DenizLojistik : LojistikFabrika
{
    public override ITasima TasimaOlustur() => new Gemi();
}

public class HavaLojistik : LojistikFabrika
{
    public override ITasima TasimaOlustur() => new Ucak();
}

// KULLANIM
class Program
{
    static void Main()
    {
        LojistikFabrika lojistik = new KaraLojistik();
        lojistik.Planla(); // Çıktı: Karayolu - Kamyon ile teslim edildi.

        lojistik = new DenizLojistik();
        lojistik.Planla(); // Çıktı: Denizyolu - Gemi ile teslim edildi.
    }
}
```

Yeni bir taşıma aracı eklemek için sadece yeni `ITasima` sınıfı ve onu döndüren bir factory eklenir; mevcut kod değişmez (OCP).

---

### Abstract Factory Tasarım Deseni

Kaynak: `pdf/Ders4_Kod.pdf` — s. 10-15

Abstract Factory, birbiriyle ilişkili veya uyumlu nesne ailelerini somut sınıflara bağımlı olmadan üretmeyi sağlayan yaratımsal bir tasarım desenidir.
Sistem hangi nesnenin oluşturulacağını bilmez, sadece soyut factory (abstract factory) ile çalışır ve nesneleri bir "aile" olarak üretir.

**Farklar:**
- **Factory Method** -> Tek bir ürün üretir (Örn: Sadece `ITasima`).
- **Abstract Factory** -> Bir ürün ailesi üretir (Örn: Veritabanı için hem `IDbConnection` hem `IDbCommand` birlikte üretilir).

**Örnek Senaryo (Veritabanı Aileleri):**
Veritabanı işlemleri (MySQL ve PostgreSQL). Her veritabanı kendi `Connection` ve `Command` nesnelerine sahiptir. `IDatabaseFactory` arayüzü, bu uyumlu aileleri (Connection+Command) birlikte üretir.

```csharp
// Abstract Products
public interface IDbConnection { void Connect(); }
public interface IDbCommand { void Execute(); }

// Concrete Products (MySQL)
public class MySqlConnection : IDbConnection { public void Connect() => Console.WriteLine("MySQL bağlandı"); }
public class MySqlCommand : IDbCommand { public void Execute() => Console.WriteLine("MySQL sorgusu"); }

// Concrete Products (PostgreSQL)
public class PostgreConnection : IDbConnection { public void Connect() => Console.WriteLine("PostgreSQL bağlandı"); }
public class PostgreCommand : IDbCommand { public void Execute() => Console.WriteLine("PostgreSQL sorgusu"); }

// Abstract Factory
public interface IDatabaseFactory
{
    IDbConnection CreateConnection();
    IDbCommand CreateCommand();
}

// Concrete Factories
public class MySqlFactory : IDatabaseFactory
{
    public IDbConnection CreateConnection() => new MySqlConnection();
    public IDbCommand CreateCommand() => new MySqlCommand();
}

public class PostgreFactory : IDatabaseFactory
{
    public IDbConnection CreateConnection() => new PostgreConnection();
    public IDbCommand CreateCommand() => new PostgreCommand();
}

// Client
class Application
{
    private IDbConnection connection;
    private IDbCommand command;

    public Application(IDatabaseFactory factory)
    {
        // İlgili ailenin uyumlu parçaları üretilir
        connection = factory.CreateConnection();
        command = factory.CreateCommand();
    }
}
```

---

## Ders 5 — Builder Tasarım Deseni

Kaynak: `pdf/Ders5.pdf` (9 sayfa) — s. 1-9

### Problem: Constructor Patlaması

Karmaşık bir nesne çok sayıda opsiyonel parametre içeriyorsa constructor zincirleri okunamaz hale gelir:

```csharp
new Araba("Kırmızı", 200);
new Araba("Kırmızı", 200, "Otomatik");
new Araba("Kırmızı", 200, "Otomatik", "Benzin");
```

Sorunlar: okunması zor, parametre sırası karışır, opsiyonel alanlar problem olur.

Setter ile inşa da yetersiz: nesne yarı kurulmuşken erişilebilir hale gelir, hangi alanların zorunlu olduğu belirsizdir.

### Builder Çözümü

Builder, karmaşık bir nesneyi adım adım inşa etmek ve aynı inşa süreciyle farklı temsiller (varyantlar) üretmek için kullanılır.

**Roller:**

- **Product:** İnşa edilecek karmaşık nesne (`Araba`).
- **Builder:** İnşa adımlarını tanımlayan arayüz veya soyut sınıf.
- **Concrete Builder:** Adımları somutlaştıran sınıf; kendi içinde `Product` örneğini biriktirir.
- **Director (opsiyonel):** Belirli sıralarda adımları çağıran orkestratör.
- **Client:** Director veya Builder'ı kullanarak istediği nesneyi oluşturan kod.

**Fluent (akıcı) API ile zincirleme kullanım:**

```csharp
public class Araba
{
    public string Renk { get; set; }
    public int BeygirGucu { get; set; }
    public string VitesTipi { get; set; }
    public string YakitTuru { get; set; }
}

public class ArabaBuilder
{
    private Araba _araba = new Araba();

    public ArabaBuilder SetRenk(string renk)
    {
        _araba.Renk = renk;
        return this; // Zincirleme için kendini döndürür
    }

    public ArabaBuilder SetHp(int hp)
    {
        _araba.BeygirGucu = hp;
        return this;
    }

    public ArabaBuilder SetVites(string vites)
    {
        _araba.VitesTipi = vites;
        return this;
    }

    public ArabaBuilder SetYakit(string yakit)
    {
        _araba.YakitTuru = yakit;
        return this;
    }

    public Araba Build()
    {
        // Doğrulama yapılabilir
        if (_araba.BeygirGucu <= 0)
            throw new InvalidOperationException("Beygir gücü pozitif olmalı!");
        return _araba;
    }
}

// Kullanım:
var araba = new ArabaBuilder()
    .SetRenk("Kırmızı")
    .SetHp(200)
    .SetVites("Otomatik")
    .SetYakit("Benzin")
    .Build();
```

Builder, opsiyonel alanları, validasyonu (`Build()` içinde) ve immutability'yi (her `Build` sonrası nesne donar) doğal olarak destekler. Singleton ile karıştırılmamalı: Builder her çağrıda yeni nesne üretir.

---

### Prototype Tasarım Deseni

Kaynak: `pdf/Ders5.pdf` — s. 9-13

Prototype Design Pattern, bir nesneyi sıfırdan oluşturmak (`new` anahtar kelimesi ile) yerine mevcut bir nesnenin kopyasını alarak yeni nesne üretme yaklaşımıdır.

**Neden Kullanılır?**
Bazı nesneleri oluşturmak pahalı (maliyetli) olabilir:
- Çok fazla ayar içeriyorsa
- Oluşturulurken uzun işlem yapıyorsa (DB/Config okuma vb.)
- Aynı nesnenin benzerlerinden çok sayıda gerekiyorsa

Bu durumda her seferinde `new Nesne(...)` yapmak yerine, hazır nesneyi `Clone` edip kullanmak (kopyalamak) daha mantıklıdır.

**Akış:**
1. Bir prototip nesne oluşturulur.
2. Bu nesne referans alınır.
3. Yeni ihtiyaç olduğunda `Clone()` çağrılır.
4. Kopyalanan nesne üzerinde küçük değişiklikler yapılır.

**Shallow Copy vs Deep Copy:**
- **Shallow Copy (Sığ Kopya):** Sadece değer tiplerini (int, string vb.) kopyalar. Referans tiplerinin adresini kopyalar (iki nesne aynı referansı paylaşır). C# içindeki `MemberwiseClone()` metodu shallow copy yapar.
- **Deep Copy (Derin Kopya):** Hem değer tiplerini hem de içindeki referans tiplerini tamamen yeni nesneler olarak kopyalar.

**Örnek Kod (Shallow Copy):**
```csharp
public class Book : ICloneable
{
    public string Title { get; set; }
    public string Content { get; set; }
    public int Year { get; set; }

    public object Clone()
    {
        // Constructor çalışmadığı için hızlıca kopya üretilir
        return this.MemberwiseClone(); 
    }
}

class Program
{
    static void Main()
    {
        var book = new Book { Title="Sefiller", Content="Roman", Year=1990 };
        var bookCopy = (Book)book.Clone(); // Yeni nesne (kopya)
    }
}
```

---

## Ders 6 — Generic Yapılar

Kaynak: `pdf/Ders6.pdf` (11 sayfa) — s. 1-11

> **Sayfa Haritası:**
> - s. 1-3: Generic nedir, neden kullanılır, tip güvenliği
> - s. 4-5: Generic sınıf örnekleri
> - s. 6-7: Generic metot yazımı
> - s. 8-9: Generic kısıtlamalar (where T : class/struct/new()/IEntity)
> - s. 10-11: var vs object farkı, Singleton giriş

### Generic Nedir?

Generics, aynı kodun farklı veri tipleriyle güvenli biçimde çalışmasını sağlar. Tip parametresi genellikle `T` ile gösterilir.

### Neden Kullanılır?

1. **Tip Güvenliği (Type Safety):** Tip hataları çalışma zamanı (runtime) yerine derleme zamanında (compile time) yakalanır.
2. **Performans:** Boxing/Unboxing ihtiyacı azaldığı için performans artar.
3. **Kod Tekrarı (Reusability):** Aynı mantık farklı tipler için tekrar yazılmaz.

### Generic Sınıf Örneği

```csharp
// T: Tip parametresi — dışarıdan hangi tip verilirse o tip ile çalışır
public class GenericDepo<T>
{
    private T _data;

    public void Ekle(T data)
    {
        _data = data;
        Console.WriteLine($"Veri eklendi: {data}");
    }

    public T Getir()
    {
        return _data;
    }
}

// Kullanım:
GenericDepo<string> metinDeposu = new GenericDepo<string>();
metinDeposu.Ekle("Merhaba Yazılım Mühendisliği!");

GenericDepo<int> sayiDeposu = new GenericDepo<int>();
sayiDeposu.Ekle(1453);
```

### Generic Metot

Generic metotlar, bir sınıfın tamamı generic olmasa bile tek bir metotta tip parametresi alabilir:

```csharp
public class Yardimci
{
    // Sınıf generic değil ama metot generic
    public void Yazdir<T>(T deger)
    {
        Console.WriteLine($"Değer: {deger}, Tip: {typeof(T).Name}");
    }
}

// Kullanım:
var y = new Yardimci();
y.Yazdir<int>(42);
y.Yazdir<string>("Merhaba");
y.Yazdir(3.14); // Tip çıkarımı ile de kullanılabilir
```

### Generic Kısıtlamalar (Constraints)

```csharp
// T sadece referans tipi olabilir ve parametresiz constructor'a sahip olmalı
public class VeriTabaniIslemleri<T> where T : class, new()
{
    public void Kaydet(T entity)
    {
        Console.WriteLine($"{entity.GetType().Name} veritabanına kaydedildi.");
    }
}
```

Sık kullanılan kısıtlamalar:

- `where T : class` → T bir referans tipi olmalı (string, class vb.)
- `where T : struct` → T bir değer tipi olmalı (int, double, bool vb.)
- `where T : new()` → T parametresiz bir constructor'a sahip olmalı
- `where T : IEntity` → T belirtilen arayüzü uygulamalı

### var vs object Farkı

- **`var`:** Derleme zamanında tip çıkarımı yapar; gerçek tip derleme zamanında belirlenir.
- **`object`:** Her şeyi kabul eder ama runtime'da boxing/unboxing gerekir ve tip güvenliği yoktur.

---

## Hafta 6 Ek — Singleton Tasarım Deseni

Kaynak: `pdf/Ders6.pdf`, s. 10-11; `pdf/yazilim_muh_07042026_ders6.pdf`

### Singleton Nedir?

Singleton, bir sınıftan uygulama boyunca yalnızca bir nesne oluşturulmasını garanti eden yaratımsal tasarım desenidir.

### Kullanım Alanları

- Veritabanı bağlantı yöneticileri
- Loglama mekanizmaları
- Konfigürasyon yöneticileri
- Sistem genelinde tek olması gereken kaynaklar

### Singleton Nasıl Uygulanır? (3 Altın Kural)

1. **Private Constructor:** Dışarıdan `new` ile nesne üretimi engellenir.
2. **Static Instance:** Sınıf kendi içinde kendi tipinden `static` değişken tutar.
3. **Global Erişim Noktası:** Dışarıdan bu tekil nesneye ulaşmak için `static` property/metot verilir.

### Thread-Safe Singleton (Double-Check Locking)

```csharp
public class Logger
{
    private static volatile Logger _instance;
    private static readonly object _lockObject = new object();

    // Dışarıdan newlenmeyi engellemek için private constructor
    private Logger()
    {
        Console.WriteLine("Logger nesnesi oluşturuldu.");
    }

    public static Logger Instance
    {
        get
        {
            if (_instance == null) // İlk kontrol (performans için)
            {
                lock (_lockObject) // Kilitleme (thread-safety)
                {
                    if (_instance == null) // İkinci kontrol (double-check)
                    {
                        _instance = new Logger();
                    }
                }
            }
            return _instance;
        }
    }

    public void LogYaz(string mesaj)
    {
        Console.WriteLine($"[LOG]: {mesaj}");
    }
}

// Kullanım:
Logger log1 = Logger.Instance;
Logger log2 = Logger.Instance;
Console.WriteLine(log1 == log2); // True — aynı nesne
```

### Modern Singleton: Lazy<T>

```csharp
public class ModernLogger
{
    private static readonly Lazy<ModernLogger> _lazyInstance =
        new Lazy<ModernLogger>(() => new ModernLogger());

    private ModernLogger() { }

    public static ModernLogger Instance => _lazyInstance.Value;
}
```

---

## Ders 7 — Adapter Tasarım Deseni

Kaynak: `pdf/Ders7.pdf` (8 sayfa) — s. 1-8

### Adapter Nedir?

Adapter, uyumsuz arayüze sahip bir sınıfı sistemin beklediği arayüze uyduran **yapısal** tasarım desenidir. Gerçek hayattaki elektrik adaptörü gibi düşün: Amerikan fişi olan bir cihazı Avrupa prizine takmak için adaptör kullanırsın.

### Ne Zaman Kullanılır?

- Yeni sistemi eski (legacy) koda veya 3rd party kütüphaneye entegre ederken.
- Kaynak kodunu değiştiremediğimiz sınıfları sisteme dahil etmek gerektiğinde.

### Roller

1. **Target (Hedef Arayüz):** Sistemin beklediği standart arayüz.
2. **Adaptee (Uyarlanacak Sınıf):** Kullanmak istediğimiz ama arayüzü uyumsuz olan sınıf.
3. **Adapter (Adaptör):** Adaptee'yi Target arayüzüne çeviren dönüştürücü sınıf.
4. **Client (İstemci):** Sadece Target arayüzünü bilir.

### C# Kod Örneği (Ödeme Sistemi)

```csharp
// TARGET — Sistemimizin beklediği arayüz
public interface IOdemeIslemi
{
    void OdemeYap(decimal miktar);
}

// ADAPTEE — Uyumsuz dış servis (koduna müdahale edemiyoruz)
public class XBankasiSistemi
{
    public void TahsilEt(double tutar)
    {
        Console.WriteLine($"X Bankası üzerinden {tutar} TL tahsilat gerçekleştirildi.");
    }
}

// ADAPTER — Araya girip çeviri yapan sınıf
public class XBankasiAdapter : IOdemeIslemi
{
    private readonly XBankasiSistemi _xBankasi;

    public XBankasiAdapter()
    {
        _xBankasi = new XBankasiSistemi();
    }

    public void OdemeYap(decimal miktar)
    {
        Console.WriteLine("Adaptör devrede: çeviri yapılıyor...");
        double donusturulenMiktar = Convert.ToDouble(miktar);
        _xBankasi.TahsilEt(donusturulenMiktar);
    }
}

// CLIENT — Sadece IOdemeIslemi'ni bilir
class Program
{
    static void Main()
    {
        IOdemeIslemi odemeSistemi = new XBankasiAdapter();
        odemeSistemi.OdemeYap(150.75m);
    }
}
```

Adapter, mevcut sistemi bozmadan yeni/uyumsuz yapıları bağlamayı sağlar (Open/Closed Principle).

---

## Ders 8 — Decorator ve Facade Tasarım Desenleri

Kaynak: `pdf/Ders8.pdf` (11 sayfa) — s. 1-11

> **Sayfa Haritası:**
> - s. 1-5: Decorator tanımı, roller, Kahve örneği
> - s. 6-8: Araba Decorator örneği
> - s. 9-11: Facade tanımı, Ev Sinema Sistemi örneği

### Decorator (Dekoratör) Tasarım Deseni

Decorator, bir nesnenin davranışını çalışma zamanında nesnenin kendisini değiştirmeden genişletmeyi sağlayan **yapısal** tasarım desenidir. Nesne aynı arayüzü uygulayan başka bir nesne tarafından sarılır.

**Kullanım alanları:** Loglama, yetkilendirme, cache, ek ücret, bildirim, ürün/servis üzerine dinamik özellik ekleme.

**Roller:**

1. **Component:** Ortak arayüz veya soyut sınıf.
2. **ConcreteComponent:** Temel gerçek nesne.
3. **Decorator:** Component arayüzünü uygular, içinde Component referansı tutar.
4. **ConcreteDecorator:** Gerçek ek davranışı ekleyen sınıf.

#### C# Kahve Örneği

```csharp
// COMPONENT — Ortak arayüz
public interface IKahve
{
    string AciklamaIste();
    decimal FiyatIste();
}

// CONCRETE COMPONENT — Temel nesne
public class SadeKahve : IKahve
{
    public string AciklamaIste() => "Sade Kahve";
    public decimal FiyatIste() => 50;
}

// DECORATOR — Soyut sarmalayıcı (içinde IKahve referansı tutar)
public abstract class KahveDekorator : IKahve
{
    protected readonly IKahve kahve;

    protected KahveDekorator(IKahve kahve)
    {
        this.kahve = kahve;
    }

    public virtual string AciklamaIste() => kahve.AciklamaIste();
    public virtual decimal FiyatIste() => kahve.FiyatIste();
}

// CONCRETE DECORATORS — Ek davranış ekleyen sınıflar
public class SutDekorator : KahveDekorator
{
    public SutDekorator(IKahve kahve) : base(kahve) { }
    public override string AciklamaIste() => kahve.AciklamaIste() + ", süt";
    public override decimal FiyatIste() => kahve.FiyatIste() + 30;
}

public class SekerDekorator : KahveDekorator
{
    public SekerDekorator(IKahve kahve) : base(kahve) { }
    public override string AciklamaIste() => kahve.AciklamaIste() + ", şeker";
    public override decimal FiyatIste() => kahve.FiyatIste() + 5;
}

public class CikolataDekorator : KahveDekorator
{
    public CikolataDekorator(IKahve kahve) : base(kahve) { }
    public override string AciklamaIste() => kahve.AciklamaIste() + ", çikolata";
    public override decimal FiyatIste() => kahve.FiyatIste() + 35;
}

// KULLANIM — İç içe constructor (zincirleme)
IKahve kahve = new CikolataDekorator(
    new SekerDekorator(
        new SutDekorator(
            new SadeKahve())));

Console.WriteLine(kahve.AciklamaIste()); // Sade Kahve, süt, şeker, çikolata
Console.WriteLine(kahve.FiyatIste());    // 120
```

### Facade (Cephe) Tasarım Deseni

Facade, karmaşık alt sistemleri tek ve sade bir arayüz arkasında toplayan **yapısal** tasarım desenidir. Kullanıcı alt sistemlerin detaylarını bilmeden Facade sınıfının sunduğu metodu çağırır.

**Roller:**

1. **Subsystem sınıfları:** Gerçek işi yapan sınıflar.
2. **Facade:** Alt sistemleri bir araya getiren kolaylaştırıcı sınıf.
3. **Client:** Facade'i kullanan taraf.

#### C# Ev Sinema Sistemi Örneği

```csharp
// ALT SİSTEMLER
class DvdPlayer
{
    public void On() => Console.WriteLine("DVD Player Açıldı");
    public void Play() => Console.WriteLine("Film Oynatılıyor");
}

class SesSistemi
{
    public void On() => Console.WriteLine("Ses Sistemi Açıldı");
    public void SesSeviyesi(int level) => Console.WriteLine($"Ses Seviyesi: {level}");
}

class Projektor
{
    public void On() => Console.WriteLine("Projektör Açıldı");
}

// FACADE — Tek bir giriş noktası
class EvSinemaSistemiFacade
{
    private DvdPlayer dvd;
    private SesSistemi ses;
    private Projektor projektor;

    public EvSinemaSistemiFacade(DvdPlayer dvd, SesSistemi ses, Projektor projektor)
    {
        this.dvd = dvd;
        this.ses = ses;
        this.projektor = projektor;
    }

    public void FilmIzle()
    {
        Console.WriteLine("Film Başlıyor...");
        dvd.On();
        ses.On();
        projektor.On();
        ses.SesSeviyesi(20);
        dvd.Play();
    }
}
```

### Decorator vs Facade Farkı

- **Decorator:** Tek bir nesneye davranış/özellik ekler.
- **Facade:** Birden fazla alt sistemi tek ve basit arayüz arkasında toplar.

---

## Ders 9 — Proxy ve Composite Tasarım Desenleri

Kaynak: `pdf/Ders9.pdf` (8 sayfa) — s. 1-8

> **Sayfa Haritası:**
> - s. 1: Proxy tanımı ve amaçları
> - s. 2-3: Proxy Örnek 1 — Kütüphane yetki senaryosu (Akademisyen/Öğrenci)
> - s. 4-5: Proxy Örnek 2 — ProxyImage (lazy loading)
> - s. 6-8: **Composite deseni** — ICalisan/Calisan/Departman ağaç yapısı, IShape/ShapeGroup

### Tanım

Proxy, bir nesneye doğrudan erişmek yerine onun yerine geçen bir aracı (proxy) üzerinden erişim sağlayan **yapısal** bir tasarım desenidir.

**Genel amaçlar:**

- Erişim kontrolü (yetki, güvenlik)
- Lazy loading (gecikmeli yükleme)
- Logging veya cache ekleme
- Uzak nesneye yerel temsil (Remote Proxy)

### Yapı

- **Subject (arayüz):** Hem RealSubject hem Proxy'nin uyduğu ortak arayüz.
- **RealSubject:** Asıl işi yapan sınıf.
- **Proxy:** RealSubject ile aynı arayüzü sağlar, çağrıyı kontrol/zenginleştirip RealSubject'e iletir.

### Sık Görülen Proxy Çeşitleri

1. **Virtual Proxy:** Pahalı bir nesneyi gerçek ihtiyaç doğana kadar oluşturmaz.
2. **Protection Proxy:** Yetki kontrolüne göre çağrıları iletir veya reddeder.
3. **Remote Proxy:** Ağdaki uzak nesneye yerel arayüz sunar (RPC tarzı).
4. **Caching Proxy:** Önceki sonuçları önbellekler.
5. **Logging Proxy:** Çağrı bilgilerini kayda alır.

### C# Kod Örneği (Yetki Proxy)

```csharp
// SUBJECT — Ortak arayüz
public interface IDosyaServisi
{
    string Oku(string yol);
}

// REAL SUBJECT — Asıl işi yapan sınıf
public class GercekDosyaServisi : IDosyaServisi
{
    public string Oku(string yol) => System.IO.File.ReadAllText(yol);
}

// PROXY — Erişim kontrolü ekleyen aracı
public class YetkiProxy : IDosyaServisi
{
    private readonly IDosyaServisi _gercek;
    private readonly string _kullanici;

    public YetkiProxy(IDosyaServisi gercek, string kullanici)
    {
        _gercek = gercek;
        _kullanici = kullanici;
    }

    public string Oku(string yol)
    {
        if (_kullanici != "admin")
            throw new UnauthorizedAccessException("Erişim reddedildi!");
        return _gercek.Oku(yol);
    }
}
```

### Adapter vs Decorator vs Proxy Farkları

| Özellik | Adapter | Decorator | Proxy |
| --- | --- | --- | --- |
| **Amaç** | Arayüz uyumsuzluğunu gidermek | Davranış/özellik eklemek | Erişimi yönetmek |
| **Arayüz** | Farklı arayüzü uydurur | Aynı arayüzü korur | Aynı arayüzü taklit eder |
| **Ne yapar** | Çeviri yapar | Özellik ekler | Kontrol/güvenlik ekler |
| **Zincirleme** | Tek seferde | İç içe sarılabilir (zincir) | Genellikle tek proxy |
| **Gerçek hayat** | Priz adaptörü | Telefon kılıfı | Güvenlik görevlisi |

---

## Ders 10 — Observer (Gözlemci) ve State Tasarım Desenleri

Kaynak: `pdf/Ders10.pdf` (10 sayfa) — s. 1-10

> **Sayfa Haritası:**
> - s. 1: Observer tanımı ve temel bileşenler
> - s. 2-4: Observer Örnek 1 — Sipariş bildirimi (Email/SMS/Log)
> - s. 5-6: Observer Örnek 2 — Öğrenci not güncelleme (Ogretmen/Veli observer)
> - s. 7-8: **State deseni** tanımı, Ödeme durumları örneği
> - s. 9-10: State + Observer birleşik örnek (Sipariş durumu + gözlemci)

### Tanım

Observer, bir nesnedeki durum değişikliğinin ona bağlı diğer nesnelere otomatik olarak bildirilmesini sağlayan **davranışsal** bir tasarım desenidir. Yayıncı-abone (publisher-subscriber) mantığı ile çalışır.

### Roller

- **Subject:** Durumu değişen ana nesne; observer listesini tutar.
- **Observer:** Subject'i takip eden nesneler; genelde `Update` / `Guncelle` metoduna sahiptir.
- **ConcreteSubject:** Gerçek veriyi tutar ve değişiklik olunca `Notify` / `Bildir` çağırır.
- **ConcreteObserver:** Bildirim geldiğinde yapılacak davranışı belirler.

### Ne Zaman Kullanılır?

- YouTube kanalına abone olan kullanıcılara yeni video bildirimi göndermek.
- Sipariş oluşunca e-posta, SMS ve log servislerini bilgilendirmek.
- Borsa fiyatı değişince ekranları veya alarmları güncellemek.
- UI olaylarında bir değişikliğin birden fazla bileşeni etkilemesi.

### C# Sipariş Bildirimi Örneği

```csharp
// OBSERVER arayüzü — Abonelerin uygulaması gereken sözleşme
public interface IObserver
{
    void Guncelle(string siparisNo);
}

// SUBJECT arayüzü — Yayıncının uygulaması gereken sözleşme
public interface ISubject
{
    void AboneEkle(IObserver observer);
    void AboneCikar(IObserver observer);
    void Bildir();
}

// CONCRETE SUBJECT — Sipariş nesnesi (yayıncı)
public class Siparis : ISubject
{
    private readonly List<IObserver> _observers = new();
    private string _siparisNo = "";

    public void SiparisOlustur(string siparisNo)
    {
        _siparisNo = siparisNo;
        Console.WriteLine($"Sipariş oluşturuldu: {siparisNo}");
        Bildir(); // Tüm abonelere haber ver
    }

    public void AboneEkle(IObserver observer) => _observers.Add(observer);
    public void AboneCikar(IObserver observer) => _observers.Remove(observer);

    public void Bildir()
    {
        foreach (var observer in _observers)
            observer.Guncelle(_siparisNo);
    }
}

// CONCRETE OBSERVERS — Bildirimi alan servisler
public class EmailServisi : IObserver
{
    public void Guncelle(string siparisNo)
        => Console.WriteLine($"📧 Email gönderildi: Sipariş {siparisNo}");
}

public class SmsServisi : IObserver
{
    public void Guncelle(string siparisNo)
        => Console.WriteLine($"📱 SMS gönderildi: Sipariş {siparisNo}");
}

public class LogServisi : IObserver
{
    public void Guncelle(string siparisNo)
        => Console.WriteLine($"📝 Log kaydedildi: Sipariş {siparisNo}");
}

// KULLANIM
class Program
{
    static void Main()
    {
        var siparis = new Siparis();

        // Aboneleri ekle
        siparis.AboneEkle(new EmailServisi());
        siparis.AboneEkle(new SmsServisi());
        siparis.AboneEkle(new LogServisi());

        // Sipariş oluştur — tüm servisler otomatik bilgilendirilir
        siparis.SiparisOlustur("SIP-001");
    }
}
```

### Neden İki Interface Var? (ISubject + IObserver)

- **ISubject:** Yayıncının sözleşmesidir; abone ekleme, çıkarma ve bildirim gönderme metotlarını zorunlu kılar.
- **IObserver:** Abonenin sözleşmesidir; güncelleme metodunu zorunlu kılar.
- Bu ayrım sayesinde Subject, observer'ların ne yaptığını bilmek zorunda kalmaz; sadece listeyi tutar ve bildirim gönderir → **gevşek bağlılık (loose coupling)**.

### Diğer Desenlerle Karşılaştırma

- **Observer:** Durum değiştiğinde bağlı nesnelere haber verir.
- **Proxy:** Gerçek nesneye erişimi kontrol eder.
- **Decorator:** Aynı arayüzü koruyarak davranış ekler.
- **Adapter:** Uyumsuz arayüzleri birbirine uydurur.

**Hatırla:** Observer'da Subject, observer'ların ne yaptığını bilmek zorunda değildir; sadece listeyi tutar ve bildirim gönderir. Bu sayede sistem gevşek bağlı olur.

---

## Ders 9 Ek — Composite (Bileşik) Tasarım Deseni

Kaynak: `pdf/Ders9.pdf`, s. 6-8

### Tanım

Composite, nesneleri ağaç yapıları halinde düzenleyerek parça-bütün hiyerarşileri oluşturmayı sağlayan **yapısal** bir tasarım desenidir. Tekil nesnelerle ve nesne bileşimlerini aynı şekilde (tek tip arayüz üzerinden) ele almayı mümkün kılar.

### Roller

1. **Component:** Hem yaprak hem bileşik nesnelerin uyguladığı ortak arayüz.
2. **Leaf (Yaprak):** Alt elemanı olmayan tekil nesne.
3. **Composite (Bileşik):** İçinde başka nesneler tutabilen nesne (recursive yapı).

### Ne Zaman Kullanılır?

- Dosya sistemi (klasör → dosya/alt klasör)
- Organizasyon şeması (departman → çalışanlar/alt departmanlar)
- UI widget ağacı (panel → buton/label/alt panel)
- Menü yapıları (ana menü → alt menüler → öğeler)

### C# Kod Örneği 1 — Çalışan/Departman (Maaş Hesaplama)

```csharp
// COMPONENT — Ortak arayüz
public interface ICalisan
{
    decimal MaasHesapla();
    void BilgiGoster(string girinti);
}

// LEAF — Tekil çalışan (yaprak düğüm)
public class Calisan : ICalisan
{
    private string isim;
    private decimal maas;

    public Calisan(string isim, decimal maas)
    {
        this.isim = isim;
        this.maas = maas;
    }

    public decimal MaasHesapla() => maas;

    public void BilgiGoster(string girinti)
    {
        Console.WriteLine($"{girinti}{isim} - Maaş: {maas}");
    }
}

// COMPOSITE — Departman (alt elemanlar içeren bileşik düğüm)
public class Departman : ICalisan
{
    private string isim;
    private List<ICalisan> uyeler = new List<ICalisan>();

    public Departman(string isim) { this.isim = isim; }

    public void Ekle(ICalisan calisan) => uyeler.Add(calisan);

    public decimal MaasHesapla()
    {
        decimal toplam = 0;
        foreach (var uye in uyeler)
            toplam += uye.MaasHesapla();
        return toplam;
    }

    public void BilgiGoster(string girinti)
    {
        Console.WriteLine($"{girinti}{isim}");
        foreach (var uye in uyeler)
            uye.BilgiGoster(girinti + "  ");
    }
}

// KULLANIM
var dev1 = new Calisan("Ali", 15000);
var dev2 = new Calisan("Ayşe", 18000);
var yazilimDept = new Departman("Yazılım Departmanı");
yazilimDept.Ekle(dev1);
yazilimDept.Ekle(dev2);

var mudur = new Calisan("Mehmet (Müdür)", 30000);
var sirket = new Departman("Şirket");
sirket.Ekle(yazilimDept);
sirket.Ekle(mudur);

sirket.BilgiGoster("");
// Şirket
//   Yazılım Departmanı
//     Ali - Maaş: 15000
//     Ayşe - Maaş: 18000
//   Mehmet (Müdür) - Maaş: 30000

Console.WriteLine($"Toplam Maaş: {sirket.MaasHesapla()}"); // 63000
```

### C# Kod Örneği 2 — Şekil Grubu

```csharp
public interface IShape
{
    void Draw();
}

public class Circle : IShape
{
    public void Draw() => Console.WriteLine("Daire çizildi");
}

public class Square : IShape
{
    public void Draw() => Console.WriteLine("Kare çizildi");
}

// COMPOSITE
public class ShapeGroup : IShape
{
    private List<IShape> shapes = new List<IShape>();

    public void Add(IShape shape) => shapes.Add(shape);

    public void Draw()
    {
        Console.WriteLine("--- Grup çiziliyor ---");
        foreach (var shape in shapes)
            shape.Draw();
    }
}
```

### Composite'in Temel Özelliği

- **Recursive (özyinelemeli) yapı:** Bileşik nesne, kendi tipindeki nesneleri içerebilir.
- Leaf ve Composite aynı arayüzü uygular → Client farkı bilmez.
- Ağaç yapısı doğal olarak oluşur.

---

## Ders 10 Ek — State (Durum) Tasarım Deseni

Kaynak: `pdf/Ders10.pdf`, s. 7-10

### Tanım

State, bir nesnenin davranışının iç durumuna göre değişmesini sağlayan **davranışsal** bir tasarım desenidir. Nesne aynı kalır ama farklı durumlarda farklı davranır — sanki sınıfı değişmiş gibi görünür.

### Roller

1. **Context (Ana nesne):** Durumu tutar ve davranışı ilgili state'e delegeler.
2. **State (Arayüz / Abstract class):** Tüm durumların implement edeceği ortak davranışları tanımlar.
3. **Concrete States (Somut durumlar):** Her biri farklı bir davranışı temsil eder.

### Neden Kullanılır?

- Büyük `if-else` veya `switch` bloklarını ortadan kaldırır.
- Her durumu ayrı sınıfta izole eder.
- Yeni durum eklemek mevcut kodu değiştirmez (OCP).

### C# Kod Örneği 1 — Ödeme Durumları

```csharp
// STATE arayüzü
public interface IOdemeState
{
    void IslemYap(Odeme odeme);
}

// CONCRETE STATES
public class BeklemdeState : IOdemeState
{
    public void IslemYap(Odeme odeme)
    {
        Console.WriteLine("Ödeme bekleniyor...");
    }
}

public class OdendiState : IOdemeState
{
    public void IslemYap(Odeme odeme)
    {
        Console.WriteLine("Ödeme başarılı! Sipariş hazırlanıyor.");
    }
}

public class IptalState : IOdemeState
{
    public void IslemYap(Odeme odeme)
    {
        Console.WriteLine("Ödeme iptal edildi.");
    }
}

// CONTEXT
public class Odeme
{
    public IOdemeState State { get; set; }

    public Odeme(IOdemeState state)
    {
        State = state;
    }

    public void IslemYap()
    {
        State.IslemYap(this);
    }
}

// KULLANIM
var odeme = new Odeme(new BeklemdeState());
odeme.IslemYap(); // Ödeme bekleniyor...

odeme.State = new OdendiState();
odeme.IslemYap(); // Ödeme başarılı! Sipariş hazırlanıyor.

odeme.State = new IptalState();
odeme.IslemYap(); // Ödeme iptal edildi.
```

### C# Kod Örneği 2 — State + Observer Birleşik (Sipariş Takip)

```csharp
// Observer + State birlikte çalışır
public interface ISiparisGozlemci
{
    void Guncelle(string siparisNo, string durum);
}

public interface ISiparisDurumu
{
    string DurumAdi { get; }
    void IslemYap(Siparis siparis);
}

public class HazirlaniyorDurumu : ISiparisDurumu
{
    public string DurumAdi => "Hazırlanıyor";
    public void IslemYap(Siparis siparis)
    {
        Console.WriteLine("Sipariş hazırlanıyor.");
    }
}

public class KargoyaVerildiDurumu : ISiparisDurumu
{
    public string DurumAdi => "Kargoya Verildi";
    public void IslemYap(Siparis siparis)
    {
        Console.WriteLine("Sipariş kargoya verildi.");
    }
}

public class Siparis
{
    private List<ISiparisGozlemci> gozlemciler = new();
    public ISiparisDurumu Durum { get; set; }
    public string SiparisNo { get; set; }

    public Siparis(string siparisNo, ISiparisDurumu durum)
    {
        SiparisNo = siparisNo;
        Durum = durum;
    }

    public void GozlemciEkle(ISiparisGozlemci g) => gozlemciler.Add(g);

    public void DurumDegistir(ISiparisDurumu yeniDurum)
    {
        Durum = yeniDurum;
        Durum.IslemYap(this);
        foreach (var g in gozlemciler)
            g.Guncelle(SiparisNo, Durum.DurumAdi);
    }
}
```

### State vs Observer Farkı

- **State:** Nesnenin **kendi iç davranışını** duruma göre değiştirir.
- **Observer:** Durumu **dışarıdaki nesnelere** bildirir.
- Birlikte kullanılabilirler: State durum değiştirince Observer bildirimi tetiklenir.
