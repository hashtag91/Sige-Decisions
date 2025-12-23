```md
# Sige-Decisions

## 📌 Présentation générale

**Sige-Decisions** est une application desktop développée en **Python (PyQt5)** permettant la **gestion des centres d’examen**, des **académies**, des **responsables (Président / Vice-présidents)** et la **génération automatique de décisions administratives au format Word (.docx) en filtrant et selectionnant le nombre de Vice-Présidents selon le nombre de salles du centre**.

L’application repose sur :
- une base de données **SQLite**
- une interface graphique PyQt5 (`.ui`)
- la manipulation de données via **Pandas**
- la génération de documents avec **python-docx**
- un système de **licence chiffrée** (Fernet)

---

## 🎯 Objectifs de l’application

- Centraliser les données des académies et centres d’examen
- Gérer les responsables (Président et Vice-présidents)
- Importer / exporter les données (Excel)
- Générer automatiquement des **décisions officielles Word**
- Sécuriser l’accès via un **système de licence à expiration**

---

## 🧱 Architecture du projet

```

Sige-Decisions/
│
├── main.py                # Point d’entrée principal de l’application
├── database.db            # Base de données SQLite (générée automatiquement)
├── main.ui                # Interface principale (Qt Designer)
├── add.ui                 # Interface ajout / modification
├── icons/                 # Icônes, GIFs, ressources graphiques
│   ├── app_icon.png
│   ├── loading2.gif
│   └── ...
├── rssrce.py              # Ressources compilées Qt (si utilisé)
└── README.md

````

---

## 🧩 Technologies utilisées

| Technologie | Rôle |
|------------|------|
| Python 3.10+ | Langage principal |
| PyQt5 | Interface graphique |
| SQLite | Base de données locale |
| Pandas / NumPy | Manipulation des données |
| python-docx | Génération de documents Word |
| cryptography (Fernet) | Chiffrement de la licence |
| PyInstaller | Packaging en `.exe` (optionnel) |

---

## ⚙️ Installation & Pré-requis

### 1️⃣ Environnement Python

```bash
python -m venv env
source env/bin/activate  # Linux/Mac
env\Scripts\activate     # Windows
````

### 2️⃣ Dépendances

```bash
pip install pyqt5 pandas numpy python-docx cryptography openpyxl
```

---

## ▶️ Lancement de l’application

```bash
python main.py
```

---

## 🔐 Système de licence

* Une licence chiffrée est générée automatiquement au premier lancement
* Le fichier est stocké dans :

  ```
  %APPDATA%/Sige/licence.bin
  ```
* Le chiffrement utilise **Fernet (clé symétrique)**
* À expiration :

  * l’interface principale est bloquée
  * un écran d’indisponibilité est affiché

⚠️ **Important :**
La clé `SECRET_KEY` est actuellement codée en dur dans `main.py`.
Pour un environnement de production, il est recommandé de la déplacer dans une variable d’environnement.

---

## 🗄️ Base de données

### Table principale : `academy`

| Champ          | Type |
| -------------- | ---- |
| AE             | TEXT |
| Centre         | TEXT |
| Responsabilité | TEXT |
| Prénom         | TEXT |
| Nom            | TEXT |
| Matricule      | TEXT |
| Service        | TEXT |
| Catégorie      | TEXT |
| Poste          | TEXT |
| Telephone      | TEXT |
| Examen         | TEXT |
| Nb salle       | REAL |

La base est créée automatiquement si inexistante.

---

## 🖥️ Fonctionnalités principales

### ✔️ Gestion des données

* Ajout manuel d’un centre
* Ajout dynamique de Vice-Présidents
* Modification d’un centre existant
* Suppression globale des données
* Recherche multi-champs instantanée

### ✔️ Import / Export

* Import Excel (`.xlsx`)
* Export Excel
* Détection des doublons

### ✔️ Génération de documents Word

* Génération automatique des décisions
* Classement par Académie → Centre
* Insertion de tableaux dynamiques
* Numérotation intelligente
* Barre de progression

---

## 🧵 Threads & performances

L’application utilise **QThread** pour :

* Remplissage progressif du tableau (`TableFillThread`)
* Import Excel (`DataUploadThread`)
* Génération Word (`GenerateWordThread`)
* Modification des données (`ModifyThread`)

➡️ Évite le gel de l’interface utilisateur.

---

## 🧠 Organisation du code

### Classes principales

* `MyApp` : fenêtre principale
* `DataUploadThread` : import Excel
* `GenerateWordThread` : génération Word
* `ModifyThread` : mise à jour DB
* `TableFillThread` : affichage progressif
* `LoadingPage` : écran de chargement
* `GlassOverlay` / `FrostedDialog` : effets visuels

---

## 🐞 Logs & erreurs

Les erreurs sont journalisées dans :

```
%APPDATA%/Sige/log.txt
```

Format :

```
date - niveau - message
```

---

## 📦 Packaging en EXE (optionnel)

```bash
PyInstaller main.py --onefile --windowed --clean --name=SigeDecision --icon "logo.ico" --add-data "main.ui;." --add-data "add.ui;." --add-data "log.txt;." --add-data "rssrce.py;." --add-data "database.db;." --add-data "icons;icons"
```

⚠️ Utiliser `resource_path()` pour accéder aux fichiers en mode EXE.

---

## 🔒 Sécurité & recommandations

* Externaliser la clé de chiffrement
* Ajouter un hash des données critiques
* Signer les documents Word (si nécessaire)
* Ajouter des tests unitaires (pytest)

---

## Critères de selection du nombre de vice president:
- 1 à 3 = 1 vice president
- 4 à 7 = 2
- 8 à 11 = 3
- 12 à 15 = 4
- 16 à 19 = 5
- 20 à 23 = 6
- 24 à 27 = 7
- 28 à 31 = 8
- (4 salles - 1 vice president)

---

## Liste de colonne:  
- "AE","Centre","Responsabilité","Prénom","Nom","Matricule","Service","Catégorie","Poste","Telephone","Examen","Nb salle"
  *NB:* Le fichier excel à charger doit avoir exactement les mêmes noms de colonnes comme la liste précedente.

Eviter au maximum des expaces vides dans l'excel.

---

## 📄 Licence du projet

Projet interne – usage administratif.
Toute redistribution ou modification doit être validée par l’administrateur du système.

---

Parfait 👍
Je vais te fournir une **documentation technique complète, classe par classe et méthode par méthode**, pensée **pour un développeur qui reprend le projet** (maintenance, évolution, debug).

👉 Le format est prêt à être intégré tel quel dans un `README.md` ou dans un dossier `/docs`.

---

# 📚 Documentation technique – Classes & Méthodes

**Projet : Sige-Decisions**

---

## 🔷 Classe `MyApp(QMainWindow)`

### 📌 Rôle

Fenêtre principale de l’application.
Elle :

* charge l’interface principale
* gère la base de données
* orchestre toutes les actions utilisateur
* pilote les threads (import, export, génération Word, etc.)

---

### `__init__(self)`

**Responsabilité :**

* Initialisation de l’application
* Chargement de la base SQLite
* Vérification / création de la licence
* Chargement de l’UI principale
* Mise en place des signaux/boutons

**Points clés :**

* Crée le dossier `%APPDATA%/Sige`
* Initialise la table `academy` si absente
* Bloque l’accès si la licence est expirée

---

### `setup(self)`

**Responsabilité :**

* Initialisation visuelle du tableau
* Chargement des académies dans la ComboBox
* Configuration de la barre de statut

---

### `search_slot(self)`

**Responsabilité :**

* Recherche dynamique dans la base de données
* Filtre sur plusieurs champs (Centre, AE, Nom, Matricule, etc.)

---

### `fillTableWidget(self, rows, columns, tableWidget)`

**Responsabilité :**

* Remplissage final du `QTableWidget`
* Méthode appelée à la fin du thread `TableFillThread`

---

### `tableFill(self, data, tableWidget)`

**Responsabilité :**

* Lance le thread de remplissage progressif
* Met à jour la barre de progression

---

### `combo_change(self)`

**Responsabilité :**

* Filtrage des données par Académie
* Rafraîchissement du tableau

---

### `upload(self)`

**Responsabilité :**

* Import d’un fichier Excel
* Lancement de `DataUploadThread`

---

### `add_slot(self, academies)`

**Responsabilité :**

* Ouverture du formulaire d’ajout
* Gestion dynamique des vice-présidents

---

### `academie_add(self)`

**Responsabilité :**

* Ajout d’une nouvelle académie dans les ComboBox

---

### `save_slot(self, dialog)`

**Responsabilité :**

* Validation des données saisies
* Insertion dans la base SQLite
* Application de la règle :

  > 1 vice-président pour 3 salles

---

### `reset_slot(self)`

**Responsabilité :**

* Suppression complète des données
* Réinitialisation de la table `academy`

---

### `generer_word(self)`

**Responsabilité :**

* Lancement de la génération Word
* Gestion des signaux de progression

---

### `export_slot(self)`

**Responsabilité :**

* Export de la base vers Excel (`.xlsx`)

---

### `modify(self)`

**Responsabilité :**

* Ouverture du formulaire de modification
* Sélection d’un centre existant

---

### `modify_data(self, widget, deleteVar)`

**Responsabilité :**

* Chargement des données du centre sélectionné
* Remplissage automatique du formulaire

---

### `modify_save(self, widget, vice_layout, deleteVar, dialog)`

**Responsabilité :**

* Lancement du thread de modification (`ModifyThread`)

---

## 🔷 Classe `DataUploadThread(QThread)`

### 📌 Rôle

Thread d’importation des données Excel.

### Signaux

* `academieList(list)`
* `concatenateDf(DataFrame)`
* `finished(bool)`

### `run(self)`

* Lecture du fichier Excel
* Fusion avec la base existante
* Insertion dans SQLite

---

## 🔷 Classe `TableFillThread(QThread)`

### 📌 Rôle

Remplissage progressif du tableau pour éviter le gel de l’UI.

### Signaux

* `progress(int)`
* `finished(rows, columns)`
* `error(columns)`

---

## 🔷 Classe `GenerateWordThread(QThread)`

### 📌 Rôle

Génération complète du document Word.

### Fonctionnalités

* Classement Académie → Centre
* Génération de tableaux dynamiques
* Calcul automatique du nombre de responsables adjoints

### Signaux

* `progress(int)`
* `request_save_path()`
* `finish(bool)`

---

## 🔷 Classe `ModifyThread(QThread)`

### 📌 Rôle

Thread de modification sécurisée des données existantes.

### `run(self)`

* Mise à jour du président
* Ajout / modification / suppression des vice-présidents
* Synchronisation avec SQLite

---

## 🔷 Classe `Vice_president(QFrame)`

### 📌 Rôle

Composant graphique représentant un vice-président.

### Fonctionnalités

* Champs dynamiques
* Bouton de suppression
* Marquage pour suppression différée

---

## 🔷 Classe `Line(QLineEdit)`

### 📌 Rôle

Champ de saisie personnalisé avec :

* style glassmorphism
* placeholder dynamique
* nom logique (mapping)

---

## 🔷 Classe `LoadingPage(QDialog)`

### 📌 Rôle

Fenêtre modale de chargement animée (GIF)

---

## 🔷 Classe `GlassOverlay(QWidget)`

**(optionnel / décoratif)**

* Floute l’arrière-plan
* Utilisé pour les écrans de chargement

---

## 🔷 Classe `FrostedDialog(QDialog)`

### 📌 Rôle

Dialog avec effet **verre dépoli (glass effect)** :

* Capture écran
* Flou dynamique
* Contenu net au premier plan

---

## 🔷 Fonctions utilitaires

### `resource_path(relative_path)`

Gère les chemins en mode script ou EXE PyInstaller.

---

### `write_licence(expiration_date)`

Crée une licence chiffrée.

---

### `read_licence()`

Vérifie la validité de la licence.

---

### `query(academie_combo)`

Retourne la liste des vice-présidents nécessaires par centre.

---

### `presidents(academie_combo)`

Retourne les présidents filtrés.

---

## ✅ Conclusion développeur

Ce projet est :

* **modulaire**
* **thread-safe**
* **orienté maintenance**
* prêt pour :

  * internationalisation
  * séparation MVC
  * refactorisation en modules

---

## ✉️ Contact développeur

> Mainteneur : **camarayacouba91@gmail.com**
> Stack : Python / PyQt5 / SQLite

---

