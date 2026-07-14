# Bilgisayar Ağları - Çalışma Planı

Bu çalışma planı, laboratuvar uygulamaları sırasına göre yapılandırılmış sıralı çalışma adımlarını içerir.

## 📅 Konu ve Çalışma Akışı

### 1. Bölüm: Uygulama 9 - VLAN (Sanal Yerel Ağlar) Temelleri ve Konfigürasyonu
- **Sıra:** 1
- **Materyal:** `ders.md` - Bölüm 2, 3, 4, 5, 6
- **İçerik:** VLAN mantığı, broadcast domain ayrımı, statik/dinamik VLAN farkları, Cisco 1900 ve 2950 switchlerde VLAN oluşturma ve port atama komutları, trunking mantığı, ISL vs IEEE 802.1Q encapsulation, switchler arası trunk yapılandırma, Router-on-a-Stick ile Inter-VLAN Routing (sanal alt arayüz) yapılandırması ve Laboratuvar config analizi.
- **Hedef:** Switch ve Router üzerinde temel VLAN, trunk ve Inter-VLAN yönlendirme konfigürasyonlarını kavramak.

### 2. Bölüm: Uygulama 10 - DHCP (Dynamic Host Configuration Protocol) Yapılandırması
- **Sıra:** 2
- **Materyal:** `ders.md` - Bölüm 7
- **İçerik:** DHCP DORA süreci, RARP ve BOOTP farkları, DHCP sunucu ve istemciler aynı ağda vs farklı ağda, DHCP Relay (IP Helper-Address) yapılandırması, Cisco Router DHCP sunucu yapılandırması (Pool, Excluded-Address, Lease) ve statik IP rezervasyonu.
- **Hedef:** Ağ genelinde IP dağıtımını otomatikleştirmeyi ve DHCP Relay mantığını kavramak.

### 3. Bölüm: Uygulama 11 - DHCP, NAT (Network Address Translation) Yapılandırması ve IP Datagram Formatı
- **Sıra:** 3
- **Materyal:** `ders.md` - Bölüm 9 ve 10
- **İçerik:** NAT çalışma mantığı (apartman benzetmesi), RFC 1918 Özel IP adres aralıkları, Statik NAT, Dinamik NAT ve PAT (NAT Overload) farkları, NAT terminolojisi (Inside Local, Inside Global, Outside Local, Outside Global), Cisco CLI NAT yapılandırmaları, doğrulama ve hata giderme komutları, IP Datagram başlık alanları ve boyutları (Flags, TTL, Protocol vb.).
- **Hedef:** Ağ geçidinde IP adres dönüşümünü (NAT/PAT) ve IP paket formatını anlamak.

### 4. Bölüm: Uygulama 12 - Statik ve Default Routing Temelleri
- **Sıra:** 4
- **Materyal:** `ders.md` - Bölüm 11, 12, 17
- **İçerik:** Yönlendirme (Routing) mantığı, Yönlendirici (Router) görevleri (Core vs Edge), Yol belirleme (Path Determination), En Uzun Eşleşme (Longest Prefix Match) ilkesi, Statik Yönlendirme türleri (Recursive, Directly Connected, Fully Specified), Administrative Distance (AD), Cisco Router statik ve varsayılan (default) route CLI yapılandırma komutları.
- **Hedef:** Statik yönlendirme türlerini kavramak, Longest Prefix Match ile yol belirleme hesaplamaları yapmak.

### 5. Bölüm: Uygulama 13 - RIP (Routing Information Protocol) Yönlendirme
- **Sıra:** 5
- **Materyal:** `ders.md` - Bölüm 13, 18
- **İçerik:** Otonom Sistem (OS) kavramı (Intra-OS ve Inter-OS), RIPv1 özellikleri, Bellman-Ford algoritması, yönlendirme tablosu güncelleme kuralları ve örnekleri, RIPv1 vs RIPv2 farkları ve mesaj formatları, RIPv2 CLI yapılandırması, passive-interface, default-information originate ve MD5 kimlik doğrulama.
- **Hedef:** Uzaklık vektör algoritması çalışma mantığını kavramak ve RIPv2'yi yapılandırmak.

### 6. Bölüm: Uygulama 14 - EIGRP ve OSPF Yönlendirme Protokolleri
- **Sıra:** 6
- **Materyal:** `ders.md` - Bölüm 14 ve 15
- **İçerik:** EIGRP hibrit yapısı, 5 paket türü ve RTP protokolü, EIGRP metrik hesabı ve tabloları, DUAL Algoritması (Successor, Feasible Successor, FD, RD) ve loop önleme, CLI EIGRP yapılandırması, auto-summary iptali ve load balancing, EIGRP-IGRP birlikte çalışması, OSPF Link-State yapısı, Dijkstra, Cost metriği, OSPF paket türleri ve komşuluk aşamaları, DR/BDR seçimi (Router ID), OSPF Area yapısı ve Single-Area OSPF CLI yapılandırması.
- **Hedef:** EIGRP ve OSPF gelişmiş dinamik yönlendirme protokollerinin çalışmasını ve yapılandırmasını yönetmek.

---

## 🔬 Uygulama ve Pratik Planı
- Her bölümün sonunda ilgili bölümün CLI komutları Packet Tracer veya laboratuvar senaryoları üzerinden pratik edilecektir.
- Konu anlatımları sonrasında senaryo bazlı kavrama soruları ile pekiştirme yapılacaktır.
- **Uygulama 12 (Statik Yönlendirme):** R1, R2 ve R3 yönlendiricilerinde recursive, directly connected ve fully specified rotaların yapılandırılması.
- **Uygulama 13 (RIPv2):** RIPv2 dinamik yönlendirmenin etkinleştirilmesi, auto-summary iptali, passive-interface ve default-information originate komutlarının pratik edilmesi.
