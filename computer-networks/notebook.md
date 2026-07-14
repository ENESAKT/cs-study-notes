# Bilgisayar Ağları - Defter Notları

Bu dosya, konularla ilgili kısa, öz defter notlarını barındırır. Yeni notlar eklendikçe dosyanın sonuna ilave (append) edilir.

---

## 27 Mayıs 2026 - 1.1 VLAN Mantığı, Amaçları ve Static/Dynamic VLAN Farkları

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`, s. 1 ve `pdf/blm316-hafta11.pdf`, s. 4-5
**Kısa Tanım:** VLAN (Sanal Yerel Ağ), fiziksel bir switch üzerindeki portların mantıksal olarak gruplandırılarak bağımsız yayın etki alanları (broadcast domain) oluşturulması yöntemidir.
**Anahtar Kavramlar:**
- **Mantıksal Gruplama:** Bilgisayarların fiziksel yerlerine (lokasyonuna) bakılmaksızın işlevlerine, departmanlarına (Örn: Muhasebe, satis) veya uygulamalara göre gruplandırılmasıdır.
- **Yayın Etki Alanı (Broadcast Domain) Sınırlandırma:** Her VLAN ayrı bir broadcast domain oluşturur. Böylece gereksiz yayın trafiği (broadcast) sınırlandırılarak ağ performansı artırılır.
- **Güvenlik İzolasyonu:** Farklı VLAN'lardaki cihazlar birbirlerinden tamamen izole edilir; bu da yetkisiz erişimleri engelleyerek ağ güvenliğini artırır.
- **Statik VLAN:** Switch portlarının ağ yöneticisi tarafından manuel olarak belirli VLAN'lara atanmasıdır. Dinamik VLAN'a göre daha güvenlidir, yönetimi ve bakımı daha kolaydır.
- **Dinamik VLAN:** Cihazların MAC adreslerinin tutulduğu merkezi bir veritabanı üzerinden otomatik olarak VLAN'lara atanmasıdır.
- **VLAN 1 (Varsayılan VLAN):** Switch üzerindeki tüm portların varsayılan olarak üye olduğu VLAN'dır. Silinemez, değiştirilemez veya yeniden adlandırılamaz.
**Örnek / Komut:** -
**Hatırla:** Farklı VLAN'lardaki cihazlar fiziksel olarak aynı switch'e bağlı olsalar bile **Layer 3 yönlendirme desteği (Router veya L3 Switch)** olmadan birbirleriyle doğrudan haberleşemezler.

---

## 27 Mayıs 2026 - 1.2 Cisco 1900 ve 2950 VLAN Oluşturma Komutları

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`, s. 2
**Kısa Tanım:** Switch üzerinde VLAN ID ve isim tanımlayarak sanal ağ oluşturmaktır.
**Anahtar Kavramlar:**
- **Cisco 1900 (Flat):** Konfigürasyon alt moduna geçmeden tek satırda tanımlanır.
- **Cisco 2950 (Hiyerarşik):** `vlan [id]` komutuyla alt moda (`config-vlan#`) geçilerek isim verilir.
- **Doğrulama:** `show vlan brief` (VLAN durumlarını ve port üyeliklerini listeler).
**Örnek / Komut:**
- **Cisco 1900:** `Switch(config)# vlan 2 name satis`
- **Cisco 2950:**
  ```text
  Switch(config)# vlan 2
  Switch(config-vlan)# name satis
  ```
**Hatırla:** VLAN 1 varsayılan VLAN'dır; silinemez, adı değiştirilemez.

---

## 27 Mayıs 2026 - 1.3 Cisco 1900 ve 2950 Portları VLAN'lara Atama

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`, s. 2-3
**Kısa Tanım:** Switch portlarını static olarak belirli bir VLAN'ın üyesi yapmaktır (access port).
**Anahtar Kavramlar:**
- **Access Port:** Sadece tek bir VLAN'a ait olabilen ve bilgisayar gibi uç cihazların bağlandığı port çeşididir.
- **Cisco 1900 Port Atama:** Arayüz modunda `vlan-membership static [vlan_id]` komutuyla yapılır.
- **Cisco 2950 Port Atama:** Arayüz modunda port `switchport mode access` yapılarak `switchport access vlan [vlan_id]` komutuyla atanır.
**Örnek / Komut (Port 2'yi VLAN 2'ye atama):**
- **Cisco 1900:**
  ```text
  Switch(config)# interface Ethernet 0/2
  Switch(config-if)# vlan-membership static 2
  ```
- **Cisco 2950:**
  ```text
  Switch(config)# interface FastEthernet 0/2
  Switch(config-if)# switchport mode access
  Switch(config-if)# switchport access vlan 2
  ```
**Hatırla:** Bir portu VLAN'a atadıktan sonra o porttaki cihazın o VLAN subnetinden bir IP adresi alması gerekir.

---

## 27 Mayıs 2026 - 1.4 Trunking Mantığı, ISL vs 802.1Q ve Switchlerde Trunk Yapılandırma

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`, s. 4
**Kısa Tanım:** Birden fazla VLAN bilgisini tek bir fiziksel hat üzerinden taşımak için portun trunk olarak yapılandırılmasıdır.
**Anahtar Kavramlar:**
- **Trunk Port:** VLAN'ların tümünü veya bir kısmını taşımak üzere switchler arası veya switch-router arası kurulan bağlantıdır.
- **ISL (Cisco proprietary):** Pakete 26 byte başlık ve 4 byte FCS ekleyen dış etiketleme (external tagging) yöntemidir. Orijinal paketi bozmaz.
- **IEEE 802.1Q (dot1q):** Çerçevenin içine 4 byte etiket (tag) yerleştiren açık standart iç etiketleme (internal tagging) yöntemidir.
- **Cisco 1900 Trunk Açma:** Arayüz modunda `trunk on` komutuyla yapılır (Sadece ISL destekler).
- **Cisco 2950 Trunk Açma:** Arayüz modunda `switchport mode trunk` komutuyla yapılır (Sadece 802.1q destekler).
**Örnek / Komut (Port 20'yi trunk yapma):**
- **Cisco 1900:**
  ```text
  Switch(config)# interface FastEthernet 0/20
  Switch(config-if)# trunk on
  ```
- **Cisco 2950:**
  ```text
  Switch(config)# interface FastEthernet 0/20
  Switch(config-if)# switchport mode trunk
  ```
**Hatırla:** Cisco 1900 sadece ISL, 2950 sadece dot1q desteklediği için **bu iki switch arasında trunk bağlantısı kurulamaz**.

---

## 27 Mayıs 2026 - 1.5 Inter-VLAN Routing (Router-on-a-Stick) Yapılandırması

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`, s. 5-6
**Kısa Tanım:** Farklı VLAN'ların Router üzerinde sanal alt arayüzler (subinterface) oluşturularak haberleşmesini sağlamaktır.
**Anahtar Kavramlar:**
- **Router-on-a-Stick:** Tek bir fiziksel router portu üzerinden switch ile trunk kurularak tüm VLAN'ları yönlendirme yöntemidir.
- **Subinterface (Alt Arayüz):** Fiziksel bir arayüzü sanal parçalara (örn: `fa0/0.2`) bölerek her VLAN'a bir default gateway atamaktır.
- **CLI Kuralı:** Ana fiziksel interface'e IP verilmez, sadece `no shutdown` yapılır. Alt arayüzde ise `encapsulation dot1Q [VLAN_ID]` (2950 için) veya `encapsulation isl [VLAN_ID]` (1900 için) girilmeden IP adresi atanamaz.
**Örnek / Komut (Switch 2950 ile bağlıyken, VLAN 2 için):**
```text
Router(config)# interface FastEthernet 0/0
Router(config-if)# no ip address
Router(config-if)# no shutdown
Router(config-if)# interface FastEthernet 0/0.2            ; VLAN 2 alt arayüzü
Router(config-subif)# encapsulation dot1Q 2                 ; 802.1Q standardı ve VLAN 2
Router(config-subif)# ip address 192.168.1.65 255.255.255.192
```
**Hatırla:** Sınavda alt arayüzün altına encapsulation komutunu yazmayı unutursan IP atama komutu hata verir.

---

## 27 Mayıs 2026 - 1.6 Inter-VLAN Laboratuvar running-config Analizi

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`, s. 7-10
**Kısa Tanım:** Lab topolojisindeki switch ve router konfigürasyon dosyalarını (show running-config) okuyarak yapının doğrulanmasıdır.
**Anahtar Kavramlar:**
- **Switch Trunking:** SwitchA ve SwitchB `Fa0/23` arayüzü üzerinden birbirine trunk (dot1q) olarak bağlıdır.
- **Router Bağlantısı:** SwitchB `Fa0/24` arayüzü üzerinden Router `Fa0/0` fiziksel portuna trunk olarak bağlanmıştır.
- **Subinterface IP & Maske Dağılımı:**
  - `Fa0/0.2` (Vlan2): `192.168.1.65 255.255.255.192` (/26 subnet, ilk kullanılabilir IP gateway yapılmıştır).
  - `Fa0/0.3` (Vlan3): `192.168.1.129 255.255.255.192`
  - `Fa0/0.4` (Vlan4): `192.168.1.193 255.255.255.192`
**Örnek / Komut:**
- `Switch# show running-config` : Cihazın aktif çalışan tüm yapılandırmasını (port modu, VLAN, IP vb.) gösterir.
- `Switch# show vlan` : Switch üzerinde oluşturulmuş VLAN'ları ve aktif port eşleşmelerini listeler.
**Hatırla:** Yönlendirme yapılan fiziksel porta (örn: Router `Fa0/0`) kesinlikle IP adresi atanmaz, sadece subinterface'lere IP atanır.

---

## 27 Mayıs 2026 - 2.1 DHCP DORA İletişim Süreci, RARP ve BOOTP Farkları

**Kaynak:** `ders.md`, Bölüm 7; `pdf/blm316-hafta13.pdf`, s. 13-14
**Kısa Tanım:** Ağa bağlanan cihazlara IP adresi, alt ağ maskesi, default gateway ve DNS sunucusu gibi ayarları otomatik atayan DHCP'nin DORA süreci ve önceki protokollerden farklarıdır.
**Anahtar Kavramlar:**
- **DHCP DORA Süreci:**
  - **Discover (Keşif):** İstemci sunucuyu bulmak için broadcast (UDP 67) gönderir. Kaynak IP `0.0.0.0`, hedef IP `255.255.255.255`'tir.
  - **Offer (Teklif):** DHCP sunucusu istemciye uygun bir IP adresi teklif eder (UDP 68).
  - **Request (İstek):** İstemci teklifi kabul ettiğini bildiren broadcast istek paketi yollar.
  - **Acknowledge (Onay):** Sunucu IP'yi istemciye kiralayarak onaylar (ACK).
- **RARP:** Yalnızca MAC adresinden IP eşleştirmesi yapar. Ağ geçidi veya DNS veremez. Sadece yerel ağda çalışır (router geçemez).
- **BOOTP:** IP, DNS ve ağ geçidi bilgilerini verebilir fakat dinamik IP havuzu ve kiralama süresi (lease) yoktur; her cihaz için elle MAC-IP eşleştirmesi gerekir.
**Örnek / Komut:** -
**Hatırla:** Discover ve Request paketleri broadcast olarak gönderilir; çünkü istemcinin henüz geçerli bir IP'si yoktur ve hangi sunucunun teklifini kabul edeceğini tüm ağa bildirmelidir.

---

## 27 Mayıs 2026 - 2.2 DHCP Sunucu ve İstemciler Aynı Ağda vs Farklı Ağda (DHCP Relay / IP Helper-Address)

**Kaynak:** `ders.md`, Bölüm 7; `pdf/h10-uygulama11-DHCP, NAT.pdf`, s. 2-3
**Kısa Tanım:** DHCP istemcisi ile sunucusunun farklı IP alt ağlarında (subnet) bulunması durumunda, istemcinin gönderdiği broadcast DHCP Discover paketlerinin router tarafından DHCP sunucusuna yönlendirilmesini sağlayan mekanizmadır.
**Anahtar Kavramlar:**
- **Aynı Ağda Olma:** İstemci ve sunucu aynı broadcast domain'de ise ek bir ayara gerek yoktur; istemcinin broadcast Discover paketleri doğrudan sunucuya ulaşır.
- **Farklı Ağda Olma:** Router'lar varsayılan olarak broadcast paketlerini geçirmez (engeller). Bu yüzden farklı alt ağdaki bir istemcinin DHCP talebi sunucuya ulaşamaz.
- **DHCP Relay Agent (IP Helper-Address):** İstemcinin bağlı olduğu router arayüzüne (Gateway portu) `ip helper-address [DHCP_Sunucu_IP]` komutu girilerek, gelen broadcast DHCP paketleri unicast'e çevrilip doğrudan sunucuya yönlendirilir.
- **İşleyiş:** İstemci -> (Broadcast Discover) -> Router Arayüzü -> (Unicast Discover) -> DHCP Sunucusu.
**Örnek / Komut (İstemcilerin bağlı olduğu FastEthernet 0/0 arayüzüne DHCP sunucu IP'sini tanımlama):**
```text
Router(config)# interface FastEthernet 0/0
Router(config-if)# ip helper-address 192.168.2.10
```
**Hatırla:** Sınavda DHCP Relay (ip helper-address) komutunun DHCP sunucusunun arayüzüne değil, **istemcilerin (IP talep edenlerin) bağlı olduğu varsayılan ağ geçidi (gateway) arayüzünün altına** yazılması gerektiğini unutma!

---

## 27 Mayıs 2026 - 2.3 Cisco Router DHCP Sunucu Yapılandırması (Pool, Excluded-Address, Lease)

**Kaynak:** `ders.md`, Bölüm 7; `pdf/h6-uygulama10-DHCP kurulumu.pdf`, s. 2-3
**Kısa Tanım:** Cisco router'ın ağdaki istemcilere dinamik IP dağıtması için DHCP sunucusu olarak yapılandırılmasıdır.
**Anahtar Kavramlar:**
- **service dhcp:** Router üzerinde DHCP servisini aktif hale getirir (varsayılan olarak aktiftir).
- **ip dhcp excluded-address:** Sunucunun dağıtmasını *istemediğimiz* statik IP aralıklarını (örn: router, switch veya yazıcı IP'leri) havuz dışı bırakır.
- **ip dhcp pool [isim]:** IP dağıtılacak ağ için bir havuz oluşturur ve havuz yapılandırma moduna geçer.
- **network [network_IP] [mask]:** Havuzun dağıtacağı IP aralığını belirler.
- **default-router:** İstemcilere gönderilecek varsayılan ağ geçidi (Gateway) IP'sini belirler.
- **dns-server:** İstemcilere gönderilecek DNS sunucu IP'lerini belirler.
- **lease [gün] [saat] [dakika]:** Dağıtılan IP'lerin kiralama süresini belirler (Örn: `lease 1` = 1 gün).
**Örnek / Komut (Cisco Router üzerinde DHCP sunucusu kurma):**
```text
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10
Router(config)# ip dhcp pool LAN_POOL
Router(config-dhcp)# network 192.168.1.0 255.255.255.0
Router(config-dhcp)# default-router 192.168.1.1
Router(config-dhcp)# dns-server 8.8.8.8
Router(config-dhcp)# lease 2 12 0  ; 2 gün 12 saat kiralama
```
**Hatırla:** Sınavda `ip dhcp excluded-address` komutunun global konfigürasyon modunda yazıldığına, havuz oluşturma ve ağ bilgilerinin ise `ip dhcp pool` altındaki havuz modunda girildiğine dikkat et!

---

## 27 Mayıs 2026 - 2.4 Cisco Router üzerinde Statik DHCP (Hardware-Address IP Rezervasyonu) Yapılandırması

**Kaynak:** `ders.md`, Bölüm 7; `pdf/blm316-hafta13.pdf`, s. 17-18
**Kısa Tanım:** Belirli bir MAC adresine sahip bir istemciye (örn: ağ yazıcısı veya sunucu), DHCP sunucusu tarafından her seferinde aynı IP adresinin tahsis edilmesini sağlamaktır.
**Anahtar Kavramlar:**
- **Statik DHCP:** İstemcide statik IP tanımlamak yerine, DHCP sunucusunda istemcinin MAC adresini (donanım adresini) belirli bir IP'ye rezerve etme işlemidir.
- **host [IP] [maske]:** Rezerve edilecek sabit IP adresini ve maskeyi belirtir.
- **client-identifier veya hardware-address:** İstemcinin kimliğini belirtmek için kullanılır. Ethernet ağlarında `client-identifier` kullanılırken MAC adresinin başına `01` (Ethernet medya tipi kodu) eklenir. `hardware-address` ise doğrudan MAC adresini tanımlamak için kullanılabilir.
- **Tekil Havuz (Unique Pool):** Her bir statik rezervasyon için ayrı, tekil bir DHCP havuzu oluşturulması gerekir.
**Örnek / Komut (MAC adresi `00-11-2f-b2-12-b2` olan cihaz için `192.168.1.100` IP'sini rezerve etme):**
```text
Router(config)# ip dhcp pool Academy-Printer
Router(config-dhcp)# host 192.168.1.100 255.255.255.0
Router(config-dhcp)# client-identifier 0100.112f.b212.b2  ; Baştaki 01: Ethernet kodu, devamı MAC.
```
**Hatırla:** Cisco Router'larda statik rezervasyon yaparken `client-identifier` kullanılıyorsa, MAC adresinin başına mutlaka Ethernet ortamını simgeleyen `01` kodunun getirilmesi gerektiğini sınavda unutma!

---

## 27 Mayıs 2026 - 2.5 Kavrama Görevi: DHCP Relay ve Statik IP Rezervasyonu CLI Analizi

**Kaynak:** `ders.md`, Bölüm 7
**Kısa Tanım:** DHCP Relay (IP Helper-Address) ve Statik IP rezervasyonuna (client-identifier) ait CLI komutlarının ve çalışma doğrulamalarının analiz edilmesidir.
**Anahtar Kavramlar:**
- **ip helper-address Doğrulaması:** `show ip route` ve `show interface [tür_id]` komutları ile yönlendirici bacağındaki IP ve yönlendirme durumu kontrol edilmelidir.
- **show ip dhcp binding:** DHCP sunucunun hangi MAC adresine hangi IP'yi kiraladığını ve kiralama süresini gösteren en kritik doğrulama komutudur. Statik rezervasyonlar da burada listelenir.
- **show ip dhcp server statistics:** DHCP sunucu paket istatistiklerini (alınan Discover, gönderilen Offer vb.) gösterir.
- **show ip dhcp conflict:** Ağda meydana gelen IP adresi çakışmalarını listeler.
**Örnek / Komut:**
- `Router# show ip dhcp binding` (IP kiralama tablosunu listeleme)
- `Router# show ip dhcp conflict` (IP çakışmalarını listeleme)
**Hatırla:** Statik olarak rezerve edilen IP'ler, `ip dhcp excluded-address` aralığında yer alsa bile `host` ve `client-identifier` tanımları sayesinde ilgili cihaza başarıyla atanacaktır. Ancak en temiz yapılandırma için havuz dışı bırakılan IP'leri rezerve etmek daha doğrudur.

---

## 28 Mayıs 2026 - 3.1 NAT Mantığı, RFC 1918 Özel IP Adresleri ve NAT Türleri (Statik, Dinamik, PAT)

**Kaynak:** `ders.md`, Bölüm 9; `pdf/blm316-hafta14.pdf`, s. 2-3
**Kısa Tanım:** Yerel ağlardaki özel (private) IP adreslerinin, internete çıkarken genel (public) IP adreslerine dönüştürülmesi teknolojisidir.
**Anahtar Kavramlar:**
- **RFC 1918 Özel IP Adresleri:** İnternette yönlendirilemeyen, sadece yerel ağlarda kullanılan IP bloklarıdır:
  - Sınıf A: `10.0.0.0 - 10.255.255.255`
  - Sınıf B: `172.16.0.0 - 172.31.255.255`
  - Sınıf C: `192.168.0.0 - 192.168.255.255`
- **Statik NAT:** Bir özel IP adresi ile bir genel IP adresi arasında bire bir (1:1) sabit eşleme yapar. Genellikle dışarıdan erişilmesi gereken yerel sunucular (Web, Mail vb.) için kullanılır.
- **Dinamik NAT:** Özel IP adreslerini, önceden tanımlanmış genel IP havuzundaki boşta olan IP'lerle dinamik olarak bire bir eşler. Havuzdaki IP'ler bitince yeni cihazlar internete çıkamaz.
- **PAT (NAT Overload - Port Address Translation):** Tek bir genel IP adresinin, kaynak port numaraları değiştirilerek birden fazla yerel cihaz tarafından ortaklaşa kullanılmasını sağlar. Ev tipi modemlerin tamamı PAT kullanır.
**Örnek / Komut:** -
**Hatırla:** Statik ve Dinamik NAT bire bir (1:1) IP eşlemesi yaparken, PAT çoktan bire (N:1) IP eşlemesi yaparak genel IP adresi tasarrufu sağlar.

---

## 28 Mayıs 2026 - 3.2 NAT Terminolojisi (Inside Local, Inside Global, Outside Local, Outside Global)

**Kaynak:** `ders.md`, Bölüm 9; `pdf/blm316-hafta14.pdf`, s. 4
**Kısa Tanım:** NAT işlemi gerçekleştiren router'ın iç ve dış ağlardaki cihazların adreslerini konumlarına (yerel/genel) göre sınıflandırmak için kullandığı 4 temel terimdir.
**Anahtar Kavramlar:**
- **Inside Local (İç Yerel) Adres:** İç ağdaki (yerel ağ) bir cihaza atanan gerçek IP adresidir. (Genellikle RFC 1918 özel IP'sidir). Örn: `192.168.1.10`.
- **Inside Global (İç Genel) Adres:** İç ağdaki cihazın dış dünya (internet) tarafından görünen, dönüştürülmüş genel IP adresidir. Örn: `203.0.113.5`.
- **Outside Local (Dış Yerel) Adres:** Dış ağdaki bir cihazın (örn: web sunucusu) yerel ağ tarafından görünen adresidir. Genellikle dış cihazın gerçek IP'siyle aynıdır.
- **Outside Global (Dış Genel) Adres:** Dış ağdaki cihazın dış dünyadaki gerçek genel IP adresidir. Örn: Google sunucusunun `8.8.8.8` adresi.
**Örnek / Komut:** -
**Hatırla:** Sınavda Inside (İç) yerel ağdaki istemciyi, Outside (Dış) ise internetteki hedefi; Local (Yerel) dönüşüm öncesi adresi, Global (Genel) ise dönüşüm sonrası internette görünen adresi temsil eder.

---

## 28 Mayıs 2026 - 3.3 Cisco NAT Yapılandırması (Inside/Outside Arayüzler, Havuz Tanımlama, PAT overload)

**Kaynak:** `ders.md`, Bölüm 9; `pdf/blm316-hafta14.pdf`, s. 7-8
**Kısa Tanım:** Cisco router üzerinde cihazların internete çıkarken hangi arayüzlerden gireceğini, hangi arayüzlerden çıkacağını ve hangi public IP'leri veya portları kullanacağını (NAT Overload/PAT) belirleme adımlarıdır.
**Anahtar Kavramlar:**
- **ip nat inside / outside:** Yönlendiricinin hangi arayüzünün yerel ağa (inside), hangi arayüzünün internete/dış ağa (outside) baktığını belirtir.
- **ip nat pool:** Dinamik NAT için kullanılacak Public IP adres havuzunu belirler.
- **access-list (ACL):** Hangi yerel IP adreslerinin (Inside Local) NAT işleminden geçebileceğini (izin verilenleri) belirler.
- **overload (PAT):** Tek bir Public IP (veya havuzdaki kısıtlı IP'ler) üzerinden çok sayıda istemcinin portları değiştirilerek çıkış yapmasını sağlar.
**Örnek / Komut:**
```text
Router(config)# interface G0/0/0
Router(config-if)# ip nat inside  (İç arayüz)
Router(config)# interface S0/1/0
Router(config-if)# ip nat outside (Dış arayüz)

(PAT - Tek Dış IP Kullanarak)
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 10 interface S0/1/0 overload
```
**Hatırla:** NAT konfigürasyonunda `overload` kelimesi unutulursa, PAT çalışmaz ve sadece birebir IP tahsisi yapılır (Dinamik NAT). Ağdaki kişi sayısı Public IP sayısından fazlaysa internete çıkış problemleri yaşanır.

---

## 28 Mayıs 2026 - 3.4 NAT Doğrulama, Sorun Giderme ve Hata Ayıklama Komutları

**Kaynak:** `ders.md`, Bölüm 9; `pdf/blm316-hafta14.pdf`, s. 9
**Kısa Tanım:** Yapılandırılan NAT işlemlerinin (hangi iç IP'nin hangi dış IP'ye çevrildiğinin) canlı olarak izlenmesi ve varsa hataların tespit edilmesi için kullanılan Cisco CLI komutlarıdır.
**Anahtar Kavramlar:**
- **NAT Translations (Çeviriler):** O an router üzerinde gerçekleştirilmiş tüm aktif adres çevirilerinin tutulduğu tablodur.
- **NAT Statistics (İstatistikler):** Kaç tane başarılı/başarısız çeviri yapıldığı, havuzdaki IP'lerin yüzde kaçının kullanıldığı gibi genel bilgileri gösterir.
- **NAT Debug (Hata Ayıklama):** Paketlerin NAT'tan geçerken geçirdiği değişimi anlık (real-time) olarak console ekranında loglar (sorun çözmede çok etkilidir ancak CPU'yu yorar).
**Örnek / Komut:**
- `Router# show ip nat translations` : Tüm aktif statik ve dinamik NAT/PAT çevirilerini tablo halinde listeler (Inside Local, Inside Global vb. görünür).
- `Router# show ip nat statistics` : Toplam çeviri sayısı, oluşturulan havuzların durumu ve Inside/Outside atanmış arayüzleri özetler.
- `Router# clear ip nat translation *` : Aktif tüm dinamik NAT çevirilerini (bağlantılarını) zorla sonlandırarak tabloyu temizler (Statik NAT'ı silmez).
- `Router# debug ip nat` : O an yapılan her yeni NAT işlemini anlık olarak ekrana yazdırır.
**Hatırla:** Ağda NAT çalışmıyorsa ilk yapman gereken `show ip nat statistics` yazıp inside/outside interface'lerin doğru portlara (bacaklara) atanıp atanmadığını kontrol etmektir. Yanlış bacağa atama en sık yapılan hatadır.

---

## 28 Mayıs 2026 - 4.1 Yönlendirme (Routing) Mantığı, Yönlendirici (Router) Görevleri (Core vs Edge)

**Kaynak:** `ders.md`, Bölüm 11; `pdf/blm316-hafta14.pdf`, s. 10-12
**Kısa Tanım:** Paketlerin kaynak adresten hedef adrese hangi ağlardan geçerek ulaşacağına (en iyi yolun seçilmesine) karar verme sürecidir.
**Anahtar Kavramlar:**
- **Yönlendirici (Router):** L3 (Ağ) katmanında çalışan, yönlendirme tablosuna (Routing Table) bakarak paketleri ileten ağ cihazıdır. LAN'ları birbirine veya WAN'a bağlar.
- **Yönlendirme Tablosu (Routing Table):** Cihazın bildiği ağ adreslerini ve o ağlara ulaşmak için paketi hangi porttan (arayüzden) çıkarması gerektiğini gösteren haritadır.
- **Core Router (Merkez/Çekirdek Yönlendirici):** Şaseli olarak üretilen, omurga ağda trafiği devasa hızlarda ileten güçlü işlemcili cihazlardır (Az özellik, yüksek bant genişliği).
- **Edge Router (Kenar Yönlendirici):** Bir veya birkaç yerel ağı (LAN) dış dünyaya (WAN/İnternet) bağlayan sınır cihazlarıdır.
**Örnek / Komut:**
- `Router# show ip route` : Cihazın yönlendirme tablosunu ve bildiği rotaları görüntüler.
**Hatırla:** Anahtarlama (Switching - L2) bir bina/LAN içinde, Yönlendirme (Routing - L3) ise binalar/farklı ağlar (LAN'lar arası) iletişimde tercih edilir.

---

## 28 Mayıs 2026 - 4.2 Yol Belirleme (Path Determination) ve En Uzun Eşleşme (Longest Prefix Match) İlkesi

**Kaynak:** `ders.md`, Bölüm 11; `pdf/blm316-hafta14.pdf`, s. 13-14
**Kısa Tanım:** Yönlendiricinin (Router), kendisine gelen bir paketi hangi yoldan göndereceğine karar verirken kullandığı en temel kuraldır.
**Anahtar Kavramlar:**
- **Yol Belirleme:** Router'ın, paketin hedef IP adresini alıp yönlendirme tablosundaki ağ adresleriyle karşılaştırarak bir çıkış arayüzü bulmasıdır.
- **Longest Prefix Match (En Uzun Eşleşme):** Eğer yönlendirme tablosunda hedefe giden birden fazla eşleşen yol (network adresi) varsa, Router her zaman **Subnet Maskesi (Prefix uzunluğu) en büyük olanı (en spesifik olanı)** seçer.
**Örnek / Komut:**
Hedef IP: `192.168.1.15` olsun. Tabloda 3 rota var:
1. `192.168.0.0 /16` (İlk 16 bit eşleşiyor)
2. `192.168.1.0 /24` (İlk 24 bit eşleşiyor)
3. `192.168.1.0 /26` (İlk 26 bit eşleşmiyor, çünkü .15 hostu /26 subnet'inin içinde ama diyelim ki /28 olsaydı veya maskesi uymasaydı elenirdi. Varsayalım tabloda uyan `/26` var.)
*Sonuç:* Eşleşen yollar içinde en büyük prefix değeri `/26` olduğu için router paketi bu yola iletir.
**Hatırla:** En uzun eşleşme, hedefe "en nokta atışı" (en dar kapsamlı) giden yoldur. Alt ağ maskesi sayısı (`/`'dan sonraki değer) ne kadar büyükse eşleşme o kadar güçlüdür.

---

## 28 Mayıs 2026 - 4.3 Statik Yönlendirme Türleri (Recursive, Directly Connected, Fully Specified) ve AD Değeri

**Kaynak:** `ders.md`, Bölüm 12 ve 17.2; `pdf/h7-uygulama12-statik routing.pdf`
**Kısa Tanım:** Ağ yöneticisinin yönlendirme tablosuna manuel olarak girdiği yollardır. Komut formatına göre farklı isimler alırlar.
**Anahtar Kavramlar:**
- **Administrative Distance (AD):** Yönlendirme rotalarının "güvenilirlik" derecesidir (Küçük olan daha güvenilirdir). Statik rotaların AD değeri varsayılan olarak **1**'dir. (Doğrudan bağlı rotaların AD'si 0'dır).
- **1. Rekürsif (Next-Hop) Statik Rota:** Çıkış arayüzü yazılmaz, sadece hedef ağ ve *bir sonraki yönlendiricinin (Next-Hop) IP adresi* yazılır. Router paketi göndereceği portu bulmak için tabloda ikinci bir arama yapmak (recursive lookup) zorundadır.
- **2. Directly Connected (Doğrudan Bağlı) Statik Rota:** Next-Hop IP adresi yerine *kendi çıkış arayüzü* (örn: `Serial0/0/0`) yazılır. Çift arama gerektirmez, daha hızlıdır. Point-to-point (seri) bağlantılarda kullanılır.
- **3. Fully Specified (Tam Belirtilmiş) Statik Rota:** Hem çıkış arayüzünün hem de Next-Hop IP adresinin aynı anda komuta yazıldığı türdür. Çoklu erişimli (Ethernet) bağlantılarda kullanılır.
**Örnek / Komut:**
- Rekürsif: `Router(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2`
- Directly Connected: `Router(config)# ip route 192.168.2.0 255.255.255.0 Serial0/0/0`
**Hatırla:** Sınavda sorulursa: `ip route` komutunda hedef IP'den sonra bir IP (Next-Hop) yazıyorsa *Rekürsif*; sadece interface adı (örn: s0/0/0) yazıyorsa *Directly Connected*'dır.

---

## 28 Mayıs 2026 - 4.4 Cisco Router Statik ve Default Route CLI Yapılandırma Komutları

**Kaynak:** `ders.md`, Bölüm 12 ve 17.2; `pdf/h7-uygulama12-statik routing.pdf`
**Kısa Tanım:** Router'a bilinmeyen ağlara veya internete (tüm bilinmeyen hedeflere) gidebilmesi için manuel olarak yol tarif etme işlemidir.
**Anahtar Kavramlar:**
- **Statik Rota Formülü:** `ip route [Hedef Ağ IP'si] [Hedef Subnet Maskesi] [Next-Hop IP veya Çıkış Arayüzü]`
- **Default Route (Varsayılan Rota):** Hedef IP adresi ne olursa olsun (yönlendirme tablosunda özel bir eşleşme bulunamazsa) paketlerin gönderileceği son çıkış kapısıdır. "Tüm ağlar" anlamına gelen `0.0.0.0 0.0.0.0` kullanılarak yazılır.
- **Gateway of Last Resort:** Varsayılan rotanın yönlendirme tablosundaki (`show ip route`) adıdır. Tabloda `S*` harfi ile gösterilir (`S` Statik, `*` Default/Aday anlamına gelir).
**Örnek / Komut:**
- *Statik Rota Ekleme:* `Router(config)# ip route 10.1.1.0 255.255.255.0 192.168.1.2` (10.1.1.0 ağına gitmek için paketi 192.168.1.2'ye yolla).
- *Default Route Ekleme:* `Router(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/1` (Bilinmeyen tüm hedeflere giden paketleri S0/0/1'den dışarı at).
**Hatırla:** Default route genellikle kenar yönlendiricilerde (Edge Router) kullanılır ve her zaman internet servis sağlayıcısına (ISP) doğru bakar.

---

## 28 Mayıs 2026 - 5.1 Otonom Sistemler (OS) ve Intra-OS vs Inter-OS Yönlendirme

**Kaynak:** `ders.md`, Bölüm 13.1; `pdf/blm316-hafta15.pdf`, s. 2-3
**Kısa Tanım:** Dinamik yönlendirme protokollerinin çalıştığı ağların (otonom sistemlerin) yapısal olarak ikiye ayrılmasıdır.
**Anahtar Kavramlar:**
- **Otonom Sistem (Autonomous System - AS):** Tek bir yönetim veya kuruluş (örn: bir üniversite, şirket veya servis sağlayıcı) tarafından kontrol edilen, ortak yönlendirme politikalarına sahip yönlendiriciler (router) ve ağlar bütünüdür.
- **Intra-OS (AS İçi - IGP) Yönlendirme:** Aynı otonom sistemin **içerisindeki** yönlendiricilerin kendi aralarında haberleştiği protokollerdir. İç kapı yönlendirme protokolleri (Interior Gateway Protocols - **IGP**) olarak bilinir. (Örn: **RIP, OSPF, EIGRP**).
- **Inter-OS (AS'ler Arası - EGP) Yönlendirme:** Farklı otonom sistemleri birbirine bağlayan, internetin omurgasını oluşturan protokollerdir. Dış kapı yönlendirme protokolleri (Exterior Gateway Protocols - **EGP**) olarak bilinir. (Örn: **BGP**).
**Örnek:**
- Kendi ofisinizdeki veya kampüsünüzdeki yönlendiriciler kendi aralarında en iyi yolu bulmak için OSPF (Intra-OS/IGP) kullanır. Ancak üniversiteniz Türk Telekom (başka bir Otonom Sistem) ile haberleşirken BGP (Inter-OS/EGP) kullanmak zorundadır.
**Hatırla:** Sınavda Intra (iç) dendiğinde aklına RIP, OSPF, EIGRP gelmeli. Inter (dış/arası) dendiğinde aklına internetin patronu olan BGP gelmeli.

---

## 28 Mayıs 2026 - 5.2 RIPv1 Özellikleri, Bellman-Ford Veritabanı ve Hat Başarısızlık Düzeltimi

**Kaynak:** `ders.md`, Bölüm 13.2 ve 13.3; `pdf/blm316-hafta15.pdf`, s. 5-7
**Kısa Tanım:** Uzaklık vektörü (Distance Vector) mantığıyla çalışan, en eski ve en temel dinamik yönlendirme protokollerinden biridir.
**Anahtar Kavramlar:**
- **RIP (Routing Information Protocol):** Yol seçimi yaparken **Atlama Sayısını (Hop Count)** kullanır. Bir ağa giderken kaç tane router üzerinden geçeceğini sayar. En az router'dan (en az atlama) geçen yol en iyisidir. Ağın hızına (bant genişliğine) bakmaz.
- **Bellman-Ford Algoritması:** Uzaklık (mesafe) vektörü mantığıdır. Router, tüm yönlendirme tablosunu her **30 saniyede bir** komşularına (broadcast olarak - 255.255.255.255) kopyalar.
- **Max Hop Count (Maksimum Atlama):** RIP'in sınırıdır. Bir paket en fazla 15 yönlendiriciden (hop) geçebilir. Eğer atlama sayısı **16** ise, hedef "Ulaşılamaz (Unreachable)" kabul edilir.
- **Sonsuz Döngüleri Engelleme Yöntemleri:**
  - *Split Horizon:* Router, bir komşusundan öğrendiği ağ bilgisini tekrar aynı komşusuna geri göndermez. (Dedikoduyu öğrendiği kişiye anlatmaz).
  - *Route Poisoning (Zehirleme):* Eğer bir ağ çökerse, Router o ağın atlama sayısını anında **16 (ulaşılamaz)** yapar ve komşularına "Bu yol zehirlendi, buraya gelmeyin" mesajı yollar.
  - *Holddown Timers:* Çöken bir ağ hakkında haber geldiğinde, ağ tam olarak oturana (kararlı hale gelene) kadar o ağla ilgili gelecek "şuradan ulaşabilirsin" yalan güncellemelerini geçici süre reddetme/bekletme süresidir.
**Örnek:**
RouterA, 192.168.2.0 ağına gitmek için biri 10 Mbps hızında ama sadece 1 atlama, diğeri 1000 Mbps hızında ama 2 atlama gerektiren iki yola sahip olsun. RIP, hıza bakmadığı için körü körüne 1 atlamalı (daha yavaş olan) yolu seçecektir.
**Hatırla:** RIP eşittir "Hop Count" (Atlama Sayısı). En fazla 15 atlama desteklediği için sadece küçük ve orta ölçekli ağlarda kullanılır.

---

## 28 Mayıs 2026 - 5.3 RIP Yönlendirme Tablosu Güncelleme Algoritması ve Örnekleri

**Kaynak:** `ders.md`, Bölüm 13.4; `pdf/blm316-hafta15.pdf`, s. 8-15
**Kısa Tanım:** Bir yönlendiricinin (Router), komşusundan gelen yeni yönlendirme tablosunu kendi tablosuyla kıyaslayarak güncellemeleri nasıl işlediği kural dizisidir.
**Anahtar Kavramlar ve Kurallar:**
1. **Yeni Bir Ağ Geldiğinde:** Komşudan gelen tabloda senin hiç bilmediğin yepyeni bir ağ (Network) varsa, bunu direkt kendi tablona eklersin. Gelen Hop (Atlama) sayısına **+1 eklersin** (çünkü komşun üzerinden geçeceksin).
2. **Daha Kısa Yol Bulunduğunda:** Senin tablonla komşunun tablosunda aynı ağa giden yollar varsa, komşunun Hop sayısı + 1 değeri senin mevcut Hop sayından **daha küçükse** (daha kısaysa), eski yolunu silip komşunun yolunu (daha iyi olanı) kaydedersin.
3. **Mevcut Yol Kötüleştiğinde (Aynı Komşu):** Eğer sen o ağa komşun (C) üzerinden gidiyorsan ve komşun (C) sana o yolun kötüleştiğini (Hop sayısının arttığını) söylüyorsa, yolu güncellersin! (Çünkü oraya zaten ondan gidiyorsun, hat zayıflamış veya bozulmuş olabilir, bu bilgiyi kabul etmek zorundasın).
4. **Mevcut Yol Kötüleştiğinde (Farklı Komşu):** Sen o ağa C üzerinden 3 Hop ile gidiyorsun. B komşun gelip "Ben de oraya 5 Hop ile gidiyorum" derse, bu bilgiyi reddedersin. Çünkü senin zaten daha kısa bir yolun (C üzerinden) var.
**Örnek / Sınav Sorusu Mantığı:**
*Eski Tablomuz:* Network1 -> Hop 3 -> Next C
*Gelen Güncelleme (C Komşusundan):* Network1 -> Hop 5
*Sonuç:* Aynı komşudan (C) geldiği için mecburen güncellemek zorundayız. Yeni tablomuz: Network1 -> Hop 6 (5+1) -> Next C olur.
**Hatırla:** RIP algoritmasında komşudan gelen hop değerine **her zaman 1 ekleyip** öyle kıyaslama yaparız.

---

## 28 Mayıs 2026 - 5.4 RIPv1 vs RIPv2 Karşılaştırması, Mesaj Formatları ve RIPv2 CLI Yapılandırması

**Kaynak:** `ders.md`, Bölüm 13.5 ve 13.6; `pdf/blm316-hafta15.pdf`, s. 16-24
**Kısa Tanım:** Eski (v1) ile güncel (v2) RIP versiyonları arasındaki teknik farklar ve Cisco router üzerinde etkinleştirilmesi komutlarıdır.
**Anahtar Kavramlar:**
- **RIPv1 Özellikleri:** Sınıflı (Classful) yönlendirme yapar. Yani alt ağ maskelerini (subnet mask) güncelleme paketlerinde **göndermez**. Sadece A, B, C sınıfı varsayılan maskeleri tanır. VLSM desteklemez. Broadcast (`255.255.255.255`) ile yayın yapar.
- **RIPv2 Özellikleri:** Sınıfsız (Classless) yönlendirme yapar. Yani gönderdiği mesajın içine **Subnet Mask (Alt ağ maskesi)** bilgisini de ekler (v1 ile en büyük farkı budur). VLSM destekler. Broadcast yerine hedefe yönelik Multicast (`224.0.0.9`) ile yayın yapar. Ayrıca paketlere şifre koyarak **Kimlik Doğrulamayı (Authentication - MD5)** destekler.
- **Mesaj Formatı (Paket Yapısı) Farkı:** RIPv1'in paketinde sadece "Network adresi ve Hop Sayısı" varken, RIPv2'nin paketinde bunlara ek olarak "Subnet Mask, Next-hop adresi ve Route Tag" bulunur.
**Örnek / Komut:**
```text
Router(config)# router rip           (RIP protokolünü başlatır)
Router(config-router)# version 2     (Versiyon 2 moduna geçirir)
Router(config-router)# no auto-summary (Ağları kendi kafasına göre sınıf bazlı yuvarlamasını/özetlemesini iptal eder)
Router(config-router)# network 192.168.1.0 (Senin kendi bacaklarında olan ve duyurmak istediğin ağları eklersin)
```
**Hatırla:** Gerçek hayatta her zaman `version 2` ve `no auto-summary` komutlarını birlikte girmelisin, yoksa router parçalanmış (VLSM) ağlarını birleştirip tek bir classful ağmış gibi hatalı bir anons yapar.

---

## 28 Mayıs 2026 - 5.5 Pasif Arayüz (Passive Interface) ve Varsayılan Rota Dağıtımı (Default-Information Originate)

**Kaynak:** `ders.md`, Bölüm 13.5; `pdf/blm316-hafta15.pdf`, s. 25-28
**Kısa Tanım:** Dinamik yönlendirme protokollerinin gereksiz yere yerel ağları (LAN) bant genişliği tüketerek meşgul etmesini önleyen ve internet çıkış kapısını tüm ağa duyuran özel komutlardır.
**Anahtar Kavramlar:**
- **Pasif Arayüz (Passive Interface):** Bir yönlendirme protokolünün (Örn: RIP), belirtilen arayüzden dışarıya güncelleme (yönlendirme tablosu) göndermesini engeller. Ancak o arayüzün ağ adresini diğer komşulara duyurmaya **devam eder**.
- **Neden Kullanılır?** LAN (İç ağ) tarafındaki portta (örneğin GigabitEthernet0/0) sadece bilgisayarlar veya switch'ler vardır, router yoktur. Bu yüzden RIP'in her 30 saniyede bir o porta koca bir yönlendirme tablosunu fırlatması tamamen bant genişliği israfı ve güvenlik açığıdır. Bu portlar "pasif" yapılmalıdır.
- **Varsayılan Rota Dağıtımı (Default-Information Originate):** Şirkette internete çıkan Edge Router'a yazdığın `0.0.0.0 0.0.0.0` varsayılan (statik) rotasını, RIP aracılığıyla içerideki **diğer tüm yönlendiricilere** (RIP komşularına) otomatik olarak dağıtmasını sağlar.
**Örnek / Komut:**
- *Pasif Arayüz Komutu:*
```text
Router(config)# router rip
Router(config-router)# passive-interface GigabitEthernet0/0
```
- *Varsayılan Rotayı RIP ile Dağıtma Komutu:*
```text
Router(config)# router rip
Router(config-router)# default-information originate
```
**Hatırla:** Uç kullanıcılara (PC, Printer) giden yollar her zaman **passive-interface** yapılmalıdır. RIP güncellemeleri sadece diğer Router'ların olduğu Seri / Trunk hatlardan gönderilmelidir.

---

## 29 Mayıs 2026 - 6.1 EIGRP Hibrit Yapısı, Paket Çeşitleri ve RTP Protokolü

**Kaynak:** `ders.md`, Bölüm 14; `pdf/h8-uygulama14- eigrp,ospf.pdf`
**Kısa Tanım:** Cisco tarafından geliştirilmiş, RIP gibi sadece atlamaya bakmayan, hız ve gecikmeyi de hesaplayan "Melez (Hibrit)" bir dinamik yönlendirme protokolüdür.
**Anahtar Kavramlar:**
- **Hibrit (Melez) Yapı:** EIGRP, hem "Distance Vector" (Uzaklık Vektörü - komşudan öğrenme) hem de "Link-State" (Bağ Durumu - ağın resmini çıkarma) özelliklerini bir arada barındırır. Çok zeki ve hızlıdır.
- **Sadece Değişiklikte Güncelleme:** RIP gibi her 30 saniyede bir tüm tabloyu çığlık atarak yollamaz. Sadece ağda bir kopukluk veya yeni bir yol eklendiğinde **kısmi güncelleme (Update)** yollar. Bu sayede bant genişliğini korur.
- **EIGRP Paket Çeşitleri:**
  1. **Hello:** Komşuları keşfetmek ve hayatta olduklarını kontrol etmek için kullanılır. `224.0.0.10` multicast adresine gönderilir.
  2. **Update:** Sadece yeni bir yol bulunduğunda veya yol değiştiğinde gönderilen güncelleme paketidir.
  3. **ACK (Acknowledge):** Güncelleme veya diğer önemli mesajların "alındığını" onaylamak için gönderilir.
  4. **Query (Sorgu):** Mevcut yol çöktüğünde ve yedek yol olmadığında komşulara "Elinde bu ağa giden başka yol var mı?" diye sorar.
  5. **Reply (Cevap):** Query sorgusuna verilen cevaptır.
- **RTP (Reliable Transport Protocol):** EIGRP'nin kendi güvenlik görevlisidir. TCP/UDP kullanmaz, paketlerin karşı tarafa güvenli ve hatasız ulaştığından (ACK gelip gelmediğinden) emin olmak için Cisco'ya özel bu protokolü kullanır.
- **Metric (Yol Maliyeti) Hesabı:** RIP sadece atlama sayısına bakıyordu. EIGRP ise yolu seçerken varsayılan olarak **K1 (Bandwidth - Bant Genişliği)** ve **K3 (Delay - Gecikme)** değerlerini hesaba katar. En hızlı ve en az gecikmeli yolu seçer.
**Hatırla:** Sınavda "Hibrit yapı", "Sadece değişiklik olduğunda güncelleme atar" veya "RTP (Reliable Transport Protocol) kullanır" laflarını görürsen cevap kesinlikle **EIGRP**'dir.

---

## 29 Mayıs 2026 - 6.2 EIGRP Tablo Yapıları ve DUAL Algoritması

**Kaynak:** `ders.md`, Bölüm 14; `pdf/h8-uygulama14- eigrp,ospf.pdf`
**Kısa Tanım:** EIGRP'nin ağın haritasını çıkarırken tuttuğu 3 temel veritabanı tablosu ve en iyi yolu/yedek yolu seçmek için kullandığı muazzam DUAL algoritmasıdır.
**Anahtar Kavramlar:**
- **EIGRP'nin 3 Temel Tablosu:**
  1. **Neighbor Table (Komşuluk Tablosu):** Doğrudan bağlı olduğu ve Hello paketiyle anlaştığı router'ların IP adreslerini tutar. (Telefon rehberi gibi).
  2. **Topology Table (Topoloji Tablosu):** Bir hedefe gitmek için komşulardan duyduğu **TÜM olası yolları** tuttuğu devasa haritadır. DUAL algoritması bu tabloya bakarak çalışır.
  3. **Routing Table (Yönlendirme Tablosu):** Topoloji tablosundaki tüm yollar içinden seçilen **SADECE en iyi yolun (Successor)** yazıldığı vitrindir. Paketler bu tabloya bakarak gönderilir.
- **DUAL (Diffusing Update Algorithm) Terimleri:**
  - **Successor (Ana Yol):** Bir hedefe giden **EN İYİ** (maliyeti en düşük) yoldur. Direkt Routing Tablosuna yazılır.
  - **Feasible Successor (Yedek Yol):** Ana yol çökerse devreye girmek üzere hazırda bekleyen 2. en iyi yoldur. Topoloji tablosunda bekler. *Sınavda sorulur: Feasible Successor olabilmek için "Komşunun sana söylediği maliyet (RD), senin mevcut en iyi maliyetinden (FD) küçük olmalıdır (Loop engellemek için)".*
  - **FD (Feasible Distance - Ulaşılabilir Mesafe):** Senin router'ından hedefe kadar olan **toplam** yoldur (toplam maliyet).
  - **RD/AD (Reported/Advertised Distance - Bildirilen Mesafe):** Komşunun sana dönüp "Kardeşim ben hedefe şu kadarlık mesafedeyim" diye bildirdiği rakamdır.
**Örnek:**
Hedefe giden 3 yol buldun. En hızlısı Successor olur ve Routing Table'a yazılır. İkinci en hızlı olan eğer loop kuralına (RD < FD) uyuyorsa Feasible Successor olur ve Topology Table'da yedekte bekler. Ana yol koparsa saniyeler içinde yedek yol devreye girer (RIP gibi 30 sn beklemez).
**Hatırla:** EIGRP'nin beyni **DUAL Algoritması**dır. En iyi yola **Successor**, yedek yola **Feasible Successor** denir.

---

## 29 Mayıs 2026 - 6.3 EIGRP CLI Yapılandırması, Auto-Summary İptali ve Variance ile Load Balancing

**Kaynak:** `ders.md`, Bölüm 14; `pdf/h8-uygulama14- eigrp,ospf.pdf`
**Kısa Tanım:** EIGRP protokolünün router üzerinde çalıştırılması, komşularla aynı Otonom Sistemde (AS) buluşturulması ve eşit olmayan yollar arasında yük dengelemesi (Load Balancing) yapılmasıdır.
**Anahtar Kavramlar ve Komutlar:**
- **Otonom Sistem (AS) Numarası:** EIGRP çalıştıran router'ların birbirleriyle komşu olabilmesi ve tablo paylaşabilmesi için **hepsinin aynı AS numarasına** sahip olması zorunludur. (Örn: `router eigrp 101` yazıldıysa, tüm komşularda 101 olmalı).
- **Wildcard Mask Kullanımı:** EIGRP'de ağları duyururken normal subnet maskesi (255.255.255.0) yerine genellikle Wildcard Mask (0.0.0.255) kullanılır.
- **No Auto-Summary:** RIPv2'de olduğu gibi EIGRP'de de ağların yanlışlıkla sınıflı (Classful) olarak yuvarlanmasını önlemek için yazılması şarttır. (EIGRP aslında bunu IOS 15'ten sonra otomatik yapar ama sınavlarda sorulur).
- **Variance (Yük Dengeleme Kuralı):** Router'lar normalde sadece metrik değeri "birebir aynı" olan yollardan yük dengelemesi yapar. Ancak EIGRP çok yeteneklidir; `variance 2` komutunu girersen, "En iyi yolumun (Successor) maliyetini 2 ile çarp, bu çıkan rakamın altındaki diğer (daha yavaş) yolları da yük dengelemesine dahil et" demiş olursun.
**Örnek / Komut:**
```text
RouterA(config)# router eigrp 101                 (101 numaralı AS içinde başlat)
RouterA(config-router)# network 192.168.4.0       (Ağı duyur)
RouterA(config-router)# network 17.5.1.16 0.0.0.3 (Wildcard maskeli duyuru)
RouterA(config-router)# no auto-summary           (Sınıfsız çalış)
RouterA(config-router)# variance 2                (Eşit olmayan yollarda yük dengesi kur)
```
**Hatırla:** EIGRP'yi başlatırken RIP'in aksine komutun yanına mutlaka bir sayı (AS Numarası) yazılır. Ve EIGRP, `variance` komutu sayesinde **farklı hızlardaki** yollardan aynı anda paket gönderip yük dengelemesi (Load Balancing) yapabilen **tek** protokoldür.

---

## 29 Mayıs 2026 - 6.4 OSPF Link-State Yapısı, Dijkstra Algoritması ve Cost Metrik Hesabı

**Kaynak:** `ders.md`, Bölüm 15; `pdf/h8-uygulama14- eigrp,ospf.pdf`
**Kısa Tanım:** OSPF (Open Shortest Path First), piyasadaki en yaygın, tamamen açık standartlı, büyük ağlarda kullanılan "Bağ-Durum (Link-State)" tabanlı dinamik yönlendirme protokolüdür.
**Anahtar Kavramlar:**
- **Link-State (Bağ Durumu):** OSPF, RIP gibi dedikoduya (komşudan duymaya) güvenmez. Her OSPF router'ı ağın kuşbakışı tam bir haritasını (kendi topolojisini) çizer ve en iyi yolu kendisi bizzat hesaplar.
- **Dijkstra Algoritması:** Ağ haritası çıkarıldıktan sonra, en kısa/en hızlı yolu matematiksel olarak kusursuz hesaplayan ünlü OSPF algoritmasıdır (Shortest Path First - SPF).
- **Metric (Maliyet - Cost) Hesabı:** OSPF'nin metric birimi "Cost" (Maliyet) olarak adlandırılır. Hız ne kadar yüksekse maliyet o kadar düşüktür. Formülü: `100.000.000 / Bant Genişliği (bps)`
  - *Örnek:* 100 Mbps (FastEthernet) bir portun OSPF Cost'u `1` iken, 10 Mbps (Ethernet) portun Cost'u `10`'dur. Cost ne kadar düşükse yol o kadar iyidir.
- **Hızlı Convergence:** Tıpkı EIGRP gibi, periyodik olarak tablo atmaz. Sadece ağda değişiklik olduğunda tetiklemeli (Triggered) güncelleme gönderir.
- **Hiyerarşik Yapı (Alanlar - Areas):** OSPF devasa ağlarda router'ların işlemcisini yormamak için ağı alanlara (Area) böler.
  - **Area 0 (Backbone Area):** Omurga alandır. Ağdaki diğer tüm alanlar fiziksel olarak mecburen **Area 0'a bağlanmak zorundadır**.
  - **ABR (Area Border Router):** Alanlar arasındaki sınırı belirleyen, bir bacağı Area 0'da diğer bacağı başka bir alanda olan sınır yönlendiricisidir.
**Hatırla:** Sınavda "Açık standartlı", "Dijkstra Algoritması", "Cost (Maliyet)" veya "Area 0 (Backbone)" kavramları geçiyorsa cevap net olarak **OSPF**'dir.

---

## 29 Mayıs 2026 - 6.5 OSPF Paket Çeşitleri, Komşuluk Kurulumu ve DR/BDR Seçimi

**Kaynak:** `ders.md`, Bölüm 15; `pdf/h8-uygulama14- eigrp,ospf.pdf`
**Kısa Tanım:** OSPF router'ların birbirleriyle nasıl tanıştığı, veritabanlarını nasıl paylaştığı ve ağdaki "Başkan (DR)" ile "Başkan Yardımcısını (BDR)" nasıl seçtiklerinin özetidir.
**Anahtar Kavramlar:**
- **OSPF Paket Çeşitleri:**
  - **Type 1 (Hello):** "Orada mısın? Benim şifrem şu, alanım Area 0. Komşu olalım mı?" mesajıdır. (Genelde 10 sn'de bir atılır).
  - **Type 2 (DBD - Database Description):** Komşuluk kurulduğunda gönderilen "Bende bu ağların haritası var" şeklindeki **özet** katalogdur.
  - **Type 3 (LSR - Link State Request):** DBD kataloğuna bakan router'ın, "Şu ağ bende yok, bunun detayını bana gönder" diye yaptığı **istektir**.
  - **Type 4 (LSU - Link State Update):** İstenilen rotaların tüm detayını içeren asıl **güncelleme** paketidir.
  - **Type 5 (LSAck):** Paketi aldım, teşekkürler onayıdır.
- **Komşuluk Kurulum Aşamaları:** Router'lar komşu olurken sırasıyla *Down -> Init -> Two-Way -> Exstart -> Exchange -> Loading -> Full* aşamalarından geçer. Sınavda en çok çıkanlar:
  - **Two-Way:** Hello paketleri karşılıklı alınmış, DR ve BDR seçiminin yapıldığı aşamadır.
  - **Full:** İki komşunun da veritabanının (haritaların) birebir aynı olduğu (senkronize), %100 komşuluğun kurulduğu son aşamadır.
- **DR (Designated Router) ve BDR (Backup Designated Router) Seçimi:** Ethernet gibi çoklu ağlarda herkes birbirine güncelleme atarsa trafik felç olur. OSPF ağda bir "Başkan (DR)" seçer. Herkes sadece Başkana (DR) güncelleme atar, Başkan da herkese dağıtır.
  - **Kriter:** Router ID'si (Kimlik Numarası) **en yüksek** olan cihaz DR olur. İkinci en yüksek BDR (Başkan Yardımcısı) olur.
  - **Router ID Nasıl Belirlenir:** Sırasıyla `router-id` komutuna bakılır. Yoksa en yüksek Loopback IP'sine, o da yoksa en yüksek fiziksel IP'ye bakılır.

---

## 29 Mayıs 2026 - 6.6 OSPF Area Yapısı ve Single-Area OSPF CLI Yapılandırması

**Kaynak:** `ders.md`, Bölüm 15; `pdf/h8-uygulama14- eigrp,ospf.pdf`
**Kısa Tanım:** OSPF protokolünün router üzerinde, komut satırından (CLI) tek alan (Single-Area - Area 0) için yapılandırılmasıdır.
**Anahtar Kavramlar ve Komutlar:**
- **Process ID:** OSPF başlatılırken EIGRP gibi yanına bir rakam yazılır (`router ospf 101`). Ancak EIGRP'den farklı olarak bu rakamın (101) komşularla **aynı olmasına gerek YOKTUR**. Bu sadece o router'ın kendi içindeki "İşlem Numarasıdır".
- **Wildcard Mask ve Area:** OSPF'de bir ağı duyururken `network` komutunun sonuna kesinlikle Wildcard Mask (Ters Maske) ve o ağın hangi alana (Area) ait olduğu yazılmak zorundadır. Aksi halde OSPF çalışmaz.
**Örnek / Komut:**
```text
RouterA(config)# router ospf 101
RouterA(config-router)# router-id 1.1.1.1                           (Başkan seçimi için kimlik ver)
RouterA(config-router)# network 192.168.34.0 0.0.0.255 area 0       (Ağı duyur ve Area 0'a ata)
RouterA(config-router)# network 17.1.1.16 0.0.0.3 area 0
```
**Hatırla:** OSPF komutunun sonundaki `area 0` yazısı omurga alanı ifade eder. OSPF doğrulama yapmak için `show ip route` yazılır ve yönlendirme tablosunda OSPF yolları **O** harfi ile görünür.
