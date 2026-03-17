Voici ta version **complète, structurée en Markdown (.md)** avec :

* ✅ Explications détaillées
* ✅ Commandes Docker complètes
* ✅ Architecture claire
* ✅ Annexe avec intégration Active Directory

---

```md
# 🚀 De la théorie à la pratique — POC Auth Microservices Sécurisé

## 📌 Contexte

Il y a quelques semaines, je partageais mon expérience sur la sécurisation d’une architecture microservices avec **Keycloak**, un **Auth Service** et une **API Gateway**.

Le retour a été fort. Une question s’est imposée :

> 💡 Et si je construisais un POC complet, reproductible, et public ?

👉 C’est désormais fait.

---

## 🎯 Objectif du POC

Construire une **architecture d’authentification moderne**, :

- ✔️ Microservices
- ✔️ Sécurisée
- ✔️ Dockerisée
- ✔️ Lançable en 1 commande
- ✔️ Prête pour production (avec améliorations)

---

## 🧱 Architecture globale

```

Client (Mobile/Web)
↓
API Gateway
↓
Auth Service
↓
Keycloak
↓
Redis (OTP)
↓
Microservices

```

---

## 🔐 Fonctionnalités implémentées

### 📲 Inscription (4 étapes)

1. Saisie du numéro
2. OTP par SMS (mock)
3. Création profil
4. Définition mot de passe

---

### 🔑 Authentification

- Login
- JWT (Access Token)
- Refresh Token
- Logout

---

### 🔁 Gestion avancée

- Mot de passe oublié
- Reset password
- OTP sécurisé avec TTL (Redis)

---

## ⚙️ Stack technique

| Composant | Technologie |
|----------|------------|
| Backend | Spring Boot 3 |
| Cloud | Spring Cloud |
| IAM | Keycloak 22 |
| Cache OTP | Redis |
| Gateway | Spring Cloud Gateway |
| Discovery | Eureka |
| Orchestration | Docker Compose |

---

## 🔐 Rôle clé de la Gateway

La Gateway agit comme :

- 🔒 Filtre de sécurité
- 🔑 Validateur JWT
- 📦 Injecteur de headers utilisateur
- 🚫 Bloqueur des accès non autorisés

---

## 📂 Structure du projet

```

POC_Auth_Microservice/
├── gateway/
├── auth-service/
├── user-service/
├── eureka-server/
├── docker-compose.yml
├── .env

````

---

## 🐳 Lancement du projet

### 🔧 Prérequis

- Docker
- Docker Compose

---

### 🚀 Commandes

```bash
# Cloner le projet
git clone https://lnkd.in/djf4Z62F

# Accéder au dossier
cd POC_Auth_Microservice

# Lancer tous les services
docker compose --env-file .env up --build -d
````

---

### 🔍 Vérification

```bash
# Voir les containers
docker ps

# Logs
docker compose logs -f
```

---

### 🛑 Stopper

```bash
docker compose down
```

---

## 🐳 Docker Compose (extrait simplifié)

```yaml
version: "3.8"

services:

  keycloak:
    image: quay.io/keycloak/keycloak:22.0
    command: start-dev
    environment:
      KEYCLOAK_ADMIN: admin
      KEYCLOAK_ADMIN_PASSWORD: admin
    ports:
      - "8080:8080"

  redis:
    image: redis:7
    ports:
      - "6379:6379"

  eureka:
    build: ./eureka-server
    ports:
      - "8761:8761"

  gateway:
    build: ./gateway
    ports:
      - "8081:8081"
    depends_on:
      - keycloak
      - eureka

  auth-service:
    build: ./auth-service
    depends_on:
      - redis
      - keycloak
```

---

## 🧪 Ce que démontre ce POC

### ✅ Architecture propre

* Séparation claire des responsabilités

---

### ✅ Mobile-first

* Authentification par numéro + OTP

---

### ✅ Environnements maîtrisés

* local / docker / prod sans duplication

---

### ✅ Observabilité

* Logs JSON prêts pour ELK

---

### ✅ Rapidité

* Setup en quelques minutes

---

## ⚠️ Limites actuelles

* ❌ SMS provider = mock
* ❌ H2 (dev only)
* ❌ ELK non intégré
* ❌ Pas de rate limiting
* ❌ Pas de circuit breaker

---

## 🚀 Améliorations recommandées

### 🔐 Sécurité

* Rate limiting
* MFA
* Protection brute force

---

### 📊 Observabilité

* ELK Stack
* Prometheus / Grafana

---

### 🗄️ Data

* PostgreSQL
* Chiffrement

---

### ☁️ Scalabilité

* Kubernetes / OpenShift

---

## 🧠 Leçons clés

1. Centraliser la sécurité (Gateway)
2. Externaliser l’identité (Keycloak)
3. Simplifier les microservices
4. Penser mobile-first
5. Anticiper la scalabilité

---

# 📎 ANNEXE — Intégration Active Directory

## 🎯 Objectif

Utiliser un annuaire existant pour :

* Authentifier les utilisateurs
* Éviter duplication
* Centraliser la gestion

---

## 🧱 Architecture avec AD

```
Client
  ↓
Gateway
  ↓
Keycloak
  ↓
Active Directory (LDAP)
```

---

## 🐳 Ajout LDAP (simulation AD)

### Docker Compose

```yaml
ldap:
  image: osixia/openldap:1.5.0
  environment:
    LDAP_ORGANISATION: "Company"
    LDAP_DOMAIN: "company.local"
    LDAP_ADMIN_PASSWORD: admin
  ports:
    - "389:389"
```

---

## ⚙️ Configuration Keycloak

1. Aller dans :

   * User Federation
2. Ajouter LDAP

### Paramètres :

```
Connection URL: ldap://ldap:389
Bind DN: cn=admin,dc=company,dc=local
Users DN: ou=users,dc=company,dc=local
```

---

## 🔄 Flux avec AD

1. Login utilisateur
2. Keycloak interroge LDAP
3. Authentification validée
4. JWT généré
5. Accès aux microservices

---

## 🔐 Bonnes pratiques AD

* Utiliser LDAPS (sécurisé)
* Mettre en cache dans Keycloak
* Mapper groupes → rôles
* Activer audit logs

---

## 🔥 Conclusion

Ce POC devient :

✔️ Enterprise-ready
✔️ Compatible SI existant
✔️ Intégrable avec AD / Cloud
✔️ Base pour IAM global

---

## 💬 Feedback

Testez, adaptez, améliorez.

Les discussions sont ouvertes 🚀

```


---

# 🚀 𝐏𝐎𝐂 𝐀𝐮𝐭𝐡 𝐌𝐢𝐜𝐫𝐨𝐬𝐞𝐫𝐯𝐢𝐜𝐞 + 𝐀𝐜𝐭𝐢𝐯𝐞 𝐃𝐢𝐫𝐞𝐜𝐭𝐨𝐫𝐲

## 🔎 1. Pourquoi intégrer Active Directory ?

Dans un SI réel (énergie, banque, administration), **les utilisateurs existent déjà dans un annuaire central**.

👉 Cet annuaire est généralement :

* Active Directory (on-premise)
* ou Azure Active Directory (cloud)

---

### 🎯 Objectif

> Éviter de recréer les utilisateurs dans Keycloak
> 👉 et **s’appuyer sur l’annuaire existant comme source de vérité**

---

## 🧱 2. Architecture cible (AD + Microservices)

### 🔗 Principe global

```
[ User / Mobile / Web ]
            ↓
      API Gateway
            ↓
        Keycloak
            ↓
     Active Directory
            ↓
     Microservices sécurisés
```

---

## 🔐 3. Rôle des composants

| Composant            | Rôle                         |
| -------------------- | ---------------------------- |
| **Active Directory** | Source des utilisateurs      |
| **Keycloak**         | Broker IAM (OAuth2 / JWT)    |
| **API Gateway**      | Sécurité et contrôle d’accès |
| **Auth Service**     | OTP / logique métier         |
| **Microservices**    | Business                     |

---

## 🔄 4. Modes d’intégration avec AD

### 🥇 Option 1 (RECOMMANDÉE) : LDAP Federation

👉 Keycloak se connecte directement à AD via LDAP

### Fonctionnement :

* L’utilisateur existe dans AD
* Keycloak interroge AD pour authentifier
* Les rôles peuvent être mappés

---

### 🔧 Configuration Keycloak

Dans Keycloak :

* User Federation → LDAP
* Paramètres :

  * URL : `ldap://ad.company.local:389`
  * Bind DN : `CN=admin,DC=company,DC=local`
  * Users DN : `OU=Users,DC=company,DC=local`

---

### 🔐 Avantages

✅ Pas de duplication utilisateurs
✅ Auth centralisée
✅ Compatible SSO entreprise
✅ Gestion RH synchronisée

---

### ⚠️ Limites

❌ Dépendance réseau AD
❌ Latence possible
❌ Gestion OTP externe nécessaire

---

## 🔁 5. Option 2 : Identity Brokering (SSO AD)

👉 Keycloak délègue l’authentification à AD (via SAML / OIDC)

---

### Flux :

1. User → Keycloak
2. Redirection → AD
3. Auth AD
4. Retour → Keycloak
5. Génération JWT

---

### Cas d’usage

* SSO corporate (intranet)
* Portails internes
* Applications métier

---

## 📱 6. Gestion OTP + AD (cas réel Afrique / Mobile-first)

👉 Problème :
AD ne gère pas les OTP SMS

---

### 💡 Solution hybride

| Fonction            | Système              |
| ------------------- | -------------------- |
| Auth login/password | AD                   |
| OTP SMS             | Auth Service + Redis |
| Tokens              | Keycloak             |

---

### 🔄 Flow combiné

1. Saisie numéro
2. OTP via Auth Service
3. Validation OTP
4. Auth via AD
5. Token Keycloak

---

## 🔐 7. Gestion des rôles (RBAC)

### Mapping AD → Keycloak

Exemple :

| Groupe AD      | Rôle Keycloak |
| -------------- | ------------- |
| `ENERGY_ADMIN` | `ROLE_ADMIN`  |
| `BILLING_USER` | `ROLE_USER`   |

---

👉 Permet :

* Gestion centralisée par IT
* Contrôle d’accès automatique

---

## 🌐 8. API Gateway avec AD

La Gateway ne parle **jamais directement à AD**.

👉 Elle valide uniquement :

* JWT Keycloak
* Claims (roles, scopes)

---

### Exemple de claims :

```json id="2k9d1"
{
  "sub": "user123",
  "email": "user@company.com",
  "roles": ["ADMIN"],
  "department": "ENERGY"
}
```

---

## 🧪 9. Extension du POC Docker

### 🐳 Ajouts nécessaires

* Container LDAP (simulation AD) :

  * OpenLDAP ou Samba AD

---

### Exemple docker-compose (simplifié)

```yaml id="1k8dj"
ldap:
  image: osixia/openldap
  environment:
    LDAP_ORGANISATION: "Company"
    LDAP_DOMAIN: "company.local"
    LDAP_ADMIN_PASSWORD: "admin"
  ports:
    - "389:389"
```

---

## ⚙️ 10. Bonnes pratiques entreprise

### 🔐 Sécurité

* LDAPS (LDAP sécurisé)
* MFA obligatoire
* Audit des connexions

---

### ⚡ Performance

* Cache utilisateurs dans Keycloak
* Timeout LDAP
* Retry intelligent

---

### 🧠 Gouvernance

* AD = source de vérité
* Keycloak = couche d’abstraction IAM
* Gateway = enforcement

---

## 🏗️ 11. Architecture cible SI (niveau avancé)

Dans ton contexte (120+ apps, énergie, IoT, ERP) :

```
              +------------------+
              |   Active Dir     |
              +------------------+
                       ↓
              +------------------+
              |   Keycloak IAM   |
              +------------------+
                       ↓
+---------+   +------------------+   +----------------+
| Mobile  |→  |   API Gateway    |→  | Microservices  |
+---------+   +------------------+   +----------------+
                       ↓
                 Kafka / ESB / IoT Hub
```

---

## 🚀 12. Cas d’usage réels

* 👷 Employés internes (AD)
* 📱 Clients mobiles (OTP)
* 🏢 Partenaires (fédération identité)
* ⚡ Agents terrain (hybride)

---

## 🧠 13. Leçons clés

👉 Une architecture moderne :

1. **Ne remplace pas AD → elle l’exploite**
2. **Ajoute une couche IAM (Keycloak)**
3. **Expose via Gateway**
4. **Ajoute OTP pour mobile**

---

## 🔥 Conclusion

👉 Cette version transforme ton POC en :

✔️ Architecture **enterprise-ready**
✔️ Compatible **SI legacy + cloud**
✔️ Prête pour **SSO global + IAM centralisé**

---


