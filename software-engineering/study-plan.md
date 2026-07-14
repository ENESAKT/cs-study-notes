# Yazılım Mühendisliği — 14 Günlük Çalışma Takvimi & Yol Haritası

> **Hazırlanma Tarihi:** 5 Haziran 2026
> **Amaç:** Tüm PDF'leri sıfırdan, bağımlılık sırasına göre interaktif öğretme
> **Sınav Formatı (el yazısı notlarından):** 50 puan KOD + 50 puan SÖZEL (toplam 20-25 soru)
> **Yöntem:** Her gün bir konuya odaklanma → Teori + Analoji + C# Kod + Kavrama Sorusu

---

## 📋 Kaynak Dosyalar — PDF Sayfa Haritası

| # | PDF | Sayfa | Konu | Sözel/Kod |
|---|-----|-------|------|-----------|
| 1 | Ders1.pdf | 23 s. (s. 1-23) | Yazılım Müh. Giriş | Sözel |
| 2 | Ders2_YazilimSurecleri.pdf | 92 s. (s. 1-92) | Süreç Modelleri | Sözel ⚠️ |
| 3 | Ders3_Kod.pdf | 5 s. (s. 1-5) | OOP / Kalıtım | Kod |
| 4 | UML.pdf | 21 s. (s. 1-21) | UML Diyagramları | Sözel ⚠️ |
| 5 | Ders4_Sunum.pdf | 22 s. (s. 1-22) | Tasarım Desenleri Giriş | Sözel |
| 6 | Ders4_Kod.pdf | 8 s. (s. 1-8) | Factory Method | Kod |
| 7 | Ders5.pdf | 9 s. (s. 1-9) | Builder Pattern | Kod ⚠️ |
| 8 | Ders6.pdf | 11 s. (s. 1-11) | Generic + Singleton | Kod |
| 9 | Ders7.pdf | 8 s. (s. 1-8) | Adapter Pattern | Kod |
| 10 | Ders8.pdf | 11 s. (s. 1-11) | Decorator + Facade | Kod ⚠️ |
| 11 | Ders9.pdf | 8 s. (s. 1-8) | Proxy + **Composite** | Kod |
| 12 | Ders10.pdf | 10 s. (s. 1-10) | Observer + **State** | Kod ⚠️ |

> ⚠️ = El yazısı notlarına göre sınavda özellikle sorulması beklenen konular

---

## 📅 14 Günlük Takvim

### 🟢 HAFTA 1 — Temeller (Sözel Ağırlıklı)

#### 📌 GÜN 1: Yazılım Mühendisliğine Giriş (Ders1.pdf, s. 1-23)
- Profesyonel yazılım geliştirme vs bireysel programlama (s. 7-9)
- Yazılım mühendisliği çeşitliliği ve 8 uygulama türü (s. 11)
- Etik: gizlilik, yeterlilik, fikri mülkiyet, kötü kullanım (s. 13)
- Durum çalışmaları: insülin pompası (s. 16), ZihinSaS (s. 20), hava istasyonu (s. 21), sayısal öğrenme (s. 22)
- ⏱️ Tahmini süre: 45 dk

#### 📌 GÜN 2: Yazılım Süreçleri — BÖLÜM 1 (Ders2_YazilimSurecleri.pdf, s. 1-58)
- Sürecin tanımı ve 4 temel aktivite (s. 4-17)
- Code and Fix (s. 30), Waterfall/Şelale (s. 33-38), V Modeli (s. 39-46)
- Evolutionary (s. 48-50), Prototyping (s. 56-58)
- ⚠️ **El yazısı notu:** Süreç tablosu kesin sorulacak! Şelale modeli özellikle önemli
- ⏱️ Tahmini süre: 60 dk

#### 📌 GÜN 3: Yazılım Süreçleri — BÖLÜM 2 + V&V (Ders2_YazilimSurecleri.pdf, s. 59-92)
- Spiral Model (s. 59-63), Component-based (s. 66), Incremental (s. 67-74)
- Unified Process (s. 77-81), Agile Manifesto (s. 82-88)
- Formal Methods (s. 64-65)
- **Verification vs Validation** (s. 45) — çift çizgili not: mantığını kavra!
- Model karşılaştırma tablosu (s. 89-90)
- ⏱️ Tahmini süre: 60 dk

#### 📌 GÜN 4: OOP Temelleri (Ders3_Kod.pdf, s. 1-5) + UML Başlangıç (UML.pdf, s. 1-6)
- Sınıf, alan, constructor, kalıtım, `virtual`/`override` (s. 1-3)
- Kisi → Ogrenci / Ogretmen örneği — C# kodunu satır satır (s. 1-5)
- UML'e giriş: nedir, neden kullanılır, tarihçe (UML.pdf s. 1-4)
- ⏱️ Tahmini süre: 75 dk

#### 📌 GÜN 5: UML Detay (UML.pdf, s. 7-21)
- Yapısal diyagramlar: sınıf (s. 8-9), nesne (s. 10), bileşen (s. 11), dağıtım (s. 12), paket (s. 12), profil (s. 12)
- Davranışsal diyagramlar: etkinlik (s. 13), use case (s. 13), zamanlama (s. 14), durum makinesi (s. 15), sıralı (s. 15), iletişim (s. 15)
- İlişkiler: Association, Aggregation, Composition, Inheritance, Dependency, Realization (s. 16-19)
- İlişkiler karşılaştırma tablosu (s. 20) + Çokluk gösterimleri (s. 21)
- ⚠️ **El yazısı notu:** UML modeller 8-9 arası soru gelecek! İlişki sembolleri ve farkları çok önemli
- ⏱️ Tahmini süre: 75 dk

---

### 🔵 HAFTA 2 — Tasarım Desenleri (Kod Ağırlıklı)

#### 📌 GÜN 6: Tasarım Desenleri Genel Bakış (Ders4_Sunum.pdf, s. 1-22)
- Desen nedir, algoritmadan farkı — yemek tarifi vs mimari plan (s. 1-5)
- Idiom → Pattern → Architectural Pattern hiyerarşisi (s. 11-14)
- Üç ana grup: Creational, Structural, Behavioral (s. 15-18)
- Gereksiz/dogmatik kullanım tuzakları (s. 19-22)
- ⏱️ Tahmini süre: 45 dk

#### 📌 GÜN 7: Factory Method (Ders4_Kod.pdf, s. 1-8)
- Factory Method amacı: nesne oluşturmayı alt sınıflara devretmek (s. 1-2)
- Avantaj/dezavantaj: Loose Coupling, OCP vs sınıf sayısı artması (s. 2-3)
- C# kodu: ITasima + Kamyon/Ucak/Gemi + LojistikFabrika (s. 4-8)
- ⏱️ Tahmini süre: 60 dk

#### 📌 GÜN 8: Builder Pattern (Ders5.pdf, s. 1-9) + Singleton (Ders6.pdf, s. 10-11)
- Constructor patlaması problemi (s. 1-3)
- Setter ile inşa vs Builder zinciri karşılaştırması (s. 4)
- Fluent API: `.Renk("Kirmizi").Hp(200).Build()` (s. 5-7)
- Director rolü, Build() doğrulaması (s. 8-9)
- Singleton: Private Constructor, Static Instance, Global Erişim (Ders6.pdf s. 10-11)
- ⚠️ **El yazısı notu:** Builder çıkabilir! Zincirleme yapıyı iyi bil. Singleton → constructor private neden?
- ⏱️ Tahmini süre: 75 dk

#### 📌 GÜN 9: Generic Yapılar (Ders6.pdf, s. 1-9)
- Generic nedir, neden kullanılır — Type Safety, Boxing/Unboxing, Reusability (s. 1-3)
- Generic sınıf ve metot yazımı (s. 4-7)
- Kısıtlamalar: `where T : class/struct/new()/IEntity` (s. 8-9)
- ⚠️ **El yazısı notu:** Generics sınıftan çıkar, metottan da olabilir. Runtime/compiletime farkı sorulabilir
- ⏱️ Tahmini süre: 60 dk

#### 📌 GÜN 10: Adapter Pattern (Ders7.pdf, s. 1-8)
- Adapter nedir, ne zaman kullanılır — Legacy code, 3rd party (s. 1-2)
- Target, Adaptee, Adapter, Client rolleri (s. 2-3)
- C# kodu: IOdemeIslemi → XBankasiAdapter (s. 4-8)
- ⚠️ **El yazısı notu:** Adapter nedir iyi bilinmesi gerekir!
- ⏱️ Tahmini süre: 45 dk

---

### 🟣 HAFTA 3 — İleri Desenler + Final Hazırlık

#### 📌 GÜN 11: Decorator + Facade (Ders8.pdf, s. 1-11)
- Decorator nedir, Adapter'dan farkı (s. 1-2)
- Dinamik davranış ekleme mantığı (s. 2-3)
- Component, ConcreteComponent, Decorator, ConcreteDecorator rolleri (s. 3)
- Kahve örneği (s. 3-5), Araba Decorator (s. 6-8) — C# kodu satır satır
- Facade: Ev Sinema Sistemi (s. 9-11)
- ⚠️ **El yazısı notu:** Decorator → özellik dahildir, iç içe constructor! Senaryo sorusu gelecek. Facade → ev sinema sistemi, ön cepheli
- ⏱️ Tahmini süre: 60 dk

#### 📌 GÜN 12: Proxy + Composite (Ders9.pdf, s. 1-8)
- Proxy nedir: erişim kontrolü, lazy loading, logging, cache (s. 1)
- Virtual / Protection / Remote / Caching / Logging Proxy çeşitleri (s. 1)
- C# kodu: Kütüphane yetki proxy (s. 2-3), ProxyImage lazy loading (s. 4-5)
- **Composite deseni:** ağaç yapısı, recursive yapı (s. 6)
- C# kodu: ICalisan/Departman maaş hesaplama (s. 6-7), IShape/ShapeGroup (s. 8)
- Adapter, Decorator, Proxy ve Composite farkları (ÇOK ÖNEMLİ!)
- ⚠️ **El yazısı notu:** Proxy/Adapter/Decorator farkları kesin sorulacak. Composite → ağaç yapısı, recursive yapı hangi model
- ⏱️ Tahmini süre: 75 dk

#### 📌 GÜN 13: Observer + State (Ders10.pdf, s. 1-10)
- Subject/Observer rolleri ve yayıncı-abone mantığı (s. 1)
- Abone ekle-çıkar-bildir akışı (s. 1-2)
- C# kodu: Sipariş bildirimi — Email/SMS/Log (s. 2-4)
- C# kodu: Öğrenci not güncelleme (s. 5-6)
- **State deseni:** if-else yapıları ortadan kalkar (s. 7)
- C# kodu: Ödeme durumları — Beklemde/Ödendi/İptal (s. 7-8)
- State + Observer birleşik örnek (s. 9-10)
- Observer ile Proxy/Decorator/Adapter farkları
- ⚠️ **El yazısı notu:** Observer'ın en belirgin özelliği update metodu. Neden iki interface var? State → if-else yapıları ortadan kalkar
- ⏱️ Tahmini süre: 75 dk

#### 📌 GÜN 14: Final Gözden Geçirme & Sınav Simülasyonu
- Tüm desenlerin karşılaştırma tablosu
- Hangi durumda hangi desen? → Karar ağacı
- E-ticaret senaryosu: tüm desenleri nereye uygularsın?
- Mini sınav simülasyonu (25 soru: 12-13 sözel + 12-13 kod)
- ⏱️ Tahmini süre: 90 dk

---

## 🔴 El Yazısı Notlarından Kritik Sınav İpuçları

1. **Yazılım süreçleri tablosu** kesin sorulacak — modellerin karşılaştırması (Ders2, s. 89-90)
2. **UML modeller** 8-9 arası soru — ilişki sembolleri, çokluk (UML.pdf, s. 16-21)
3. **Builder** çıkabilir — zincirleme, Fluent API (Ders5.pdf, s. 5-7)
4. **Singleton** → constructor private neden yapılır, hangi model? → Proxy model (Ders6.pdf, s. 10-11)
5. **Generics** → sınıftan çıkar, metottan da olabilir; runtime/compiletime farkı (Ders6.pdf, s. 1-9)
6. **Adapter** iyi bilinmeli (Ders7.pdf, s. 1-8)
7. **Decorator** → özellik dahildir, iç içe constructor (Ders8.pdf, s. 1-8)
8. **Proxy/Adapter/Decorator farkları** kesin sorulacak (Ders9.pdf + genel)
9. **Observer** → update özelliği, neden iki interface, abstract class olan versiyon (Ders10.pdf, s. 1-6)
10. **Facade** → ev sinema sistemi, ön cepheli (Ders8.pdf, s. 9-11)
11. **Composite** → ağaç yapısı, recursive yapı hangi model (Ders9.pdf, s. 6-8)
12. **State** → if-else yapıları ortadan kalkar (Ders10.pdf, s. 7-10)
13. **50 puan kod / 50 puan sözel** formatı

---

## GitHub'a Giden Dosyalar

- `ders.md`
- `defter_notlari.md`
- `calisma_plani.md`

Hafta dosyaları, pratik dosyaları, scriptler ve yerel çalışma dosyaları GitHub'a varsayılan olarak gönderilmez.

## Yeni Materyal İşleme Kuralı

Yeni haftalık dosya veya PDF geldiğinde ayrı dosya olarak pushlanmaz. İçeriği `ders.md` içine eklenir ve bu plan gerekirse güncellenir.
