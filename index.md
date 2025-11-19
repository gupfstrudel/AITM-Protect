---
title: AITM Schutzprojekt | Cybersecurity
theme: jekyll-theme-minimal
---

# AITM Schutzprojekt | Maturaprojekt Cybersecurity

**Achtung: Ihr 2FA-Code ist möglicherweise nicht mehr sicher.**

## Der unsichtbare Angreifer

Adversary-in-The-Middle (AITM) Angriffe umgehen moderne Sicherheitsstandards in Echtzeit. Diese Seite erklärt, wie sie funktionieren und wie Sie sich verteidigen können.

## 1. Was ist ein AITM-Angriff?

Ein Adversary-in-The-Middle-Angriff (oft auch "Man-in-the-Middle" genannt) ist eine hochentwickelte Form des Phishings.

Anders als bei klassischem Phishing, wo Daten auf einer statischen Fake-Seite gesammelt werden, klinkt sich der Angreifer live in die Kommunikation zwischen Ihnen und dem Dienst (z.B. Microsoft 365 oder Google) ein.

### Der Unterschied

| Merkmal | Klassisches Phishing | AITM Angriff (Phishing 2.0) |
|---------|---------------------|-----------------------------|
| **Ziel** | Passwort stehlen | Aktive Sitzung (Cookie) stehlen |
| **Ablauf** | Statische Fake-Seite | Live-Proxy (Echtzeit-Weiterleitung) |
| **2FA-Schutz** | Blockiert den Angreifer oft | Wird live umgangen |
| **Gefahr** | Mittel | **Extrem Hoch** |

## 2. Wie funktioniert es? (Der Ablauf einfach erklärt)

Stellen Sie sich vor, der Angreifer sitzt wie ein Postbote in der Mitte zwischen Ihnen und der echten Webseite. Er fängt Ihre Briefe ab, liest sie, schreibt sich wichtige Informationen auf und schickt sie dann weiter – so schnell, dass Sie nichts davon merken.

**So läuft ein AITM-Angriff ab:**

### Schritt 1: Der falsche Köder
Der Angreifer schickt Ihnen eine E-Mail mit einem Link zu einer Login-Seite. Diese Seite sieht absolut echt aus und verhält sich auch so, aber in Wirklichkeit gehört sie dem Angreifer. Sie ist nur ein "Fenster" zu seinem Server.

### Schritt 2: Die Live-Weiterleitung Ihrer Daten
Sie geben Ihre Zugangsdaten (Benutzername und Passwort) auf dieser gefälschten Seite ein. Der Angreifer fängt diese Daten sofort ab und leitet sie in Echtzeit an die echte Webseite weiter.

### Schritt 3: Die Umgehung des 2FA
Die echte Webseite sendet als Antwort eine Aufforderung für Ihren 2FA-Code (z.B. per SMS oder Authenticator-App). Auch diese Anforderung wird vom Angreifer abgefangen und Ihnen über seine gefälschte Seite angezeigt. Sie geben den 2FA-Code ein, und der Angreifer leitet auch diesen sofort an die echte Webseite weiter.

### Schritt 4: Der Diebstahl Ihrer digitalen "Eintrittskarte"
Nach erfolgreichem Login sendet die echte Webseite ein "Session Cookie" zurück. Dieses Cookie ist wie eine digitale Eintrittskarte, die beweist, dass Sie eingeloggt sind. Der Angreifer fängt auch dieses Cookie ab und speichert es.

**Das Ergebnis:** Der Angreifer besitzt nun Ihr Session Cookie. Er kann sich mit dieser "Eintrittskarte" auf der echten Webseite als Sie ausgeben, ohne Ihr Passwort oder Ihren 2FA-Code erneut eingeben zu müssen. Für Sie sieht es aus, als hätten Sie sich ganz normal eingeloggt.

## 3. Warum 2FA nicht mehr ausreicht

Viele Nutzer fühlen sich sicher, weil sie einen Code per SMS oder App bekommen. Das ist ein Trugschluss.

- **Session-Übernahme:** Traditionelles 2FA schützt nur den Login-Prozess, nicht die Sitzung danach.
- **Automatisierung:** Angreifer nutzen Tools (wie Evilginx), die diesen Prozess vollautomatisch in Millisekunden abwickeln.
- **Massenware:** Solche Angriffs-Kits sind billig verfügbar. Es braucht keinen Profi-Hacker mehr.
- **Statistik:** Erfolgreiche AITM-Kampagnen haben eine Erfolgsquote von bis zu 90%, selbst bei aktivierter Multi-Faktor-Authentifizierung.

## 4. Wie schützen wir uns?

Technologie muss Phishing-resistent werden. Hier sind die einzigen wirksamen Gegenmaßnahmen:

### 1. Passkeys und FIDO2 (Empfohlen)
Die sicherste Methode. Passkeys basieren auf Kryptographie. Der Login funktioniert technisch nur, wenn die URL der echten Webseite zu 100% stimmt.

*Beispiel: FaceID Login bei Google, TouchID bei Microsoft.*

### 2. Hardware Security Keys
Geräte wie YubiKeys erfordern eine physische Berührung. Ein Hacker in einem anderen Land kann den Stick an Ihrem Schlüsselbund nicht drücken.

### 3. Verhaltensbewusstsein
- Prüfen Sie die URL genau (ist es microsoft-login-secure.com statt microsoft.com?).
- Seien Sie misstrauisch bei Dringlichkeit ("Ihr Konto wird sofort gesperrt!").

## Sicherheits-Checkliste

Kopieren Sie diese Liste und prüfen Sie Ihre Accounts:

- [ ] **Passkeys aktivieren:** Wo immer möglich (Google, Apple, Amazon, etc.).
- [ ] **SMS-2FA deaktivieren:** Dies ist die unsicherste Methode.
- [ ] **Authenticator-App nutzen:** Steigen Sie auf Microsoft Authenticator oder Google Authenticator um.
- [ ] **Sitzungen prüfen:** Schauen Sie in den Einstellungen unter "Aktive Geräte" nach Unbekannten.

<footer>
<small>@ 2025 Maturaprojekt Cybersecurity. Nur für Bildungszwecke.</small>
</footer>
