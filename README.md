# Projekt LAN: Návrh a konfigurace firemní sítě v simulátoru Cisco Packet Tracer

Tento repozitář obsahuje kompletní model lokální sítě (LAN) vytvořený v simulátoru Cisco Packet Tracer. Cílem projektu je demonstrovat propojení switchů, správu síťových služeb a implementaci adresního plánu na základě specifického výpočtu.

## 1. Výpočet síťového segmentu (Varianta Janoušek)
Síťové parametry jsou odvozeny z ordinálních hodnot písmen příjmení velkým písmem bez diakritiky (ASCII) pomocí operace modulo 256.

* **Příjmení:** JANOUSEK
* **ASCII výpočet:** J(74) + A(65) + N(78) + O(79) + U(85) + S(83) + E(69) + K(75) = **608**
* **Výpočet X:** 608 mod 256 = **96**
* **Přidělená síť:** `192.168.96.0 / 24`
* **Maska sítě:** `255.255.255.0`
* **Výchozí brána (Gateway):** `192.168.96.1`

## 2. Hardwarová architektura a topologie
Síť je rozdělena na dva logické segmenty propojené páteřní linkou.

### Použitá zařízení:
* **Switch S1 (Cisco 2960):** Slouží pro připojení koncových stanic PC1 a PC2.
* **Switch S2 (Cisco 2960):** Slouží pro připojení serverové farmy.
* **Propojení switchů:** Použit Copper Cross-Over kabel (porty GigabitEthernet 0/1).
* **Správa:** Laptop připojený přes Console port (RS232 -> RJ45) k S1.

### Adresní plán zařízení:
| Zařízení | IP Adresa | Metoda | Role v síti |
| :--- | :--- | :--- | :--- |
| **SRV1** | 192.168.96.10 | Statická | DHCP Server, DNS Server |
| **SRV2** | 192.168.96.11 | Statická | Web (HTTP) Server |
| **PC1** | 192.168.96.20 | Statická | Klientská stanice |
| **PC2** | 192.168.96.x | DHCP | Klientská stanice (pool 100+) |

## 3. Implementace síťových služeb

### DNS (Domain Name System)
Služba DNS běží na **SRV1**. Je nakonfigurována autoritativní zóna pro doménu `janousek.cz`, která směřuje (A záznam) na IP adresu Web serveru (`192.168.96.11`).

### DHCP (Dynamic Host Configuration Protocol)
Služba DHCP běží na **SRV1**. Zajišťuje automatickou konfiguraci pro PC2:
* **Rozsah (Pool):** 192.168.96.100 - 192.168.96.254
* **Předávané info:** Mask, Default Gateway (192.168.96.1), DNS (192.168.96.10).

### HTTP (Web Server)
Na **SRV2** je aktivní služba HTTP. Soubor `index.html` byl editován a obsahuje personalizovaný nadpis `<h1>Jan Janousek</h1>`.

## 4. Ověření a testování sítě
Správnost konfigurace byla ověřena následujícími kroky na PC1:
1.  **ipconfig /all**: Ověření, že PC1 má správnou masku a zná svůj DNS server.
2.  **ping 192.168.96.11**: Test základní konektivity na Web server (ICMP protokol).
3.  **ping janousek.cz**: Test funkčnosti DNS překladu.
4.  **Web Browser**: Úspěšné načtení stránky přes doménové jméno.

## 5. Dokumentace (Snímky obrazovky)
*(Zde doplňte vlastní obrázky nahrané do repozitáře)*
- **Konzolové nastavení switchů:** ![Laptop Terminal](img/terminal.png)
- **Výpis ipconfig na PC1:** ![PC1 Config](img/ipconfig.png)
- **Ping testy:** ![Ping Test](img/ping.png)
- **Zobrazení webové stránky:** ![Web Browser](img/web.png)
