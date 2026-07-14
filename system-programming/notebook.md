# Sistem Programlama Defter Notları

## 2026-05-14 - 0. Bölüm: C Temelleri (Sistem Programlama Bakışı)

**Kısa Tanım:** C sistem programlamada işletim sistemiyle konuşulan ara katmandır; hata kontrolü, tanımlı davranış ve bellek modelinin doğru anlaşılması kritiktir.

**Anahtar Kavramlar:**
- Derleme: `gcc -Wall -Wextra -g`; uyarıyı hata gibi gör (`-Werror`).
- Kesin boy tipler: `<stdint.h>` (`int32_t`, `uint64_t`) ve `<stddef.h>` (`size_t` → `%zu`).
- Pointer: `&` adres alma, `*` dereference; NULL/uninitialized pointer dereference UB.
- Array/pointer: dizi adı ilk elemana pointer gibi davranır; `sizeof(a)` ile `sizeof(p)` farklıdır.
- Hata sözleşmesi: dönüş değeri durum içindir, çıktı out parametre ile.
- `argv` parse + `fgets`+`strtol` (scanf yerine).
- Stack vs Heap: yerel diziler stack'te (overflow riski), `malloc/free` heap'te.
- `realloc` altın kural: `tmp = realloc(a, n); if (tmp) a = tmp;`
- Function pointer: callback için (`qsort`, sinyal handler, event loop).

**Örnek / Komut:**
```c
int *tmp = realloc(a, 200 * sizeof(int));
if (tmp == NULL) { /* a hala geçerli */ }
else { a = tmp; }
```

**Hatırla:** Sistem programlamada "boyut bilmeden kopyalama yok" prensibi güvenlik için kritiktir; her `malloc`'a bir `free` kuralı leak/double-free/UAF hatalarını engeller.

---

## 1.1 fork() Nedir ve Nasıl Çalışır?
- `fork()` sistem çağrısı, kendisini çağıran sürecin (parent) neredeyse birebir kopyasını (child) oluşturur.
- Parent süreç için `fork()` child'ın PID'sini döndürürken, child süreç için `0` döndürür. Eğer hata olursa `-1` döner.
- **Parent ve Child Farkları:**
  - Bellek alanları (adres uzayları) ayrıdır (Copy-on-Write mantığıyla çalışır, ancak mantıksal olarak izoledir).
  - Child sürecin kendine ait benzersiz bir PID'si (Process ID) vardır ve PPID (Parent Process ID) ebeveynin PID'sine eşittir.

## 1.2 exec() Sistem Çağrısı
- `exec()` sistem çağrısı ailesi (execl, execv vb.), mevcut sürecin adres uzayını (bellek imajını) siler ve yerine tamamen yeni bir program yükler.
- Yeni bir süreç yaratmaz (PID değişmez); sadece aynı kimlikle farklı bir program çalışmaya başlar.
- Genellikle `fork()` ile bir child süreç oluşturulduktan hemen sonra, child'ın farklı bir iş yapması için kullanılır.

## 1.3 PID (Process ID) Kavramı ve Özel PID'ler
- **PID:** Her sürecin sistemde benzersiz bir kimlik numarası vardır.
- **PID 0 (Swapper/Idle):** İşletim sisteminin boşta olduğu zaman çalışan donanım seviyesi süreçtir. Kullanıcı alanında görülmez.
- **PID 1 (init/systemd):** Çekirdek (kernel) yüklendikten sonra başlayan **ilk** kullanıcı seviyesi süreçtir. Sistemin geri kalan tüm süreçlerinin atasıdır. Yetim (orphan) kalan süreçleri sahiplenir.
- Bir sistemde aynı anda bulunabilecek maksimum süreç sayısı `pid_max` değeri ile sınırlıdır (genellikle varsayılan 32768'dir, ancak modern sistemlerde daha yüksektir).

## 2026-05-14 - 6. exec Ailesi ve fork (Ders 2 detay)

**Kısa Tanım:** `exec` ailesi mevcut süreç adres uzayını yeni programla değiştirir; `fork` yeni süreç (child) üretir. Copy-on-Write `fork+exec` desenini hızlandırır.

**Anahtar Kavramlar:**
- exec çeşitleri: `l` (liste arg) vs `v` (vektör arg), `p` (PATH araması), `e` (kendi env).
- Asıl çağrı `execve`; diğerleri libc wrapper.
- `fork` dönüşü: parent=child pid, child=0, hata=-1.
- COW: sayfa yazılana kadar paylaşılır; `fork+exec` ucuzdur.
- `vfork`: tarihi, risklidir; modern kodda kullanılmaz.

**Örnek / Komut:** `execvp("ls", argv)` PATH'te arar, vektörle argüman verir.

**Hatırla:** Başarılı bir `exec` geri dönmez; dönüyorsa muhtemelen hata var.

---

## 2026-05-14 - 7. exit, _exit ve atexit

**Kısa Tanım:** `exit` stdio tamponlarını flush eder ve `atexit` fonksiyonlarını LIFO çağırır; `_exit` doğrudan çekirdeğe gider, flush yapmaz.

**Anahtar Kavramlar:**
- `exit(status)` → atexit + flush + tmpfile temizliği + `_exit`.
- `_exit(status)` / `_Exit(status)` → flush yok, atexit yok. fork sonrası child için uygun.
- `on_exit` (glibc) → `atexit`'in `status + arg` alan versiyonu; taşınabilirlik için `atexit` tercih.
- atexit içinde `exit` çağırma; sonsuz özyineleme riski.

**Örnek / Komut:** `atexit(temizle);`

**Hatırla:** Fork edip exec yapmayacaksan ve standart I/O tamponları varsa parent `_exit` ile çıkmamalı; flush kaybolur.

---

## 2026-05-14 - 8. wait/waitpid Status Makroları

**Kısa Tanım:** `wait` herhangi bir child'ı bekler; `waitpid` hedefli bekleme ve seçenek (WNOHANG vb.) sağlar.

**Anahtar Kavramlar:**
- `WIFEXITED(s)` + `WEXITSTATUS(s)`: normal çıkış kodu.
- `WIFSIGNALED(s)` + `WTERMSIG(s)`: hangi sinyalle öldü.
- `WCOREDUMP(s)`: core dump üretti mi (Linux).
- `WIFSTOPPED/WSTOPSIG/WIFCONTINUED`: debugger/job-control.
- `waitpid` pid değerleri: `-1` (her child), `>0` (tam eşleşen), `0` (aynı PG), `<-1` (belirli PG).
- options: `WNOHANG`, `WUNTRACED`, `WCONTINUED`.

**Örnek / Komut:** `waitpid(pid, &status, WNOHANG);`

**Hatırla:** wait edilmeyen child zombie kalır; parent ölürse init reparent eder ve temizler.

---

# 2. Süreç Kimlikleri ve Yetkilendirme

## 2.1 RUID (Real UID) ve EUID (Effective UID)
- **RUID (Real User ID):** Süreci gerçekte başlatan kullanıcının kimliğidir. Genellikle süreçlere sinyal göndermek (örn: `kill`) veya sürecin gerçek sahibini takip etmek için kullanılır.
- **EUID (Effective User ID):** Sistemin izin kontrollerini (bir dosyayı okuma, yazma, silme) yaparken baktığı "aktif" kimliktir.
- Normal şartlarda RUID ve EUID birbirine eşittir. Ancak özel durumlarda (`setuid` biti olan dosyalar çalıştırıldığında) süreç geçici olarak yetki yükseltebilir ve EUID değişir.

## 2.2 setuid Biti ve Saved UID
- **setuid Biti:** Bir çalıştırılabilir dosyanın yetki izinlerinde bulunan özel bir bayraktır. Bu bayrak aktifse, dosyayı kim çalıştırırsa çalıştırsın, programın **EUID'si dosyanın sahibinin UID'sine** eşitlenir. (Örn: `passwd` programının root'a ait olması ve setuid bitinin açık olması).
- **Saved UID (SUID):** Bir süreç yetkilerini geçici olarak değiştirdiğinde (EUID değiştiğinde), eski orijinal EUID değerini güvende tutmak için kopyalandığı yerdir. Süreç, yetkili işlemini bitirince Saved UID'deki değere geri dönerek normal haklarına (düşük yetkiye) geri dönebilir. (Güvenlik için en iyi pratik).

## 2026-05-14 - 9. UID/GID ve setuid/seteuid

**Kısa Tanım:** Real/Effective/Saved UID birbirinden ayrıdır; izin kontrolleri EUID'ye, sahiplik/sinyal RUID'ye dayanır.

**Anahtar Kavramlar:**
- Real UID: kaynak sahipliği ve sinyal yönetimi.
- Effective UID: dosya/sistem yetki kontrolleri.
- Saved UID: yetki yükseltme/azaltma arasında geçiş.
- `setuid`: root ise üçünü de ayarlar; nonroot için sadece real/saved arası geçiş.
- `seteuid`: root herhangi euid'ye geçebilir; nonroot real/saved euid'ye.
- `setuid` biti olan binary (`passwd`) EUID'yi dosya sahibine çekebilir.

**Örnek / Komut:** `seteuid(0)` ile geçici olarak root yetkisine geç, işi bitince eski euid'ye dön.

**Hatırla:** Least privilege ilkesi; gerekli en az yetkiyle çalış, yükseltmeyi dar tut.

---

# 3. Özel Süreç Durumları (Zombi ve Yetim)

## 3.1 Zombi (Zombie) Süreçler
- **Tanım:** Bir child (çocuk) süreç işini bitirip `exit()` ile kapandığında hemen sistemden silinmez. İşletim sistemi, parent (ebeveyn) sürecin çocuğunun nasıl kapandığını (hata var mı vs.) okuyabilmesi için onu "zombi" durumunda bekletir.
- **Tehlikesi:** Zombiler RAM (bellek) harcamazlar, ancak süreç tablosunda (PID listesi) bir satır işgal ederler. Eğer sistemde çok fazla zombi birikirse, yeni süreç oluşturmak için boş PID kalmaz ve sistem kilitlenir.
- **Nasıl Temizlenir?** Parent sürecin `wait()` veya `waitpid()` sistem çağrısını kullanarak çocuğunun ölüm raporunu okuması (reaping) gerekir. Rapor okunduğu an zombi sistemden silinir.

## 3.2 Yetim (Orphan) Süreçler
- **Tanım:** Bir child süreç çalışmaya devam ederken, onu yaratan parent süreç aniden ölürse (kapanırsa), child süreç "yetim" kalır.
- **Ne Olur?** İşletim sistemi yetim kalan süreçleri sahipsiz bırakmaz. Anında `init` veya `systemd` (PID 1) süreci tarafından evlat edinilirler (reparenting). Artık yeni ebeveynleri PID 1 olur.

# 4. İş Kontrolü ve Arka Plan (Daemon) Süreçleri

## 4.1 Süreç Grupları (PGID) ve Oturumlar (Session - SID)
- **Süreç Grubu (PGID):** Birbiriyle ilişkili çalışan süreçlerin (örneğin terminalde boru `|` ile bağlanan komutların) oluşturduğu gruptur. Amacı, iptal sinyali gibi sinyallerin (örn: `CTRL+C`) tek tek süreçlere değil, **tüm gruba aynı anda** gönderilebilmesidir.
- **Oturum (Session / SID):** Bir terminal ekranının temsil ettiği en üst yönetici yapıdır. İçinde birden fazla süreç grubu barındırabilir.
- **SIGHUP Sinyali:** Bir terminal penceresini kapattığında (çarpıya bastığında), işletim sistemi o Oturum'a (Session) ait çalışan tüm süreçlere `SIGHUP` (Hangup - Kapatma) sinyali yollar ve hepsini öldürür. Arka planda (`&` ile) çalıştırsan bile terminal kapanınca süreç ölür.

## 4.2 Daemon (Arka Plan Servisi) Reçetesi
Terminal kapandığında ölmek istemeyen, arka planda sonsuza kadar (veya bilgisayar kapanana kadar) çalışacak olan güvenli bir Daemon yazmak için klasik bir C reçetesi uygulanır:
1. **`fork()` yap ve Parent'ı öldür (`exit(0)`):** Komut satırı (terminal) kullanıcıya geri dönsün diye asıl program kapatılır. Child süreç "yetim" kalarak `init` (PID 1) sürecine bağlanır.
2. **`setsid()` çağır:** Child süreç kendine ait **yeni bir Session (Oturum)** başlatır. Böylece eski terminalin oturumundan tamamen kopar. Eski terminal kapansa bile `SIGHUP` sinyali artık buna ulaşamaz.
3. **`chdir("/")` yap:** Süreç kök dizine (/) geçer. Çünkü eğer bir USB bellek klasöründe başlatılmışsa, o klasör "meşgul" görüneceği için USB bellek bilgisayardan çıkarılamaz.
4. **Dosya Betimleyicileri Kapat (veya yönlendir):** Standart Girdi (0), Standart Çıktı (1) ve Standart Hata (2) kapatılır veya `/dev/null` isimli "kara delik" dosyasına yönlendirilir. (Çünkü artık ekrana yazı yazabileceği bir terminali yoktur).

## 4.3 Daemon Yazarken Sık Yapılan Hatalar
- **Ekrana Çıktı Vermek:** Daemon sürecin içinde `printf` veya `scanf` kullanmak mantıksızdır ve çoğu zaman hataya yol açar. Çıktılar log dosyalarına (`syslog`) yazılmalıdır.
- **Durdurma (Öldürme):** Terminalden tamamen koptuğu için `CTRL+C` ile durdurulamaz. Durdurmak için dışarıdan terminale `pkill program_adi` veya `kill PID` komutu yazılmalıdır.

---

## 2026-05-14 - 10. Process Group, Session ve setsid

**Kısa Tanım:** Process group sinyalleri toplu göndermek için; session terminal/oturum yönetimi için.

**Anahtar Kavramlar:**
- PGID = group leader pid; pipeline tek PG'dir.
- Session foreground/background PG içerir; controlling terminal'e bağlıdır.
- Ctrl-C → SIGINT (fg), terminal kapanışı → SIGHUP, çıkış → SIGQUIT.
- `setsid()` yeni session+PG oluşturur (controlling tty yok). Hata: PG leader ise EPERM.
- Güvenli desen: fork → parent exit → child setsid (daemon).

**Örnek / Komut:** `pid_t s = setsid();`

**Hatırla:** `setsid()` mevcut PG lideri ise başarısız; bu yüzden önce fork edilir.

---

## 2026-05-06 - Ders 3 Not Düzeltme Kuralı

**Kısa Tanım:** Bu bölümdeki notlar `pdf/Ders_3.pdf` slayt sırasına göre yeniden düzenlendi.
**Ana Kural:** Hocanın slaytında birebir kod varsa deftere kod olarak alınır; slaytta kod yoksa harici C kodu yazılmaz, sadece kavram ve yorum mantığı yazılır.
**Önemli:** Bu düzeltme yol haritasında tamamlandı anlamına gelmez; hiçbir tik atılmamalıdır.

---

## 2026-05-06 - 5.1 Süreç Adres Alanı, Sanal Adres Alanı ve Bellek Koruması

**Kısa Tanım:** Linux ve modern işletim sistemleri fiziksel belleği soyutlar; süreçler RAM'e doğrudan erişmez.
**Ana Kavramlar:**
- Çekirdek her sürece sıfırdan başlayan, doğrusal ve benzersiz bir sanal adres alanı verir.
- Sürecin gördüğü adresler sanal adrestir; fiziksel RAM'deki gerçek yerler farklı olabilir.
- Bu yapı süreçlerin birbirinin belleğine karışmasını engeller; buna bellek koruması denir.
**Hocanın Vurgusu:** Program kendi belleğini düzenli ve kendine aitmiş gibi görür; arka tarafta çekirdek ve donanım fiziksel bellek eşlemesini yönetir.
**Hatırla:** Sanal adres alanı swap demek değildir; swap bu sistemin sadece bir parçasıdır.

---

## 2026-05-06 - 5.2 Sayfalar ve Sayfalama

**Kısa Tanım:** Sanal adres alanı, sayfa (page) adı verilen sabit boyutlu bloklardan oluşur.
**Ana Kavramlar:**
- Sayfa boyutunu sistem mimarisi ve donanım belirler.
- Tipik sayfa boyutları: 32-bit sistemlerde 4 KB, 64-bit sistemlerde 8 KB olarak verilmiş.
- MMU, CPU içindeki donanımdır; sanal adresleri fiziksel sayfa çerçevelerine çevirir.
- Uygulamalar sayfa boyutunu çalışma zamanında `getpagesize()` gibi fonksiyonlarla öğrenebilir.
**PDF Örneği:** Sanal adres 0, sanal sayfa 0 içindedir. Eşleme tablosuna göre bu sayfa fiziksel sayfa çerçevesi 2'ye denk gelirse fiziksel aralık 8192-12287 olur.
**PDF Örneği:** Sanal adres 20500, sanal sayfa 5'in başlangıcından 20 bayt ileridedir. Eğer bu sayfa fiziksel olarak 12288 başlangıcına eşlenmişse fiziksel adres 12288 + 20 = 12308 olur.
**Hatırla:** Sayfa numarası değişir, ofset aynı kalır.

---

## 2026-05-06 - 5.3 Geçerli ve Geçersiz Sayfalar

**Kısa Tanım:** Her sanal sayfanın fiziksel bellekte veya disk/swap tarafında karşılığı olmak zorunda değildir.
**Ana Kavramlar:**
- Geçerli sayfa: Fiziksel bellekteki bir sayfayla veya disk üzerindeki takas alanı/dosyayla ilişkilendirilmiş sayfadır.
- Geçersiz sayfa: Hiçbir fiziksel veya ikincil alanla eşleşmeyen, kullanılmayan adres bölgesidir.
- Sanal adres alanı doğrusal görünür; fakat içinde erişilemeyen, işletim sistemine ait veya boş birçok alan bulunabilir.
**Hatırla:** Sanal adres alanının büyük ve doğrusal görünmesi, her adresin kullanılabilir olduğu anlamına gelmez.

**Segmentation Fault ve Page Fault:**

**Kısa Tanım:** Geçersiz sayfaya erişmek segmentation fault; geçerli ama RAM'de olmayan sayfaya erişmek page fault oluşturabilir.
**Ana Kavramlar:**
- Segmentation fault / violation: Geçersiz veya izin verilmeyen sayfaya erişimdir; süreç genellikle `SIGSEGV` alır.
- Page fault: Sayfa geçerlidir ama o anda RAM'de değildir; swap veya ikincil depolama tarafında olabilir.
**Page Fault Sırası:**
- Süreç ilgili adrese erişmeye çalışır.
- MMU page fault üretir.
- Çekirdek bu kesmeyi yakalar.
- Veri RAM'e taşınır.
- Program çalışmasına devam eder.
**Hatırla:** Page fault her zaman program hatası değildir; sanal bellek sisteminin normal işleyişi olabilir.

---

## 2026-05-06 - 5.4 TLB ve Sayfa Tablosu

**Kaynak Notu:** Gönderilen `Ders_3.pdf` içinde bu başlık ayrı bir TLB slaytı olarak görünmüyor; PDF'te MMU'nun sanal adresleri fiziksel sayfa çerçevelerine çevirdiği anlatılıyor.
**Kısa Tanım:** TLB, sanal adres çevirilerini hızlandırmak için kullanılan donanımsal önbellek mantığıdır.
**Ana Kavramlar:**
- Sayfa tablosu sanal sayfa ile fiziksel çerçeve ilişkisini tutar.
- MMU çeviri yaparken bu eşlemeyi kullanır.
- TLB, sık kullanılan çevirileri hızlı erişim için tutabilir.
**Hatırla:** Bu başlıkta hocanın gönderdiği PDF'te birebir C kodu yok; kavram olarak MMU/sayfa tablosu ilişkisiyle beraber düşünülmelidir.

---

## 2026-05-06 - 5.5 Sayfa Değişimi: Paging In / Paging Out

**Kısa Tanım:** Sanal bellek fiziksel bellekten büyük olabilir; bu yüzden çekirdek RAM ile disk/swap arasında sayfa taşıyabilir.
**Ana Kavramlar:**
- Page-in: Disk/swap tarafındaki sayfanın RAM'e alınmasıdır.
- Page-out: RAM'deki bazı fiziksel sayfaların diske kaydedilmesidir.
- Çekirdek, yeni sayfalara yer açmak için bazı fiziksel sayfaları dışarı atabilir.
- Performans için yakın gelecekte en az kullanılma ihtimali olan veriler swap'ta tutulabilir.
- Bu seçimde LRU gibi algoritmalar kullanılabilir.
**Hatırla:** Çok fazla page-in/page-out olursa disk erişimi yüzünden program yavaşlar.

---

## 2026-05-06 - 5.6 Sanal Adres Hesabı ve Page Fault Kavrama Notu

**Kısa Tanım:** Sanal adres çevrilirken sayfa numarası ve sayfa içi uzaklık (ofset) ayrılır; page fault olduğunda çekirdek sayfayı RAM'e getirip programı sürdürebilir.
**PDF Hesap Mantığı:**
- 0K-4K aralığı, adreslerin 0'dan 4095'e kadar olduğunu belirtir.
- 4K-8K aralığı, adreslerin 4096'dan 8191'e kadar olduğunu belirtir.
- Sanal adres 20500, sanal sayfa 5'in başlangıcından 20 bayt uzaktadır.
- İlgili fiziksel çerçeve 12288'den başlıyorsa fiziksel adres 12288 + 20 = 12308 olur.
**Page Fault Mantığı:** Sayfa geçerli ama RAM'de değilse MMU page fault üretir; çekirdek veriyi RAM'e taşır ve program devam eder.
**Hatırla:** Ofset korunur; sanal sayfa numarası fiziksel çerçeveye çevrilir.

---

## 2026-05-06 - 6.1 Bellek Bölgeleri: Memory Regions / Segments

**Kısa Tanım:** Çekirdek süreç adres alanını izinlerine ve amaçlarına göre bölgelere ayırır.
**Ana Kavramlar:**
- Bu alanlara bellek bölgesi, segment veya mapping denebilir.
- Her sürecin adres alanında 5 temel segment bulunur:
- Text segmenti
- Data segmenti
- BSS segmenti
- Heap segmenti
- Stack segmenti
**Hatırla:** Segmentler sadece isim değil, izin ve kullanım amacı farkıdır.

**Segment Ayrıntıları:**

**Text Segmenti:** Program kodunu, machine code'u, string sabitlerini ve diğer salt okunur verileri içerir. Linux'ta genelde salt okunur işaretlenir.
**Data Segmenti:** Başlangıç değeri açıkça atanmış global ve statik değişkenler burada bulunur. Slayttaki örnek: `int maksimum_hiz = 120;`
**BSS Segmenti:** İlklendirilmemiş global değişkenleri içerir. C standardına göre başlangıç değerleri sıfırdır. Slayttaki örnek: `int count;`
**Heap Segmenti:** Çalışma zamanında programcının manuel olarak tahsis ettiği bellek alanıdır. Aşağıdan yukarıya, yani düşük adresten yüksek adrese doğru büyür.
**Stack Segmenti:** Fonksiyon çağrılarını, yerel değişkenleri ve dönüş adreslerini tutar. Yukarıdan aşağıya, yani yüksek adresten düşük adrese doğru büyür.
**Hatırla:** Başlangıç değeri varsa data, yoksa BSS; manuel tahsis heap, yerel çağrı bilgileri stack.

---

## 2026-05-06 - 6.2 Stack ve Heap Büyüme Yönleri

**Ana Fark:**
- Heap programcı tarafından yönetilir.
- Stack fonksiyon çağrılarıyla otomatik büyür ve fonksiyonlar döndükçe küçülür.
- Heap düşük adresten yüksek adrese doğru büyür.
- Stack yüksek adresten düşük adrese doğru büyür.
**Hatırla:** Yerel değişken deyince stack; çalışma zamanında manuel tahsis deyince heap.

---

## 2026-05-17 - 6.2 Stack ve Heap Büyüme Yönleri

**Kaynak:** ders.md
**Kısa Tanım:** Stack fonksiyon çağrılarıyla otomatik yönetilen alan; heap ise programcının `malloc/calloc/realloc/free` ile çalışma zamanında yönettiği alandır.
**Anahtar Kavramlar:**
- Stack: yerel değişkenler, dönüş adresi ve çağrı bilgilerini tutar; fonksiyon bitince otomatik temizlenir.
- Heap: boyutu çalışma zamanında belli olan veriler için kullanılır; ayrılan alan açıkça serbest bırakılmalıdır.
- Tipik gösterimde heap düşük adresten yüksek adrese, stack yüksek adresten düşük adrese doğru büyür.
- Yerel değişkenin adresini fonksiyon dışına döndürmek hatalıdır; heap ile alınan adres fonksiyon dışında da kullanılabilir.
**Örnek / Komut:** `int *p = malloc(100 * sizeof(int));` ile heap'te 100 elemanlık alan ayrılır; iş bitince `free(p);` gerekir.
**Hatırla:** Stack hızlı ve otomatik; heap esnek ama sorumluluk ister.

---

## 2026-05-06 - 6.3 BSS Segmenti ve Linux Optimizasyonu

**Kısa Tanım:** BSS sadece sıfırlardan oluştuğu için bu sıfırlar binary dosyasına fiziksel olarak yazılmaz.
**Disk Optimizasyonu:** Linker, BSS içindeki sıfırları çalıştırılabilir dosyaya tek tek koymaz; dosya boyutu küçülür.
**Bellek Optimizasyonu:** Çekirdek BSS'i belleğe yüklerken COW mantığıyla sıfırlarla dolu tek bir sayfaya eşleyebilir.
**Sonuç:** Çok büyük ilklendirilmemiş dizi tanımlansa bile, yazma yapılana kadar fiziksel bellekte gerçek yer kaplamayabilir.
**Hatırla:** BSS'in sıfır olması hem disk hem bellek açısından optimizasyon sağlar.

---

## 2026-05-17 - 6.3 BSS Segmenti ve Linux Optimizasyonu

**Kaynak:** ders.md
**Kısa Tanım:** BSS, başlangıç değeri verilmeyen global/statik değişkenlerin bulunduğu segmenttir; bu değişkenler program başında sıfır kabul edilir.
**Anahtar Kavramlar:**
- BSS içindeki sıfırlar çalıştırılabilir dosyaya tek tek yazılmaz; dosyada sadece bu alanın boyutu/konumu tutulur.
- Linker ve loader bu sayede binary dosyasını gereksiz sıfırlarla şişirmez.
- Çekirdek, ihtiyaç olduğunda BSS alanını sıfır dolu sayfalara eşleyebilir.
- Sayfaya yazma yapılana kadar fiziksel RAM tüketimi ertelenebilir; yazma anında gerçek sayfa ayrılır.
**Örnek / Komut:** `static int tablo[1000000];` BSS'te durur; dosyanın içine bir milyon adet sıfır yazılmaz.
**Hatırla:** BSS hem disk alanını hem de başlangıçtaki fiziksel bellek kullanımını azaltan önemli bir optimizasyon noktasıdır.

---

## 2026-05-06 - 6.4 Sayfaların Paylaşımı, Ortak Kütüphaneler ve Copy-on-Write

**Kısa Tanım:** Farklı süreçlerin sanal sayfaları aynı fiziksel RAM sayfasını işaret edebilir; yazma gerektiğinde COW ile özel kopya oluşturulabilir.
**Ana Kavramlar:**
- Örnek olarak ortak `libc` kütüphanesi birçok süreç tarafından paylaşılabilir.
- Paylaşılan veri salt okunur veya okunur-yazılır olabilir.
- Paylaşılan sayfa salt okunur işaretlenmişse ve süreç yazmaya çalışırsa MMU fault üretir.
- Çekirdek bu sayfanın fiziksel kopyasını o süreç için oluşturur.
- Yazma işlemi yeni özel kopyaya yapılır.
**Hocanın Vurgusu:** `fork()` COW sayesinde hızlı çalışır; bütün bellek baştan kopyalanmaz.
**Hatırla:** COW tüm süreci değil, değiştirilen sayfayı kopyalar.

---

## 2026-05-17 - 6.4 Sayfa Paylaşımı ve Copy-on-Write

**Kaynak:** ders.md
**Kısa Tanım:** Birden fazla süreç aynı fiziksel RAM sayfasını gösterebilir; yazma gerektiğinde çekirdek sadece yazılan sayfa için özel kopya oluşturur.
**Anahtar Kavramlar:**
- Sanal adresler farklı süreçlerde ayrı görünür, ama sayfa tabloları aynı fiziksel sayfayı gösterebilir.
- Ortak kütüphaneler ve salt okunur kod sayfaları birçok süreç tarafından paylaşılabilir.
- COW'da paylaşılan sayfa yazmaya karşı korumalı işaretlenir.
- Yazma denemesinde page fault oluşur; çekirdek ilgili süreç için yeni fiziksel sayfa ayırır.
- `fork()` hızlıdır çünkü başlangıçta tüm adres alanı kopyalanmaz, sayfalar yazılana kadar paylaşılır.
**Örnek / Komut:** `fork()` sonrası child bir global değişkene yazarsa parent'ın değeri değişmez; yazılan sayfa child için kopyalanır.
**Hatırla:** COW bütün programı değil, yalnızca yazılan sayfayı kopyalar.

---

## 2026-05-06 - 6.5 fork() Sonrası COW Kavrama Notu

**Kısa Tanım:** `fork()` sonrası parent ve child başlangıçta aynı fiziksel sayfaları paylaşabilir; yazma olunca ilgili sayfa ayrılır.
**Beklenen Yorum:** Child bir sayfaya yazdığında parent'ın değeri değişmiş sayılmaz; çekirdek o sayfayı child için özel kopyaya çevirebilir.
**Hatırla:** COW'un maliyeti `fork()` anında değil, ilk yazma anında ortaya çıkabilir.

---

## 2026-05-06 - 7.1 Dinamik Bellek Tahsisi

**Kısa Tanım:** Dinamik bellek, çalışma zamanında ayrılan bellektir.
**Ana Kavramlar:**
- Otomatik stack değişkenleri ve statik BSS/Data değişkenlerinin boyutu derleme zamanında bilinir.
- Dinamik bellek, ihtiyaç duyulan miktar veya kullanım süresi program başlamadan bilinmiyorsa gerekir.
**PDF Senaryosu:** Kullanıcıdan gelen bir metin dosyasını belleğe okumak istiyoruz. Dosyanın boyutu önceden bilinmez; buffer'ın büyümesi gerekebilir.
**Hatırla:** Boyut çalışma zamanında belli oluyorsa heap/dinamik tahsis düşünülür.

**malloc():**

**Kısa Tanım:** `malloc()` klasik C dinamik bellek tahsis arayüzüdür.
**Ana Kavramlar:**
- Belirtilen `size` kadar bayt bellek tahsis eder.
- Başarılı olursa bloğun başlangıç adresini döndürür.
- Başarısız olursa `NULL` döndürür ve `errno = ENOMEM` yapar.
- Tahsis edilen belleğin içeriği garanti edilmez; çöp veri içerebilir.
**Hatırla:** `malloc()` sonrası içerik sıfır kabul edilmez.

---

## 2026-05-17 - 7.1 malloc(), NULL Kontrolü ve ENOMEM

**Kaynak:** ders.md
**Kısa Tanım:** `malloc(size)` heap'ten `size` baytlık alan ister; başarılı olursa başlangıç adresini, başarısız olursa `NULL` döndürür.
**Anahtar Kavramlar:**
- `malloc()` ile alınan belleğin içeriği sıfır garanti edilmez; çöp veri olabilir.
- Dönüş değeri mutlaka `NULL` kontrolünden geçirilmelidir.
- Başarısız tahsiste neden çoğunlukla yeterli bellek/adres alanı olmamasıdır; `errno` değeri `ENOMEM` olabilir.
- `perror("malloc")` hata sebebini standart hataya yazdırmak için kullanılabilir.
- Başarılı tahsisten sonra iş bitince `free()` çağrılmalıdır.
**Örnek / Komut:** `int *dizi = malloc(100 * sizeof(int)); if (dizi == NULL) { perror("malloc"); exit(EXIT_FAILURE); }`
**Hatırla:** `malloc()` başarılı olsa bile içindeki değerler hazır/sıfır kabul edilmez; önce yaz, sonra oku.

---

## 2026-05-06 - 7.2 Struct ve Dizi Tahsisinde sizeof

**Kısa Tanım:** Dinamik olarak bir struct oluşturmak için `sizeof()` operatörü kullanılır.
**Hocanın Materyalindeki Kod:**

```c
struct treasure_map *map;

/* Yapı (struct) için bellek tahsisi */
map = malloc(sizeof(struct treasure_map));

if (!map) {
    perror("malloc hatası");
    exit(EXIT_FAILURE);
}

/* Artık map göstericisini kullanabiliriz */
map->x = 10;
map->y = 20;
```

**Beklenen Yorum:** `sizeof(struct treasure_map)` kullanımı farklı işlemci mimarilerinde taşınabilirlik sağlar.
**Hatırla:** Struct boyutunu elle yazmak yerine `sizeof` kullanılır.

---

## 2026-05-17 - 7.2 Struct ve Dizi Tahsisinde sizeof

**Kaynak:** ders.md
**Kısa Tanım:** Dinamik tahsiste kaç byte gerektiğini elle yazmak yerine `sizeof` kullanılır; böylece kod tip değişikliklerine ve mimari farklarına daha dayanıklı olur.
**Anahtar Kavramlar:**
- Dizi tahsisinde eleman sayısı `* sizeof(eleman_tipi)` ile çarpılır.
- Struct tahsisinde `sizeof(struct ad)` veya daha güvenli olarak `sizeof *ptr` kullanılabilir.
- `sizeof(int)` her mimaride aynı olmak zorunda değildir; byte hesabını derleyiciye bırakmak taşınabilirlik sağlar.
- Pointer boyutu ile gösterilen verinin boyutu karıştırılmamalıdır.
**Örnek / Komut:** `struct kayit *k = malloc(sizeof *k);` ifadesi `k` hangi struct tipini gösteriyorsa onun kadar alan ayırır.
**Hatırla:** `malloc(100)` yerine çoğu zaman `malloc(100 * sizeof *p)` daha güvenli ve okunur bir kalıptır.

---

## 2026-05-06 - 7.3 Güvenli Tahsis: xmalloc() Sarmalayıcısı

**Kısa Tanım:** `malloc()` başarısız olduğunda durumu tek noktada yönetmek için wrapper fonksiyon kullanılabilir.
**Hocanın Materyalindeki Kod:**

```c
void *xmalloc(size_t size) {
    void *p = malloc(size);
    if (!p) {
        fprintf(stderr, "Bellek yetersiz!\n");
        exit(EXIT_FAILURE);
    }
    return p;
}
```

**Hocanın Slayt Mantığı:** Aynı `malloc()` ve `NULL` kontrolünü her yerde tekrar etmek yerine `xmalloc()` kullanılır.
**Hatırla:** `xmalloc()` başarısızlıkta programı sonlandırır; normal dönüşte geçerli pointer beklenir.

---

## 2026-05-17 - 7.3 Güvenli Tahsis: xmalloc()

**Kaynak:** ders.md
**Kısa Tanım:** `xmalloc()`, `malloc()` çağrısını ve başarısızlık kontrolünü tek yerde toplayan yardımcı fonksiyondur.
**Anahtar Kavramlar:**
- Her tahsisten sonra `NULL` kontrolü yazmak tekrar oluşturur.
- `xmalloc()` başarısızlıkta hata mesajı basıp programı sonlandırabilir.
- Normal dönüşte çağıran taraf geçerli bir pointer aldığını varsayabilir.
- Bu yaklaşım küçük/orta araçlarda hata yönetimini sadeleştirir.
- Kütüphane kodlarında doğrudan `exit()` etmek her zaman uygun olmayabilir; bu tasarım kararıdır.
**Örnek / Komut:** `int *a = xmalloc(100 * sizeof *a);`
**Hatırla:** `xmalloc()` kontrolü ortadan kaldırmaz; kontrolü merkezi bir yere taşır.

---

## 2026-05-06 - 7.4 calloc() ve CoW Destekli Hızlı Sıfırlama

**Kısa Tanım:** `calloc()` dizi tahsisi için eleman sayısı ve eleman boyutu alır; tahsis edilen belleği sıfırlar.
**Ana Kavramlar:**
- `nr`: Eleman sayısı.
- `size`: Her elemanın bayt cinsinden boyutu.
- En önemli fark: Dönen bellek sıfırlanmıştır.
**calloc ve Hızlı Sıfırlama:** Geliştiriciler bazen `malloc()` sonrası `memset()` ile sıfırlar; ancak `calloc()` daha hızlı olabilir.
**Neden:** Çekirdek COW stratejisi sayesinde tamamen sıfırlarla dolu sayfaları hızlıca verebilir.
**Hatırla:** Sıfırlanmış dizi gerekiyorsa `calloc()` güçlü adaydır.

---

## 2026-05-17 - 7.4 calloc() ve Sıfırlanmış Bellek

**Kaynak:** ders.md
**Kısa Tanım:** `calloc(nr, size)` toplamda `nr * size` bayt alan ayırır ve dönen belleği sıfırlanmış halde verir.
**Anahtar Kavramlar:**
- `nr` eleman sayısı, `size` her elemanın byte cinsinden boyutudur.
- `malloc()` belleği sıfırlamaz; `calloc()` sıfırlı başlangıç sağlar.
- Dizi veya struct alanlarının başlangıçta 0 olmasını istiyorsan `calloc()` uygun olabilir.
- Büyük sıfır alanlarda sistem sıfır sayfa/COW benzeri optimizasyonlardan yararlanabilir.
- Başarısız olursa `calloc()` da `NULL` döndürür; kontrol yine gereklidir.
**Örnek / Komut:** `int *dizi = calloc(50, sizeof *dizi);`
**Hatırla:** Sıfırlanmış bellek istiyorsan `calloc()`, rastgele/çöp içerik kabul edilebiliyorsa `malloc()` düşünülebilir.

---

## 2026-05-06 - 7.5 realloc() ve Geçici Pointer

**Kısa Tanım:** `realloc()` mevcut bir bellek bloğunu büyütür veya küçültür.
**Ana Kavramlar:**
- Devamında yeterli yer yoksa veriyi yeni bir adrese kopyalar.
- Eski adres otomatik serbest bırakılabilir.
- Başarısız olursa `NULL` döner.
- Çok önemli: Başarısızlıkta eski bellek sağlam kalır, durumu değişmez.
**Uç Durumlar:**
- `size == 0`: `free(ptr)` ile aynı işi yapar.
- `ptr == NULL`: Yeni bir `malloc(size)` çağrısı gibi davranır.
**Hatırla:** `realloc()` sonucu doğrudan eski pointer'a atanırsa başarısızlıkta eski adres kaybedilebilir.

---

## 2026-05-17 - 7.5 realloc() ve Geçici Pointer

**Kaynak:** ders.md
**Kısa Tanım:** `realloc(ptr, size)` mevcut heap bloğunu yeni boyuta büyütür veya küçültür; gerekirse veriyi yeni adrese taşır.
**Anahtar Kavramlar:**
- Devamında yeterli yer varsa aynı adres büyüyebilir.
- Yeterli yer yoksa yeni blok ayrılır, eski içerik kopyalanır ve eski blok serbest bırakılır.
- Başarısız olursa `NULL` döner ve eski blok hala geçerlidir.
- Sonucu doğrudan eski pointer'a atamak başarısızlıkta eski adresi kaybettirir.
- Güvenli kalıp için önce geçici pointer kullanılır.
**Örnek / Komut:** `int *tmp = realloc(dizi, 200 * sizeof *dizi); if (tmp != NULL) dizi = tmp;`
**Hatırla:** `realloc` adresi değiştirebilir; eski pointer'a güvenli şekilde geçmek için önce `tmp` kullan.

---

## 2026-05-06 - 7.6 free() Kuralları

**Kısa Tanım:** Heap'ten ayrılan dinamik bellek programcı tarafından sisteme manuel olarak iade edilmelidir.
**Ana Kurallar:**
- `ptr`, `malloc`, `calloc` veya `realloc` ile döndürülen bir adres olmalıdır.
- Belleğin sadece bir kısmı `free()` edilemez.
- Tüm blok serbest bırakılmalıdır.
**Hatırla:** Dinamik bellek kalıcıdır; programcı bırakmazsa süreç boyunca tutulur.

---

## 2026-05-17 - 7.6 free() Kuralları

**Kaynak:** ders.md
**Kısa Tanım:** `free()`, `malloc/calloc/realloc` ailesiyle alınan heap bloğunu sisteme geri verir.
**Anahtar Kavramlar:**
- `free()` sadece tahsis bloğunun başlangıç adresiyle çağrılmalıdır.
- Bloğun ortasındaki bir adres serbest bırakılamaz.
- Aynı blok iki kez `free()` edilmemelidir.
- `free()` sonrası pointer'ı kullanmak use-after-free hatasıdır.
- `malloc/calloc/realloc` ile alınan alan `free()` ile; `mmap()` ile alınan alan `munmap()` ile bırakılır.
**Örnek / Komut:** `int *p = malloc(10 * sizeof *p); free(p); p = NULL;`
**Hatırla:** Alanı almak kadar doğru aileyle ve doğru başlangıç adresiyle bırakmak da sistem programlamanın parçasıdır.

---

## 2026-05-06 - 7.7 malloc/calloc/realloc/free Hata Bulma Notu

**Kısa Tanım:** Bu başlıkta hocanın PDF'inde ayrı bir hatalı kod bloğu yok; ancak slaytlardaki kurallardan hata bulma mantığı çıkarılır.
**Bakılacak Hatalar:**
- `malloc()` başarısızlığı kontrol edilmemiş mi?
- `calloc()` yerine sıfırlanmamış bellek yanlış varsayılmış mı?
- `realloc()` başarısız olursa eski pointer kaybediliyor mu?
- `free()` sadece bloğun başlangıç adresine mi uygulanıyor?
- Aynı adres iki kez `free()` ediliyor mu?
**Hatırla:** Dinamik bellek sorularında hata genellikle tahsis ailesi, `NULL` kontrolü veya yanlış serbest bırakma çevresindedir.

---

## 2026-05-17 - 7.7 Dinamik Bellek Hata Bulma

**Kaynak:** ders.md
**Kısa Tanım:** Dinamik bellek kodlarında hata ararken tahsis, yeniden boyutlandırma, kullanım ve serbest bırakma adımları ayrı ayrı kontrol edilir.
**Anahtar Kavramlar:**
- `malloc/calloc` sonucu `NULL` kontrolünden geçirilmiş mi?
- `malloc()` ile gelen bellek sıfır sanılmış mı?
- `realloc()` sonucu önce geçici pointer'a alınmış mı?
- `free()` bloğun başlangıç adresine mi uygulanmış?
- `free()` sonrası pointer tekrar kullanılmış mı veya aynı blok iki kez bırakılmış mı?
**Örnek / Komut:** `p = realloc(p, n);` risklidir; başarısızlıkta eski adres kaybolabilir.
**Hatırla:** Bellek hatalarında önce “adres kayboldu mu, yanlış adres mi free edildi, serbest bırakılmış alan mı kullanıldı?” sorularını sor.

---

## 2026-05-06 - 8.1 Bellek Yönetimi Hataları

**Memory Leak:** Tahsis edilen belleğin serbest bırakılmasının unutulmasıdır; süreç sonlanana kadar bellek tüketimi artar.
**Use-After-Free (UAF):** `free()` çağrıldıktan sonra o adresteki veriyi okumak veya değiştirmektir; güvenlik açıklarına sebep olabilir.
**Double Free:** Aynı adresi iki kez `free()` ile serbest bırakmaya çalışmaktır; heap yapısını bozar ve `SIGABRT` ile programı durdurabilir.
**Hatırla:** Hocanın bu slaytta özellikle hata türü ve sonucunu sorması olasıdır.

---

## 2026-05-17 - 8.1 Memory Leak, Use-After-Free ve Double Free

**Kaynak:** ders.md
**Kısa Tanım:** Heap yönetiminde üç temel hata; ayrılan alanı bırakmamak, bırakılan alanı kullanmak ve aynı alanı iki kez bırakmaktır.
**Anahtar Kavramlar:**
- Memory leak: `malloc/calloc/realloc` ile alınan alanın `free()` edilmeden kaybedilmesi.
- Use-after-free: `free()` edilen pointer üzerinden tekrar okuma/yazma yapılması.
- Double free: aynı heap bloğunun iki kez `free()` edilmesi.
- Bu hatalar bellek tüketimi artışı, çökme, güvenlik açığı veya heap yapısının bozulmasına yol açabilir.
- `free(p); p = NULL;` alışkanlığı bazı tekrar kullanım hatalarını erken görünür yapabilir.
**Örnek / Komut:** `free(p); p[0] = 42;` use-after-free hatasıdır.
**Hatırla:** Heap bloğunun yaşam döngüsü: al, kullan, bir kez bırak, bıraktıktan sonra kullanma.

---

## 2026-05-06 - 8.2 Veri Hizalama

**Kısa Tanım:** İşlemciler bellek adreslerinin veri tipinin boyutunun katları olmasını ister.
**PDF Örneği:** 4 baytlık bir `int`, bellekte 4'e tam bölünebilen bir adreste başlamalıdır.
**Ana Kavramlar:**
- Uyumsuz erişim bazı mimarilerde donanım hatasına yol açabilir.
- Bazı mimarilerde ciddi performans kaybı oluşturabilir.
- Standart `malloc()`, C'deki tüm tipler için doğru hizalanmış bellek döndürmeyi garanti eder.
**Hatırla:** Hizalama hem doğruluk hem performans meselesidir.

**Özel Hizalama: `posix_memalign()`:**

**Kısa Tanım:** Donanım I/O gibi bazı durumlarda sayfa sınırına hizalanmış özel bellek gerekebilir.
**Hocanın Materyalindeki Kod:**

```c
#include <stdlib.h>
int posix_memalign(void **memptr, size_t alignment, size_t size);

char *buf;
/* 256 bayt sınırına hizalanmış 1024 bayt ayır */
int ret = posix_memalign((void **)&buf, 256, 1024);
if (ret) {
    fprintf(stderr, "Hizalama hatası\n");
}
free(buf); // standart free kullanılır
```

**Hatırla:** `posix_memalign()` ile alınan bellek standart `free()` ile serbest bırakılır.

---

## 2026-05-17 - 8.2 Veri Hizalama, Padding ve posix_memalign()

**Kaynak:** ders.md
**Kısa Tanım:** Veri hizalama, bir tipin bellekte işlemcinin verimli/doğru okuyabileceği adres sınırlarından başlamasıdır.
**Anahtar Kavramlar:**
- 4 baytlık `int` genelde 4'e bölünebilen adreste daha verimli okunur.
- Uyumsuz hizalama bazı mimarilerde performans kaybı, bazılarında donanım hatası doğurabilir.
- Struct içinde derleyici alanları hizalamak için padding denilen boş byte'lar ekleyebilir.
- Standart `malloc()`, C'deki normal tipler için yeterli hizalamayı sağlar.
- Sayfa sınırı, DMA veya özel I/O gereksinimlerinde `posix_memalign()` kullanılabilir.
**Örnek / Komut:** `posix_memalign((void **)&buf, 256, 1024);` 1024 baytlık alanı 256 bayt sınırına hizalı ayırmayı dener.
**Hatırla:** Hizalama çoğu sıradan `malloc` kullanımında otomatik çözülür; özel donanım/I/O durumlarında açık hizalama gerekebilir.

---

## 2026-05-06 - 8.3 Program Break: brk() ve sbrk()

**Kısa Tanım:** Çekirdek heap'i program break adı verilen sınırla yönetir.
**Ana Kavramlar:**
- `malloc`, arka planda sistem çağrılarıyla bu sınırı ileri taşıyarak bellek tahsis edebilir.
- `sbrk(0)`: Mevcut heap sınırını öğrenmek için kullanılır.
- `sbrk(increment)`: Sınırı belirtilen bayt kadar artırır.
**Hatırla:** `brk()` ve `sbrk()` düşük seviyeli mekanizmalardır; normal C programında çoğu zaman doğrudan kullanılmaz.

---

## 2026-05-19 - 8.3 Program Break, brk() ve sbrk()

**Kaynak:** ders.md
**Kısa Tanım:** Program break, sürecin veri/heap alanının geleneksel bitiş sınırıdır; heap bu sınır ileri alınarak büyütülebilir.
**Anahtar Kavramlar:**
- `malloc()` küçük/orta tahsislerde arka planda heap sınırını büyütebilir.
- `sbrk(0)` mevcut program break değerini öğrenmek için kullanılır.
- `sbrk(increment)` sınırı belirtilen miktar kadar artırmayı veya azaltmayı dener.
- `brk(addr)` program break değerini doğrudan verilen adrese taşımayı dener.
- Günlük C kodunda `brk/sbrk` doğrudan kullanılmaz; hizalama, yeniden kullanım ve hata yönetimi için `malloc` ailesi tercih edilir.
**Örnek / Komut:** `void *son = sbrk(0);` mevcut heap sınırını öğrenmeye yarar.
**Hatırla:** `brk/sbrk` heap'in alt seviye mekanizmasını anlamak içindir; normal tahsis arayüzün `malloc/free` olmalıdır.

---

## 2026-05-06 - 8.4 Anonim Eşleme: mmap() ve munmap()

**Kısa Tanım:** Çok büyük bellek tahsislerinde glibc, `brk` yerine `mmap` ile doğrudan eşleme kullanabilir.
**Ana Kavramlar:**
- Slaytta genellikle 128 KB civarı büyük tahsislerden söz edilir.
- `mmap`, heap parçalanmasını azaltabilir.
- `mmap` ile alınan bellek `free()` ile değil, `munmap()` ile serbest bırakılır.
**Kod Notu:** PDF sayfa 32'de `/dev/zero` üzerinden `mmap()` ve `munmap()` örnek kodu var. Bu kod uzun olduğu için defterde ana adımlar özetlendi:
- `/dev/zero` okuma-yazma modunda açılır.
- `mmap()` ile 1 sayfalık alan eşlenir.
- Başarısızlık `MAP_FAILED` ile kontrol edilir.
- İş bitince dosya tanımlayıcısı kapatılır.
- Bellek sıfırlarla dolu alan gibi kullanılır.
- Sonunda `munmap(p, getpagesize())` ile iade edilir.
**Hatırla:** `malloc/free` ailesi ile `mmap/munmap` ailesi karıştırılmaz.

---

## 2026-05-19 - 8.4 Anonim mmap() ve munmap()

**Kaynak:** ders.md
**Kısa Tanım:** `mmap()`, dosya destekli veya anonim bir bellek bölgesini sürecin sanal adres alanına eşler; büyük tahsislerde heap yerine kullanılabilir.
**Anahtar Kavramlar:**
- Anonim `mmap()`, dosya içeriğine bağlı olmayan bellek alanı oluşturur.
- Büyük bloklarda `glibc`, `brk()` yerine arka planda `mmap()` kullanabilir.
- `mmap()` ile alınan alan `MAP_FAILED` ile kontrol edilir, `NULL` ile değil.
- `mmap()` alanı `free()` ile değil `munmap(adres, boyut)` ile bırakılır.
- `malloc/free` ailesi ile `mmap/munmap` ailesi karıştırılmamalıdır.
**Örnek / Komut:** `mmap(NULL, boyut, PROT_READ | PROT_WRITE, MAP_PRIVATE | MAP_ANONYMOUS, -1, 0);`
**Hatırla:** `mmap()` bellek bölgesi eşler; bırakırken adresle beraber boyutu da `munmap()`a vermek gerekir.

---

## 2026-05-06 - 8.5 mallopt(), mallinfo() ve malloc_stats()

**mallopt():** Uygulamanın doğasına göre tahsis algoritmalarında ince ayar yapabilir.
**Hocanın Materyalindeki Kod:**

```c
#include <malloc.h>
int mallopt(int param, int value);

mallopt(M_MMAP_THRESHOLD, 64 * 1024);
```

**Parametre Notları:**
- `M_MMAP_THRESHOLD`: Bir bellek talebinin `mmap` ile karşılanması için gereken minimum boyuttur.
- `M_MMAP_MAX`: Aynı anda kaç tane `mmap` alanı açılabileceğini sınırlar.
- `M_MXFAST`: Çok küçük blokların fast bin listesinde tutulmasını etkiler.
- `M_TOP_PAD`: `sbrk` ile heap genişletilirken fazladan padding bırakır.
- `M_CHECK_ACTION`: Bellek hatalarında ne yapılacağını belirler.
**mallinfo():** Heap'in genel kullanım istatistiklerini verir.
**malloc_stats():** Bu bilgileri doğrudan standart hataya (`stderr`) yazdırır.

---

## 2026-05-19 - 8.5 mallopt(), mallinfo() ve malloc_stats()

**Kaynak:** ders.md
**Kısa Tanım:** Bu arayüzler glibc `malloc` tahsisçisinin davranışını ayarlamak veya heap kullanımını gözlemek için kullanılır.
**Anahtar Kavramlar:**
- `mallopt()` bazı malloc parametrelerini ayarlayabilir; normal uygulamalarda çoğu zaman gerekmez.
- `M_MMAP_THRESHOLD`, hangi büyüklükten sonra tahsisin `mmap()` ile karşılanabileceğini etkiler.
- `M_MMAP_MAX`, aynı anda kullanılabilecek `mmap` tabanlı tahsis sayısını sınırlayabilir.
- `mallinfo()` heap istatistikleri verir; modern sistemlerde sınırlı/eskimiş olabilir.
- `malloc_stats()` özet tahsis bilgilerini standart hataya yazdırır.
**Örnek / Komut:** `mallopt(M_MMAP_THRESHOLD, 64 * 1024);`
**Hatırla:** Bunlar günlük tahsis aracı değil, malloc davranışını izleme/ince ayar araçlarıdır.

---

## 2026-05-06 - 8.6 alloca() ve VLA

**alloca():** Belleği heap yerine geçerli fonksiyonun stack çerçevesinden ayırır.
**Avantaj:** Fonksiyon döndüğünde bellek otomatik serbest kalır; `free()` gerekmez.
**Risk:** Çok büyük boyutlar stack overflow'a neden olur.
**VLA:** C99 ile gelen değişken uzunluklu dizi özelliğidir; dizi boyutu derleme zamanında değil çalışma zamanında belirlenir.
**Hatırla:** `alloca()` ve VLA stack tabanlıdır; küçük ve kısa ömürlü alanlar için düşünülür.

---

## 2026-05-19 - 8.6 alloca() ve VLA

**Kaynak:** ders.md
**Kısa Tanım:** `alloca()` ve VLA, heap yerine stack üzerinde geçici alan ayıran yaklaşımlardır.
**Anahtar Kavramlar:**
- `alloca()` geçerli fonksiyonun stack çerçevesinden alan ayırır.
- Fonksiyon döndüğünde bu alan otomatik geçersiz olur; `free()` çağrılmaz.
- Büyük boyutlar stack overflow'a yol açabilir.
- `alloca()` başarısızlığı `malloc()` gibi güvenilir biçimde raporlamaz.
- VLA, boyutu çalışma zamanında belirlenen stack dizisidir; ömrü fonksiyon çağrısıyla sınırlıdır.
**Örnek / Komut:** `int gecici[n];` ifadesi VLA olabilir; `n` çalışma zamanında belirlenir.
**Hatırla:** Stack tabanlı geçici alanlar küçük ve kısa ömürlü olmalıdır; fonksiyon dışına taşınmamalıdır.

---

## 2026-05-06 - 8.7 memset(), memcpy() ve memmove()

**memset():** Bir bellek bloğunun tamamını belirli bir bayt değeriyle doldurur.
**Kullanım Alanı:** Struct içindeki padding alanlarını veya şifreleri temizlemek için kullanılabilir.
**memcpy():** Hızlıdır; ancak kaynak ve hedef bellek alanları kesişiyorsa veriyi bozabilir.
**memmove():** Kaynak ve hedef kesişse bile güvenli kopyalama yapar; önce geçici belleğe alır.
**Hatırla:** Çakışma ihtimali varsa `memmove()`, yoksa `memcpy()` düşünülebilir.

---

## 2026-05-19 - 8.7 memset(), memcpy() ve memmove()

**Kaynak:** ders.md
**Kısa Tanım:** `memset` bellek bloğunu doldurur; `memcpy` çakışmayan alanları kopyalar; `memmove` çakışma olsa bile güvenli kopyalar.
**Anahtar Kavramlar:**
- `memset(ptr, value, size)` alanı bayt bayt aynı değerle doldurur.
- `memset(&k, 0, sizeof k)` struct veya diziyi sıfırlamak için kullanılabilir.
- `memcpy(hedef, kaynak, boyut)` kaynak ve hedef çakışmıyorsa hızlıdır.
- Kaynak ve hedef üst üste biniyorsa `memcpy` güvenli değildir.
- `memmove()` çakışan alanlarda güvenli taşımayı sağlar; aynı dizi içinde kaydırmada tercih edilir.
**Örnek / Komut:** `memmove(dizi + 1, dizi, n * sizeof *dizi);`
**Hatırla:** Alanlar çakışabilir diyorsan `memmove`; kesin çakışmıyorsa `memcpy`.

---

## 2026-05-06 - 8.8 mlock(), mlockall(), Overcommit ve OOM Killer

**mlock():** Çekirdeğin kullanılmayan sayfaları swap alanına yazmasını engellemek için belirli belleği RAM'de tutmaya çalışır.
**Neden Kullanılır:**
- Güvenlik: Şifrelerin diske yazılması istenmez.
- Performans: Gerçek zamanlı sistemler page fault gecikmesini beklemek istemez.
**mlockall():** Sürecin tüm adres alanını kilitlemek için kullanılır.
- `MCL_CURRENT`: Şu an tahsis edilmiş tüm sayfaları kilitler.
- `MCL_FUTURE`: Gelecekte tahsis edilecek sayfaları da kilitli ayırır.
**Overcommit:** Linux, büyük `malloc` isteklerine gerçek RAM'den fazla olsa bile iyimser biçimde izin verebilir.
**OOM Killer:** Fiziksel RAM ve swap tamamen biterse çekirdek en uygun gördüğü süreci `SIGKILL` ile öldürür.
**Ayar Dosyası:** `/proc/sys/vm/overcommit_memory`
- `0`: Heuristic, varsayılan tahmini davranış.
- `1`: Her zaman izin ver.
- `2`: Sıkı hesaplama, OOM riskini azaltmaya çalışır.
**Hatırla:** `malloc()` başarılı döndü diye fiziksel RAM'in gerçekten ayrıldığı garanti değildir.

---

## 2026-05-19 - 8.8 mlock(), mlockall(), Overcommit ve OOM Killer

**Kaynak:** ders.md
**Kısa Tanım:** `mlock/mlockall` sayfaları RAM'de tutmaya çalışır; overcommit Linux'un bellek isteğine iyimser izin vermesi, OOM Killer ise bellek bitince süreç öldürme mekanizmasıdır.
**Anahtar Kavramlar:**
- `mlock(addr, len)` belirli adres aralığının swap'a atılmasını engellemeye çalışır.
- `mlockall(MCL_CURRENT)` mevcut eşlenmiş sayfaları kilitlemeyi ister.
- `mlockall(MCL_FUTURE)` gelecekte ayrılacak sayfaların da kilitli olmasını ister.
- Overcommit nedeniyle büyük `malloc()` başarılı dönebilir ama fiziksel RAM hemen ayrılmış olmayabilir.
- RAM/swap kritik tükenirse OOM Killer bir süreci `SIGKILL` ile sonlandırabilir.
**Örnek / Komut:** `/proc/sys/vm/overcommit_memory` overcommit davranışını gösteren/ayarlayan Linux dosyasıdır.
**Hatırla:** `malloc()` başarılı döndü diye o kadar fiziksel RAM'in kesin ayrıldığı anlamına gelmez.

---

## 2026-05-06 - 8.9 Bellek Tahsis Yaklaşımları ve Seçim Mantığı

**malloc():** Kolay, basit, yaygın; dönen bellek sıfırlanmış olmak zorunda değildir.
**calloc():** Dizi ayırmayı kolaylaştırır ve belleği sıfırlar; dizi ayırmıyorsak arayüzü gereksiz kalabilir.
**realloc():** Mevcut ayırmayı yeniden boyutlandırır; sadece mevcut ayırmalar için anlamlıdır.
**brk/sbrk:** Heap üzerinde düşük seviyeli kontrol sağlar; çoğu kullanıcı için riskli veya karmaşıktır.
**anonymous mmap:** Büyük eşlemeler için uygundur; paylaşılabilir, koruma seviyeleri ayarlanabilir, kernel'e ipucu verilebilir.
**posix_memalign():** Belleği istenen hizalama sınırında ayırır; hizalama kritik değilse gereksiz olabilir.
**alloca():** Çok hızlıdır ve `free()` gerekmez; büyük ayırmalar için uygun değildir.
**VLA:** `alloca()` gibidir; kapsam dışına çıkınca otomatik serbest kalır.
**Hatırla:** Hangi tahsis yönteminin seçileceği; boyut, ömür, sıfır ihtiyacı, hizalama ihtiyacı ve serbest bırakma biçimine göre belirlenir.

---

## 2026-05-19 - 8.9 Bellek Tahsis Yöntemi Seçimi

**Kaynak:** ders.md
**Kısa Tanım:** Bellek tahsis yöntemi; alanın boyutuna, ömrüne, sıfır ihtiyacına, hizalama ihtiyacına ve nasıl serbest bırakılacağına göre seçilir.
**Anahtar Kavramlar:**
- `malloc()`: Genel amaçlı heap tahsisi; hızlı ve yaygın, bellek sıfırlı gelmez.
- `calloc()`: Başlangıçta sıfırlanmış dizi/alan gerektiğinde uygundur.
- `realloc()`: Var olan heap bloğunu büyütmek veya küçültmek için kullanılır.
- `mmap()`: Büyük veya özel koruma/eşleme isteyen bellek bölgelerinde düşünülebilir; `munmap()` ile bırakılır.
- `posix_memalign()`: DMA, sayfa sınırı veya özel hizalama gerektiğinde kullanılır; `free()` ile bırakılır.
- `alloca()` / VLA: Küçük, kısa ömürlü, fonksiyon içi geçici alanlarda düşünülebilir; stack overflow riski vardır.
**Örnek / Komut:** Sıfırdan başlayan 100 sayaç için `calloc(100, sizeof *sayaclar)` mantıklıdır.
**Hatırla:** Doğru seçim için önce “ne kadar büyük, ne kadar yaşayacak, sıfır/hizalama gerekiyor mu, nasıl bırakılacak?” sorularını sor.

---

## 2026-05-19 - 9.1 Unix Dosya G/Ç Modeli ve File Descriptor

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** Unix'te dosya işlemleri çekirdek üzerinden yapılır; açılan her dosya süreç içinde `int` tipinde bir file descriptor ile temsil edilir.
**Anahtar Kavramlar:**
- Temel akış: `open` ile aç, `read/write` ile işlem yap, `close` ile kapat.
- File descriptor negatif olmayan bir tamsayıdır; çekirdeğin süreç için tuttuğu açık dosyalar tablosuna indeks gibi çalışır.
- Açık dosya kaydında inode bağlantısı, erişim modu ve güncel offset gibi bilgiler tutulur.
- fd dosyanın kendisi değildir; sürecin o dosyaya erişim koludur.
- Sistem çağrılarında hata çoğunlukla `-1` ile bildirilir ve sebep `errno` üzerinden okunur.
**Örnek / Komut:** `int fd = open("notlar.txt", O_RDONLY);`
**Hatırla:** fd = dosyaya verilen çekirdek taraflı numara; dosya işlemlerinin çoğu bu numara üzerinden yürür.

---

## 2026-05-19 - 9.2 Standart File Descriptor'lar

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** Her süreç normalde üç standart file descriptor ile başlar: standart girdi, standart çıktı ve standart hata.
**Anahtar Kavramlar:**
- `0` standart girdi kanalıdır; C tarafında `stdin`, POSIX tarafında `STDIN_FILENO`.
- `1` standart çıktı kanalıdır; C tarafında `stdout`, POSIX tarafında `STDOUT_FILENO`.
- `2` standart hata kanalıdır; C tarafında `stderr`, POSIX tarafında `STDERR_FILENO`.
- `printf()` genelde standart çıktıya, hata mesajları ise standart hataya yazılır.
- Sistem çağrılarında fd numarasıyla çalışılır; bu yüzden `write(STDERR_FILENO, ...)` doğrudan hata kanalına yazar.
**Örnek / Komut:** `write(STDERR_FILENO, "hata\n", 5);`
**Hatırla:** `stdin/stdout/stderr` C kütüphanesi akışlarıdır; `STDIN_FILENO/STDOUT_FILENO/STDERR_FILENO` ise sistem çağrılarında kullanılan fd numaralarıdır.

---

## 2026-05-19 - 9.3 Her Şey Bir Dosyadır Felsefesi

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** Unix'te yalnızca normal metin/binary dosyalar değil; cihazlar, pipe'lar, soketler ve dizinler de file descriptor tabanlı G/Ç arayüzüyle ele alınabilir.
**Anahtar Kavramlar:**
- File descriptor sadece disk dosyalarını temsil etmez.
- Aygıt dosyaları, pipe ve socket gibi iletişim uçları da fd ile yönetilebilir.
- Dizinler de dosya sistemi nesnesidir; özel fonksiyonlarla okunabilir.
- Ortak fikir: okunabilen/yazılabilen birçok kaynak için benzer sistem çağrısı mantığı kullanılır.
- Bu yaklaşım Unix'te araçları birbirine bağlamayı ve yönlendirmeyi kolaylaştırır.
**Örnek / Komut:** Terminal çıktısını dosyaya yönlendirmek: `program > sonuc.txt`
**Hatırla:** “Her şey dosyadır” demek, her şey düz metin dosyasıdır demek değil; birçok kaynağın fd benzeri arayüzle yönetilebilmesidir.

---

## 2026-05-19 - 9.4 open() ve Temel Erişim Modları

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** `open()` bir dosyayı açar ve başarı durumunda file descriptor döndürür; temel erişim modu `O_RDONLY`, `O_WRONLY` veya `O_RDWR` seçeneklerinden biriyle belirlenir.
**Anahtar Kavramlar:**
- `open(yol, flags)` dosyayı belirtilen bayraklarla açmayı dener.
- Başarılı olursa negatif olmayan fd döner; hata durumunda `-1` döner ve `errno` ayarlanır.
- `O_RDONLY`: sadece okuma için açar.
- `O_WRONLY`: sadece yazma için açar.
- `O_RDWR`: hem okuma hem yazma için açar.
- Bu üç temel erişim modundan tam biri seçilmelidir; ek davranış bayrakları `|` ile eklenir.
**Örnek / Komut:** `int fd = open("veri.txt", O_RDONLY);`
**Hatırla:** `open()` dosyanın amacını baştan belirler; sadece okuma açılmış fd ile yazma yapılamaz.

---

## 2026-05-19 - 9.5 open() Davranış Bayrakları

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** `open()` temel erişim moduna ek olarak davranış bayrakları alabilir; bu bayraklar `|` ile birleştirilir.
**Anahtar Kavramlar:**
- `O_APPEND`: her yazmayı dosyanın sonuna ekler; log dosyalarında kullanışlıdır.
- `O_CREAT`: dosya yoksa oluşturur; bu bayrak varsa `mode` parametresi verilmelidir.
- `O_TRUNC`: dosya varsa ve yazma izniyle açılmışsa içeriği sıfırlar.
- `O_EXCL`: `O_CREAT` ile birlikte kullanılır; dosya zaten varsa hata döndürür.
- `O_SYNC`: yazmanın fiziksel diske ulaşmasını bekler; daha güvenli ama yavaştır.
- `O_NONBLOCK`: bekleme gerektiren işlemlerde bloklanmadan hemen dönmeye çalışır.
**Örnek / Komut:** `int fd = open("log.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);`
**Hatırla:** Temel mod dosyayı ne için açtığını, davranış bayrakları açma/yazma davranışının nasıl olacağını belirler.

---

## 2026-05-19 - 9.6 creat() Kısayolu ve open() İlişkisi

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** `creat(yol, mode)`, dosyayı yazmak için oluşturup varsa içeriğini sıfırlayan eski Unix kısayoludur.
**Anahtar Kavramlar:**
- `creat(yol, mode)` başarı durumunda fd döndürür, hata durumunda `-1` döndürür.
- Mantıksal olarak `open(yol, O_WRONLY | O_CREAT | O_TRUNC, mode)` ile aynıdır.
- Dosya yoksa oluşturur.
- Dosya varsa ve izin uygunsa içeriğini siler, boyutu 0 olur.
- Güncel kodda çoğu zaman davranış daha açık görünsün diye `open()` bayraklarıyla yazmak tercih edilir.
**Örnek / Komut:** `int fd = creat("sonuc.txt", 0644);`
**Hatırla:** `creat()` okuma için değil, yazma/oluşturma ve varsa sıfırlama kısayoludur.

---

## 2026-05-20 - 9.7 Yeni Dosya Sahipliği, POSIX İzinleri ve umask

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** Yeni dosya oluşturulurken sahiplik sürecin kimliğinden gelir; verilen `mode` ise `umask` tarafından filtrelenerek gerçek izne dönüşür.
**Anahtar Kavramlar:**
- Yeni dosyanın sahibi genellikle sürecin Effective UID değerinden gelir.
- Grup bilgisi genellikle Effective GID veya dizin kurallarına göre belirlenir.
- İzinler octal değerlerle (`0644`, `0600`) veya POSIX sabitleriyle verilebilir.
- POSIX sabitleri: `S_IRUSR`, `S_IWUSR`, `S_IXUSR`, `S_IRGRP`, `S_IROTH` gibi.
- `umask` izin eklemez; sadece verilen izinlerden bazılarını düşürür.
**Örnek / Komut:** `gerçek_mode = mode & ~umask`
**Hatırla:** Kodda `0644` yazman dosyanın kesin `0644` oluşacağı anlamına gelmez; `umask` son izni daraltabilir.

---

## 2026-05-20 - 9.8 read() Dönüş Değerleri, EOF ve EINTR

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** `read(fd, buf, len)` en fazla `len` byte okumayı dener; dönüş değeri okumanın sonucunu bildirir.
**Anahtar Kavramlar:**
- Dönüş değeri `len` ise istenen miktarın tamamı okunmuştur.
- Dönüş değeri `0 < n < len` ise kısmi okuma vardır; bu hata olmak zorunda değildir.
- Dönüş değeri `0` ise EOF, yani dosya sonudur; EOF hata değildir.
- Dönüş değeri `-1` ise hata oluşmuştur ve `errno` kontrol edilir.
- `errno == EINTR` ise çağrı sinyalle kesilmiş olabilir; çoğu durumda `read()` tekrar denenir.
**Örnek / Komut:** `while ((n = read(fd, buf, len)) == -1 && errno == EINTR) { }`
**Hatırla:** `read()` her zaman istediğin kadar byte döndürmek zorunda değildir; kısmi okuma ihtimaliyle kod yaz.

---

## 2026-05-20 - 9.9 close() ve Dönüş Değeri Kontrolü

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** `close(fd)` açık file descriptor'ı kapatır; dönüş değeri kontrol edilmelidir çünkü bazı G/Ç hataları kapanış sırasında raporlanabilir.
**Anahtar Kavramlar:**
- `close(fd)` süreçteki açık dosya tablosu girişini serbest bırakır.
- Başarı durumunda `0`, hata durumunda `-1` döndürür.
- `EBADF` geçersiz fd anlamına gelebilir.
- Yazma işlemleri performans için geciktirilebilir; buna deferred write denir.
- Disk/yazma hataları bazen `write()` sırasında değil, `close()` sırasında ortaya çıkabilir.
**Örnek / Komut:** `if (close(fd) == -1) { perror("close"); }`
**Hatırla:** Özellikle yazılmış dosyalarda `close()` dönüşünü görmezden gelmek veri kaybı hatasını kaçırmana neden olabilir.

---

## 2026-05-20 - 9.10 lseek() ve Dosya Konumlandırma

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** `lseek()` açık bir dosyanın güncel offset değerini değiştirir; okuma/yazma yapmadan dosya içinde konum atlatır.
**Anahtar Kavramlar:**
- `lseek(fd, pos, SEEK_SET)` konumu dosya başlangıcına göre `pos` yapar.
- `lseek(fd, pos, SEEK_CUR)` konumu mevcut offset'e göre değiştirir; `pos` negatif olabilir.
- `lseek(fd, pos, SEEK_END)` konumu dosya sonuna göre belirler.
- `lseek()` veri okumaz veya yazmaz; yalnızca offset değiştirir.
- Pipe/socket gibi rasgele erişim desteklemeyen kaynaklarda başarısız olur ve `-1` döner.
**Örnek / Komut:** `lseek(fd, 0, SEEK_SET);` dosya konumunu başa alır.
**Hatırla:** `lseek()` normal dosyalarda konumlandırma içindir; pipe üzerinde anlamlı değildir.

---

## 2026-05-20 - 9.11 Dosya G/Ç Kavrama Görevi

**Kaynak:** `pdf/Ders_4__1_.pdf`, ders.md
**Kısa Tanım:** Güvenli dosya okuma akışı; dosyayı `open()` ile açmak, `read()` dönüşlerini doğru yorumlamak, çıktıyı uygun fd'ye yazmak ve `close()` sonucunu kontrol etmektir.
**Anahtar Kavramlar:**
- `open()` başarısız olursa `-1` döner; `perror()` ile sebep yazdırılır.
- `read()` pozitif değer döndürürse o kadar byte okunmuştur.
- `read()` `0` döndürürse EOF'tur.
- `read()` `-1` döndürürse hata vardır; `EINTR` durumunda tekrar denenebilir.
- Hata/diagnostic çıktı için `STDERR_FILENO` kullanılabilir.
- `close()` sonucu da kontrol edilmelidir.
**Örnek / Komut:** `open -> read döngüsü -> write(STDERR_FILENO, ...) -> close`
**Hatırla:** Güvenli sistem çağrısı kodu her dönüş değerini ciddiye alır.

---

## 2026-05-30 - 10.1 Çoklu G/Ç (Multiplexing) Problemi

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 26
**Kısa Tanım:** Bir program birden fazla fd'den aynı anda veri beklemek zorunda kalabilir; blocking `read()` bunu engeller, çözüm multiplexing'dir.
**Anahtar Kavramlar:**
- Normal `read()` engelleyici (blocking) çalışır; fd'de veri yoksa program o noktada takılır.
- Bir fd'de beklerken diğer fd'lerde hazır olan veri kaçırılır veya program donmuş gibi davranır.
- Çözüm: işletim sistemine "hangi fd okumaya/yazmaya hazır?" diye sormaktır → **multiplexing**.
- Tek bir iş parçacığı (thread) ile yüzlerce soket/fd bu modelle yönetilebilir.
- Temel araçlar: `select()`, `poll()`, Linux'a özgü `epoll`.
**Örnek / Komut:** Bir chat sunucusu 500 istemciden aynı anda veri bekler; her biri için ayrı thread açmak yerine tek thread multiplexing ile tüm soketleri izler.
**Hatırla:** Multiplexing olmadan çoklu bağlantı yönetmek ya thread başına fd ya da non-blocking döngü (busy-wait) gerektirir; ikisi de verimsizdir.

---

## 2026-05-30 - 10.2 select() ve poll() Yaklaşımları

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 27-28
**Kısa Tanım:** `select()` ve `poll()` çekirdekten "hangi fd'ler hazır?" bilgisini alan ilk nesil multiplexing araçlarıdır; ikisi de fd sayısı arttıkça O(n) performans düşüşü yaşar.
**Anahtar Kavramlar:**
- `select()`: izlenecek fd'leri `fd_set` kümeleri içinde tutar (okuma, yazma, hata için ayrı küme).
- Sabit boyutlu `fd_set` yapısı (genelde `FD_SETSIZE = 1024`) → fd numarası bu sınırı aşarsa sorun.
- Her `select()` çağrısında küme yeniden kurulmalı — çekirdek kümeleri değiştirir.
- `poll()`: `fd_set` yerine `struct pollfd` dizisi kullanır; fd ve izlenecek olay (event) bilgisi daha açık.
- `poll()`'da sabit üst sınır yok; dizi boyutu kadar fd izlenebilir.
- Her iki yöntemde de çekirdek tüm listeyi baştan sona tarar → büyük fd kümelerinde performans O(n) ile düşer.
**Örnek / Komut:** `select(maxfd+1, &readfds, NULL, NULL, &timeout);` — readfds kümesindeki fd'lerden hangisi okumaya hazır?
**Hatırla:** `select` ve `poll` küçük fd sayılarında yeterlidir; binlerce bağlantıda `epoll` gerekir.

---

## 2026-05-30 - 10.3 Linux epoll Arayüzü

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 29-30
**Kısa Tanım:** `epoll`, Linux çekirdeğinin ölçeklenebilir G/Ç izleme çözümüdür; çekirdek sadece hazır olan fd'leri bildirir, tüm listeyi taramaz.
**Anahtar Kavramlar:**
- `epoll_create1(0)`: çekirdekte bir epoll nesnesi oluşturur; dönen fd ile bu nesneye erişilir.
- `epoll_ctl(epfd, EPOLL_CTL_ADD, fd, &event)`: izlenecek fd'yi epoll nesnesine ekler.
- `epoll_ctl(epfd, EPOLL_CTL_DEL, fd, NULL)`: fd'yi izleme listesinden çıkarır.
- `epoll_wait(epfd, events, maxevents, timeout)`: hazır olan fd'leri bekler ve sadece hazır olanları döndürür.
- Performans: O(1) — 10.000 fd olsa bile sadece hazır olan 5 tanesi döner, geri kalanı taranmaz.
- `select/poll` her çağrıda O(n) tarar; `epoll` sadece hazır olanları raporlar.
**Örnek / Komut:** `epoll_create1(0)` → `epoll_ctl(ADD)` → döngü: `epoll_wait()` → hazır fd'leri işle.
**Hatırla:** `epoll` Linux'a özgüdür; taşınabilirlik gerekiyorsa `poll()` tercih edilebilir, ama performans kritikse `epoll` zorunludur.

---

## 2026-05-30 - 10.4 Level-Triggered (LT) ve Edge-Triggered (ET) Tetikleme

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 31
**Kısa Tanım:** `epoll` olay bildirimlerinde iki mod vardır: Level-Triggered (varsayılan) ve Edge-Triggered. ET modunda fd mutlaka non-blocking olmalıdır.
**Anahtar Kavramlar:**
- **Level-Triggered (LT):** Tampon bellekte veri olduğu sürece `epoll_wait()` her çağrıldığında tetiklenmeye devam eder. Güvenlidir ama fazla sistem çağrısı üretir.
- **Edge-Triggered (ET):** Sadece yeni veri geldiğinde (durum değiştiğinde) bir kez tetiklenir. Veri tamamen tüketilmezse yeni veri gelene kadar bir daha bildirim alınmaz.
- **Non-blocking (O_NONBLOCK) Zorunluluğu:** ET modunda tampon tamamen tükenene kadar (`EAGAIN` alınana kadar) döngüyle okunmalıdır. Blocking fd kullanılırsa veri bittiğinde `read` çağrısı bloke olur.
**Örnek / Komut:** `ev.events = EPOLLIN | EPOLLET;` -> Edge-triggered modu aktif edilir.
**Hatırla:** Edge-triggered çok yüksek performans sağlar ama tampondaki veri tamamen boşaltılmazsa kilitlenmelere yol açar.

---

## 2026-05-30 - 10.5 VFS (Virtual Filesystem) ve Ortak Dosya Modeli

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 31
**Kısa Tanım:** VFS (Sanal Dosya Sistemi), Linux çekirdeğinin farklı dosya sistemlerini (ext4, NTFS, FAT vb.) ortak bir arayüz arkasında soyutlayan katmanıdır.
**Anahtar Kavramlar:**
- **Soyutlama (Abstraction):** Programcı hangi disk formatının kullanıldığını bilmek zorunda kalmadan `open()`, `read()`, `write()` çağrılarını kullanır.
- **Ortak Dosya Modeli:** VFS, çekirdeğin anladığı ortak bir yapı (inodes, dentry, file structs) sunar.
- **İşlem Yönlendirme:** Program `read(fd, ...)` çağrısı yaptığında VFS bunu arka tarafta o fd'nin bağlı olduğu gerçek dosya sisteminin operasyonuna yönlendirir.
**Örnek / Komut:** Bir USB bellek (FAT32) ve diskteki (ext4) iki dosyayı da aynı C kodu ile okuyup yazabilmek VFS sayesindedir.
**Hatırla:** VFS sayesinde Linux'ta "Her şey bir dosyadır" ilkesi (cihazlar, soketler, pipe'lar dahil) ortak API ile hayata geçirilir.

---

## 2026-05-30 - 10.6 Page Cache, Yerellik İlkesi ve Swappiness

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 32, 35-36
**Kısa Tanım:** Page Cache, disk okuma/yazma maliyetini düşürmek amacıyla çekirdeğin disk bloklarını RAM'de tuttuğu önbellek alanıdır.
**Anahtar Kavramlar:**
- **Zamansal Yerellik (Temporal Locality):** Yakın zamanda erişilmiş veriye kısa süre içinde tekrar erişileceği varsayımıdır (veri disk yerine RAM'den gelir).
- **Mekansal Yerellik ve Önden Okuma (Readahead):** Sıralı okumada, okunan bloğun peşindeki blokların da isteneceği varsayımıyla diskin önceden RAM'e okunmasıdır.
- **Swappiness (`/proc/sys/vm/swappiness`):** Bellek dolduğunda çekirdeğin uygulama sayfalarını swap'e mi atacağı yoksa page cache'i mi budayacağı dengesidir (0-100 arası değer alır; 100 swap'e agresif yönelir).
**Örnek / Komut:** `cat /proc/sys/vm/swappiness` -> varsayılan değer genellikle Linux sistemlerde 60'tır.
**Hatırla:** Page Cache donanım yavaşlığını RAM kullanarak kapatır; sıralı dosya okumalarında readahead performansı çok ciddi artırır.

---

## 2026-05-30 - 10.7 Gecikmeli Yazma (Deferred Writes) ve Senkronizasyon

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 37-38
**Kısa Tanım:** C'deki `write()` veriyi doğrudan diske yazmaz, önce RAM'de "dirty" işaretli sayfalara yazar; diske kesin kayıt için senkronizasyon gerekir.
**Anahtar Kavramlar:**
- **Deferred Writes (Writeback):** Verinin disk yerine hızlı RAM tamponuna yazılıp, çekirdek arka plan servislerince daha sonra topluca diske yazılmasıdır.
- **sync():** Sistem genelindeki tüm açık ve değiştirilmiş tampon sayfalarını diske yazılmak üzere kuyruğa alır.
- **fsync(fd):** Belirtilen dosyanın hem verisini hem de meta verisini (boyut, modifikasyon zamanı vb.) diske kalıcı yazıp işlem bitince döner.
- **fdatasync(fd):** Yalnızca dosya verisini diske yazar, metadata güncellemelerini (boyut değişmedikçe) atlar. `fsync`'e göre daha hızlıdır.
**Örnek / Komut:** `fsync(fd)` çağrısı veritabanı log yazımlarında (WAL) veri güvenliği için her işlem (transaction) sonunda çağrılır.
**Hatırla:** `write()` fonksiyonunun dönmesi verinin diskte güvende olduğunu garanti etmez; elektrik kesintisinde koruma için `fsync()` şarttır.

---

## 2026-05-30 - 10.8 mmap() ile Bellek Eşleme (Memory Mapping)

**Kaynak:** `pdf/Ders_4__1_.pdf`, s. 39-40
**Kısa Tanım:** `mmap()`, bir dosyanın içeriğini sürecin sanal adres uzayıyla eşleyerek, dosyaya standart pointer işlemleriyle (sanki RAM'deki bir diziymiş gibi) erişmeyi sağlar.
**Anahtar Kavramlar:**
- **Koruma Bayrakları (Protections):** `PROT_READ` (okunabilir), `PROT_WRITE` (yazılabilir), `PROT_EXEC` (çalıştırılabilir), `PROT_NONE` (erişilemez).
- **MAP_SHARED:** Alanda yapılan değişiklikler dosyaya yazılır ve alanı paylaşan diğer süreçlerce görülür (IPC için kullanılır).
- **MAP_PRIVATE:** Değişiklikler CoW (Copy-on-Write) ile sürece özel kalır, orijinal dosyaya yansımaz.
- **Performans Avantajı:** `read/write` sistem çağrılarının getirdiği kullanıcı/çekirdek alanı arası veri kopyalama (double-buffering) maliyetini sıfıra indirir.
**Örnek / Komut:** `char *map = mmap(NULL, size, PROT_READ, MAP_SHARED, fd, 0);` -> dosyayı belleğe salt okunur eşler.
**Hatırla:** `mmap()` ile eşlenen alanla işimiz bittiğinde `munmap(map, size)` ile bellek alanı serbest bırakılmalıdır.

---

## 2026-05-30 - 10.9 Bölüm 10 Kavrama Görevi ve Tasarım Özeti

**Kaynak:** `ders.md`
**Kısa Tanım:** Çoklu istemci yöneten ve güvenli log yazan bir sunucu mimarisinde `epoll`, `O_NONBLOCK`, `fdatasync()` ve `mmap()` entegrasyonu.
**Anahtar Kavramlar:**
- **Milyon Bağlantı Yönetimi:** `select/poll` O(n) tarama yapar; 10.000 soket için O(1) çalışan `epoll` zorunludur.
- **Edge-Triggered (ET) Modu:** `epoll` olayları tek bir kez bildirir. `O_NONBLOCK` soketler kullanılarak veriler `EAGAIN` hatası alınana kadar `read()` döngüsünde çekilmelidir. Aksi halde program kilitlenir.
- **Veri Güvenliği ve Hız:** Log dosyası yazımında `write()` veriyi sadece RAM (page cache) üzerine yazar. Güvenlik için disk kafasının gereksiz hareket etmesini önleyen ve daha hızlı çalışan `fdatasync()` tercih edilmelidir.
- **Rastgele Büyük Dosya Erişimi:** Büyük dosyalarda çift bellek kopyalamasını (double-buffering) önlemek ve verimli random access yapmak için `mmap()` ile bellek eşleme yapılmalıdır.
**Örnek / Komut:** Yüksek performanslı web sunucuları (Nginx vb.) bu 4 temel mekanizmanın birleşimini kullanır.
**Hatırla:** Performans ve veri bütünlüğü (data integrity) arasındaki dengeyi kurabilmek işletim sistemi programlamasının temelidir.

---






