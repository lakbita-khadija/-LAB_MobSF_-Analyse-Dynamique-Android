# Lab MobSF - Analyse Dynamique Android

## Objectif
Analyser l'application vulnérable DIVA (Damn Insecure and Vulnerable App)
avec MobSF en mode dynamique pour identifier les vulnérabilités mobiles.

## Environnement technique
- **OS** : Windows 11
- **MobSF** : v4.5.0 (Python 3.12.8)
- **Émulateur** : Android Studio - Pixel5 - Android 11 (API 30)
- **Image système** : Google APIs x86_64 (sans Google Play)
- **Application** : DIVA APK (Damn Insecure and Vulnerable App)

## Installation et configuration

### Prérequis
- Android Studio + SDK
- Python 3.12
- MobSF v4.5.0
- Visual C++ Build Tools

### Lancement de l'émulateur
```bash
emulator.exe -avd Pixel_5 -writable-system -no-snapshot
adb root
adb remount
```
<img width="892" height="162" alt="image" src="https://github.com/user-attachments/assets/8e957487-96b6-45c1-a481-a3a58b866be5" />

### Lancement de MobSF
```bash
cd Mobile-Security-Framework-MobSF
run.bat
```
<img width="999" height="689" alt="Capture d&#39;écran 2026-05-02 164320" src="https://github.com/user-attachments/assets/f0d15943-cdf8-4fc0-9ba2-500a94828a60" />

Accès : http://127.0.0.1:8000
Credentials : mobsf/mobsf

<img width="1912" height="654" alt="Capture d&#39;écran 2026-05-02 162514" src="https://github.com/user-attachments/assets/a3f01bf5-485f-4655-868e-21f686004b54" />

## Analyse Statique
MobSF analyse automatiquement :
- Permissions dangereuses
- Code source décompilé
- Certificats
- Trackers
- Vulnérabilités connues

<img width="1915" height="933" alt="Capture d&#39;écran 2026-05-02 163141" src="https://github.com/user-attachments/assets/ba1f8697-1674-4922-8eb7-b89eb38c1524" />


## Analyse Dynamique
MobSF injecte automatiquement :
- **Frida** pour l'instrumentation runtime
- **Proxy HTTPS** sur le port 1337
- **File Monitor** pour les accès fichiers
  
<img width="1910" height="714" alt="Capture d&#39;écran 2026-05-02 163149" src="https://github.com/user-attachments/assets/c475881e-c6bf-4689-8833-b08afb644f62" />
<img width="1911" height="865" alt="Capture d&#39;écran 2026-05-02 163207" src="https://github.com/user-attachments/assets/7d50135e-efee-4d2a-8c9e-260d3cb81564" />

<img width="996" height="246" alt="Capture d&#39;écran 2026-05-02 164515" src="https://github.com/user-attachments/assets/34a971ec-a38e-4080-9670-bb309ba7dc83" />

<img width="1913" height="944" alt="Capture d&#39;écran 2026-05-02 164533" src="https://github.com/user-attachments/assets/c7bb4757-e253-4c90-928c-9948a8d3aa09" />

<img width="484" height="972" alt="Capture d&#39;écran 2026-05-02 164804" src="https://github.com/user-attachments/assets/97a5ff90-2eae-47f9-8dc2-3991c8b101fd" />


## Vulnérabilités découvertes

### 1. Hardcoded Credentials (Critique)
- **Challenge** : DIVA - Hardcoding Issues Part 1
- **Description** : Credentials sensibles codés en dur dans le code source
- **Données exposées** :
  - API Key : `123secretapikey123`
  - Username : `diva`
  - Password : `p@ssword`
- **Impact** : Extraction possible par décompilation de l'APK
- **Remediation** : Utiliser des variables d'environnement ou un vault sécurisé

<img width="445" height="931" alt="Capture d&#39;écran 2026-05-02 164619" src="https://github.com/user-attachments/assets/139ddbfa-f740-4389-9b30-1d2ada20d415" />

### 2. SQL Injection (Critique)
- **Challenge** : DIVA - Input Validation Issues Part 1
- **Description** : Absence de validation des entrées utilisateur
- **Payload** : `' OR '1'='1`
- **Données extraites** :

| Username | Password | Credit Card |
|----------|----------|-------------|
| admin | passwd123 | 1234567812345678 |
| diva | p@ssword | 1111222233334444 |
| john | password123 | 5555666677778888 |

- **Impact** : Extraction complète de la base de données
- **Remediation** : Utiliser des requêtes paramétrées (PreparedStatement)

<img width="446" height="965" alt="Capture d&#39;écran 2026-05-02 165937" src="https://github.com/user-attachments/assets/571258ca-c633-403a-bc9c-6ef907f835a2" />

### 3. Insecure Logging (Élevée)
- **Challenge** : DIVA - Insecure Logging
- **Description** : L'application enregistre des informations sensibles 
  dans les logs système Android (Logcat)
<img width="1895" height="950" alt="Capture d&#39;écran 2026-05-02 165426" src="https://github.com/user-attachments/assets/359ba86e-bb99-4b39-b985-599d87b49f2b" />

