# Mikroişlemciler - 8051 Defter ve Ders Notları

Kaynak: `defter/mikroslemci_zt.pdf`

Bu dosyada Mikroişlemciler için ana kaynak defterdir. Aynı PDF `pdf/` altında da bulunabilir, fakat sıralama ve kaynakça için `defter/mikroslemci_zt.pdf` esas alınır.

## 0. Defter Kaynak Sırası ve Ön Koşul Konuları

**Kaynak:** `defter/mikroslemci_zt.pdf`, s. 1-47

Defterin ilk bölümü, 8051 komutlarına geçmeden önce gerekli altyapıyı verir. Bu kısım çalışma sırasında başa alınmalı, aritmetik/dallanma/stack konularından sonra tekrar en sona atılmamalıdır.

Sıra şu şekildedir:

1. **Sayı sistemleri:** Binary, decimal ve hexadecimal dönüşümleri; bit, nibble ve byte kavramları.
2. **Mikroişlemci ve mikrodenetleyici farkı:** Mikroişlemci dış çevre birimlerine daha bağımlıdır; mikrodenetleyicide CPU, RAM, ROM/Flash, I/O portları, timer ve seri haberleşme aynı çip içinde bulunur.
3. **8051 mimarisi:** Akümülatör (`A`), `B` saklayıcısı, `PSW`, `SP`, `PC`, `DPTR`, iç RAM, SFR alanı ve portlar.
4. **Bellek ve adresleme mantığı:** Genel amaçlı RAM bankları, bit adreslenebilir alan, SFR, doğrudan/dolaylı/saklayıcı/ivedi adresleme.
5. **Veri taşıma ve mantıksal komutlar:** `MOV`, `MOVC`, `MOVX`, `PUSH`, `POP`, `ANL`, `ORL`, `XRL`, `CLR`, `CPL` gibi komutların yeri.
6. **Keil/Proteus uygulama akışı:** Proje oluşturma, assembly kodu yazma, derleme, register/RAM penceresinden sonucu kontrol etme.

Bu bölüm, mevcut çalışmanın ön koşul arşividir. Aktif ilerleme, defterde `Stack Pointer ve Program Counter` sonrasındaki timer/kesme konularından devam eder.

## 1. Aritmetik Komutlar

Aritmetik komutlar toplama, çıkarma, çarpma ve bölme işlemlerini yapar.

- Toplama: `ADD`, `ADDC`, `INC`
- Çıkarma: `SUBB`, `DEC`
- Çarpma: `MUL`
- Bölme: `DIV`

8051'de toplama ve çıkarma işlemleri 8 bit + 8 bit şeklinde yapılır. 16 bitlik işlemler doğrudan tek komutla yapılmaz; düşük byte ve yüksek byte ayrı ayrı işlenir.

### 1.1 ADD ve ADDC

`ADD` ve `ADDC` iki operandlı toplama komutlarıdır.

```assembly
ADD A, kaynak
ADDC A, kaynak
```

Birinci operand her zaman akümülatördür (`A`). İkinci operand doğrudan, dolaylı, saklayıcı veya ivedi adresleme ile belirtilebilir.

`ADDC`, `ADD` komutu gibidir fakat Carry (`C`) bayrağını da toplama sonucuna ekler. Bu yüzden uzun tam sayı toplamalarında kullanılır.

16 bit toplamada sıra önemlidir:

1. Önce düşük byte'lar `ADD` ile toplanır.
2. Oluşan elde (`C`) yüksek byte toplamına `ADDC` ile eklenir.

Örnek:

```assembly
; X = 1234H, Y = 12EFH
; XL=78H, XH=79H, YL=7AH, YH=7BH adreslerinde tutulsun.

MOV 78H, #34H
MOV 79H, #12H
MOV 7AH, #0EFH
MOV 7BH, #12H

MOV A, 78H
ADD A, 7AH
MOV 78H, A

MOV A, 79H
ADDC A, 7BH
MOV 79H, A
```

Sonuç düşük byte olarak `78H`, yüksek byte olarak `79H` adresine yazılır.

### 1.2 SUBB

Çıkarma işlemi `SUBB` ile yapılır.

```assembly
SUBB A, kaynak
```

Akümülatör ilk operanddır. İkinci operand akümülatörden çıkarılır ve sonuç yine akümülatöre yazılır.

Carry bayrağı çıkarma işleminde ödünç alma mantığıyla kullanılır. Eğer çıkarma bir ödünç almaya ihtiyaç duyarsa Carry bayrağı belirlenir.

Uzun tam sayı çıkarmada işlem sırası toplamanın tersine dikkat ister:

1. Önce Carry temizlenir: `CLR C`
2. Önce düşük byte çıkarılır.
3. Sonra yüksek byte, önceki işlemdeki Carry dikkate alınarak çıkarılır.

Örnek:

```assembly
; X = 1234H, Y = 1135H

MOV 78H, #34H
MOV 79H, #12H
MOV 7AH, #35H
MOV 7BH, #11H

CLR C
MOV A, 78H
SUBB A, 7AH
MOV 78H, A

MOV A, 79H
SUBB A, 7BH
MOV 79H, A
```

### 1.3 INC ve DEC

`INC` bir artırma, `DEC` bir azaltma komutudur.

```assembly
INC A
DEC A
INC R0
DEC R0
```

Örnek:

```assembly
MOV A, #05H
INC A       ; A = 06H
DEC A       ; A = 05H

MOV R0, #30H
INC R0      ; R0 = 31H
DEC R0      ; R0 = 30H
```

## 2. Çarpma ve Bölme

Çarpma ve bölme komutları saklayıcıya özel komutlardır. Akümülatör (`A`) ve `B` saklayıcısını kullanır.

### 2.1 MUL AB

```assembly
MUL AB
```

`MUL AB`, `A` ve `B` saklayıcılarındaki iki işaretsiz 8 bit tam sayıyı çarpar.

- Sonucun düşük byte'ı `A` içine yazılır.
- Sonucun yüksek byte'ı `B` içine yazılır.
- Sonuç `FFFFH` değerinden büyük olamaz.
- Carry bayrağı kullanılmaz.
- Sonuç `FFH` değerinden büyükse Overflow (`OV`) bayrağı 1 olur.
- `B = 00H` olması, üst byte'ın sıfır olduğu anlamına gelir.

### 2.2 DIV AB

```assembly
DIV AB
```

`DIV AB`, akümülatördeki 8 bit işaretsiz sayıyı `B` saklayıcısındaki 8 bit işaretsiz sayıya böler.

- Bölüm `A` içinde tutulur.
- Kalan `B` içinde tutulur.
- Carry bayrağı her zaman temizlenir.
- Eğer bölme öncesinde `B = 00H` ise sonuç belirsizdir ve `OV` bayrağı 1 olur.
- `OV = 1` ise bölme işlemi yapılmamış kabul edilir.

## 3. Döndürme Komutları

Döndürme komutları akümülatör üzerinde çalışan saklayıcıya özel komutlardır.

Dört temel döndürme komutu vardır:

- `RL A`
- `RLC A`
- `RR A`
- `RRC A`

Bu komutlar akümülatördeki 8 bit sayıyı sağa veya sola bir bit döndürür. Carry kullanılan komutlarda Carry de döndürme zincirine katılır.

### 3.1 RL A

```assembly
RL A
```

Akümülatörü sola döndürür. Bit 7, bit 0 konumuna geçer.

### 3.2 RLC A

```assembly
RLC A
```

Akümülatörü Carry biti üzerinden sola döndürür. Bit 7 Carry'ye gider, eski Carry bit 0'a gelir.

### 3.3 RR A

```assembly
RR A
```

Akümülatörü sağa döndürür. Bit 0, bit 7 konumuna geçer.

### 3.4 RRC A

```assembly
RRC A
```

Akümülatörü Carry biti üzerinden sağa döndürür. Bit 0 Carry'ye gider, eski Carry bit 7'ye gelir.

Döndürme komutları maskeleme byte'ı oluşturmakta, 2'nin katlarıyla çarpma/bölme benzeri işlemlerde ve bit kaydırma mantığında kullanılır.

Önemli:

- Sola döndürme 2 ile çarpma etkisi oluşturabilir.
- Sağa döndürme 2 ile bölme etkisi oluşturabilir.
- Ancak çıkan bitin değeri dikkate alınmazsa sonuç hatalı olabilir.
- Carry üzerinden döndürmede işlem sonrasındaki Carry değeri mutlaka izlenmelidir.

## 4. Karşılıklı Değiştirme Komutları

### 4.1 SWAP A

```assembly
SWAP A
```

Akümülatörün yüksek nibble'ı ile düşük nibble'ı yer değiştirir.

Örnek:

```assembly
MOV A, #7BH
SWAP A      ; A = B7H
```

### 4.2 XCH

```assembly
XCH A, kaynak
```

Akümülatör ile belirtilen byte verinin içeriklerini karşılıklı değiştirir.

Kullanım biçimleri:

```assembly
XCH A, direct
XCH A, @Ri
XCH A, Rn
```

Örnek:

```assembly
MOV R0, #25H
MOV 25H, #78H
MOV A, #0FH

XCH A, @R0
```

Bu işlemden sonra `A = 78H`, `25H = 0FH` olur.

### 4.3 XCHD

```assembly
XCHD A, @Ri
```

Akümülatörün düşük nibble'ı ile `Ri` tarafından gösterilen adresteki verinin düşük nibble'ı yer değiştirir. Yüksek nibble'lar değişmez.

## 5. Program Akışı Kontrol Komutları

Dallanma komutları mikrodenetleyicinin yürütme sırasında farklı işlemler yapmasını sağlar. Örneğin bir tuşa veya kontrol sinyaline göre motoru durdurma/çalıştırma kararlarında kullanılır.

Dallanma komutları:

- `JMP`
- `CALL`
- `RET`

Dallanma komutları ikiye ayrılır:

- Durumdan bağımsız dallanma komutları
- Duruma bağlı dallanma komutları

## 6. Durumdan Bağımsız Dallanma

İlk üç dallanma komutu benzerdir. En son verilen indisli dallanma komutu ise `A` ile `DPTR` içeriklerini toplayarak bir sonraki okunacak ve yürütülecek komutun adresini hesaplar.

```assembly
JMP address
JMP @A+DPTR
```

Atlama aralıkları:

- `SJMP`: 128 byte'lık dallanma yapar.
- `AJMP`: En fazla 2 KB alan içinde dallanır.
- `LJMP`: 64 KB alan içinde dallanır.

## 7. Duruma Bağlı Dallanma

Koşullu dallanma komutlarında koşul sağlanırsa program akışı belirtilen adrese dallanır. Koşul sağlanmazsa program bir alt satırdan devam eder.

Komutlar:

```assembly
JZ address       ; A = 00H ise dallan
JNZ address      ; A != 00H ise dallan
JC address       ; C = 1 ise dallan
JNC address      ; C = 0 ise dallan
JB bit,address   ; bit = 1 ise dallan
JNB bit,address  ; bit = 0 ise dallan
JBC bit,address  ; bit = 1 ise biti temizle ve dallan
CJNE byte1,byte2,address
DJNZ byte,address
```

Kısa notlar:

- `JZ`, akümülatör sıfırsa dallanır.
- `JNZ`, akümülatör sıfır değilse dallanır.
- `JC`, Carry 1 ise dallanır.
- `JNC`, Carry 0 ise dallanır.
- `JB`, belirtilen bit 1 ise dallanır.
- `JNB`, belirtilen bit 0 ise dallanır.
- `JBC`, bit 1 ise önce biti sıfırlar, sonra dallanır.
- `CJNE`, iki değeri karşılaştırır; eşit değilse dallanır.
- `DJNZ`, değeri 1 azaltır; yeni değer sıfır değilse dallanır.

## 8. Alt Programlar

Bir program yazılırken program içinde birden fazla kez koşturulacak kodlar olabilir. Bu kodların her yerde tekrar yazılması program hafızasının şişmesine ve programın izlenebilirliğinin azalmasına neden olur.

Bu nedenle tekrar eden kod parçalarının alt program olarak yazılması uygundur.

Alt program çağırma ve dönüş komutları:

```assembly
ACALL alt_program
LCALL alt_program
RET
```

- `ACALL`: Alt program çağırır ve 2 KB alan içinde çalışır.
- `LCALL`: Alt program çağırır ve 64 KB alan içinde çalışır.
- `RET`: Alt programdan çıkar.

Alt programlar `RET` komutunu görene kadar çalışır. `RET` ile program akışı, alt programın çağrıldığı komuttan sonraki komuta döner.

Önemli:

- Alt programa girerken mutlaka `ACALL` veya `LCALL` kullanılmalıdır.
- Alt programdan çıkmak için mutlaka `RET` olmalıdır.
- Ana programın yanlışlıkla alt programa düşmesini önlemek için uygun yerde `SJMP` gibi bir atlama kullanılmalıdır.

## 9. Stack Pointer ve Program Counter

### 9.1 Stack Pointer

Stack pointer, alt programlardan veya kesme hizmet alt programlarından ana programa geri dönüş adresinin tutulduğu yığıt adresini yönetir. `PUSH` ve `POP` komutlarıyla veri yazılıp okunabilen saklayıcıdır.

Yığın işaretçisinin hatalı kullanımı programın çökmesine veya istenmeyen durumlara neden olabilir.

### 9.2 Program Counter

Program Counter (`PC`) genellikle 16 bittir ve programdaki bir sonraki yürütülecek komutun adresini saklar.

### 9.3 ACALL ve LCALL sırasında olanlar

Alt program çağrısında dönüş adresi üretilir:

```text
PC <- PC + 2    ; ACALL
PC <- PC + 3    ; LCALL
```

Dönüş adresinin düşük 8 biti RAM'e yazılır:

```text
SP <- SP + 1
@SP <- PCL
```

Dönüş adresinin yüksek 8 biti RAM'e yazılır:

```text
SP <- SP + 1
@SP <- PCH
```

Sonra program akışı alt program adresine gider:

```text
PC <- alt_program_adresi
```

### 9.4 RET sırasında olanlar

`RET` komutunda dönüş adresi stack'ten geri alınır:

```text
PCH <- @SP
SP <- SP - 1
PCL <- @SP
SP <- SP - 1
```

Böylece `PC` geri dönüş adresini alır ve ana program kaldığı yerden devam eder.

---

## 10. Timer/Counter ve Kesme Mantığı

**Kaynak:** `defter/mikroslemci_zt.pdf`, s. 64-70

8051'de timer/counter birimi zaman gecikmesi üretmek, belirli aralıklarla olay tetiklemek veya dış darbeleri saymak için kullanılır.

- **Timer modu:** İç makine çevrimlerini sayar; gecikme ve periyodik işlem için kullanılır.
- **Counter modu:** Dışarıdan gelen pulse/darbe sayılır; olay sayma mantığına uygundur.
- **THx/TLx:** Timer değerinin yüksek ve düşük byte alanlarıdır.
- **TMOD:** Timer modunu ve timer/counter seçimini belirleyen kontrol saklayıcısıdır.
- **TCON:** Timer başlatma/durdurma ve taşma bayrakları gibi çalışma durumlarını tutar.
- **TR0/TR1:** Timer 0 veya Timer 1'i başlatır.
- **TF0/TF1:** Timer taşınca set edilen bayraktır; polling ile okunabilir veya kesme tetikleyebilir.

Timer hesabında temel fikir şudur:

1. İstenen gecikme süresi belirlenir.
2. Kristal frekansı ve makine çevrimi dikkate alınır.
3. Timer'ın kaç sayım yapması gerektiği bulunur.
4. Başlangıç değeri `THx/TLx` içine yüklenir.
5. Timer başlatılır ve taşma bayrağı beklenir.

Kısa akış:

```assembly
MOV TMOD, #01H   ; Timer0 Mode 1
MOV TH0, #...    ; yüksek byte başlangıç değeri
MOV TL0, #...    ; düşük byte başlangıç değeri
SETB TR0         ; Timer0 başlat
; TF0 set olana kadar bekle
CLR TR0          ; Timer0 durdur
CLR TF0          ; taşma bayrağını temizle
```

**Hatırlatma:** Timer konusu, stack/PC'den sonra gelir; çünkü timer kesmesi oluştuğunda işlemci yine dönüş adresi ve program akışını yönetmek zorundadır.

## 11. Kesme, Bit Düzeyi Kontrol ve Final Tekrarı

**Kaynak:** `defter/mikroslemci_zt.pdf`, s. 68-74

Kesme (interrupt), işlemcinin ana programı geçici olarak durdurup acil bir olaya ait kesme hizmet alt programına gitmesidir. Kesme tamamlanınca işlemci ana programa geri döner.

Temel kavramlar:

- **Kesme kaynağı:** Timer taşması, dış kesme pini veya seri haberleşme gibi olaylar.
- **Kesme vektörü:** Kesme geldiğinde programın gideceği sabit adres.
- **Kesme hizmet alt programı:** O olaya cevap veren kısa program parçası.
- **Global izin:** Genel kesmeleri açıp kapatan ana izin.
- **Özel izin:** Belirli bir kaynağın kesmesini açıp kapatan izin.
- **Öncelik:** Aynı anda birden fazla kesme gelirse hangisinin önce işleneceğini belirler.

Bit düzeyi kontrolde kullanılan komutlar:

```assembly
SETB bit   ; biti 1 yap
CLR bit    ; biti 0 yap
CPL bit    ; biti tersle
JB bit, etiket
JNB bit, etiket
JBC bit, etiket
```

Port işlemlerinde `P0`, `P1`, `P2`, `P3` pinleri bit bit kontrol edilebilir. Örneğin LED yakma/söndürme, buton okuma veya timer taşmasına bağlı pin değiştirme gibi sorular finalde bu mantığa bağlanır.

**Sınav sırası açısından:** Önce sayı sistemi ve 8051 mimarisi, sonra veri taşıma/mantık, sonra aritmetik, dallanma, alt program-stack, en son timer/kesme çalışılmalıdır.
