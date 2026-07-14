# Mikroişlemciler Çalışma Planı

**Son güncelleme:** 23 Mayıs 2026

## Kaynak Kuralı

Mikroişlemciler için ana kaynak `defter/` klasörüdür. `defter/mikroslemci_zt.pdf` gerçek ders sırasını verir. Aynı dosyanın `pdf/mikroslemci_zt.pdf` altında bulunması durumunda `defter/` kopyası esas alınır.

## Kaynak Envanteri

| Sıra | Kaynak | Sayfa | Kullanım |
| --- | --- | --- | --- |
| 1 | `defter/mikroslemci_zt.pdf` | s. 1-5 | Sayı sistemleri ve bit/byte temeli |
| 2 | `defter/mikroslemci_zt.pdf` | s. 6-15 | Mikrodenetleyici, 8051 mimarisi, saklayıcılar |
| 3 | `defter/mikroslemci_zt.pdf` | s. 16-28 | Adresleme, veri taşıma, mantıksal komutlar |
| 4 | `defter/mikroslemci_zt.pdf` | s. 29-47 | Keil/Proteus, 8051 uygulama görüntüleri |
| 5 | `defter/mikroslemci_zt.pdf` | s. 48-63 | Aritmetik, dallanma, alt program, stack ve PC |
| 6 | `defter/mikroslemci_zt.pdf` | s. 64-74 | Timer/counter, kesme, bit düzeyi kontrol ve final tekrarı |

## Güncel Durum

Defter notları ve mevcut `defter_notlari.md` birlikte okunduğunda son tamamlanan aktif konu **Stack Pointer ve Program Counter** olarak görünüyor. Bu yüzden sıradaki konu başa dönmek değil, defterdeki devam sırasına göre **Timer/Counter Mantığı** olmalıdır.

## Önerilen Çalışma Sırası

1. Timer ve counter farkını kavra.
2. `TMOD`, `TCON`, `TH0/TL0`, `TH1/TL1`, `TR0/TR1`, `TF0/TF1` alanlarını öğren.
3. Basit timer taşma akışını çiz.
4. Timer kesmesinin ana programı nasıl böldüğünü açıkla.
5. `SETB`, `CLR`, `CPL`, `JB`, `JNB`, `JBC` ile port/bit senaryolarını çöz.
6. Keil/Proteus üzerinde register, RAM, port ve timer sonucunu takip et.

## Sıradaki Konu

**8. Bölüm: Timer/Counter Mantığı**

## Yeni Materyal İşleme Kuralı

Mikroişlemcilerde yeni kaynak geldiğinde önce `defter/` kontrol edilir. Faydalı içerik `ders.md` içine Türkçe ve sıralı biçimde eklenir; `yol_haritasi.md` defter sayfa sırasını bozmayacak şekilde güncellenir.
