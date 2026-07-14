# Mikroişlemciler - Defter Notları

Bu dosya yalnızca `defter/mikroslemci_zt.pdf` içindeki defter notlarından hazırlanmıştır.

---

## 26 Nisan 2026 - Aritmetik Komutlar

**Kısa Tanım:** 8051'de aritmetik işlemler 8 bitlik parçalar halinde yapılır.

**Anahtar Kavramlar:**
- `ADD`: A ile kaynak veriyi toplar.
- `ADDC`: A ile kaynak veriyi Carry dahil ederek toplar.
- `SUBB`: A'dan kaynak veriyi Carry/ödünç dahil ederek çıkarır.
- `INC` / `DEC`: Bir artırma ve bir azaltma yapar.

**Örnek / Komut:** `ADD A, 7AH`, `ADDC A, 7BH`, `SUBB A, 7AH`

**Hatırla:** 16 bit toplamada önce düşük byte `ADD`, sonra yüksek byte `ADDC` ile yapılır; çıkarmada önce `CLR C` gerekir.

---

## 26 Nisan 2026 - Çarpma ve Bölme

**Kısa Tanım:** Çarpma ve bölme, doğrudan `A` ve `B` saklayıcılarıyla yapılan özel komutlardır; normal `ADD`/`SUBB` gibi bellek adresiyle yazılmaz.

**Anahtar Kavramlar:**
- `MUL AB`: `A` içindeki 8 bit işaretsiz sayı ile `B` içindeki 8 bit işaretsiz sayıyı çarpar.
- Çarpma sonucu 16 bit olabilir: düşük byte `A`'ya, yüksek byte `B`'ye yazılır.
- `MUL AB` işleminde sonuç `FFFFH` değerinden büyük olamaz; Carry bayrağı kullanılmaz.
- Çarpma sonucu `FFH` değerini aşarsa `OV = 1` olur. Bu, `B` saklayıcısında yüksek byte oluştuğunu gösterir.
- `DIV AB`: `A` içindeki sayıyı `B` içindeki sayıya böler.
- Bölme sonucunda bölüm `A`'da, kalan `B`'de tutulur.
- `DIV AB` işleminde Carry her zaman temizlenir.
- Bölme öncesi `B = 00H` ise sıfıra bölme olur; sonuç belirsizdir ve `OV = 1` olur.

**Örnek / Komut:**
```assembly
MOV A, #12H
MOV B, #10H
MUL AB      ; Sonuç 0120H: A = 20H, B = 01H

MOV A, #17H
MOV B, #05H
DIV AB      ; 17H / 05H: A = bölüm, B = kalan
```

**Hatırla:** Çarpmada `A` düşük byte, `B` yüksek byte olur; bölmede `A` bölüm, `B` kalan olur. `B = 00H` iken `DIV AB` yapılmaz.

---

## 26 Nisan 2026 - Döndürme Komutları

**Kısa Tanım:** Döndürme komutları A içindeki bitleri sağa veya sola kaydırır/döndürür.

**Anahtar Kavramlar:**
- `RL A`: A'yı sola döndürür.
- `RLC A`: A'yı Carry üzerinden sola döndürür.
- `RR A`: A'yı sağa döndürür.
- `RRC A`: A'yı Carry üzerinden sağa döndürür.

**Örnek / Komut:** `RL A`, `RRC A`

**Hatırla:** Carry üzerinden döndürmede işlem sonrasındaki Carry değeri sonucu etkiler; 2 ile çarpma/bölme mantığında çıkan biti unutma.

---

## 26 Nisan 2026 - Karşılıklı Değiştirme Komutları

**Kısa Tanım:** `SWAP`, `XCH` ve `XCHD` veri parçalarının yerini değiştirir.

**Anahtar Kavramlar:**
- `SWAP A`: A'nın yüksek ve düşük nibble'ını değiştirir.
- `XCH A, kaynak`: A ile kaynak byte yer değiştirir.
- `XCHD A, @Ri`: A'nın ve gösterilen adresin düşük nibble'larını değiştirir.

**Örnek / Komut:** `MOV A, #7BH` ardından `SWAP A` sonucu `A = B7H`

**Hatırla:** `XCHD` sadece düşük nibble'ları değiştirir; yüksek nibble'lar aynı kalır.

---

## 26 Nisan 2026 - Program Akışı ve Dallanma

**Kısa Tanım:** Dallanma komutları programın hangi komuttan devam edeceğini değiştirir.

**Anahtar Kavramlar:**
- `SJMP`: Kısa atlama, 128 byte.
- `AJMP`: 2 KB alan içinde atlama.
- `LJMP`: 64 KB alan içinde atlama.
- `JZ` / `JNZ`: A sıfır veya sıfır değilse dallanma.
- `JC` / `JNC`: Carry durumuna göre dallanma.
- `CJNE`: Karşılaştır ve eşit değilse dallan.
- `DJNZ`: Azalt ve sıfır değilse dallan.

**Örnek / Komut:** `JZ X1`, `DJNZ R0, X2`

**Hatırla:** Koşul sağlanırsa adrese dallanılır; sağlanmazsa bir alt satırdan devam edilir.

---

## 26 Nisan 2026 - Alt Program, Stack Pointer ve PC

**Kısa Tanım:** Alt programlar tekrar eden kodları tek yerde tutar; dönüş adresi stack üzerinde saklanır.

**Anahtar Kavramlar:**
- `ACALL`: 2 KB alan içinde alt program çağırır.
- `LCALL`: 64 KB alan içinde alt program çağırır.
- `RET`: Alt programdan çıkar.
- `SP`: Dönüş adresinin RAM'de tutulacağı yeri yönetir.
- `PC`: Bir sonraki çalışacak komutun adresini tutar.

**Örnek / Komut:** `ACALL alt1`, `RET`

**Hatırla:** Alt program `RET` olmadan çıkamaz; ana program yanlışlıkla alt programa düşmesin diye atlama komutu kullanılmalıdır.

---

## 27 Mayıs 2026 - Timer ve Counter Mantığı

**Kaynak:** `defter/mikroslemci_zt.pdf`, s. 64-67
**Kısa Tanım:** 8051'de timer ve counter birimi, zaman gecikmeleri üretmek veya dış darbeleri saymak için kullanılan SFR donanımlarıdır.

**Anahtar Kavramlar:**
- **Timer Modu:** İç makine çevrimlerini (kristal frekansına bağlı olarak) sayar. Gecikme üretmekte kullanılır.
- **Counter Modu:** Dışarıdan (`T0`/`T1` pinleri) gelen darbeleri (1'den 0'a geçişleri) sayar. Olay sayma işlemlerinde kullanılır.
- **TMOD saklayıcısı:** Timer/Counter modunu ve çalışma şeklini (Mod 0-3) belirler. Bit adreslenemez.
- **TCON saklayıcısı:** Timer'ları başlatma/durdurma (`TR0`/`TR1` bitleri) ve taşma bayraklarını (`TF0`/`TF1`) barındırır. Bit adreslenebilir.
- **THx ve TLx:** 16 bitlik timer değerinin yüksek (`TH`) ve düşük (`TL`) byte saklayıcılarıdır.

**Örnek / Komut:**
```assembly
MOV TMOD, #01H   ; Timer 0'ı 16 bitlik Mod 1 olarak ayarla
MOV TH0, #0FFH   ; Başlangıç değerinin yüksek byte'ı
MOV TL0, #00H   ; Başlangıç değerinin düşük byte'ı
SETB TR0         ; Timer 0'ı başlat
JNB TF0, $       ; TF0 taşma bayrağı 1 olana kadar burada bekle (polling)
CLR TR0          ; Timer 0'ı durdur
CLR TF0          ; Taşma bayrağını temizle
```

**Hatırla:** 8051'de her 12 osilatör çevrimi 1 makine çevrimi (machine cycle) yapar. 12 MHz kristal kullanılan bir sistemde 1 makine çevrimi tam 1 mikrosaniyedir ($\mu s$).

---

## 27 Mayıs 2026 - Kesme (Interrupt) Mantığı

**Kaynak:** `defter/mikroslemci_zt.pdf`, s. 68-70
**Kısa Tanım:** Kesme, işlemcinin ana programı koştururken öncelikli bir dış olay veya timer taşması nedeniyle mevcut işini yarıda kesip kesme alt programına gitmesidir.

**Anahtar Kavramlar:**
- **Kesme Vektörü (Interrupt Vector):** Kesme oluştuğunda işlemcinin otomatik olarak dallandığı sabit ROM adresleridir (Örn: Timer 0 Kesmesi için `000BH`).
- **IE (Interrupt Enable):** Kesmeleri genel (`EA`) ve özel (`ET0`, `EX0` vb.) olarak açıp kapatan SFR saklayıcısıdır.
- **IP (Interrupt Priority):** Birden fazla kesme aynı anda geldiğinde hangisinin önce çalışacağını belirleyen öncelik saklayıcısıdır.
- **RETI:** Kesme hizmet alt programının (ISR) bittiğini ve ana programa dönüleceğini belirten özel dönüş komutudur.

**Örnek / Komut:**
```assembly
SETB ET0  ; Timer 0 kesmesini etkinleştir
SETB EA   ; Genel kesme iznini aç (Global Interrupt Enable)
; Kesme alt programı (ISR) tanımı
ORG 000BH ; Timer 0 Kesme Vektör adresi
LJMP T0_ISR
; ...
T0_ISR:
CPL P1.0  ; Port 1.0 pinindeki LED'i tersle
RETI      ; Kesmeden dön
```

**Hatırla:** Normal alt programlar `RET` ile biterken, kesme alt programları mutlaka `RETI` ile bitmelidir. `RETI`, kesme kontrol mantığına kesmenin bittiğini bildirir.

---

## 27 Mayıs 2026 - Bit Düzeyi Kontrol ve Port İşlemleri

**Kaynak:** `defter/mikroslemci_zt.pdf`, s. 70-74
**Kısa Tanım:** 8051'de bit adreslenebilir RAM alanı veya SFR portları üzerinde doğrudan tek bir bitin değerini değiştiren ya da sorgulayan komutlardır.

**Anahtar Kavramlar:**
- **SETB / CLR / CPL:** Sırasıyla hedef biti 1 yapar, 0 yapar ve tersler (tümleyenini alır).
- **JB / JNB:** Belirtilen bit 1 ise (`JB`) veya 0 ise (`JNB`) adrese dallanır.
- **JBC:** Bit 1 ise belirtilen adrese dallanır ve dallanırken o biti otomatik olarak sıfırlar (`CLR`).
- **Port Kontrolü:** `P0`, `P1`, `P2`, `P3` portlarının her bir pini (örneğin `P1.2`) bağımsız giriş veya çıkış olarak yönlendirilebilir.

**Örnek / Komut:**
```assembly
SETB P1.0    ; Port 1.0 pinini logik 1 yap (LED yak)
CLR P1.1     ; Port 1.1 pinini logik 0 yap (LED söndür)
CPL P1.2     ; Port 1.2 pinini tersle
JNB P3.2, BTN_BASILDI ; P3.2 pinindeki butona basıldıysa (0 ise) dallan
```

**Hatırla:** Bit düzeyinde dallanma komutları (`JB`, `JNB`, `JBC`) sadece kısa atlama (`SJMP` gibi -128 ile +127 byte aralığında) yapabilir.
