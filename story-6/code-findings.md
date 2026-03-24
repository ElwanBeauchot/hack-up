# Code Security Findings

## 1. Exposition d’une base Firebase

**Fichier :** non identifié (recherche globale)

**Extrait :**

```text
https://application-client-nickel.firebaseio.com
```

**Analyse :**
L’application contient une URL vers une base Firebase Realtime Database. Ce type de backend est souvent mal configuré en production.

**Pourquoi c’est problématique :**
Si les règles de sécurité Firebase ne sont pas correctement définies, n’importe qui peut :

* lire les données de la base
* modifier ou injecter des données
* accéder à des informations sensibles (utilisateurs, tokens, etc.)

**Impact potentiel :**
Compromission complète des données backend.

**Recommandation :**
Vérifier et restreindre immédiatement les règles Firebase (lecture/écriture), et imposer une authentification stricte.

---

## 2. Gestion des mots de passe côté client

**Fichier :** `com/oblador/keychain/KeychainModule.smali`

**Extraits :**

```smali
.field public final passwordBytes:[B
```

```smali
iget-object v5, v2, Lcom/oblador/keychain/cipherStorage/CipherStorage$CipherResult;->password:Ljava/lang/Object;
```

**Analyse :**
Le code montre que des mots de passe sont manipulés et potentiellement stockés localement sous forme de données brutes.

**Pourquoi c’est problématique :**
Même si une librairie sécurisée est utilisée, une mauvaise implémentation peut exposer les credentials via reverse engineering.

**Impact potentiel :**
Vol de mots de passe ou compromission de comptes utilisateurs.

**Recommandation :**
Utiliser exclusivement le Android Keystore et éviter tout stockage local de mots de passe en clair.

---

## 3. Validation des identifiants côté client

**Fichier :** `com/oblador/keychain/KeychainModule.smali`

**Extrait :**

```smali
const-string p2, "you passed empty or null username/password"
```

**Analyse :**
La présence de ce message indique qu’une partie de la validation des identifiants est effectuée côté client.

**Pourquoi c’est problématique :**
Le code client peut être modifié facilement. Un attaquant peut contourner ces vérifications.

**Impact potentiel :**
Contournement de l’authentification et accès non autorisé.

**Recommandation :**
Déplacer toutes les validations critiques côté serveur.

---

## 4. Utilisation potentielle de HTTP non sécurisé

**Fichier :** `com/pushwoosh/repository/RegistrationPrefs.smali`

**Extrait :**

```smali
const-string v0, "http://"
```

**Analyse :**
La présence de chaînes HTTP suggère que certaines communications pourraient ne pas être chiffrées.

**Pourquoi c’est problématique :**
Les communications HTTP peuvent être interceptées et modifiées.

**Impact potentiel :**
Attaques de type Man-in-the-Middle (MITM), fuite de données.

**Recommandation :**
Forcer l’utilisation de HTTPS et bloquer HTTP via une configuration de sécurité réseau.

---

## 5. Gestion des tokens

**Fichier :** plusieurs fichiers (`grep "token"`)

**Analyse :**
Le code contient plusieurs références à des tokens, probablement utilisés pour l’authentification ou les API.

**Pourquoi c’est problématique :**
Si ces tokens sont mal stockés ou exposés, ils peuvent être réutilisés par un attaquant.

**Impact potentiel :**
Accès non autorisé aux services backend.

**Recommandation :**
Stocker les tokens de manière sécurisée et implémenter une expiration et rotation.

---

## 6. Présence possible de secrets dans le code

**Fichier :** non identifié (`grep "secret"`)

**Analyse :**
Une occurrence du mot "secret" a été trouvée dans le code.

**Pourquoi c’est problématique :**
Les secrets ne doivent jamais être embarqués dans une application cliente.

**Impact potentiel :**
Compromission d’API ou de services tiers.

**Recommandation :**
Externaliser tous les secrets côté serveur.

---

# Conclusion

L’application présente plusieurs points d’attention en matière de sécurité.

Le risque principal concerne l’utilisation de Firebase, qui peut exposer directement les données si mal configuré.
D’autres points comme la gestion des mots de passe, des tokens et la logique d’authentification côté client augmentent la surface d’attaque.

---

# Recommandations globales

* Auditer en priorité la configuration Firebase
* Ne jamais faire confiance au code client
* Éviter de stocker des données sensibles localement
* Utiliser des communications sécurisées (HTTPS uniquement)
* Externaliser toute logique critique et les secrets côté serveur

---