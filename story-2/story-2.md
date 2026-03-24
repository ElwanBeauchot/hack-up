# US 1: Téléchargement et vérification de l’APK

## Résumé MobSF (version finale)

### Scores
- **Security Score** : 49 / 100  
- **Trackers détectés** : 3 / 432  

⚠️ Score faible → présence de vulnérabilités.

---

## Vulnérabilités critiques (HIGH)

### Support d’anciennes versions Android
- **minSdk = 19 → Android 4.4.4**
- Aujourd’hui : Android 14+

**Risque :**
- systèmes non sécurisés  
- failles connues non corrigées  

---

### Trafic en clair autorisé
- **usesCleartextTraffic = true**
- Communication possible en **HTTP**

**Risque :**
- interception des données  
- attaques réseau (WiFi public)  

---

### Cryptographie faible
- Utilisation de **AES-CBC (padding PKCS5/7)**

**Risque :**
- chiffrement vulnérable à certaines attaques  

---

## Permissions sensibles

### Accès aux contacts
- **READ_CONTACTS**

**Risque :**
- accès aux données personnelles  
- possible exfiltration des contacts  

---

### Accès au stockage externe
- **WRITE_EXTERNAL_STORAGE**

**Risque :**
- données accessibles par d’autres applications  
- modification ou suppression de fichiers  

---

## Conclusion

L’application présente un niveau de sécurité insuffisant (**49/100**) avec plusieurs risques importants :

- compatibilité avec des Android obsolètes  
- utilisation de communications non sécurisées  
- présence de permissions sensibles  
- faiblesses cryptographiques  