# Ders 1: C Temelleri (Sistem Programlama Bakışıyla)

Kaynak: `pdf/Ders_1__1_.pdf`

## 1. Sistem Programlama Perspektifi

C, sistem programlamada bir "uygulama yazma dili" olmaktan çok işletim sistemiyle konuşulan ara katmandır. Dosya okuma/yazma gibi işlemler aslında çekirdeğin sunduğu bir hizmeti çağırmaktır. Bu yüzden üç şey hayatidir:

- **Hata kontrolü:** Her çağrı başarısız olabilir. Örnek: `open()` dosya yoksa `-1` döner.
- **Tanımlı davranış:** C sınırları kontrol etmez; dizinin dışına yazmak Undefined Behavior (UB) ve güvenlik riskidir.
- **Bellek modelini anlama:** Pointer yanlışsa program çöker; NULL pointer dereference klasik hatadır.

## 2. Derleme Zinciri

Bir C kaynağı ikili (binary) hale şu zincirle gelir: `.c` → ön işlemci → derleyici → bağlayıcı.

Hızlı komut:

```bash
gcc -Wall -Wextra -g main.c -o app
```

- `-Wall -Wextra`: uyarıları yakalar.
- `-g`: hata ayıklama sembolleri ekler.

Uyarılar gelecekteki bug'ların habercisidir; çok sıkı bir disiplin için `-Wall -Wextra -Wpedantic -Werror` kullanılır.

## 3. C Programının İskeleti ve Çıkış Kodları

```c
#include <stdio.h>

int main(void) {
    printf("Hello\n");
    return 0;          /* başarı kodu */
}
```

- `#include`: başlık dosyası ekler.
- `main`: programın giriş noktasıdır.
- `return 0`: işletim sistemine başarı bildirir.
- Komut satırında `$?` önceki komutun çıkış kodunu verir. Sistem programlamada çıkış kodları çok kullanılır.

## 4. Standart Veri Tipleri

`char`, `short`, `int`, `long`, `long long` ve bunların `signed`/`unsigned` varyantları temel tiplerdir. Sistem programlamada `float`/`double` daha az kullanılır.

Boyutlar platforma göre değişebileceği için kesin boy gerektiğinde `<stdint.h>` kullanılır:

- `int8_t`, `int16_t`, `int32_t`, `int64_t`
- `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t`

`<stddef.h>` içindeki `size_t`, bellek boyutu ve dizi uzunluğu gibi değerleri güvenle tutmak için işaretsiz bir tamsayı tipidir; `sizeof` operatörünün dönüş tipidir.

`printf` formatları:

- `%d` (int), `%u` (unsigned), `%ld` (long), `%lld` (long long)
- `%f` (double), `%c` (char), `%s` (string)
- `%p` (pointer adresi), `%zu` (size_t)

`sizeof` bir operatördür, fonksiyon değildir; türün veya ifadenin byte cinsinden boyutunu verir.

## 5. Değişkenler, Kapsam ve Sabitler

- **Blok kapsamı:** `{ }` içinde tanımlananlar.
- **Dosya kapsamı:** global değişkenler. Sistem programlamada global state dikkatli yönetilmelidir.
- `const`: derleme zamanı garantisi değil, niyet bildirimidir.
- `#define`: makro sabitler.
- `enum`: okunabilirlik ve sembolik değer için.

## 6. Koşul ve Kontrol Yapıları

- `if` / `else`: `==` karşılaştırma, `=` atamadır. `if (x = 0)` klasik hatadır.
- `&&` ve `||` kısa devre (short-circuit) çalışır; yan etkili IO/sistem çağrılarında önemlidir.
- `switch`-`case`: `break` unutulursa fall-through olur (bazen istenir, ama açıkça belirtilmelidir).
- Döngüler: `while`, `do`-`while`, `for`. `break` ve `continue` ile kontrol değiştirilir.

## 7. Fonksiyonlar ve Dönüş Değeri

Fonksiyonlar tek sorumluluk, test edilebilirlik ve yeniden kullanım için kullanılır. Sistem programlamada hata yönetimini tek yerde toplamak için kritiktir.

Sistem programlamada yaygın kalıp: dönüş değeri durum/hata içindir; gerçek çıktı pointer (out parametre) ile verilir.

```c
int sonuc;
if (oku(&sonuc) != 0) {
    /* hata */
}
```

- 0: başarı
- -1: hata + `errno`

## 8. Komut Satırı Argümanları

```c
int main(int argc, char *argv[]) {
    for (int i = 0; i < argc; i++) {
        printf("argv[%d] = %s\n", i, argv[i]);
    }
    return 0;
}
```

- `argc`: argüman sayısı.
- `argv`: argüman dizisi (string pointer'ları).

Örnek: `./ornek merhaba dunya 123` → argc=4, argv[0]="./ornek", argv[1]="merhaba" …

## 9. Standart Giriş/Çıkış Akışları

- `stdin` (0): standart girdi.
- `stdout` (1): normal çıktı.
- `stderr` (2): hata/uyarı mesajları.

Yönlendirme `>`, `2>` ile yapılır. Arka planda kabuk şu adımları izler:

1. `fork()` yapılır (yeni proseste 3 fd varsayılan olarak aktiftir).
2. Yönlendirilecek dosya `open()` ile açılır.
3. `dup2(fd, 1)` ile stdout dosyaya yönlendirilir.
4. `exec()` ile asıl komut çalıştırılır.

Sistem programlamada `scanf` yerine `fgets` + `strtol` tercih edilir çünkü hata kontrolü çok daha güvenlidir.

## 10. Pointer'a Giriş

Bellekte her değişkenin bir adresi vardır. `&` adres alma, `*` dereference (adresteki değere erişme) operatörüdür.

```c
int x = 5;
int *p = &x;     /* p, x'in adresi */
printf("%d\n", *p);  /* 5 */
```

Pointer + `const` kombinasyonları:

- `const int *p`: gösterilen değer değişmez.
- `int *const p`: pointer'ın tuttuğu adres değişmez.
- `const int *const p`: ikisi de değişmez.

NULL pointer her zaman kullanılmadan önce kontrol edilir; başlatılmamış (uninitialized) pointer dereference UB'dir.

## 11. Array, Pointer ve `&a` ile `a` Farkı

- Dizi adı çoğu bağlamda ilk elemana pointer gibi davranır.
- `sizeof(a)` (dizi) ile `sizeof(p)` (pointer) farklıdır.
- `a` ifadesi çoğu yerde `&a[0]` gibi davranır (tipi `int *`).
- `&a` ise dizi pointer'ıdır (tipi `int (*)[N]`). `(&a)+1` tüm diziyi atlar.

Pointer aritmetiği tipin boyutuna göre adım atar:

```c
int *p; p+1;     /* int boyu kadar ileri */
char *c; c+1;    /* 1 byte ileri */
```

Aynı dizide iki pointer farkı `ptrdiff_t` tipinde ve eleman sayısını verir.

## 12. String'ler ve Güvenli Kopyalama

C string'i `\0` ile biten char dizisidir. `"abc"` aslında `'a','b','c','\0'` dizisidir. `strlen` null'a kadar sayar.

`char a[] = "Merhaba"` ile `char *p = "Merhaba"` farklıdır:

- `a[0]='M'` yapılabilir.
- `p[0]='M'` UB olabilir (genelde salt okunur bölgededir).

Güvenli kopyalama için:

- Boyutu bil: `sizeof(dst)`.
- Sınırla: `snprintf`, dikkatli `strncpy` kullanımı.
- "Boyut bilmeden kopyalama yok" prensibi sistem programlamada güvenlik için kritiktir.

## 13. Stack vs Heap ve Bellek Tahsisi

- **Stack:** otomatik ömür; fonksiyon bittiğinde yerel değişkenler gider. `int a[1000];` gibi büyük yerel diziler stack overflow riski taşır.
- **Heap:** program kontrolünde, `malloc`/`free` (stdlib.h) ile yönetilir.

Heap API'si:

- `malloc(n)`: n byte ayırır, içerik belirsizdir.
- `calloc(k, sz)`: k×sz byte ayırır ve sıfırlar.
- `realloc(p, n)`: yeniden boyutlandırır.
- `free(p)`: serbest bırakır.

Altın kural: her `malloc`'a bir `free`. Leak, double free ve use-after-free klasik hatalardır.

`realloc` güvenli kullanımı:

```c
int *tmp = realloc(a, n * sizeof(int));
if (tmp) {
    a = tmp;
} else {
    /* a hala geçerli */
}
```

Doğrudan `a = realloc(a, ...)` yazmak başarısızlıkta eski adresi kaybeder ve leak'e yol açar.

`free(p); p = NULL;` deseni double free riskini azaltır.

## 14. Fonksiyon Parametreleri ve Out Parametre

C pass-by-value çalışır. Değiştirilmek istenen değer için adres (pointer) gönderilir; bu sistem çağrısı API'lerinde çok yaygındır.

`const correctness`: okuma amaçlı pointer parametreleri `const` olmalıdır; hem hata önler hem API'yi açıklar.

Fonksiyon imzasında `int a[]` aslında `int *a` gibi davranır; içerideki `sizeof(a)` pointer boyutu verir, dizi boyutu vermez. Boyut ayrı parametre ile geçirilmelidir.

Tipik tasarım: `int f(const int *a, size_t n, ...)` ve net bir hata sözleşmesi:

- `return 0;` başarı.
- `return -1;` geçersiz parametre.
- `return -2;` bellek yok.
- `return -3;` I/O hatası.

## 15. Function Pointer ve Callback

Fonksiyonların da adresi vardır; callback "bir işi nasıl yapacağını fonksiyon olarak gönder" mantığıdır. Sistem programlamada sinyal handler, event loop ve `qsort` gibi yerlerde sıkça karşılaşılır.

Function pointer imzası örneği:

```c
int (*cmp)(const void *, const void *);
```

`qsort(base, nmemb, size, cmp)` callback üzerinden eleman karşılaştırması yapar.

---

# Sistem Programlama: Süreç (Proses) Yönetimi Özeti

#### 1. Süreç Kavramı ve Oluşturma
Unix ve Linux sistemlerde bir programın çalıştırılması iki temel adıma ayrılmıştır:
* **fork():** Mevcut sürecin (parent) neredeyse birebir kopyası olan yeni bir süreç (child) oluşturur.
* **exec():** Mevcut sürecin adres uzayını (bellek imajını) yeni bir programla değiştirir. Bu sayede aynı PID ile farklı bir program çalışmaya başlar.
* **PID (Process ID):** Her sürecin benzersiz bir kimlik numarası vardır.
  * **PID 0:** Boşta (idle) süreci.
  * **PID 1:** `init` süreci. Sistemin geri kalanını başlatır ve yetim (orphan) süreçleri sahiplenir.
  * `pid_max` varsayılan olarak 32768'dir ancak güncel sistemlerde 4 milyonun üzerine çıkarılabilir.

#### 2. Süreç Kimlikleri ve Yetkilendirme (UID/GID)
Süreçlerin dosya erişimi ve sistem yetkileri kullanıcı (UID) ve grup (GID) kimliklerine dayanır:
* **Real UID (RUID):** Süreci başlatan gerçek kullanıcı. Sinyal gönderimi gibi durumlarda kullanılır.
* **Effective UID (EUID):** İzin kontrollerinde (dosya okuma/yazma vb.) kullanılan aktif kimliktir. `setuid` bitine sahip dosyalar çalıştırıldığında EUID dosya sahibinin kimliğine bürünebilir.
* **Saved UID:** Yetki geçişleri (yükseltme/azaltma) sırasında eski EUID değerini saklamak için kullanılır.
* **Kimlik Okuma:** `getuid()`, `geteuid()`, `getgid()`, `getegid()` fonksiyonları ile bu bilgiler öğrenilebilir.

#### 3. Özel Süreç Durumları
* **Zombi Süreçler:** Bir çocuk süreç sonlandığında, ebeveyni `wait()` sistem çağrısı ile onun durumunu okumazsa süreç "zombi" olarak kalır ve sistem tablosunda yer kaplar.
* **Yetim (Orphan) Süreçler:** Ebeveyni ölen çocuk süreçlerdir. Bu süreçler otomatik olarak `init` (PID 1) sürecine devredilir (**reparenting**). `init` süreci periyodik olarak `wait()` yaparak zombileri temizler.

#### 4. İş Kontrolü (Job Control) ve Oturumlar (Sessions)
* **Process Group (PGID):** Bir veya daha fazla sürecin oluşturduğu gruptur. Sinyallerin (örneğin sonlandırma sinyali) tüm gruba aynı anda gönderilmesini sağlar. `cat | grep | sort` gibi pipeline komutları tek bir süreç grubu olarak çalışır. `setpgid(0, 0)` komutu ile süreç kendi PID'sini PGID yaparak yeni bir grup lideri olabilir.
* **Sessions (SID):** Bir veya daha fazla süreç grubunun koleksiyonudur. Örneğin; "2 Shell -> 2 tane session" demektir. Terminal kapandığında (`SIGHUP` sinyali) tüm süreçlerin kalıntıları temizlenir. Ön plan (foreground) ve arka plan (background) ayrımı burada çok önemlidir; ön planda aynı anda sadece bir süreç çalışabilir.
* **setsid() Fonksiyonu:** Yeni bir session oluşturur ve parametre almaz. Çağrıldıktan sonra 3 ID (PID, PGID, SID) birbirine eşitlenir.
* **Daemon (Arka Plan) Süreç Oluşturma Reçetesi:** Terminalden tamamen bağımsız çalışan daemon süreçler için klasik "güvenli desen" uygulanır:
  1. `fork()` yapılır.
  2. Ebeveyn `exit()` ile sonlandırılır. (Böylece komut satırı kullanıcıya geri döner, çocuk yetim kalır).
  3. Çocuk süreç `setsid()` çağrısı yaparak yeni bir oturum lideri olur ve terminal bağlantısını tamamen koparır.
  4. `chdir("/")` ile kök dizine geçilir (Dizin unmount/çıkartılma sorunlarını önlemek için).
  5. Tüm açık dosya betimleyicileri (fd) kapatılır ve standart girdiler/çıktılar (0, 1, 2) `/dev/null` ("kara delik") adresine yönlendirilir.
* **Pratik İpuçları:** Daemon süreçlerin bir terminali olmadığı için içlerinde `printf` kullanmak mantıksızdır ve hatadır. Arka plan süreçlerini sonlandırmak için `pkill` komutu kullanılır. `sleep` kullanılsa bile süreç arka planda olduğu için komut satırı hemen kullanıcıya döner.

#### 5. C Dili Örnek Uygulamaları
* **Program Çalıştırma:** `execl("/bin/vi", "vi", "dosya.txt", NULL);` komutu, mevcut süreci kapatıp yerine `vi` editörünü belirtilen dosya ile açar. Başarılı olursa kodun geri kalanı çalışmaz.
* **Yetki Değiştirme:** `seteuid()` ile süreç, izin verilen sınırlar dahilinde aktif kimliğini değiştirerek yetki yükseltip azaltabilir.

#### 6. Detaylı exec Ailesi (Ders_2__1_.pdf)

`exec` ailesi mevcut süreç adres uzayını yeni programla değiştirir. Başarılı bir `exec` geri dönmez; hata olursa `-1` döner ve `errno` ayarlanır. PID/PPID, öncelik ve sahip kullanıcı/grup gibi süreç kimliği bilgileri değişmez.

| Fonksiyon | PATH Araması | Argüman Tipi | Kendi Env'ini Vermek |
| --- | --- | --- | --- |
| `execl`   | Yok          | Liste (`l`)   | Hayır |
| `execlp`  | Var (`p`)    | Liste (`l`)   | Hayır |
| `execle`  | Yok          | Liste (`l`)   | Evet (`e`) |
| `execv`   | Yok          | Vektör (`v`)  | Hayır |
| `execvp`  | Var (`p`)    | Vektör (`v`)  | Hayır |
| `execve`  | Yok          | Vektör (`v`)  | Evet (`e`) |

Linux'ta asıl sistem çağrısı `execve()`; diğerleri libc wrapper'larıdır.

`exec` sonrası değişen/sıfırlanan: bekleyen sinyaller kaybolur, yakalanan sinyaller default'a döner, bellek kilitleri düşer, atexit kayıtları silinir. Değişmeyen: pid/ppid, öncelik, sahip kullanıcı/grup, açık file descriptor'lar (kapatılmadıysa miras alınır).

#### 7. fork() ve Copy-on-Write

`fork()` çağıran sürecin neredeyse aynısı bir child üretir.

- Parent'ta dönüş değeri: child pid.
- Child'ta dönüş değeri: 0.
- Hata: `-1` ve `errno`.

Modern sistemlerde fork sonrası bellek hemen kopyalanmaz; sayfalar paylaşılır ve yazma denemesinde **Copy-on-Write** ile kopyalanır. Çoğu child hemen `exec` yaptığı için bu israfı önemli ölçüde azaltır.

`vfork()` ise tarihi bir kalıntıdır: parent'ı child `exec` veya `_exit` yapana kadar askıya alır, COW yoktur. Riskli olduğundan modern kodda kullanılmaz.

#### 8. exit() ve _exit() Farkı

| Özellik | `exit()` | `_exit()` / `_Exit()` |
| --- | --- | --- |
| Kütüphane | Standart C (`stdlib.h`) | POSIX (`unistd.h`) / C99 (`stdlib.h`) |
| Tamponlar (flush) | Evet, veriler diske/ekrana yazılır | Hayır, veriler kaybolabilir |
| `atexit()` kayıtları | Çalıştırılır | Çalıştırılmaz |
| Kullanım alanı | Normal program bitişi | `fork` sonrası çocuk süreçler |

`exit()` çağrısının kullanıcı-uzayı kapanış adımları:

1. `atexit()` / `on_exit()` ile kayıtlı fonksiyonları LIFO sırada çağırır.
2. Açık standart I/O stream'lerini flush eder.
3. `tmpfile()` ile oluşturulan geçici dosyaları kaldırır.
4. En son `_exit` sistem çağrısını yapar.

`atexit` fonksiyonları kendi içinde tekrar `exit()` çağırmamalıdır (sonsuz özyineleme); gerekirse `_exit()` kullanılır.

`on_exit()` (glibc'ye özel) `fn(int status, void *arg)` imzasıyla çağrılır; taşınabilirlik için `atexit()` tercih edilir.

#### 9. SIGCHLD, wait() ve waitpid()

Child sonlanınca kernel parent'a `SIGCHLD` gönderir (default: ignore). Child hemen yok edilmez; parent durum bilgisini alabilsin diye zombie kalır. Parent `wait()` yapınca kernel zombie'yi temizler.

```c
pid_t wait(int *status);
pid_t waitpid(pid_t pid, int *status, int options);
```

`wait()` hata durumları:

- Child yoksa: `-1` ve `errno = ECHILD`.
- Sinyal ile kesilirse: `-1` ve `errno = EINTR`.

`status` çözümleme makroları:

- `WIFEXITED(status)` + `WEXITSTATUS(status)`: normal çıkış mı, çıkış kodu ne?
- `WIFSIGNALED(status)` + `WTERMSIG(status)`: sinyalle mi öldü, hangi sinyal?
- `WCOREDUMP(status)`: core dump üretildi mi? (Linux desteği vardır.)
- `WIFSTOPPED` / `WSTOPSIG` / `WIFCONTINUED`: debugger/job-control durumları.

`waitpid` `pid` parametresi:

- `-1`: herhangi bir child (`wait` ile aynı).
- `>0`: pid'si tam eşleşen child.
- `0`: aynı process group içindeki child'lar.
- `<-1`: PGID = `|pid|` olan process group'taki child'lar.

`options`:

- `WNOHANG`: child bitmediyse beklemeden hemen dön.
- `WUNTRACED`: child "stop" olduysa (Ctrl+Z gibi) bunu da raporla.
- `WCONTINUED`: stop'tan sonra SIGCONT ile devam ederse raporla.

#### 10. UID/GID Detayları

| Kimlik Türü | Tanım | Ne İçin Kullanılır? |
| --- | --- | --- |
| Real UID | Gerçek kullanıcı | Kaynak sahipliği ve sinyal yönetimi |
| Effective UID | Aktif kimlik | Dosya sistemi ve sistem yetki kontrolleri |
| Saved UID | Kaydedilmiş kimlik | Yetki yükseltme/azaltma arasında geçiş |

`setuid` binary çalıştırılınca effective UID dosya sahibine ayarlanabilir (örnek: `/usr/bin/passwd`).

- `setuid(uid)`: RUID üzerinde işlem. Root ise real/effective/saved birlikte ayarlanır; root değilse sadece real veya saved UID'ye geçişe izin verilir.
- `seteuid(euid)`: EUID üzerinde işlem. Root ise herhangi bir euid'ye geçilebilir; root değilse real veya saved euid'ye geçilebilir.
- Pratik: nonroot için `seteuid`, root süreçler için ihtiyaca göre `setuid`/`seteuid`.

BSD/HP-UX tarzı API'ler (`setreuid`, `setregid`, `setresuid`, `setresgid`) çoğu durumda gereksizdir; standart `setuid`/`seteuid` yeterlidir.

#### 11. Process Group, Session ve setsid()

- **Process group:** Bir veya daha çok süreç. Sinyaller grup olarak gönderilebilir. PGID, group leader'ın pid'sine eşittir. Pipeline (`cat | grep | sort`) tek process group olarak çalışır.
- **Session:** Bir veya daha fazla process group koleksiyonu. Terminal ve oturum yönetimini kolaylaştırır. Foreground ve background grupları içerir.
- Terminal olayları: çıkışta `SIGQUIT`, disconnect'te `SIGHUP`, Ctrl-C ile `SIGINT` foreground gruba gider.

```c
pid_t setsid(void);
```

Süreç mevcut bir process group lideri değilse `setsid()`:

- Yeni session oluşturur (controlling tty yok).
- Yeni process group oluşturur.
- İkisinin de lideri olur (ID = kendi pid'i).

Hata: `-1`, `errno = EPERM` (süreç zaten PG lideri ise). Pratik çözüm: `fork`, parent `exit`, child `setsid()` (daemon'larda standart desen).

`setpgrp() ≡ setpgid(0, 0)` ve `getpgrp() ≡ getpgid(0)`. `getsid(pid)` session ID döndürür; `pid=0` çağıran sürecin session'ını verir.

#### 12. Daemon Reçetesi ve daemon() Yardımcısı

Daemon iki şartı sağlamalıdır: init'in çocuğu gibi reparent edilmek ve terminal ilişkisinin koparılmış olması.

Klasik 6 adım:

1. `fork()`
2. Parent: `exit()` (child PG leader olmasın + üst süreci rahatlat)
3. Child: `setsid()` (yeni session+process group, controlling tty yok)
4. `chdir("/")` (filesystem unmount sorunlarından kaçın)
5. Tüm fd'leri kapat
6. 0,1,2'yi `/dev/null`'a yönlendir

Birçok Unix'te kısa yol: `daemon(nochdir, noclose)`. `nochdir != 0` ise `chdir("/")` yapılmaz; `noclose != 0` ise fd'ler kapatılmaz.

---
> **Not:** Bu bilgiler Sistem Programlama dersine ait güncel `pdf/Ders_2__1_.pdf`, `defter/Ders_2.pdf` ve el yazısı notların taranması sonucu derlenmiştir. Bu süreçlerin doğru yönetimi, sistem kaynaklarının etkin kullanılması ve yetkilendirmenin güvenli bir şekilde sürdürülebilmesi için oldukça önemlidir.

---

# Ders 3: Bellek Yönetimi

Kaynaklar: `pdf/Ders_3.pdf`, `defter/30042026.pdf`

## 1. Süreç Adres Alanı

Modern işletim sistemlerinde süreçler fiziksel belleğe doğrudan erişmez. Her sürece sıfırdan başlayan doğrusal ve benzersiz bir sanal adres alanı verilir. Bu yapı, süreçlerin birbirinin belleğine müdahale etmesini engeller ve bellek koruması sağlar.

Süreç adres alanında genellikle şu bölgeler bulunur:

- **Text segmenti:** Çalışan program kodunun bulunduğu bölgedir.
- **Data segmenti:** Başlangıç değeri atanmış global/statik veriler.
- **BSS segmenti:** Başlangıç değeri atanmamış global/statik veriler. Sıfır kabul edilir.
- **Heap:** Çalışma zamanında dinamik olarak ayrılan bellek. Genellikle düşük adresten yüksek adrese doğru büyür.
- **Stack:** Fonksiyon çağrıları, yerel değişkenler ve dönüş adresleri. Genellikle yüksek adresten düşük adrese doğru büyür.

## 2. Sayfalar ve Sayfalama

Sanal adres alanı sabit boyutlu bloklara ayrılır; bu bloklara **sayfa (page)** denir. Fiziksel bellekteki karşılıklarına **sayfa çerçevesi (page frame)** denir. MMU, sanal adresleri fiziksel adreslere çevirir.

Tipik sayfa boyutları:

- 32-bit sistemlerde sık kullanılan değer: 4 KB
- 64-bit sistemlerde sistem mimarisine göre daha büyük sayfa boyutları görülebilir

Bir sanal adres çevrilirken sanal sayfa numarası ve ofset ayrılır. Ofset sayfa içindeki konumu gösterir; fiziksel bellekte de aynı ofset korunur. Sanal sayfa numarası ise fiziksel sayfa çerçevesine çevrilir.

## 3. TLB

**TLB (Translation Lookaside Buffer)**, sanal adreslerin fiziksel adreslere çevrilmesini hızlandıran küçük ve hızlı bir donanım önbelleğidir. Sayfa tablosuna sürekli gitmek pahalı olduğu için en son kullanılan sayfa çevirileri TLB içinde tutulur.

TLB girişlerinde genellikle şu bilgiler bulunur:

- Valid bit: Giriş geçerli mi?
- Virtual page: Sanal sayfa numarası.
- Page frame: Fiziksel sayfa çerçevesi.
- Protection: Okuma/yazma/çalıştırma izinleri.
- Modified/dirty bilgisi: Sayfa değiştirildi mi?

TLB'de kayıt varsa işlem hızlı yapılır. Kayıt yoksa sayfa tablosuna gidilir; gerekirse TLB güncellenir.

## 4. Geçerli ve Geçersiz Sayfalar

Geçerli sayfa, fiziksel bellekte veya disk üzerindeki takas alanında karşılığı bulunan sanal sayfadır. Geçersiz sayfa ise hiçbir fiziksel veya ikincil alanla eşleşmeyen, kullanılmayan veya erişilmemesi gereken adres bölgesidir.

Geçersiz bir sayfaya erişmek çoğu zaman **segmentation fault** hatasına yol açar. Buna karşılık geçerli ama RAM'de olmayan bir sayfaya erişilirse **page fault** oluşur.

## 5. Segmentation Fault ve Page Fault

**Segmentation fault**, sürecin erişme izni olmayan veya geçersiz bir bellek alanına erişmeye çalışmasıdır.

**Page fault**, erişilen sayfanın geçerli olduğu halde o anda RAM'de bulunmamasıdır. Bu durumda işletim sistemi:

1. Sürecin ilgili adrese erişmeye çalıştığını yakalar.
2. MMU page fault üretir.
3. Çekirdek devreye girer.
4. Sayfayı diskten/swap alanından RAM'e taşır.
5. Program kaldığı yerden devam eder.

Page fault her zaman hata değildir; sanal bellek sisteminin doğal bir parçasıdır.

## 6. Sayfa Değişimi ve LRU

Sanal bellek fiziksel bellekten büyük olabilir. RAM dolduğunda çekirdek bazı sayfaları diske yazar; buna **page-out** denir. Diskteki bir sayfanın tekrar RAM'e alınmasına **page-in** denir.

Hangi sayfanın çıkarılacağına karar vermek için sayfa değiştirme algoritmaları kullanılır. **LRU (Least Recently Used)** mantığı, en uzun süredir kullanılmayan sayfayı çıkarmayı hedefler. Donanımsal LRU pahalı olabileceği için işletim sistemleri çoğu zaman yaklaşık/yazılımsal LRU yaklaşımları kullanır.

## 7. Sayfaların Paylaşımı ve Copy-on-Write

Farklı süreçlerin sanal sayfaları aynı fiziksel RAM sayfasını gösterebilir. Bu özellikle ortak kütüphaneler veya salt okunur kod bölümleri için verimlidir.

Bir süreç paylaşılan sayfaya yazmak isterse **Copy-on-Write (COW)** devreye girer:

1. Paylaşılan sayfa salt okunur işaretlenir.
2. Yazma denemesinde fault oluşur.
3. Çekirdek o sayfanın fiziksel kopyasını ilgili süreç için oluşturur.
4. Yazma işlemi bu özel kopyaya yapılır.

Bu sayede `fork()` gibi işlemler çok hızlı yapılabilir; tüm bellek baştan kopyalanmaz, sadece değiştirilen sayfalar kopyalanır.

## 8. Bellek Bölgeleri

Çekirdek, sürecin adres alanını izinlerine ve amaçlarına göre bölgelere ayırır. Text, data, BSS, heap ve stack bu bölgelerin temel örnekleridir.

Önemli noktalar:

- String literal gibi değişmemesi gereken veriler salt okunur bölgede tutulabilir.
- BSS segmentindeki başlangıç değeri atanmamış değişkenler başlangıçta sıfır kabul edilir.
- Heap çalışma zamanında `malloc`, `calloc`, `realloc` gibi fonksiyonlarla yönetilir.
- Stack fonksiyon çağrılarıyla büyür ve fonksiyon dönüşlerinde küçülür.

## 9. BSS ve Linux Optimizasyonu

BSS içinde yalnızca sıfırlardan oluşan başlangıç verileri bulunduğu için bu verileri çalıştırılabilir dosyada fiziksel olarak saklamak gereksizdir. Bağlayıcı dosyada bu alanı işaretler; çekirdek programı yüklerken gerektiğinde sıfır dolu sayfalarla eşler.

Bu durum dosya boyutunu küçültür ve bellek kullanımını verimli hale getirir. Ayrıca sıfır dolu sayfalar COW mantığıyla paylaşılabilir.

## 10. Dinamik Bellek Tahsisi

Dinamik bellek çalışma zamanında ayrılır. Ne kadar belleğe ihtiyaç olacağı derleme zamanında bilinmiyorsa heap kullanılır.

### malloc

`malloc(size_t size)` belirtilen bayt kadar bellek ayırır. Başarılı olursa ayrılan bloğun başlangıç adresini döndürür; başarısız olursa `NULL` döndürür. Ayrılan belleğin içeriği garanti edilmez, eski/çöp veri içerebilir.

```c
int *dizi = malloc(100 * sizeof(int));
if (dizi == NULL) {
    perror("malloc");
    exit(EXIT_FAILURE);
}
```

### calloc

`calloc(nr, size)` eleman sayısı ve eleman boyutu alır. Toplamda `nr * size` bayt ayırır ve belleği sıfırlar. Diziler için başlangıçta tüm değerlerin sıfır olması isteniyorsa kullanışlıdır.

```c
int *dizi = calloc(50, sizeof(int));
```

### realloc

`realloc(ptr, size)` mevcut bellek bloğunu büyütür veya küçültür. Devamında yeterli yer yoksa veriyi yeni bir adrese kopyalar ve eski adresi serbest bırakır.

Güvenli kullanım için dönüş değeri doğrudan eski pointer'a atanmaz:

```c
int *tmp = realloc(dizi, 200 * sizeof(int));
if (tmp == NULL) {
    /* dizi hala geçerlidir */
} else {
    dizi = tmp;
}
```

## 11. Güvenli Tahsis Sarmalayıcısı

`malloc` başarısız olduğunda `NULL` döndürdüğü için dönüş değeri mutlaka kontrol edilmelidir. Büyük projelerde her yerde aynı kontrolü tekrar etmek yerine `xmalloc` gibi sarmalayıcı fonksiyonlar yazılır.

```c
void *xmalloc(size_t size)
{
    void *p = malloc(size);
    if (p == NULL) {
        fprintf(stderr, "Bellek yetersiz\n");
        exit(EXIT_FAILURE);
    }
    return p;
}
```

## 12. Bellek Yönetimi Hataları

Sık görülen hatalar:

- **Memory leak:** Ayrılan belleğin `free()` ile serbest bırakılmaması.
- **Use-after-free:** `free()` edilen adresin tekrar kullanılmaya çalışılması.
- **Double free:** Aynı adresin iki kez `free()` edilmesi.

Bu hatalar programın çökmesine, güvenlik açıklarına veya heap yapısının bozulmasına neden olabilir.

## 13. Veri Hizalama

İşlemciler bazı veri tiplerinin bellekte kendi boyutlarının katı olan adreslerden başlamasını ister. Örneğin 4 baytlık `int`, 4'e bölünebilen bir adreste daha verimli okunur.

Uyumsuz adreslere erişim bazı mimarilerde performans kaybına, bazılarında donanım hatasına neden olabilir. `malloc`, C'deki standart tipler için uygun hizalanmış bellek döndürmeyi garanti eder.

Özel hizalama gereken durumlarda `posix_memalign()` kullanılabilir. Özellikle DMA, I/O veya sayfa sınırı hizalaması gereken işlemlerde tercih edilir.

## 14. Program Break: `brk()` ve `sbrk()`

Heap alanının geleneksel alt seviye sınırına **program break** denir. Program break, sürecin veri/heap alanının bittiği noktayı gösterir. `malloc()` çoğu zaman arka planda bu sınırı ileri taşıyarak küçük ve orta büyüklükteki heap tahsislerini karşılar.

- `sbrk(0)`: Mevcut heap sınırını öğrenmek için kullanılır.
- `sbrk(increment)`: Program break değerini belirtilen bayt kadar artırır veya azaltır.
- `brk(addr)`: Program break değerini doğrudan verilen adrese taşımayı dener.

Bu çağrılar düşük seviyelidir. Günlük C programlarında doğrudan `brk()`/`sbrk()` kullanmak yerine `malloc()` ailesi tercih edilir; çünkü `malloc()` boşlukları, hizalamayı, yeniden kullanımı ve hata durumlarını daha güvenli yönetir.

## 15. Anonim Bellek Eşleme: `mmap()` ve `munmap()`

`mmap()`, bir dosyayı veya dosyadan bağımsız anonim bir bellek bölgesini sürecin sanal adres alanına eşler. Büyük bellek tahsislerinde `glibc`, heap'i `brk()` ile büyütmek yerine doğrudan anonim `mmap()` kullanabilir. Yaygın eşik değerlerinden biri yaklaşık 128 KB'tır; bu değer sistem ve ayarlara göre değişebilir.

Anonim `mmap()` özellikle büyük bloklarda işe yarar:

- Heap parçalanmasını azaltabilir.
- Eşlenen bölgenin koruma izinleri ayarlanabilir.
- Büyük alanlar `munmap()` ile çekirdeğe daha doğrudan iade edilebilir.

`mmap()` ile alınan alan `free()` ile değil, `munmap()` ile serbest bırakılır. `malloc()` ile alınan alanı `free()` etmek, `mmap()` ile alınanı `munmap()` etmek gerekir; bu iki aile karıştırılmamalıdır.

```c
void *alan = mmap(NULL, boyut, PROT_READ | PROT_WRITE,
                  MAP_PRIVATE | MAP_ANONYMOUS, -1, 0);
if (alan == MAP_FAILED) {
    perror("mmap");
    exit(EXIT_FAILURE);
}

if (munmap(alan, boyut) == -1) {
    perror("munmap");
}
```

## 16. `mallopt()`, `mallinfo()` ve `malloc_stats()`

`mallopt()`, `malloc()` davranışını uygulamanın ihtiyacına göre ince ayarlamak için kullanılan Linux/glibc odaklı bir arayüzdür. Normal uygulamalarda çoğu zaman gerekmez; daha çok performans, gömülü sistem veya özel bellek davranışı gereken durumlarda düşünülür.

Önemli parametreler:

- `M_MMAP_THRESHOLD`: Hangi boyuttan büyük tahsislerin `mmap()` ile karşılanacağını belirler.
- `M_MMAP_MAX`: Aynı anda kullanılabilecek `mmap()` tabanlı tahsis sayısını sınırlar.
- `M_MXFAST`: Küçük blokların hızlı yeniden kullanım listelerinde tutulmasını etkiler.
- `M_TOP_PAD`: Heap büyütülürken gelecekteki istekler için fazladan alan bırakılmasını sağlar.
- `M_CHECK_ACTION`: Bellek hatası algılandığında sessiz kalma, hata yazma veya programı durdurma davranışını belirler.

`mallinfo()` heap kullanımıyla ilgili istatistik verir; modern sistemlerde bazı alanları yetersiz kalabildiği için daha yeni arayüzler tercih edilebilir. `malloc_stats()` ise özet bilgiyi doğrudan standart hataya yazar.

## 17. Stack Üzerinden Tahsis: `alloca()` ve VLA

`alloca()`, belleği heap yerine geçerli fonksiyonun stack çerçevesinden ayırır. Fonksiyon döndüğünde bu bellek otomatik olarak geçersiz olur; ayrıca `free()` çağrısı yapılmaz.

Avantajları:

- Küçük ve kısa ömürlü geçici alanlar için hızlıdır.
- Manuel `free()` gerektirmez.

Riskleri:

- Başarısızlığı güvenilir biçimde bildiremez.
- Büyük boyutlar stack overflow'a neden olabilir.
- Dönen pointer fonksiyon dışına taşırılırsa geçersiz adres kullanımı oluşur.

C99 ile gelen **VLA (Variable Length Array / Değişken Uzunluklu Dizi)** de boyutu çalışma zamanında belirlenen stack tabanlı dizi davranışı sağlar:

```c
void oku(size_t n)
{
    int gecici[n];
    /* gecici sadece bu fonksiyon çağrısı boyunca geçerlidir */
}
```

VLA ve `alloca()` küçük geçici işler için kullanılabilir; kullanıcıdan gelen kontrolsüz büyük boyutlarda heap tabanlı güvenli tahsis daha doğru olur.

## 18. Bellek İşleme Fonksiyonları

### `memset()`

`memset(ptr, value, size)` bir bellek bloğunu bayt bayt aynı değerle doldurur. Sık kullanım alanları:

- Bir struct veya dizi alanını sıfırlamak.
- Padding alanlarını temizlemek.
- Hassas veriyi kullanımdan sonra silmeye çalışmak.

```c
struct kayit k;
memset(&k, 0, sizeof(k));
```

### `memcpy()` ve `memmove()`

`memcpy()` kaynak ve hedef alanlar çakışmadığında hızlı kopyalama yapar. Kaynak ve hedef bellek bölgeleri üst üste biniyorsa davranış güvenli değildir.

`memmove()` ise çakışma olsa bile güvenli kopyalama yapar. Bu yüzden aynı dizi içinde kaydırma gibi işlemlerde `memmove()` tercih edilir.

```c
memcpy(hedef, kaynak, boyut);   /* alanlar çakışmıyorsa */
memmove(dizi + 1, dizi, boyut); /* alanlar çakışabilir */
```

## 19. Belleği RAM'de Tutmak: `mlock()` ve `mlockall()`

İşletim sistemi kullanılmayan sayfaları swap alanına yazabilir. Bazı durumlarda bir sayfanın diske gitmesi istenmez:

- Şifre, anahtar gibi hassas veriler diske yazılmamalıdır.
- Gerçek zamanlı sistemlerde page fault gecikmesi kabul edilemeyebilir.

`mlock(addr, len)` belirli bir adres aralığını RAM'de kilitlemeye çalışır. `mlockall()` ise sürecin adres alanını daha geniş şekilde kilitlemek için kullanılır.

- `MCL_CURRENT`: Şu anda eşlenmiş sayfaları kilitler.
- `MCL_FUTURE`: Gelecekte ayrılacak sayfaların da kilitli gelmesini ister.

Bu çağrılar kaynak tüketimini artırabileceği için yetki ve limitlere bağlıdır.

## 20. Linux Overcommit ve OOM Killer

Linux, çoğu kurulumda iyimser bellek tahsis stratejisi uygular. Bir program çok büyük bir `malloc()` isteğinde bulunduğunda çekirdek, programın bu sanal alanın tamamını gerçekten kullanmayacağını varsayarak isteği kabul edebilir. Buna **overcommit (aşırı tahsis)** denir.

Sonuç olarak `malloc()` başarılı dönebilir; fakat program ayrılan sanal alanın büyük kısmına gerçekten yazmaya başladığında fiziksel RAM ve swap tükenebilir.

Fiziksel bellek kritik düzeyde biterse **OOM Killer (Out of Memory Killer)** devreye girer. Çekirdek, sistemi rahatlatmak için bir süreci seçip `SIGKILL` ile sonlandırır. Seçim genellikle bellek tüketimi ve süreç önceliği gibi ölçütlere dayanır.

Overcommit davranışı şu dosyadan izlenip ayarlanabilir:

```text
/proc/sys/vm/overcommit_memory
```

Değerlerin genel anlamı:

- `0`: Varsayılan sezgisel davranış.
- `1`: İsteklere geniş biçimde izin ver.
- `2`: Daha sıkı hesaplama yap; OOM riskini azaltmaya çalış.

## 21. Ders Notlarından Pratik Vurgular

`30042026.pdf` içindeki derste tutulan notlar, Ders 3 slaytlarına şu pratik hatırlatmaları ekler:

- Sanal adres alanı süreç için sıfırdan başlar; fiziksel bellekteki gerçek yerler farklı adreslerden başlayabilir.
- MMU, sanal sayfa numarasını fiziksel sayfa çerçevesine çevirir; ofset aynı kalır.
- TLB, bellek yönetimi için donanım içindeki hızlı bir önbellek gibi düşünülebilir. TLB'de kayıt varsa çeviri hızlı yapılır; yoksa sayfa tablosuna gidilir.
- Page fault her zaman program hatası değildir. Sayfa geçerli ama RAM'de değilse çekirdek onu swap/disk alanından RAM'e getirip programı kaldığı yerden sürdürebilir.
- LRU mantığı "en son kullanılanlar kalsın, uzun süredir kullanılmayanlar çıksın" fikrine dayanır. Gerçek sistemlerde donanım maliyeti nedeniyle yaklaşık/yazılımsal LRU yaklaşımları kullanılır.
- Ortak kütüphaneler, özellikle `libc`, birçok süreç tarafından aynı fiziksel sayfalar üzerinden paylaşılabilir.
- COW sayesinde bir süreç ortak sayfaya yazmaya çalıştığında tüm alan değil, yalnızca ilgili sayfa kopyalanır.
- BSS içindeki sıfır değerli alanlar çalıştırılabilir dosyada tek tek tutulmaz; çekirdek sıfır sayfa mantığıyla gerektikçe eşleme yapar. Yazma olduğunda ilgili sayfa kopyalanır.
- `realloc()` dönüşü doğrudan eski pointer'a atanırsa başarısızlıkta eski adres kaybedilebilir; bu da bellek sızıntısına yol açar. Bu yüzden geçici pointer kullanılmalıdır.
- Bellek tahsisçisi, heap içindeki dolu ve boş parçaları izlemek için linked-list benzeri yapılar kullanabilir; heap'te delikler oluşması daha büyük bir alan kopyalama veya yeni yer arama maliyeti doğurur.
- Hizalama, erişim ve işlem hızını artırır. DMA gibi donanım I/O durumlarında sayfa başına veya özel sınıra hizalanmış alan gerekebilir.

---

# Ders 4: Dosya G/Ç (File I/O)

Kaynak: `pdf/Ders_4__1_.pdf`, `defter/14052026.pdf`

## 1. Unix'te Dosya G/Ç Modeli

Unix'te dosyadan okuma ve dosyaya yazma çekirdeğin sunduğu temel hizmettir. Çekirdek, donanım ile kullanıcı arasındaki köprüdür. Sezgisel olarak bir dosya bir deftere benzetilebilir: önce kapağını açarsın (`open`), içinde işlem yaparsın (`read`/`write`), işin bitince kapatırsın (`close`).

## 2. Dosya Tanımlayıcıları (File Descriptors)

Açılan her dosya, çekirdek tarafından negatif olmayan bir tamsayı ile indekslenir; bu **file descriptor (fd)** olarak adlandırılır. C dilinde `int` tipi ile temsil edilir.

Çekirdek her süreç için açık dosyalar tablosu tutar. Bu tabloda her giriş şunları içerir:

- Dosyanın bellekteki i-düğümü (inode) kopyasına işaretçi.
- Erişim modları (okuma, yazma).
- Güncel dosya konumu (offset).

fd değerleri 0'dan başlar; Linux'ta varsayılan limit 1024, maksimum 1.048.576'dır.

## 3. Standart Dosya Tanımlayıcıları

Aksi belirtilmedikçe her süreç üç temel fd ile başlatılır:

- `0`: standart girdi (`stdin`)
- `1`: standart çıktı (`stdout`)
- `2`: standart hata (`stderr`)

POSIX makroları: `STDIN_FILENO`, `STDOUT_FILENO`, `STDERR_FILENO`.

## 4. "Her Şey Bir Dosyadır" Felsefesi

Dosya tanımlayıcıları sadece metin belgelerini değil aşağıdakileri de yönetir:

- Aygıt dosyaları (device files)
- Borular (pipes) ve soketler (sockets)
- Dizinler

Sonuç olarak okunabilen veya yazılabilen hemen her şeye standart G/Ç arayüzüyle erişilebilir.

## 5. open() Sistem Çağrısı

```c
#include <fcntl.h>

int fd = open(yol, flags);
int fd = open(yol, flags, mode);  /* O_CREAT varsa */
```

- Başarı: dosya tanımlayıcı (fd) döner.
- Hata: `-1` döner ve `errno` ayarlanır.

`flags` parametresi dosyanın hangi amaçla açılacağını belirtir. Aşağıdakilerden tam biri zorunludur:

- `O_RDONLY`: sadece okuma.
- `O_WRONLY`: sadece yazma.
- `O_RDWR`: hem okuma hem yazma.

Davranış değiştirici bayraklar bitwise-OR (`|`) ile birleştirilir:

- `O_APPEND`: yazma her zaman dosyanın sonuna yapılır (log dosyaları için kritik).
- `O_CREAT`: dosya yoksa oluşturur; bu bayrak varsa `mode` zorunludur.
- `O_TRUNC`: dosya varsa ve yazma izniyle açılmışsa içeriğini siler, boyutu 0 olur.
- `O_EXCL`: `O_CREAT` ile birlikte; dosya zaten varsa başarısız olur. Yarış (race) hatalarını önler.
- `O_SYNC`: senkron G/Ç. Veri fiziksel olarak diske yazılana kadar fonksiyon dönmez.
- `O_NONBLOCK`: engellemesiz mod. İşlem beklemeyi gerektiriyorsa uyumaz, hemen döner.

Örnek:

```c
int fd = open("log.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);
if (fd == -1) {
    perror("open");
    return -1;
}
```

## 6. creat() Sistem Çağrısı

`O_WRONLY | O_CREAT | O_TRUNC` kombinasyonu çok sık kullanıldığı için Unix kısayol sağlar:

```c
int fd = creat(yol, mode);
```

Güncel sistemlerde `creat` arka planda `open`'a yönlendirilir.

## 7. Yeni Dosyaların Sahipliği ve İzinler

Bir dosya oluşturulduğunda kimlik bilgileri süreci başlatan kişiden alınır:

- **Owner:** sürecin Effective UID'si.
- **Group:** genellikle sürecin Effective GID'si.

İzinler octal (sekizlik) sistemle (`0644`) ya da POSIX sabitleriyle tanımlanır:

- Kullanıcı: `S_IRUSR`, `S_IWUSR`, `S_IXUSR`.
- Grup: `S_IRGRP`, `S_IWGRP`, `S_IXGRP`.
- Diğer: `S_IROTH`, `S_IWOTH`, `S_IXOTH`.

## 8. umask Kavramı

Kodda belirtilen `mode` diskte tam olarak aynı oluşmayabilir. Sistem `umask` adı verilen güvenlik maskesi uygular:

```text
gerçek_mode = mode & ~umask
```

`umask` koddaki yetkileri hiçbir zaman artıramaz, sadece düşürür (filtre).

## 9. read() Sistem Çağrısı

```c
ssize_t n = read(fd, buf, len);
```

- `fd`: dosya tanımlayıcı.
- `buf`: okunan verinin yazılacağı bellek adresi.
- `len`: en fazla okunmak istenen byte sayısı.

Dönüş değerleri:

- `= len`: istenen veri tamamen okundu (ideal).
- `> 0 && < len`: kısmi okuma. Veri tam hazır değildi (örn. socket) ya da sinyal kesti.
- `0`: dosya sonuna (EOF) ulaşıldı.
- `-1`: hata; `errno` ayarlanır.

EOF hata değildir. Veri bitmediği halde o an veri yoksa `read()` uykuya yatar (block); istenmiyorsa dosya `O_NONBLOCK` ile açılır.

Güvenli okuma kalıbı sinyal kesmesi için `errno == EINTR` ile döngüye alır:

```c
while ((n = read(fd, buf, len)) == -1) {
    if (errno == EINTR) continue;
    perror("read");
    break;
}
```

## 10. close() Sistem Çağrısı

```c
int rv = close(fd);
```

Dosya tablosundaki referansı temizler, bellek yapılarını serbest bırakır. Hata durumunda `-1` döner; örnek `errno`: `EBADF` (geçersiz fd), `EIO` (G/Ç hatası).

**Close dönüş değeri neden kontrol edilmeli?** İşletim sistemi performans için yazma işlemlerini hemen diske indirmez (deferred write). Verinin diske inerken oluşacak donanımsal hatalar ancak `close()` sırasında raporlanabilir. Çoğu programcı bunu kontrol etmez; bu büyük bir hatadır.

## 11. lseek() ile Dosya Konumlandırma

`read()`/`write()` dosyayı sıralı işler. Rasgele bir noktaya atlamak için `lseek()` kullanılır:

```c
off_t lseek(int fd, off_t pos, int origin);
```

`origin` parametresi referans noktasını belirler:

- `SEEK_SET`: mutlak başlangıç. Konum = `pos`.
- `SEEK_CUR`: güncel konum. Konum = güncel + `pos` (negatif olabilir).
- `SEEK_END`: dosya sonu. Konum = boyut + `pos`.

`lseek()` okuma/yazma yapmaz; yalnızca dosya offset'ini değiştirir.

Pipe gibi rasgele erişimi desteklemeyen aygıtlarda `lseek` başarısız olur ve `-1` döner.

Kaynak: `pdf/Ders_4__1_.pdf`, s. 23-25

## 12. Çoklu G/Ç (Multiplexing)

Kaynak: `pdf/Ders_4__1_.pdf`, s. 26

Bir program aynı anda birden fazla dosya tanımlayıcısından veya soketten veri beklemek zorunda kalabilir. Normal `read()` engelleyici çalışırsa, program tek bir fd üzerinde beklerken diğer fd'lerde hazır olan veriyi kaçırabilir veya tüm program donmuş gibi davranır.

Çözüm, işletim sistemine "hangi fd okumaya/yazmaya hazır?" diye sormaktır. Bu yaklaşıma **çoklu G/Ç (multiplexing)** denir. Tek bir iş parçacığı, yüzlerce soketi bu modelle yönetebilir.

Temel araçlar:

- `select()`
- `poll()`
- Linux'a özgü `epoll`

## 13. select() Sistem Çağrısı

Kaynak: `pdf/Ders_4__1_.pdf`, s. 27

`select()`, izlenecek fd'leri kümeler içinde tutar ve şu soruya cevap arar: "Bu fd'lerden hangileri okumaya hazır, hangileri yazmaya hazır?"

Program, hazır bir olay oluşana kadar uyuyabilir. Böylece boş yere döngüyle sürekli kontrol yapmak yerine çekirdeğin haber vermesi beklenir.

Dezavantajları:

- Sabit boyutlu `fd_set` yapısı kullanır.
- fd sayısı arttıkça performans lineer biçimde düşer (`O(n)`).
- Büyük ölçekli sunucu yazılımları için sınırlayıcı olabilir.

## 14. poll() Sistem Çağrısı

Kaynak: `pdf/Ders_4__1_.pdf`, s. 28

`poll()`, `select()` fikrinin daha esnek halidir. `fd_set` yerine `struct pollfd` dizisi kullanır. Bu sayede izlenecek fd ve olay bilgileri daha açık biçimde tutulur.

Ancak temel problem devam eder: çok büyük fd kümelerinde çekirdeğin tüm listeyi taraması gerekir. Bu yüzden `poll()` de büyük bağlantı sayılarına çıkıldığında performans sıkıntısı yaşayabilir.

## 15. epoll Arayüzü

Kaynak: `pdf/Ders_4__1_.pdf`, s. 29-30

`epoll`, Linux çekirdeğinin ölçeklenebilir G/Ç izleme çözümüdür. Özellikle binlerce bağlantı yöneten sunucu yazılımlarında kullanılır.

Temel çağrılar:

- `epoll_create1()`: çekirdekte bir izleme nesnesi oluşturur.
- `epoll_ctl()`: izlenecek fd'leri ekler, siler veya değiştirir.
- `epoll_wait()`: hazır olayları bekler ve döndürür.

Tetikleme modelleri:

- **Level-triggered:** Olay durumu devam ettiği sürece bildirim gelir. Örneğin boruda veri kaldığı sürece tekrar tekrar uyarı alınır.
- **Edge-triggered:** Durum değiştiğinde yalnızca bir kez bildirim gelir. Örneğin boruya veri geldiği anda uyarı alınır; kalan veriyi tamamen tüketmek programın sorumluluğudur.

Kenar tetiklemeli kullanım performanslıdır, fakat dikkat ister. fd non-blocking moda alınmaz ve tampon tamamen boşaltılmazsa program olayın devamını beklerken takılabilir.

## 16. Sanal Dosya Sistemi (VFS)

Kaynak: `pdf/Ders_4__1_.pdf`, s. 31

**Virtual Filesystem (VFS)**, çekirdeğin farklı dosya sistemlerini ortak bir arayüz altında soyutladığı katmandır. `ext4`, `NTFS`, `FAT` gibi dosya sistemleri farklı iç yapılara sahip olsa da kullanıcı programı çoğu zaman aynı `open()`, `read()`, `write()`, `close()` çağrılarını kullanır.

VFS'nin görevi:

- Ortak dosya modelini sunmak.
- Sistem çağrılarını alttaki gerçek dosya sisteminin ilgili operasyonlarına yönlendirmek.
- Programın disk formatını bilme zorunluluğunu ortadan kaldırmak.

Bu yüzden bir program açısından `read(fd, ...)` çağrısı aynıdır; VFS arka tarafta ilgili dosya sisteminin kayıtlı operasyonlarını çağırır.

## 17. Sayfa Önbelleği (Page Cache)

Kaynak: `pdf/Ders_4__1_.pdf`, s. 32, 35-36

Disk erişimi RAM ve işlemciye göre yavaştır. Çekirdek bu maliyeti azaltmak için okunan disk bloklarını RAM'deki **page cache** içinde tutar. Aynı veri kısa süre sonra yeniden istenirse disk yerine RAM'den karşılanır. Bu, **zamansal yerellik (temporal locality)** ilkesine dayanır.

Sıralı okumalarda **mekansal yerellik (sequential locality)** de kullanılır. Çekirdek, bir blok okunduğunda arkasından gelen blokların da okunacağını tahmin ederek önden okuma (**readahead**) yapabilir. Sıralı erişim tespit edilirse readahead penceresi dinamik olarak büyütülür.

Bellek dolduğunda çekirdek iki seçenek arasında denge kurar:

- Uygulama verisini swap alanına taşımak.
- Page cache içeriğini budamak.

Linux'taki `/proc/sys/vm/swappiness` değeri bu dengeyi etkiler. Yüksek değer, cache'i koruyup uygulama sayfalarını swap'e itme eğilimini artırır.

## 18. Gecikmeli Yazma ve Senkronizasyon

Kaynak: `pdf/Ders_4__1_.pdf`, s. 37-38

`write()` çağrısı çoğu durumda veriyi doğrudan fiziksel diske indirmez. Veri önce RAM'deki tamponlara yazılır ve bu sayfalar **dirty (kirli)** olarak işaretlenir. Arka plandaki çekirdek iş parçacıkları bu kirli verileri uygun zamanda topluca diske yazar. Bu modele **deferred writes** veya **writeback** denir.

Avantajı performanstır; çok sayıda küçük yazma yerine toplu disk yazımı yapılabilir. Riski ise sistem çökmesi durumunda henüz diske inmeyen verinin kaybolabilmesidir.

Kritik verilerde senkronizasyon çağrıları kullanılır:

- `fsync(fd)`: dosya verisini ve gerekli metadata'yı diske zorlar.
- `fdatasync(fd)`: çoğunlukla dosya verisine odaklanır; her metadata değişimini zorlamak zorunda değildir.
- `sync()`: sistem genelinde kirli tamponların yazılmasını ister.

Her `write()` sonrasında `fsync()` çağırmak güvenilirliği artırabilir, fakat ciddi performans maliyeti doğurur. Bu nedenle güvenilirlik ve performans arasında bilinçli seçim yapılır.

## 19. Bellek Eşleme (mmap)

Kaynak: `pdf/Ders_4__1_.pdf`, s. 39-40

Standart dosya G/Ç'de program bir buffer ayırır, `read()` ile veri çeker ve döngülerle işler. **Memory mapping** yaklaşımında dosyanın bir bölümü süreç adres alanına eşlenir. Program dosyayı büyük bir bellek dizisi gibi görür ve pointer üzerinden okuma/yazma yapar.

Temel imza:

```c
void *mmap(void *addr, size_t len, int prot, int flags, int fd, off_t offset);
```

- `addr`: önerilen başlangıç adresi; çoğunlukla `NULL` verilir.
- `len`: eşlenecek byte sayısı.
- `prot`: bellek koruma izinleri.
- `flags`: eşlemenin paylaşım davranışı.
- `fd`: eşlenecek dosya tanımlayıcısı.
- `offset`: dosyada eşlemenin başlayacağı konum.

Başarılı olursa eşlenen alanın başlangıç pointer'ı döner; hata durumunda `MAP_FAILED` döner.

## 20. mmap Koruma ve Haritalama Bayrakları

Kaynak: `pdf/Ders_4__1_.pdf`, s. 41-42

`prot` parametresi eşlenen sayfalara nasıl erişilebileceğini belirtir:

- `PROT_READ`: sayfalar okunabilir.
- `PROT_WRITE`: sayfalara yazılabilir.
- `PROT_EXEC`: sayfalar kod olarak çalıştırılabilir.

Modern işlemciler ve işletim sistemleri güvenlik için NX (No-Execute) bitini destekler. Bir bellek bölgesinin okunabilir olması, otomatik olarak çalıştırılabilir olduğu anlamına gelmez.

`flags` parametresi eşlemenin davranışını belirler:

- `MAP_SHARED`: bellekte yapılan değişiklikler arkadaki dosyaya yansıtılabilir ve diğer süreçlerle paylaşılabilir.
- `MAP_PRIVATE`: Copy-on-Write mantığıyla çalışır; yazmalar özel kopyaya gider, asıl dosya değişmez.

Genel kullanım sezgisi:

- Büyük dosyayı rastgele okuyacaksan `mmap` pratik olabilir.
- Klasik akış tabanlı okuma/yazma için `read()`/`write()` daha sade olabilir.
- Paylaşımlı dosya veya süreçler arası bellek benzeri kullanımda `MAP_SHARED` önemlidir.
