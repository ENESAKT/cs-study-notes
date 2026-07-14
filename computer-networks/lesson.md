# Bilgisayar Ağları - Ders Notları

---

## 1. Kampüs Ağ Tasarımı

İyi bir ağ tasarımı modüler, hiyerarşik ve net fonksiyonlara sahip olmalıdır. Üç katmanlı Cisco Hiyerarşik Tasarımı şu şekildedir:

- **Core (Çekirdek) Katmanı:** Çok az değişiklik, birkaç özellik, yüksek bant genişliği, güçlü CPU. Tek amacı paketleri olabildiğince hızlı iletmektir.
- **Distribution (Dağıtım) Katmanı:** Erişim katmanındaki switch'leri toplar (aggregation) ve yedekleme (redundancy) sağlar. Politika ve yönlendirme sınırları burada çizilir.
- **Access (Erişim) Katmanı:** Yoğun port sayısına sahiptir. Uç cihazların (PC, IP Telefon vb.) ağa dahil olduğu katmandır. Güvenlik özellikleri (port security vb.) barındırır; ekleme, güncelleme ve değişimlerin en yoğun olduğu yerdir.

### Bina İçi ve Katman 2 İletişimi:
- Bir binanın içerisinde anahtarlama (**switching - Katman 2**), binalar arasında ise yönlendirme (**routing - Katman 3**) tercih edilir.
- Çok küçük ağlarda, binalar arasında anahtarlama yapılabilir. Çok büyük ağlarda ise binalar içerisinde yönlendirme yapılabilir.
- **Ethernet** bugün uygulamada olan (de-facto) ağ standardıdır. Basit, ucuz ve hızlı imal edilebilir olması en büyük avantajlarıdır.

---

## 2. Sanal Yerel Ağlar (VLAN - Virtual Local Area Networks)

VLAN, OSI referans modelinin 2. katmanında (Veri Bağı) yer alır ve en az Layer-2 bir cihaz kullanılarak oluşturulabilir.

### Temel Mantık:
- Switch cihazını ayrı sanal switch cihazlarına dönüştürmeye izin verir.
- VLAN'lar, kurumun ağ bağlantısına ya da fiziksel lokasyonuna bakmaksızın fonksiyonlara, departmanlara veya uygulamalara göre mantıksal alt ağlar (dilimler) oluşturur. Bu mantıksal ağlar aslında bölümlenmiş birer **broadcast domain**'dir.
- Ağ içerisindeki broadcast paketlerin sayısı azalır, gereksiz trafik önlenir ve güvenlik daha etkin bir biçimde düzenlenir.
- **Varsayılan Durum:** Switch üzerindeki her bir port için varsayılan VLAN, **VLAN 1**'dir ve silinemez. Diğer tüm portlar alternatif VLAN'lara yeniden atanabilir.
- **Haberleşme:** Eğer VLAN oluşturulan ortamda Layer-3 işlevi görecek bir cihaz (Router veya L3 Switch) yoksa VLAN’lar arası haberleşme sağlanamaz.

---

## 3. VLAN Port Çeşitleri

### 3.1 Access Port:
Cihazların bir VLAN'a switch üzerinden üye edilmesiyle oluşan fiziksel hattır. Sadece tek bir VLAN'a üye olabilir. Port bazlı VLAN atama (Static VLAN) iki adımdan oluşur:
1. Düğümü switch üzerinde doğru porta bağlamak (VLAN ataması host üzerinde değil, switch portunda yapılır).
2. VLAN üyeliğine bağlı olarak cihaza doğru IP adresi (ilgili subnetten) atamak.

### 3.2 Trunk Port:
Bir switch'in başka bir switch'e veya router'a bağlanması ile oluşan ve üzerindeki tüm VLAN bilgilerini taşıyan fiziksel hattır.
- Birden fazla VLAN'dan gelen trafiği birbirleri arasında aktarabilir.
- Her çerçeve (frame) hangi VLAN'a ait olduğunu tanımlayan bir etiket (tag) taşır.

#### VLAN Etiketleme (Tagging) Yöntemleri:
1. **ISL (Inter-Switch Link):** Cisco'ya özel (proprietary) yöntemdir. "External tagging" adı verilen, paketin orijinal boyutunu değiştirmeyen ancak **26 byte'lık ISL başlığı** ve **4 byte'lık FCS (hata kontrol)** alanı ekleyen yöntemdir. Sadece ISL tanıyan cihazlar tarafından kullanılır.
2. **IEEE 802.1Q:** Ortak endüstri standardıdır. Ethernet çerçevesinin içine **4 byte'lık etiket (tag)** yerleştirir (Internal tagging).
   - **TPID (16 bit):** Tag Protocol Identifier (0x8100)
   - **PCP (3 bit):** Priority Code Point (Öncelik)
   - **CFI (1 bit):** Format göstergesi
   - **VID (12 bit):** VLAN Identifier (VLAN ID'si - 4096 adede kadar VLAN tanımlayabilir)
   - *Not:* Kenar portlar (access) etiketlenmez. Sadece trunk portlardan geçen çerçeveler etiketlenir.

---

## 4. Uygulama 10: VLAN Yapılandırma Örneği

### 4.1 Adresleme Tablosu

| Aygıt | Arayüz | IP Adresi | Alt Ağ Maskesi | VLAN |
| :--- | :--- | :--- | :--- | :---: |
| **PC1** | NIC | `172.17.10.21` | `255.255.255.0` | 10 |
| **PC2** | NIC | `172.17.20.22` | `255.255.255.0` | 20 |
| **PC3** | NIC | `172.17.30.23` | `255.255.255.0` | 30 |
| **PC4** | NIC | `172.17.10.24` | `255.255.255.0` | 10 |
| **PC5** | NIC | `172.17.20.25` | `255.255.255.0` | 20 |
| **PC6** | NIC | `172.17.30.26` | `255.255.255.0` | 30 |

### 4.2 VLAN Listesi
- **VLAN 10:** Faculty/Staff
- **VLAN 20:** Students
- **VLAN 30:** Guest(Default)
- **VLAN 99:** Management&Native
- **VLAN 150:** VOICE

### 4.3 VLAN Yapılandırma ve Port Atama Komutları (S1, S2, S3)

#### VLAN Oluşturma (Cisco 2960 / 2950):
```text
Switch(config)# vlan 10
Switch(config-vlan)# name Faculty/Staff
Switch(config-vlan)# vlan 20
Switch(config-vlan)# name Students
Switch(config-vlan)# vlan 30
Switch(config-vlan)# name Guest(Default)
Switch(config-vlan)# vlan 99
Switch(config-vlan)# name Management&Native
Switch(config-vlan)# vlan 150
Switch(config-vlan)# name VOICE
Switch(config-vlan)# exit
```
*(Doğrulama komutu: `show vlan brief`)*

#### Access Port Atama (S2 ve S3 üzerinde):
```text
Switch(config)# interface f0/11
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
!
Switch(config)# interface f0/18
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
!
Switch(config)# interface f0/6
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 30
```

#### Voice (Ses) VLAN Yapılandırması (S3 F0/11):
S3 F0/11 portu hem PC4'e (VLAN 10) hem de IP Telefona (VLAN 150) bağlıdır. IP Telefon entegre portlardan geçişi yönetir.
```text
S3(config)# interface f0/11
S3(config-if)# mls qos trust cos
S3(config-if)# switchport voice vlan 150
```

### 4.4 Bağlantı Testi Analizi
- **Soru:** Access portları uygun VLAN'lara atanmış olmasına rağmen PC1'den PC4'e atılan pingler başarılı olur mu?
- **Cevap:** **Hayır, başarısız olur.** Çünkü switchler arasındaki bağlantılar (S2-S1 ve S1-S3 arasındaki Gigabit/Fast portları) henüz **Trunk** olarak yapılandırılmamıştır. Switchler varsayılan olarak birbirlerine sadece VLAN 1 trafiğini taşır.
- **Çözüm:** Switchler arasındaki bağlantıların (Trunk linkler) her iki ucundaki arayüzler `switchport mode trunk` komutuyla trunk olarak yapılandırılmalıdır.

---

## 5. VTP - VLAN Trunking Protocol

VTP, switchler üzerinde oluşturulan VLAN bilgilerinin ağdaki diğer switchler ile trunk portlar üzerinden otomatik olarak paylaşılmasını sağlayan Cisco protokolüdür.

### VTP Modları:
1. **Server (Sunucu) Modu:** VLAN oluşturulabilir, değiştirilebilir ve silinebilir. VLAN bilgilerini ağa yayar ve güncellemeleri NVRAM'e kaydeder.
2. **Client (İstemci) Modu:** Üzerinde VLAN oluşturulamaz, değiştirilemez veya silinemez. Server'dan gelen VLAN bilgilerini öğrenir, günceller ve diğer switchlere iletir. NVRAM'e kayıt yapmaz.
3. **Transparent (Şeffaf) Mod:** Sadece yerel olarak bağımsız VLAN oluşturabilir/silebilir. VTP veritabanıyla senkronize olmaz ancak gelen VTP mesajlarını diğer switchlere iletebilir. NVRAM'e kayıt yapar.

### VTP Yapılandırma Örnekleri:

```text
; ----- SERVER SWITCH (S1) -----
S1(config)# vtp version 2
S1(config)# vtp domain Muhendislik
S1(config)# vtp password cisco
S1(config)# vtp mode server

; ----- CLIENT SWITCH (S2) -----
S2(config)# vtp version 2
S2(config)# vtp domain Muhendislik
S2(config)# vtp password cisco
S2(config)# vtp mode client

; ----- TRANSPARENT SWITCH (S3) -----
S3(config)# vtp version 2
S3(config)# vtp domain Muhendislik
S3(config)# vtp password cisco
S3(config)# vtp mode transparent
```
*(Doğrulama komutu: `show vtp status`)*

---

## 6. VLAN'lar Arasında Yönlendirme (Inter-VLAN Routing)

**Kaynak:** `pdf/h7-uygulama9-vlan.pdf`

Fiziksel olarak aynı switch üzerinde bulunan ancak farklı VLAN'lara atanmış bilgisayarlar (farklı broadcast domain'lerde oldukları için) L3 yönlendirme desteği olmadan doğrudan haberleşemezler. Farklı VLAN'lar arasındaki iletişimi sağlamak için bir yönlendirici (Router) veya Layer-3 (L3) Switch kullanılmalıdır.

### 6.1 Cisco 1900 ve Cisco 2950 Serisi CLI Konfigürasyon Farkları
Cisco 1900 serisi (eski) switchler ile Cisco 2950 serisi (modern) switchler arasında VLAN ve Trunk CLI yapılandırma komutlarında belirgin farklar bulunur:

| İşlem | Cisco 1900 Serisi | Cisco 2950 Serisi |
| :--- | :--- | :--- |
| **VLAN Oluşturma** | `vlan [id] name [isim]` | `vlan [id]` <br> `name [isim]` |
| **Port VLAN Atama** | `vlan-membership static [id]` | `switchport mode access` <br> `switchport access vlan [id]` |
| **Trunk Etkinleştirme** | `trunk on` | `switchport mode trunk` |
| **Desteklenen Encapsulation**| Sadece **ISL** | Sadece **802.1Q (dot1q)** |

> [!IMPORTANT]
> Cisco 1900 serisi switchler sadece Cisco'ya özel **ISL** protokolünü desteklerken, 2950 serisi sadece açık standart olan **IEEE 802.1Q (dot1q)** protokolünü destekler. Encapsulation uyumsuzluğu nedeniyle bu iki switch serisi arasında **Trunk bağlantısı kurulamaz**.

### 6.2 Router-on-a-Stick Yöntemi ile Inter-VLAN Yapılandırması
Router'ın tek bir fiziksel FastEthernet/GigabitEthernet arayüzünü switch'e trunk port üzerinden bağlayarak, bu fiziksel port altında her bir VLAN için mantıksal alt arayüzler (**subinterface**) oluşturma yöntemidir.

#### Yapılandırma Kuralları:
1. Yönlendiricinin fiziksel ana arayüzünde (örn: `FastEthernet 0/0`) IP adresi tanımlanmamalıdır, sadece `no shutdown` ile aktif edilmelidir.
2. Her VLAN için bir alt arayüz (örn: `fa0/0.1`) oluşturulur ve encapsulation tipi switch'e göre (ISL veya dot1q) ve ilişkili VLAN ID'si girilerek IP adresi (VLAN'ın default gateway adresi) atanır.

#### Router CLI Komutları (ISL - Cisco 1900 ile bağlıyken):
```text
Router(config)# interface fastethernet 0/0
Router(config-if)# no ip address
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface fastethernet 0/0.1
Router(config-subif)# encapsulation isl 1                  ; VLAN 1 için ISL encapsulation
Router(config-subif)# ip address 192.168.1.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fastethernet 0/0.2
Router(config-subif)# encapsulation isl 2                  ; VLAN 2 için ISL encapsulation
Router(config-subif)# ip address 192.168.2.1 255.255.255.0
```

#### Router CLI Komutları (dot1q - Cisco 2950 ile bağlıyken):
```text
Router(config)# interface fastethernet 0/0.1
Router(config-subif)# encapsulation dot1q 1 native         ; VLAN 1 native olarak dot1q encapsulation
Router(config-subif)# ip address 192.168.1.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface fastethernet 0/0.2
Router(config-subif)# encapsulation dot1q 2                 ; VLAN 2 için dot1q encapsulation
Router(config-subif)# ip address 192.168.2.1 255.255.255.0
```

### 6.3 Inter-VLAN Laboratuvar Çalışması running-config Analizleri
Bu laboratuvar senaryosunda, SwitchA ve SwitchB `fa0/23` portları üzerinden trunk olarak bağlıdır. SwitchB, `fa0/24` portu üzerinden Router'ın `fa0/0` portuna bağlıdır. Tüm trunk hatlarında `dot1q` standardı kullanılmaktadır.

#### 1. SwitchA running-config ve VLAN Durumu:
```text
hostname SwitchA
!
interface FastEthernet0/1
 switchport mode access
!
interface FastEthernet0/2
 switchport access vlan 2
 switchport mode access
!
interface FastEthernet0/3
 switchport access vlan 3
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 4
 switchport mode access
!
interface FastEthernet0/23
 switchport mode trunk                   ; SwitchB'ye giden trunk hattı
!
interface FastEthernet0/24
 switchport mode access
!
interface Vlan1
 ip address 192.168.1.11 255.255.255.192
!
ip default-gateway 192.168.1.1
```
*(Doğrulama: `show vlan` komutu çalıştırıldığında VLAN 2 `egitim`, VLAN 3 `muhasebe`, VLAN 4 `yonetim` olarak tanımlanmıştır. `fa0/23` trunk port olduğundan listelenmez).*

#### 2. SwitchB running-config ve VLAN Durumu:
```text
hostname SwitchB
!
interface FastEthernet0/1
 switchport mode access
!
interface FastEthernet0/2
 switchport access vlan 2
 switchport mode access
!
interface FastEthernet0/3
 switchport access vlan 3
 switchport mode access
!
interface FastEthernet0/4
 switchport access vlan 4
 switchport mode access
!
interface FastEthernet0/23
 switchport mode trunk                   ; SwitchA'ya giden trunk hattı
!
interface FastEthernet0/24
 switchport mode trunk                   ; Router'a giden trunk hattı
!
interface Vlan1
 ip address 192.168.1.12 255.255.255.192
!
ip default-gateway 192.168.1.1
```

#### 3. Router running-config Ayrıntıları:
```text
hostname Router
!
interface FastEthernet0/0
 no ip address                           ; Fiziksel porta IP verilmez
!
interface FastEthernet0/0.1
 encapsulation dot1Q 1 native            ; VLAN 1 Native olarak etiketlenir
 no ip address
!
interface FastEthernet0/0.2
 encapsulation dot1Q 2                   ; VLAN 2 için subinterface
 ip address 192.168.1.65 255.255.255.192
!
interface FastEthernet0/0.3
 encapsulation dot1Q 3                   ; VLAN 3 için subinterface
 ip address 192.168.1.129 255.255.255.192
!
interface FastEthernet0/0.4
 encapsulation dot1Q 4                   ; VLAN 4 için subinterface
 ip address 192.168.1.193 255.255.255.192
!
ip classless
```

---

## 7. DHCP (Dynamic Host Configuration Protocol)

DHCP, ağa bağlanan cihazlara otomatik olarak IP adresi, alt ağ maskesi, default gateway ve DNS sunucusu gibi ağ ayarlarını dağıtan protokoldür. IP adresleri belirli bir süreliğine kiralanır.

### DHCP DORA İletişim Süreci:
1. **Discover (Keşif):** İstemci, sunucunun IP'sini bilmediğinden broadcast (`255.255.255.255`) paket gönderir. (Kaynak IP: `0.0.0.0`, Portlar: Kaynak UDP 68, Hedef UDP 67).
2. **Offer (Teklif):** DHCP sunucuları istemciye uygun bir IP teklifi sunar (DHCPOFFER).
3. **Request (İstek):** İstemci teklifi kabul ettiğini belirten istek gönderir (DHCPREQUEST).
4. **Acknowledge (Onay):** Sunucu kiralama bilgilerini veritabanına yazar ve onay gönderir (DHCPACK).

### DHCP Öncesi Protokoller:
- **RARP:** MAC adresinden IP öğrenir. Sadece IP adresi verir, DNS veya default gateway sağlayamaz. Broadcast kullanır ve tek ağ ile sınırlıdır.
- **BOOTP:** IP adresi, gateway, DNS ve boot dosyası bilgilerini dağıtabilir. Ancak her cihaz için manuel kayıt gerektirir ve otomatik/dinamik IP dağıtımı yoktur.

### 6.1 Cisco Router DHCP Sunucusu Yapılandırması

```text
; 1. DHCP Servisini Aktifleştirme
Router(config)# service dhcp

; 2. Havuz Oluşturma ve Ağ Tanımlama
Router(config)# ip dhcp pool Academy
Router(config-dhcp)# network 192.168.0.0 255.255.0.0

; 3. IP Adreslerini Dağıtımdan Hariç Tutma (Excluded Addresses)
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.10

; 4. DNS, Gateway ve Domain Bilgileri
Router(config-dhcp)# domain-name academy.com
Router(config-dhcp)# dns-server 172.16.1.2
Router(config-dhcp)# default-router 172.16.1.1
Router(config-dhcp)# lease 2 12 30                  ; 2 gün 12 saat 30 dakika

; 5. MAC Adresine IP Rezerve Etme (Statik DHCP)
Router(config)# ip dhcp pool Academy-Lab
Router(config-dhcp)# host 192.168.1.100 255.255.0.0
Router(config-dhcp)# client-identifier 0100.112f.b212.b2  ; 01: Ethernet + MAC
```

#### DHCP Relay (IP Helper-Address) Yapılandırması:
Farklı ağdaki DHCP istekleri (broadcast) router'ı geçemez. Router'ın arayüzünde istekleri sunucuya unicast olarak yönlendirmek için:
```text
Router(config)# interface g0/0
Router(config-if)# ip helper-address 192.168.2.10
```

---

## 8. IPv6 (İnternet Protokolü Sürüm 6)

**Kaynak:** `pdf/blm316-hafta10.pdf`

IPv6, 32 bitlik IPv4 adreslerinin tükenmesi üzerine IETF tarafından geliştirilen 128 bitlik yeni adresleme standardıdır. IPv6 sadece adres alanı genişletmekle kalmayıp, paket başlığı sadeleştirmesi ve otomatik adres yapılandırması (SLAAC) gibi birçok yenilik de getirmiştir.

### 8.1 IPv4 ve IPv6 Birlikte Çalışma Yöntemleri
IPv6'ya geçiş süreci kademeli olacağından, her iki protokolün birlikte çalışabilmesi için 3 temel mekanizma kullanılır:
1. **Dual Stack (Çift Yığın):** Cihazların hem IPv4 hem de IPv6 protokol yığınlarını aynı anda çalıştırmasıdır. Ağ genelinde en yaygın geçiş yöntemidir.
2. **Tünelleme (Tunneling):** IPv6 paketlerinin, sadece IPv4 destekleyen ağlardan geçebilmesi için bir IPv4 paketinin içine kapsüllenerek (encapsulate edilerek) taşınması yöntemidir.
3. **NAT64 (Çeviri):** Sadece IPv6 destekleyen bir cihazın, sadece IPv4 destekleyen bir cihazla iletişim kurabilmesi için paket başlığının çevrilmesi yöntemidir.

### 8.2 IPv6 Adres Gösterimi ve Kısaltma Kuralları
IPv6 adresleri **128 bit** uzunluğundadır. Onaltılık (Hexadecimal) tabanda yazılır ve 8 adet 16 bitlik gruptan (hektet) oluşur. Gruplar birbirlerinden iki nokta üst üste (`:`) ile ayrılır:
`Tercih Edilen Gösterim: xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx`
Örnek: `2001:0db8:0000:aaaa:cafe:0000:0000:0001`

#### Adres Kısaltma Kuralları:
1. **Baştaki Sıfırları Kaldırma (Leading Zeros):** Her hekstetin solundaki sıfırlar yazılmayabilir. Hekstetin tamamı sıfır ise tek bir `0` bırakılır. (Sondaki sıfırlar silinemez!)
   - `2001:0db8:0000:aaaa:cafe:0000:0000:0001` -> `2001:db8:0:aaaa:cafe:0:0:1`
2. **Çift Kolon Kullanımı (Double Colon - `::`):** Tamamen sıfırlardan oluşan ardışık hekstetlerin yerine tek bir kez `::` kullanılabilir. Bir adreste `::` **yalnızca bir kez** kullanılabilir.
   - `2001:db8:0:aaaa:cafe:0:0:1` -> `2001:db8:0:aaaa:cafe::1`
   - Sıkıştırma Egzersiz Örnekleri:
     - `fe80:0000:0000:0000:0000:0000:0111:aaaa` -> `fe80::111:aaaa`
     - `2001:0db8:4455:0667:cafe:0000:0000:0110` -> `2001:db8:4455:667:cafe::110`
     - `cca0:0f12:000b:0123:00fe:0000:0000:0001` -> `cca0:f12:b:123:fe::1`
     - `2020:1923:0606:0001:00aa:000c:0678:0101` -> `2020:1923:606:1:aa:c:678:101`

### 8.3 IPv6 Adres ve Prefix Yapısı
- **Prefix (Önek) Uzunluğu:** IPv6 adresinin ağ kısmını (network mask) belirtir ve `/` ile gösterilir. LAN'lar için önerilen prefix uzunluğu **/64**'tür.
- **Bölümler:** IPv6 adresleri 64 bitlik **Network Prefix** and 64 bitlik **Interface ID** (Arayüz Kimliği - Host kısmı) olarak ikiye ayrılır.

### 8.4 IPv6 Adres Türleri
1. **Unicast (Tek Noktaya Yayın):** Tek bir cihaz arayüzünü benzersiz olarak tanımlar.
2. **Multicast (Çoklu Yayın):** Tek bir paketi birden fazla cihaza (multicast grubuna üye olanlar) gönderir. `ff00::/8` ile başlar.
3. **Anycast:** Birden çok cihaza aynı unicast adresi atanabilir. Anycast adresine gönderilen paket, yönlendirme açısından o adrese en yakın olan cihaza teslim edilir.
*Not: IPv6'da Broadcast adresleme yoktur. Bunun yerine tüm düğümlere ulaşan multicast adresleri kullanılır.*

#### Önemli Unicast Adres Türleri:
- **Global Unicast Adres (GUA):** İnternette yönlendirilebilen, global olarak benzersiz adreslerdir (IPv4 Public IP gibidir). Şu anda sadece `2000::/3` aralığındaki (ilk hanesi 2 veya 3 olan) adresler atanmaktadır (toplam alanın 1/8'i).
  - *GUA Yapısı:* `Global Yönlendirme Öneki (48 bit)` + `Alt Ağ Kimliği (16 bit)` + `Arayüz Kimliği (64 bit)`
- **Link-Local Adres (LLA):** Aynı lokal fiziksel link (alt ağ) üzerindeki cihazların iletişimi için zorunludur. İnternete veya başka alt ağlara yönlendirilemez. `fe80::/10` (yani `fe80::` ile `febf::` arası) aralığındadır. Router'lar routing güncellemesi yollarken ve hostlar default gateway olarak komşu router'ın LLA'sını kullanırlar.
- **Unique Local Adres (ULA):** Özel kurumsal ağlarda intranet içi iletişimde kullanılır. Internet ortamında yönlendirilmez. `fc00::/7` (yani `fc00::` ile `fdff::` arası) aralığındadır (IPv4 Private IP gibidir).

### 8.5 IPv6 GUA için Dinamik Adresleme Yöntemleri
Cihazlar, ağdaki IPv6 yönlendiricilerinden gelen **ICMPv6 RA (Router Advertisement)** ve istemciden giden **RS (Router Solicitation)** mesajları vasıtasıyla dinamik adres alırlar:
1. **SLAAC (State-less Address Autoconfiguration):** Cihazın DHCPv6 sunucusu olmadan kendi IP adresini oluşturmasıdır. Ağ önekini RA mesajından alır. 64 bitlik arayüz kimliğini (Interface ID) ise **EUI-64** yöntemiyle veya rastgele üretir.
2. **SLAAC + Durumsuz (Stateless) DHCPv6:** Cihaz kendi GUA adresini SLAAC ile oluşturur. Ancak DNS sunucu adresi, domain adı gibi ek bilgileri durumsuz DHCPv6 sunucusundan çeker. DHCPv6 sunucu IP dağıtımı yapmaz ve durum tutmaz.
3. **Durumlu (Stateful) DHCPv6:** Cihaz IP adresini, önekini ve DNS bilgilerini tamamen DHCPv6 sunucusundan alır (IPv4 DHCP gibidir). RA mesajı cihazı yönlendirir ancak varsayılan ağ geçidi olarak yine RA'yı gönderen router'ın LLA adresi kullanılır.

### 8.6 Interface ID Oluşturma Yöntemleri
SLAAC veya DHCPv6 kullanırken cihazın 64 bitlik arayüz kimliği üretme yöntemleri:
1. **EUI-64 Süreci:** Cihazın 48 bitlik fiziksel MAC adresinin tam ortasına 16 bitlik `fffe` değeri eklenir. Ardından MAC adresinin soldan 7. biti (U/L - Universal/Local biti) ters çevrilir (0 ise 1, 1 ise 0).
   - *Örnek:* MAC adresi `fc:99:47:75:ce:e0` olsun.
     - Araya `fffe` ekleme: `fc:99:47:ff:fe:75:ce:e0`
     - 7. biti ters çevirme: `fc` binary olarak `11111100`'dır. 7. bit (sondan ikinci bit) 0'dır, 1 yapılır. Yeni değer `11111110` yani `fe` olur.
     - EUI-64 Arayüz Kimliği: `fe99:47ff:fe75:cee0`
2. **Rastgele Oluşturma:** İşletim sisteminin rastgele 64 bitlik değer üretmesidir (Windows Vista ve sonrasında varsayılandır).
- **DAD (Duplicate Address Detection):** Dinamik adres üreten cihaz, adresin ağda çakışıp çakışmadığını test etmek için komşuluk ARP benzeri DAD işlemi yürütür. Yanıt gelmezse adresi benzersiz kabul edip kullanmaya başlar.

### 8.7 İyi Bilinen (Well-Known) IPv6 Multicast Adresleri (`ff02::/16`)
- `ff02::1` : **Tüm Düğümler (All-nodes) Multicast Grubu:** Ağa bağlı tüm IPv6 cihazlarını kapsar. IPv4 broadcast adresiyle eş değerdir.
- `ff02::2` : **Tüm Yönlendiriciler (All-routers) Multicast Grubu:** Ağdaki tüm IPv6 yönlendiricilerini kapsar. (Yönlendiricide `ipv6 unicast-routing` aktif edildiğinde bu gruba üye olur).

### 8.8 IPv6 Alt Ağ Oluşturma (Subnetting) ve CLI Yapılandırması
Kurumlara genellikle ISP'ler tarafından `/48` boyutunda Global Routing Prefix atanır. Kurum içindeki 16 bitlik Subnet ID alanı kullanılarak $2^{16} = 65,536$ adet `/64` boyutunda alt ağ oluşturulabilir.
Örnek: `2001:db8:cafe::/48` önekiyle alt ağ oluşturma:
- 1. Alt Ağ: `2001:db8:cafe:0000::/64` (kısaca `2001:db8:cafe::/64`)
- 2. Alt Ağ: `2001:db8:cafe:0001::/64` (kısaca `2001:db8:cafe:1::/64`)
- 3. Alt Ağ: `2001:db8:cafe:0002::/64` (kısaca `2001:db8:cafe:2::/64`)

#### Cisco Yönlendirici IPv6 Yapılandırma Komutları:
IPv6 yönlendirmesini aktifleştirmek için mutlaka global modda `ipv6 unicast-routing` komutu girilmelidir.
```text
Router(config)# ipv6 unicast-routing

; --- Arayüz GUA ve LLA Statik Yapılandırması ---
Router(config)# interface gigabitEthernet 0/0/0
Router(config-if)# ipv6 address 2001:db8:cafe:1::1/64
Router(config-if)# ipv6 address fe80::1:1 link-local       ; Hatırlaması kolay LLA ataması
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface gigabitEthernet 0/0/1
Router(config-if)# ipv6 address 2001:db8:cafe:2::1/64
Router(config-if)# ipv6 address fe80::2:1 link-local
Router(config-if)# no shutdown
```

---

## 9. NAT (Network Address Translation)


NAT, iç (özel) ağdaki RFC 1918 özel IP adreslerinin, internette yönlendirilebilir genel (public) IP adreslerine dönüştürülmesini sağlar. Ağ mimarisini dışarıdan gizlediği için ek güvenlik sağlar.

### Özel IP Adres Aralıkları (RFC 1918):
- `10.0.0.0` - `10.255.255.255`
- `172.16.0.0` - `172.31.255.255`
- `192.168.0.0` - `192.168.255.255`

### NAT Terminolojisi:
- **Inside Local:** İç ağdaki cihazın kendi gerçek özel IP adresi.
- **Inside Global:** İç ağdaki cihazın dış dünyaya çıkarken dönüştürüldüğü genel IP adresi (Router dış bacağı IP'si).
- **Outside Global:** Dış ağdaki bilgisayara atanan public IP adresi.
- **Outside Local:** Dış ağdaki cihazı iç ağa tanıtan adres (Genelde Outside Global ile aynıdır).

### NAT Sorunları:
- İstemci-sunucu (HTTP, FTP) modelleri sorunsuz çalışırken, dışarıdan başlatılan TCP/UDP paketleri engellendiği için Peer-to-Peer (P2P) bağlantılar NAT arkasında sorun yaşar.

### NAT Çeşitleri ve Yapılandırma Komutları:

#### 1. Static NAT (Birebir Dönüşüm):
```text
Router(config)# interface G0/0/0
Router(config-if)# ip nat inside
Router(config-if)# exit
Router(config)# interface S0/1/0
Router(config-if)# ip nat outside
Router(config-if)# exit
Router(config)# ip nat inside source static 172.16.1.1 154.80.1.67
```

#### 2. Dynamic NAT (Havuz Kullanımı):
```text
Router(config)# ip nat pool POOLNAME 154.80.1.1 154.80.1.55 netmask 255.255.255.0
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
Router(config)# ip nat inside source list 10 pool POOLNAME
```

#### 3. NAT Overload (PAT - Port Address Translation):
Çok sayıda yerel cihazın tek bir genel IP üzerinden port bilgileriyle dışarı çıkarılmasıdır (IP Maskeleme & Oturum Takibi).
```text
Router(config)# access-list 10 permit 172.16.1.0 0.0.0.255
Router(config)# ip nat inside source list 10 interface S0/1/0 overload
```

#### NAT Doğrulama ve Hata Giderme:
```text
Router# show ip nat translations        ; Aktif çevirileri gösterir
Router# show ip nat statistics          ; NAT istatistiklerini ve yön arayüzlerini gösterir
Router# debug ip nat                    ; Gerçek zamanlı çevirileri izler
Router# clear ip nat translation *      ; Dinamik NAT tablosunu temizler
```

---

## 10. IP Datagram Formatı

Ağ katmanında taşınan IP paketinin (datagram) başlık alanları ve boyutları şu şekildedir:

- **Version (4 bit):** IP sürümünü gösterir. IPv4 için `4`'tür.
- **HLEN (4 bit):** IP başlığının uzunluğunu 32 bitlik kelime birimi olarak gösterir (5 ile 15 arasında değer alır, yani 20-60 bayt).
- **Type of Service (ToS - 8 bit):** Kalite (QoS) ayarları, düşük gecikme veya yüksek güvenilirlik gibi servis tipleri.
- **Total Length (16 bit):** Başlık ve verinin toplam boyutunu bayt cinsinden belirtir.
- **Identification (16 bit):** Parçalanmış (fragmented) IP paketlerini birleştirmek için kullanılan kimlik.
- **Flags (3 bit):** Parçalama bayraklarıdır:
  - *R (Reserved):* Ayrılmış bit (0).
  - *DF (Don't Fragment):* Paketi parçalama.
  - *MF (More Fragments):* Arkadan gelen başka parçalar da var.
- **Fragment Offset (13 bit):** Parçalanan verinin orijinal paket içindeki sırasını belirler.
- **Time to Live (TTL - 8 bit):** Paketin ağda sonsuz döngüye girmesini önleyen yaşam süresi (atlama sayısı). Her yönlendirici TTL'i 1 azaltır.
- **Protocol (8 bit):** Verinin taşınacağı üst katman protokolünü belirtir (Örn: TCP için 6, UDP için 17, ICMP için 1).
- **Header Checksum (16 bit):** IP başlığında hata olup olmadığını kontrol etmek için kullanılır.
- **Source IP (32 bit) & Destination IP (32 bit):** Gönderen ve alıcının mantıksal IP adresleri.

---

## 11. Yönlendirme (Routing) Temelleri

Yönlendirme; bir IP paketinin kaynak noktadan hedefe hangi yoldan gideceğine karar verme sürecidir. Yönlendiriciler (Router) bu işlem için **Yönlendirme Tablosunu (Routing Table)** kullanır.

### 11.1 Yönlendirici (Router) Görevleri
- show ip route tablosuna bakarak en uygun yolu (Best Path) seçmek.
- Paketleri ilgili arayüzden hedefe iletmek.
- OSI modelinin ilk 3 katmanına (Fiziksel, Veri Bağı, Ağ) sahiptir. LAN ile WAN teknolojileri arasında köprü görevi görür.
- **Yönlendirici Çeşitleri:**
  - *Core Router (Merkez):* Şaseli üretilir. Trafiğin toplandığı ve paketlerin omurga ağda çok hızlı iletildiği güçlü cihazlardır.
  - *Edge Router (Kenar):* 1-2 LAN'ın WAN'a bağlanmasını sağlayan daha basit işlem gücüne sahip cihazlardır.

### 11.2 Yol Belirleme ve En Uzun Eşleşme (Longest Prefix Match)
Ağdaki en iyi yol, tablodaki **en uzun eşleşmeye (en büyük prefix maskesi)** sahip rotadır.
Örnek: Hedef IP `172.16.0.10` olsun. Tabloda 3 adet eşleşen rota vardır:
1. `172.16.0.0/12`
2. `172.16.0.0/18`
3. `172.16.0.0/26`
*Sonuç:* En uzun maskeye (`/26`) sahip olan 3. yol en uzun eşleşmedir ve paket bu arayüze yönlendirilir.

---

## 12. Statik Yönlendirme (Static Routing)

Küçük ölçekli ağlarda yöneticiler tarafından manuel olarak tanımlanan yönlendirme satırıdır.
- Komut formatı: `ip route [hedef network] [subnet mask] [next-hop IP / çıkış interface'i] [distance]`
- **Administrative Distance (AD):** Rotaların güvenilirlik derecesidir. Statik rota için varsayılan AD değeri **1**'dir.

#### DCE / DTE ve Clock Rate:
Seri port WAN bağlantı simülasyonlarında, DCE olan uçta mutlaka saat hızı (clock rate) tanımlanmalıdır, aksi halde hat çalışmaz.
```text
RouterA(config)# interface Serial0/0
RouterA(config-if)# ip address 10.0.0.1 255.255.255.0
RouterA(config-if)# clock rate 64000
```

#### Yönlendirme Tablosu (`show ip route`) Terimleri:
- **C (Directly Connected):** Arayüze doğrudan bağlı olan ağları gösterir.
- **S (Static):** Statik rotaları gösterir.
- **Parent / Child Route:** Yönlendirme tablosunda `172.16.0.0` ağının listelendiği ana satır Parent, onun altındaki alt ağlar (`172.16.2.0`, `172.16.3.0`) ise Child Route'dur.

#### Default Routing (Varsayılan Yönlendirme):
Yönlendirme tablosunda eşleşen rota bulunmadığında paketin gönderileceği varsayılan çıkış yoludur (`0.0.0.0/0`).
```text
Router(config)# ip route 0.0.0.0 0.0.0.0 10.0.3.2
```

#### Discard Route (Null0) ve `no ip classless`:
- **Discard Route:** Döngüleri (routing loops) engellemek amacıyla ulaşılamayan rotaları `null0` (sanal çöp kutusu) arabirimine yönlendirip drop etmektir:
  ```text
  Router(config)# ip route 172.16.0.0 255.255.0.0 null0
  ```
- **no ip classless:** Router'ın, hedef IP alt ağını parent network altında bulamadığında default route'a göndermeyip paketi doğrudan drop etmesini sağlayan komuttur (Önerilmez).

---

## 13. Dinamik Yönlendirme (Dynamic Routing)

Yönlendiriciler kendi doğrudan bağlı ağlarını komşu yönlendiricilere dinamik protokoller ile duyururlar.

### 13.1 Otonom Sistemler (Autonomous System - OS)
İnternet, birbirine bağlanmış birçok **Otonom Sistemden (OS)** oluşur. OS, tek bir yönetimsel otorite (organizasyon, servis sağlayıcı veya kurum) tarafından yönetilen yönlendiriciler ve ağlar grubudur.
- Dış dünyaya tek bir yönlendirme yapısı olarak görünürler.
- OS içerisinde herhangi bir düğüm çifti arasında (olağanüstü durumlar hariç) mutlaka bir yol bulunur.
- **İki Seviyeli Yönlendirme:**
  - **Intra-OS Yönlendirme (AS İçi):** Otonom sistemin kendi içerisindeki yönlendiriciler arasındaki yönlendirme protokolleridir (RIP, OSPF, EIGRP vb.). **IGP (Interior Gateway Protocols)** olarak adlandırılır.
  - **Inter-OS Yönlendirme (AS'ler Arası):** Farklı otonom sistemleri birbirine bağlayan yönlendirme protokolleridir (BGP). **EGP (Exterior Gateway Protocols)** olarak adlandırılır.

### 13.2 Dinamik Yönlendirme Protokol Sınıfları
Yönlendirme algoritmalarına ve yöntemlerine göre 3 sınıfa ayrılır:
1. **Distance Vector (Uzaklık Vektörü) Protokoller:** Belirli periyotlarda (örn: RIP her 30 sn) yönlendirme tablosunun tamamını komşularıyla paylaşırlar. Yönün hangi arayüz (vektör) ve mesafenin ne kadar (uzaklık) olduğunu bilirler, tüm ağ topolojisini göremezler. (Örn: RIPv1, RIPv2, IGRP)
2. **Link-State (Bağlantı Durumu) Protokoller:** Sadece ağda bir değişiklik olduğunda güncelleme gönderirler. Komşulukları "Hello" paketleriyle kontrol ederler ve tüm ağın haritasını (topology database) çıkarırlar. (Örn: OSPF, IS-IS)
3. **Hybrid (Hibrit) Protokoller:** Hem distance vector hem de link-state özelliklerini bir arada barındırırlar. (Örn: Cisco'ya özel EIGRP)

---

### 13.3 RIP (Routing Information Protocol) - RIPv1

RIP, Bellman-Ford (Uzaklık Vektörü) algoritmasını kullanan en eski yönlendirme protokollerinden biridir.

#### Temel Özellikleri:
- En iyi yol seçiminde tek kriter olarak **Hop Sayısına (Sıçrama Sayısı / Geçilen Yönlendirici Sayısı)** bakar.
- Maksimum atlama (hop) sayısı **15**'tir. 16. hop erişilemez kabul edilir (*Destination Unreachable*), bu yüzden büyük ağlarda ölçekleme yapamaz.
- Her **30 saniyede bir** yönlendirme tablosunu komşularına broadcast (`255.255.255.255`) olarak gönderir.
- İletişim için taşıma katmanında **UDP 520** nolu portu kullanır.
- **Classful** (sınıflı) bir protokoldür; güncellemelerinde alt ağ maskesi (subnet mask) bilgisini göndermez. Dolayısıyla VLSM ve CIDR desteklemez.

#### RIP Bellman-Ford Veri Tabanı Girdileri:
Her bir rota girişi için şu bilgiler tutulur:
- **Adres:** Ulaşılacak hedef ağın veya bilgisayarın IP adresi.
- **Yönlendirici (Gateway):** Hedefe ulaşmak için paketin gönderileceği bir sonraki yönlendiricinin (next-hop) IP adresi.
- **Ağ Donanım Birimi (Interface):** Paket gönderilirken kullanılacak çıkış arayüzü.
- **Metrik:** Hedefe olan uzaklık değeri (hop sayısı).
- **Sayaç:** Tablo girdisinin en son güncellendiği andan itibaren geçen süre (saniye).

#### RIP Hat Başarısızlık Durumu ve Düzeltimi:
- Eğer bir komşudan **180 saniye** boyunca güncelleme mesajı (yayın) gelmezse veya rotanın metrik değeri **15'ten büyük** olursa, o hat/yönlendirici **ölü (down)** kabul edilir.
- İlgili rota geçersiz kılınır ve komşu yönlendiricilere anında yeni güncelleme gönderilerek hat hatası bildirilir.

#### RIP Kararlılık ve Döngü Önleme Mekanizmaları:
- **Load Balancing:** Eşit metriğe (hop sayısına) sahip birden fazla yol varsa, trafiği bu yollar arasında paylaştırır.
- **Split Horizon:** Bir arayüzden öğrenilen bir rota bilgisinin, aynı arayüzden geri gönderilmesini engeller. Bu sayede iki router arasında sonsuz döngü (loop) oluşması engellenir.
- **Route Poisoning:** Çöken bir ağın metrik değerini anında `16` (erişilemez) yaparak diğer yönlendiricilere hemen bildirir.
- **Holddown Timers:** Bir rotanın çöktüğü bilgisi alındıktan sonra, ağın kararlı hale gelmesi için belirli bir süre boyunca (holddown süresi) o rota hakkında gelen daha kötü metrikli veya kararsız yönlendirme güncellemeleri göz ardı edilir.
- **Triggered Updates:** 30 saniyelik periyodik süreyi beklemeden, ağ topolojisinde bir değişiklik olduğu anda güncellemelerin anında gönderilmesidir.

#### Floating Static Route (Yedek Hat):
Dinamik yönlendirme protokolünün (Örn: RIP) çalışmadığı durumlarda devreye giren yedek statik rotalardır. Bunun için statik rotaya RIP'in Administrative Distance (AD) değerinden (RIP AD = 120) daha büyük bir AD (örn: 130) atanmalıdır:
```text
RouterA(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2 130
```

---

### 13.4 RIP Yönlendirme Tablosu Güncelleme Algoritması ve Örnekleri

#### Örnek 1: D Yönlendirme Tablosu Güncellemesi
D yönlendiricisinin mevcut tablosu ve komşu A yönlendiricisinden gelen güncelleme mesajına göre tablonun güncellenmesi:

**D Yönlendirme Tablosu (Eski Hali):**
| Hedef Ağ | Sonraki Yönlendirici | Hedefe Atlama Sayısı |
| :--- | :---: | :---: |
| **w** | A | 2 |
| **y** | B | 2 |
| **z** | B | 7 |
| **x** | - (Doğrudan) | 1 |

**A'dan Gelen RIP Güncelleme Mesajı:**
- w ağını A üzerinden 1 hop mesafede biliyor.
- x ağını A üzerinden 1 hop mesafede biliyor.
- z ağını C üzerinden 4 hop mesafede biliyor.

**Güncelleme Adımları:**
A'dan gelen paket D'ye ulaştığı için tüm mesafeler **1 artırılır** (çünkü D'den A'ya gitmek için +1 hop gerekir):
- A üzerinden yeni mesafeler: w = 2, x = 2, z = 5 (4+1).
- **w analizi:** Eski yol zaten A üzerinden 2 idi. Gelen yeni bilgi A üzerinden 2 diyor. Değişiklik yok.
- **y analizi:** A'dan y hakkında bilgi yok. B üzerinden 2 olarak kalır. Değişiklik yok.
- **x analizi:** Eski yol doğrudan bağlıydı (1 hop). A üzerinden gelen yol 2 hop. Doğrudan bağlı olan daha kısa olduğu için güncellenmez.
- **z analizi:** Eski yol B üzerinden 7 hop idi. A üzerinden gelen yeni yol **5 hop** (daha kısa). Tablo güncellenir: Sonraki yönlendirici A, atlama sayısı 5 olur.

**D Yönlendirme Tablosu (Yeni Hali):**
| Hedef Ağ | Sonraki Yönlendirici | Hedefe Atlama Sayısı |
| :--- | :---: | :---: |
| **w** | A | 2 |
| **y** | B | 2 |
| **z** | **A** | **5** |
| **x** | - (Doğrudan) | 1 |

#### Örnek 2: Detaylı Tablo Güncelleme Algoritması
Bir yönlendiricinin eski yönlendirme tablosu ile komşu **C** yönlendiricisinden gelen güncelleme mesajına göre yeni tablonun oluşturulması:

**Eski Yönlendirme Tablosu:**
| Hedef | Atlama (Mesafe) | Sonraki |
| :--- | :---: | :---: |
| **Network1** | 7 | A |
| **Network2** | 2 | C |
| **Network6** | 8 | F |
| **Network8** | 4 | E |
| **Network9** | 4 | F |

**C'den Gelen RIP Mesajı (Mesafeler):**
- Network2: 4 hop
- Network3: 8 hop
- Network6: 4 hop
- Network8: 3 hop
- Network9: 5 hop

**C'den Gelen Mesafelerin 1 Artırılması (C Üzerinden Yeni Rotalar):**
- Network2: 5 hop (C üzerinden)
- Network3: 9 hop (C üzerinden)
- Network6: 5 hop (C üzerinden)
- Network8: 4 hop (C üzerinden)
- Network9: 6 hop (C üzerinden)

**Karşılaştırma ve Karar Tablosu:**
- **Network1:** C'den bilgi gelmedi. Eski durum (7, A) aynen korunur.
- **Network2:** Eski sonraki yönlendirici de C idi. **Aynı komşudan gelen yönlendirme bilgileri kötüleşse bile güncellenir** (çünkü o yöndeki hattın/koşulların değiştiğini gösterir). Tablo güncellenir: Atlama 5, Sonraki C.
- **Network3:** Eski tabloda yoktu. **Yeni ağ bilgisi** olduğu için doğrudan tabloya eklenir: Atlama 9, Sonraki C.
- **Network6:** Eski yol F üzerinden 8 hop idi. Yeni yol C üzerinden 5 hop. **Daha kısa yol** olduğu için güncellenir: Atlama 5, Sonraki C.
- **Network8:** Eski yol E üzerinden 4 hop idi. Yeni yol C üzerinden 4 hop. Mesafe aynı olduğu için değişiklik yapılmaz.
- **Network9:** Eski yol F üzerinden 4 hop idi. Yeni yol C üzerinden 6 hop. Daha uzun yol olduğu için güncellenmez.

**Yeni Yönlendirme Tablosu:**
| Hedef | Atlama (Mesafe) | Sonraki |
| :--- | :---: | :---: |
| **Network1** | 7 | A |
| **Network2** | **5** | C |
| **Network3** | **9** | **C** |
| **Network6** | **5** | **C** |
| **Network8** | 4 | E |
| **Network9** | 4 | F |

---

### 13.5 RIPv2 (Routing Information Protocol Sürüm 2)

RIPv1'in sınırlamalarını gidermek ve modern ağ ihtiyaçlarını karşılamak üzere geliştirilmiştir.

#### RIPv1 vs RIPv2 Karşılaştırma Tablosu:

| Parametre | RIPv1 | RIPv2 |
| :--- | :--- | :--- |
| **RFC Standardı** | RFC 1058 | RFC 1721, 1722, 2453 |
| **Yönlendirme Tipi** | **Classful** (Sınıflı) | **Classless** (Sınıfsız) |
| **Güncelleme Yöntemi** | **Broadcast** (`255.255.255.255`) | **Multicast** (`224.0.0.9`) |
| **Subnet Mask Gönderimi** | Göndermez | **Gönderir** |
| **VLSM & CIDR** | Desteklemez | **Destekler** |
| **Kimlik Doğrulama (Auth)** | Desteklemez | **Destekler** (Clear Text / MD5) |
| **Dağınık Ağ Desteği** | Desteklemez | **Destekler** |

#### RIP-v1 ve RIP-v2 Mesaj Formatları:

##### RIP-v1 Mesaj Yapısı (20 Byte Rota Giriş Başına):
- **Command (8 bit):** Mesaj tipini belirtir (1: Request/İstek, 2: Response/Yanıt).
- **Version (8 bit):** Protokol sürümü (RIPv1 için 1'dir).
- **Reserved (16 bit):** Boş alan (Tamamen 0'dır).
- **Address Family Identifier - AFI (16 bit):** Kullanılan adres ailesi (IP için 2'dir).
- **All 0s (16 bit):** Kullanılmayan alan.
- **Network Address (32 bit):** Hedef ağ adresi.
- **All 0s (32 bit):** Boş alan.
- **All 0s (32 bit):** Boş alan.
- **Distance / Metric (32 bit):** Hop sayısı (1-16 arası değer).

##### RIP-v2 Mesaj Yapısı (Alt ağ maskesi ve sonraki atlama adresi eklenmiştir):
- **Command (8 bit) & Version (8 bit) & Reserved (16 bit):** RIPv1 ile aynıdır (Version 2'dir).
- **Address Family Identifier (16 bit):** Adres ailesi.
- **Route Tag (16 bit):** Dış ağlardan gelen rotaları ayırt etmek için kullanılan etiket.
- **Network Address (32 bit):** Hedef ağ IP adresi.
- **Subnet Mask (32 bit):** Hedef ağın alt ağ maskesi.
- **Next-hop Address (32 bit):** Paketlerin iletileceği sonraki yönlendiricinin adresi. `0.0.0.0` ise paketi gönderen router'ın kendisi next-hop'tır.
- **Distance / Metric (32 bit):** Hop sayısı.

---

### 13.6 RIPv2 Gelişmiş Yapılandırma ve Komutları

#### 1. Temel RIPv2 Etkinleştirme:
RIPv2'nin sınıfsız çalışması ve ağları doğru yayması için mutlaka `version 2` ve `no auto-summary` komutları girilmelidir.
```text
RouterA(config)# router rip
RouterA(config-router)# version 2
RouterA(config-router)# no auto-summary
RouterA(config-router)# network 10.0.0.0
RouterA(config-router)# network 192.168.3.0
```

#### RIPv2 Kimlik Doğrulama (Authentication):
RIPv2 konuşan routerlar arasında güvenlik için şifreli (MD5) veya açık metin (Clear text) doğrulama yapılabilir.
```text
RouterA(config)# key chain Ozcan
RouterA(config-keychain)# key 1
RouterA(config-keychain-key)# key-string Yildiz
RouterA(config-keychain-key)# exit
RouterA(config-keychain)# exit

RouterA(config)# interface fastethernet 0/0
RouterA(config-if)# ip rip authentication key-chain Ozcan
RouterA(config-if)# ip rip authentication mode md5
```

---

## 14. EIGRP (Enhanced Interior Gateway Routing Protocol)

EIGRP, Cisco tarafından geliştirilmiş, hem distance vector hem de link-state özellikleri taşıyan **Hibrit** bir protokoldür.

### Temel Özellikleri:
- Belirli periyotlarda tüm yönlendirme tablosunu göndermek yerine sadece değişiklik olduğunda güncelleme gönderir.
- Komşularının durumunu test etmek için küçük **Hello** paketleri gönderir (224.0.0.10 multicast adresi kullanılır).
- Güvenilir paket iletimi için Cisco'ya özel **RTP (Reliable Transport Protocol)** protokolünü kullanır.
- Metric hesabında varsayılan olarak **K1 (Bandwidth - Bant Genişliği)** ve **K3 (Delay - Gecikme)** değerlerini kullanır.

### EIGRP Tablo Yapısı:
1. **Neighbor Table:** Komşu router'ların IP adresleri, Hold Time ve paket sürelerini barındırır.
2. **Topology Table:** Hedef ağa giden tüm yolları tutar. **DUAL Algoritması** ile bu tablodan en iyi ve yedek yolları seçer.
   - **Successor:** En iyi yol (FD metriği en küçük olan yol). Routing tablosuna bu yol yazılır.
   - **Feasible Successor:** Yedek yol. Reported Distance (RD) değeri, Successor'ın Feasible Distance (FD) değerinden küçük olmalıdır (Loop önleme kuralı).
3. **Routing Table:** Sadece Successor olan en iyi rotaları barındırır.

### EIGRP Yapılandırma Komutları:
```text
RouterA(config)# router eigrp 101            ; 101: Otonom Sistem (AS) numarası
RouterA(config-router)# network 17.5.1.16 0.0.0.3  ; Wildcard mask kullanılabilir
RouterA(config-router)# network 192.168.4.0
RouterA(config-router)# no auto-summary
```

#### EIGRP Load Balancing:
EIGRP eşit olmayan metrikli yollar arasında da yük dengelemesi yapabilir. Bunun için `variance [n]` komutu kullanılır:
```text
RouterA(config-router)# variance 2         ; En düşük metriğin 2 katına kadar olan yollarda yük dengeler
```

---

## 15. OSPF (Open Shortest Path First)

OSPF, Dijkstra (Shortest Path First) algoritmasını kullanan açık standartlı bir **Link-State** yönlendirme protokolüdür.

### Temel Özellikleri:
- Metrik olarak bant genişliğiyle ters orantılı olan **Cost (Maliyet)** değerini kullanır.
  $$\text{Cost} = \frac{100,000,000}{\text{Bant Genişliği (bps)}}$$
- Hızlı convergence (yakınsama) özelliğine sahiptir. periyodik güncellemeler yerine sadece değişiklik olduğunda tetiklemeli güncellemeler yollar.
- OSPF hiyerarşik alan (**Area**) mimarisiyle çalışır. Merkez alan **Area 0 (Backbone Area)** adını alır. Tüm diğer alanlar Area 0'a doğrudan bağlı olmak zorundadır. Alanlar arası yönlendirmeyi yapan cihazlara **ABR (Area Border Router)** denir.

### 15.1 OSPF Paket Çeşitleri:
- **Type 1: Hello:** Komşuları keşfetmek, komşuluk ilişkisini sürdürmek ve DR/BDR seçimi için kullanılır (10 saniyede bir gönderilir, Dead interval Hello'nun 4 katıdır).
- **Type 2: DBD (Database Description):** Link durumları hakkında özet veri tabanı bilgilerini içerir.
- **Type 3: LSR (Link State Request):** DBD ile öğrenilen bilgilerin detayını komşu router'dan istemek için kullanılır.
- **Type 4: LSU (Link State Update):** İstenen LSA (Link State Advertisement) detaylarını taşıyan güncelleme paketidir.
- **Type 5: LSAck:** Paketlerin alındığına dair onay bilgisini taşır.

### 15.2 OSPF Komşuluk Aşamaları:
1. **Down:** Hello paketlerinin henüz alınamadığı başlangıç aşaması.
2. **Init:** Komşudan Hello paketinin alındığı ancak komşunun bizi henüz listelemediği aşama.
3. **Two-Way:** Karşılıklı Hello paketlerinde birbirlerinin Router ID'lerini gördükleri ve komşuluğun kurulduğu aşama. Bu aşamada DR/BDR seçimi yapılır.
4. **Exstart:** Yönlendirme bilgisi alışverişini başlatacak Master/Slave ilişkisinin belirlendiği aşama.
5. **Exchange:** Yönlendiricilerin birbirlerine DBD (veritabanı özeti) paketleri gönderdiği aşama.
6. **Loading:** Eksik veya yeni rotaların detaylarının LSR/LSU paketleriyle istendiği ve yüklendiği aşama.
7. **Full:** Veri tabanlarının tamamen senkronize olduğu ve komşuluğun tam kurulduğu aşama.

### 15.3 DR (Designated Router) ve BDR (Backup Designated Router) Seçimi:
Çoklu erişimli (Ethernet) ağlarda yönlendirme trafiği karmaşasını azaltmak için bir DR ve yedek olarak bir BDR seçilir. Cihazlar yönlendirme güncellemelerini sadece DR'a iletir, DR da tüm ağa dağıtır.
- **Seçim Kriteri:** En yüksek **Router ID**'ye sahip olan cihaz DR, ikinci en yüksek olan BDR seçilir.
- **Router ID Nasıl Belirlenir?**
  1. `router-id` komutuyla manuel atanan adres.
  2. Router'daki en yüksek IP'ye sahip aktif **Loopback** arayüzü.
  3. Router'daki en yüksek IP'ye sahip aktif fiziksel arayüz.

### 15.4 OSPF Yapılandırma Komutları:
```text
RouterA(config)# router ospf 101                 ; 101: Process ID (Yerel olarak anlamlıdır)
RouterA(config-router)# network 17.1.1.16 0.0.0.3 area 0  ; Wildcard mask ve alan belirtilir
RouterA(config-router)# network 192.168.34.0 0.0.0.255 area 0
```
*(Doğrulama komutları: `show ip route` (OSPF rotaları **O** harfiyle listelenir), `show ip ospf neighbor`, `show ip ospf interface`).*

---

## 16. Taşıma Katmanı Protokolleri (TCP ve UDP)

Taşıma katmanı, bilgisayar ağlarında uçtan uca veri iletişimini yönetir.

### 16.1 TCP (Transmission Control Protocol)
Bağlantıya dayalı, uçtan uca güvenilir veri iletişimi sağlayan protokoldür.
- **3-Way Handshake (Bağlantı Kurulumu):** Veri gönderilmeden önce sanal bağlantı kurulur.
  1. Gönderici `SYN` gönderir.
  2. Alıcı `SYN + ACK` gönderir.
  3. Gönderici `ACK` gönderir.
- **Bağlantı Sonlandırma:** `FIN` ve `ACK` mesajları ile sanal bağlantı kapatılır.
- **Güvenilirlik Mekanizması:** ACK onay numaraları, paket kaybı durumunda zaman aşımı (timeout) sonrası yeniden gönderim (retransmission) ve paketlerin alıcıda sıraya dizilmesi (`Sequence number`) ile sağlanır.

#### TCP Segment Başlık Yapısı (20-60 bayt):
- **Source/Destination Port (16 bit):** Kaynak ve hedef portlar.
- **Sequence Number (32 bit):** Verinin bayt bazındaki sırası.
- **Acknowledgment Number (32 bit):** Alıcının beklediği bir sonraki bayt numarası.
- **HLEN (4 bit):** Başlık uzunluğu.
- **Window Size (16 bit):** Akış kontrolü için alıcının kabul edebileceği tampon veri miktarı.
- **Bayraklar (Control Bits):** `SYN` (bağlantı kurma), `ACK` (onay), `FIN` (bağlantı kapatma), `RST` (bağlantıyı sıfırlama), `PSH` (veriyi hemen ilet), `URG` (acil veri).

---

### 16.2 UDP (User Datagram Protocol)
Bağlantısız, onay mekanizması olmayan ve hız odaklı taşıma katmanı protokolüdür.
- Güvenilirlik kontrolü yapmaz (ACK yoktur, kayıp paketleri yeniden göndermez).
- Sıralama garantisi yoktur (paketler farklı sırayla gidebilir, sıralamayı üst katmanlar yapar).
- Sabit **8 bayt** başlık uzunluğuna sahiptir. Hızlı ve düşük gecikmelidir.
- Canlı yayın, online oyunlar, VoIP ve DNS gibi hızın kritik olduğu yerlerde tercih edilir.

#### UDP Segment Yapısı (8 bayt):
- **Source Port** (16 bit)
- **Destination Port** (16 bit)
- **Length** (16 bit) - datagram uzunluğu
- **Checksum** (16 bit) - hata kontrolü

---

### 14.3 TCP ve UDP Karşılaştırma Tablosu

| Özellik | TCP | UDP |
| :--- | :--- | :--- |
| **Bağlantı Durumu** | Bağlantı yönelimli (Connection-oriented) | Bağlantısız (Connectionless) |
| **Güvenilirlik** | Güvenilir (Paket teslim garantisi var) | Güvenilmez (Paket teslim garantisi yok) |
| **Hız** | Yavaş (Kontrol mekanizmalarından dolayı) | Çok Hızlı (Kontrol mekanizması yok) |
| **Sıralama** | Sıralı teslimat yapar (Sequence number) | Sırasız teslimat yapar |
| **Akış Kontrolü** | Var (Sliding window) | Yok |
| **Kullanım Alanları** | Web (HTTP), E-posta (SMTP), Dosya (FTP) | Canlı yayın, VoIP, Oyun, DNS sorguları |

---

### 14.4 Well-Known (Bilinen) Port Numaraları

#### 1. TCP Portları:
- **Port 20 / 21:** FTP (Dosya transferi - 20 veri, 21 kontrol)
- **Port 22:** SSH (Güvenli uzaktan bağlantı)
- **Port 23:** Telnet (Şifrelenmemiş uzaktan bağlantı)
- **Port 25:** SMTP (E-posta gönderimi)
- **Port 53:** DNS (Zone transfer/büyük veri transferi için)
- **Port 80:** HTTP (Standart web trafiği)
- **Port 110:** POP3 (E-posta alma)
- **Port 143:** IMAP (E-posta yönetimi)
- **Port 179:** BGP (Yönlendirme protokolü)
- **Port 443:** HTTPS (Güvenli şifreli web trafiği)
- **Port 445:** SMB (Dosya paylaşımı)

#### 2. UDP Portları:
- **Port 53:** DNS (Hızlı isim çözümleme sorguları)
- **Port 67 / 68:** DHCP (67 sunucu portu, 68 istemci portu)
- **Port 69:** TFTP (Basit dosya transferi)
- **Port 123:** NTP (Saat senkronizasyonu)
- **Port 161 / 162:** SNMP (Ağ yönetimi)
- **Port 500:** ISAKMP / IKE (VPN bağlantı kurulumu)
- **Port 514:** Syslog (Log kayıtları toplama)
- **Port 520:** RIP (Dinamik yönlendirme güncellemeleri)

---

## 17. Uygulama 12: IPv4 Statik ve Default Rota Yapılandırması

### 17.1 Bölüm 1: Ağın İncelenmesi ve Yönlendirme Analizi

#### Topoloji Detayları:
- **Toplam Ağ Sayısı:** **5 adet** ağ mevcuttur.
  - *3 adet LAN:* PC1 LAN (`172.31.1.0/25`), PC2 LAN (`172.31.0.0/24`), PC3 LAN (`172.31.1.128/26`).
  - *2 adet WAN:* R1-R2 WAN (`172.31.1.192/30`), R2-R3 WAN (`172.31.1.196/30`).
- **Doğrudan Bağlı Ağlar:**
  - **R1:** 2 ağ doğrudan bağlı (`172.31.1.0/25` ve `172.31.1.192/30`).
  - **R2:** 3 ağ doğrudan bağlı (`172.31.0.0/24`, `172.31.1.192/30`, `172.31.1.196/30`).
  - **R3:** 2 ağ doğrudan bağlı (`172.31.1.128/26` ve `172.31.1.196/30`).
- **Gerekli Statik Rota Sayısı (Uzak ağlara ulaşmak için):**
  - **R1** için: **3 adet** statik rota gerekir.
  - **R2** için: **2 adet** statik rota gerekir.
  - **R3** için: **3 adet** statik rota veya tek bir varsayılan (default) rota gerekir.
- **İlk Durum Bağlantı Testi:** Yönlendirmeler yapılandırılmadan önce PC1'den PC2 ve PC3'e ping atılamaz. Çünkü yönlendiriciler doğrudan bağlı olmadıkları ağların yollarını bilmezler ve paketleri drop ederler.

---

### 17.2 Bölüm 2: Statik ve Default Rota Çeşitleri Yapılandırması

#### 1. R1 Üzerinde Rekürsif (Next Hop) Statik Rota Yapılandırması:
- **Rekürsif Statik Yol Nedir?** Çıkış arayüzü belirtilmeden, sadece bir sonraki yönlendiricinin (next hop) IP adresinin belirtildiği statik yönlendirme türüdür.
- **Neden Çift Arama Gerektirir?** Router önce hedef ağa gitmek için sonraki yönlendiricinin IP adresini bulur, ardından o IP adresine ulaşmak için hangi kendi fiziksel çıkış interface'ini kullanacağını bulmak üzere yönlendirme tablosunu **ikinci kez** arar (recursive lookup).
- **R1 Komutları:**
```text
R1(config)# ip route 172.31.0.0 255.255.255.0 172.31.1.193
R1(config)# ip route 172.31.1.196 255.255.255.252 172.31.1.193
R1(config)# ip route 172.31.1.128 255.255.255.192 172.31.1.193
```

#### 2. R2 Üzerinde Doğrudan Bağlı (Directly Connected) Statik Rota Yapılandırması:
- **Farkı Nedir?** Next hop IP adresi yerine doğrudan router'ın kendi fiziksel çıkış arayüzünün (örn: Serial0/0/0) yazıldığı yönlendirme türüdür. Recursive lookup gerektirmez, daha hızlı yönlendirir.
- **R2 Komutları:**
```text
R2(config)# ip route 172.31.1.0 255.255.255.128 Serial0/0/0
R2(config)# ip route 172.31.1.128 255.255.255.192 Serial0/0/1
```
- **Faydalı Sorgular:**
  - Sadece doğrudan bağlı (connected) ağları görmek için: `show ip route connected`
  - Sadece statik rotaları görmek için: `show ip route static`
  - *Not:* Yönlendirme tablosunda Directly Connected statik yolların başında **S** harfi bulunur ve *"is directly connected"* yazar. Doğrudan bağlı ağların başında ise **C** harfi bulunur.

#### 3. R3 Üzerinde Varsayılan Rota (Default Route) Yapılandırması:
- **Varsayılan Rota Nedir?** Yönlendirme tablosunda hedef IP ile eşleşen hiçbir spesifik rota bulunamadığında, paketin gönderileceği son çıkış kapısıdır (`0.0.0.0/0`). Ağda sadece tek bir çıkış yolu (Stub network) olan router'lar için idealdir.
- **R3 Komutları:**
```text
R3(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/1
```
- *Yönlendirme Tablosu Gösterimi:* Tabloda varsayılan rota `S* 0.0.0.0/0` şeklinde yıldız işaretiyle (`*`) gösterilir ve en üstte *"Gateway of last resort"* olarak atanır.

#### 4. Tam Olarak Belirlenmiş (Fully Specified) Rota Hesaplamaları:
- **Tanım:** Hem çıkış arayüzünün hem de bir sonraki yönlendiricinin (next hop) IP adresinin aynı anda belirtildiği statik yönlendirmedir. Özellikle çoklu erişimli (Ethernet) ağlarda ARP karmaşasını önlemek için kullanılır.
- **R3'ten R2 LAN'a (Hesaplanan):**
  `ip route 172.31.0.0 255.255.255.0 Serial0/0/1 172.31.1.197`
- **R3'ten R1-R2 WAN'a (Hesaplanan):**
  `ip route 172.31.1.192 255.255.255.252 Serial0/0/1 172.31.1.197`
- **R3'ten R1 LAN'a (Hesaplanan):**
  `ip route 172.31.1.0 255.255.255.128 Serial0/0/1 172.31.1.197`

---

## 18. Uygulama 13: RIPv2 Yapılandırması

RIPv2, sınıfsız (classless) yönlendirme yapan ve güncellemelerini `224.0.0.9` multicast adresinden gönderen taşıma katmanında UDP 520 nolu portu kullanan dinamik yönlendirme protokolüdür.

### 18.1 Router RIPv2 Yapılandırmaları

#### R1 Üzerinde RIPv2 ve Default Route Dağıtımı:
R1 dış dünyaya bağlanan kenar yönlendirici ise, varsayılan rotasını diğer yönlendiricilere dinamik olarak RIP üzerinden dağıtabilir:
```text
R1(config)# ip route 0.0.0.0 0.0.0.0 Serial0/0/1
R1(config)# router rip
R1(config-router)# version 2
R1(config-router)# no auto-summary
R1(config-router)# network 172.31.1.0
R1(config-router)# network 172.31.1.192
R1(config-router)# passive-interface GigabitEthernet0/0    ; LAN arayüzüne boşuna update göndermeyi durdurur
R1(config-router)# default-information originate           ; Default route'u RIP ile diğerlerine duyurur
```

#### R2 Üzerinde RIPv2 Yapılandırması:
```text
R2(config)# router rip
R2(config-router)# version 2
R2(config-router)# no auto-summary
R2(config-router)# network 172.31.0.0
R2(config-router)# network 172.31.1.192
R2(config-router)# network 172.31.1.196
R2(config-router)# passive-interface GigabitEthernet0/0
```

#### R3 Üzerinde RIPv2 Yapılandırması:
```text
R3(config)# router rip
R3(config-router)# version 2
R3(config-router)# no auto-summary
R3(config-router)# network 172.31.1.128
R3(config-router)# network 172.31.1.196
R3(config-router)# passive-interface GigabitEthernet0/0
```

### 18.2 Yönlendirme Doğrulaması
Yönlendiriciler üzerinde `show ip route` komutu girildiğinde:
- RIP aracılığıyla öğrenilen rotalar satır başında **R** harfiyle listelenir.
- Komşu yönlendiriciler (R2 ve R3), R1'den gelen varsayılan rotayı `R* 0.0.0.0/0` şeklinde otomatik olarak öğrenirler.
- Tüm cihazlar yerel ağlardaki diğer tüm IP'lere ve interneti simüle eden dış Web sunucusuna başarılı şekilde ping atabilirler.
