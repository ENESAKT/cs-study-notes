# Sistem Programlama Çalışma Planı

## GitHub'a Giden Dosyalar

- `ders.md`
- `defter_notlari.md`
- `calisma_plani.md`

`yol_haritasi.md`, `oturum_durumu.md`, `pdf/`, `defter/`, PDF dosyaları ve el yazısı kaynaklar yerelde kalır.

## Kaynak Kuralı

- `pdf/`: hocanın slayt/ders materyalleri.
- `defter/`: derste tutulan el yazısı/kullanıcı notları.
- İki kaynak aynı konuyu anlatıyorsa önce hoca materyaliyle ana iskelet kurulur, sonra defterdeki pratik vurgu ve örnekler `ders.md` içine eklenir.

## Sıra

1. Süreç yönetimi: `fork`, `exec`, PID, UID/GID, zombi/yetim süreçler.
2. İş kontrolü: process group, session, daemon süreç reçetesi.
3. Bellek yönetimi: sanal bellek, sayfalama, TLB, page fault.
4. Bellek bölgeleri: text, data, BSS, heap, stack.
5. Dinamik bellek tahsisi: `malloc`, `calloc`, `realloc`, `free`.
6. Bellek paylaşımı ve Copy-on-Write: ortak kütüphaneler, sıfır sayfa, `fork()` sonrası sayfa kopyalama.
7. Bellek hataları ve hizalama: memory leak, use-after-free, double free, padding, `posix_memalign`.
8. Alt seviye tahsis mekanizmaları: program break, `brk`, `sbrk`, anonim `mmap`, `munmap`.
9. Tahsisçi ayarları ve istatistikleri: `mallopt`, `mallinfo`, `malloc_stats`.
10. Stack tabanlı geçici tahsis ve bellek yardımcıları: `alloca`, VLA, `memset`, `memcpy`, `memmove`.
11. RAM'de kilitleme ve Linux bellek politikası: `mlock`, `mlockall`, overcommit, OOM Killer.
12. Dosya G/Ç temelleri: fd, `open`, `creat`, izinler, `umask`, `read`, `close`, `lseek`.
13. İleri dosya G/Ç: `select`, `poll`, `epoll`, VFS, page cache, readahead, writeback, `fsync`, `mmap`.

## Son Eklenen Materyaller

- `Ders_3.pdf`: Bellek yönetimi slaytları `ders.md` içine yapılandırılmış biçimde eklendi.
- `defter/30042026.pdf`: Derste tutulan el yazısı notlardaki pratik vurgular `ders.md` içindeki Ders 3 notlarına işlendi.
- `Ders_4__1_.pdf`: Dosya G/Ç devam slaytlarındaki multiplexing, VFS, page cache, writeback ve `mmap` başlıkları `ders.md` içine eklendi.

## Önerilen Sonraki Konu

Yol haritasındaki sıraya göre sıradaki eksik konu: **10. Bölüm: İleri Dosya G/Ç ve Çekirdek G/Ç Mekanizmaları**.

## Yeni Materyal İşleme Kuralı

Sistem Programlama için yeni PDF veya ders materyali geldiğinde kaynak dosya GitHub'a eklenmez. İçerik `ders.md` içine özetlenir, gerekiyorsa bu plan güncellenir.
