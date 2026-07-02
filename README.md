[README.md](https://github.com/user-attachments/files/29604257/README.md)
# Wiki — Portfolio FinOps M365 (Anthony Bouilleau)

## Vue d'ensemble

Ce repository héberge une **webapp HTML autonome** présentant un portfolio de consulting FinOps Microsoft 365. Elle est destinée à être partagée avec des partenaires potentiels (notamment Know & Decide) et à fonctionner intégralement depuis un seul fichier HTML sans backend, sans dépendances externes, sans installation.

**URL de démo :** `https://[pseudo].github.io/[nom-repo]/`

---

## Structure du repository

```
/
├── index.html          # Webapp principale (à renommer depuis finops_v6.html)
└── README.md           # Ce fichier
```

Un seul fichier suffit. Toutes les données (184 SKUs Microsoft, 27 endpoints Graph API, analyse concurrentielle) sont embarquées directement dans le HTML via des balises `<script type="application/json">`.

---

## Contenu de la webapp — 7 onglets

### 1. Expertise
La méthode de consulting en 3 actes : **Qualifier → Décider → Piloter**, et les 4 vues fondamentales de tout dashboard FinOps M365 (Consommation & coûts, Adoption workloads, Refacturation interne, Gouvernance & alertes).

### 2. Framework FinOps
18 KPIs structurés autour des 4 questions business de Know & Decide :
- **Q1** — Qu'est-ce que je possède ? (5 KPIs)
- **Q2** — Par qui c'est utilisé ? (4 KPIs)
- **Q3** — Comment c'est utilisé ? (6 KPIs)
- **Q4** — Axes d'économie et d'optimisation (4 KPIs)

Chaque KPI inclut : formule de calcul, source de données K&D, seuil d'alerte.

### 3. Cas GEODIS
Étude de cas complète — groupe logistique international, 2023-2024. 5 livrables documentés :
- Matrice de qualification des comptes inactifs (7 catégories)
- CDC Dashboard (cadrage amont)
- Work Units Dashboard v1 (oct. 2023)
- M365 Operational Dashboard v2 (nov. 2024)
- REX Power365 public (-30% dépenses, 12 000 comptes optimisés)

Panneau de notes persistantes (localStorage) pour annotations lors des RDVs.

### 4. Cas Printemps
Étude de cas complète — grand groupe retail, 3 500 utilisateurs migrés en 3 mois. Couvre :
- Contexte de renégociation contrat cadre Microsoft (3+2 ans)
- 3 signaux surprenants découverts (provisioning sans gouvernance, silos RH/Finance/IT, absence d'étude d'impact)
- 4 livrables d'intervention (scénarios de mitigation, analyse éligibilité downgrade, roadmap 3 ans, accompagnement Copilot)
- Timeline en 4 phases
- Encadré "Ce que j'aurais fait différemment" avec lien explicite vers la valeur K&D

Panneau de notes persistantes (localStorage).

### 5. Marché
Analyse concurrentielle des 3 solutions du segment FinOps M365 :
- **SamBox.io (Elée)** — SAM pur, conformité éditeur Microsoft, moteur True Up/Down
- **CoreView** — Gouvernance M365 native, remédiation automatique, suivi Copilot
- **Know & Decide** — ITAM transverse, multi-sources, qualité données, CMDB

Contient :
- 3 fiches solution avec forces, limites et "Ce que K&D devrait absorber"
- Tableau comparatif sur 12 dimensions FinOps M365
- Le modèle FinOps en 4 étapes (Collecter → Qualifier → Piloter → Optimiser) avec couverture par solution
- Tableau des 6 gaps K&D à adresser (avec priorité et action recommandée)
- Message de synthèse pour le RDV partenariat

Panneau de notes persistantes (localStorage).

### 6. SKUs Microsoft
Référentiel de 184 plans de licences Microsoft 365 filtrés (hors variantes GOV/GCC/EDU/Academic). Fonctionnalités :
- Recherche en temps réel par nom, String_Id ou GUID
- Filtres par famille (13 familles : Enterprise E-plans, Business, Frontline, EMS/Security, Teams, Exchange, Copilot, Power BI, Viva, M365 Apps, SharePoint, OneDrive, Autre)
- Compteur de résultats dynamique
- Source : fichier officiel Microsoft `Product_names_and_service_plan_identifiers_for_licensing.csv`

### 7. Graph API
27 endpoints Microsoft Graph API sélectionnés parmi 18 000+ pour le pilotage FinOps M365. Organisés en 5 catégories métier :
- **Licences & SKUs** (5 endpoints) — subscribedSkus, licenseDetails, assignedLicenses
- **Utilisateurs & Comptes** (6 endpoints) — users, mailboxSettings, manager, drive
- **Activité & Adoption** (7 endpoints) — auditLogs/signIns, reports Office365/Teams/OneDrive/SharePoint
- **Groupes & Provisioning** (4 endpoints) — groups, members, GBL, auditLogs/provisioning
- **Organisation & Gouvernance** (5 endpoints) — organization, directoryRoles, devices, conditionalAccess

Accordéons dépliables, recherche libre, badge v1.0 sur chaque endpoint.

---

## Architecture technique

### Principe fondamental
**Zéro dépendance externe. Zéro backend. Zéro build.**

Le fichier `index.html` est autosuffisant et s'ouvre directement dans n'importe quel navigateur moderne.

### Stockage des données
Toutes les données structurées sont embarquées dans des balises `<script type="application/json">` et lues en JavaScript via `JSON.parse(document.getElementById('...').textContent)` :

```html
<script type="application/json" id="d-marche">{...}</script>
<script type="application/json" id="d-skus">[...]</script>
<script type="application/json" id="d-graph">{...}</script>
```

Ce pattern évite tout conflit de syntaxe JavaScript lié aux apostrophes françaises ou aux caractères spéciaux — problème récurrent quand on interpole du JSON directement dans du JS.

### JavaScript
- **Pattern IIFE** — tout le code dans `(function() { ... })()` pour éviter les collisions de variables globales
- **Aucun `onclick` inline** — tous les événements via `addEventListener`
- **Aucune dépendance** — vanilla JS pur, pas de jQuery, pas de framework

### Persistance
Les notes K&D (3 panneaux : GEODIS, Printemps, Marché) sont sauvegardées dans le `localStorage` du navigateur sous les clés `kd_geodis`, `kd_printemps`, `kd_marche`. Les notes survivent aux rechargements de page mais restent locales au navigateur.

### Déploiement GitHub Pages
Le fichier doit s'appeler **`index.html`** à la racine du repository. GitHub Pages le sert automatiquement à l'URL racine sans avoir à spécifier le nom de fichier.

---

## Mise à jour du contenu

### Modifier les données SKUs
Les données SKUs proviennent du fichier officiel Microsoft `Product_names_and_service_plan_identifiers_for_licensing.csv`. Pour mettre à jour :
1. Télécharger la version à jour sur le portail Microsoft
2. Relancer le script Python de filtrage (voir section Génération)
3. Remplacer le contenu de la balise `<script type="application/json" id="d-skus">` dans `index.html`

### Modifier l'analyse concurrentielle
Les données du marché sont dans la balise `<script type="application/json" id="d-marche">`. Le JSON contient :
- `solutions[]` — fiches des 3 solutions (SamBox, CoreView, K&D)
- `modele_finops.etapes[]` — les 4 étapes du modèle avec couverture par outil
- `modele_finops.gaps_kd[]` — les gaps K&D à adresser

### Modifier le texte des cas clients
Le contenu GEODIS et Printemps est dans le HTML statique (pas dans le JSON). Éditer directement les sections `<div class="tab" id="tab-geodis">` et `<div class="tab" id="tab-printemps">` dans `index.html`.

### Ajouter un onglet
1. Ajouter un bouton `<button class="ntab" id="btn-newTab">Label</button>` dans la `<nav>`
2. Ajouter `<div class="tab" id="tab-newTab">...</div>` dans le body
3. Ajouter `'newTab'` dans le tableau `var TABS = [...]` dans le JS

---

## Génération (contexte de développement)

La webapp a été générée via des scripts Python qui :
1. Lisent le CSV officiel Microsoft (SKUs) et le JSON Graph API (18 000+ endpoints)
2. Filtrent et structurent les données pertinentes FinOps
3. Sérialisent en JSON valide via `json.dumps()` (gestion automatique des apostrophes)
4. Injectent dans le template HTML via concaténation de strings Python (jamais via interpolation dans du JS)

**Règle d'or du développement :** ne jamais mettre de données textuelles françaises directement dans une string JavaScript entre guillemets simples. Toujours passer par `<script type="application/json">` + `JSON.parse()`.

---

## Contexte métier

Ce portfolio illustre un **modèle FinOps M365 répliquable** construit sur deux missions terrain :

| Mission | Client | Périmètre | Résultat clé |
|---|---|---|---|
| Lead admin M365 | GEODIS | Multi-régions, multi-entités | -30% dépenses licences, 12 000 comptes optimisés, +200K€ réalloués |
| Consulting FinOps | Printemps | 3 500 utilisateurs | Migration en 3 mois, mission Copilot obtenue en suite |

Le modèle en 4 étapes (Collecter → Qualifier → Piloter → Optimiser) est conçu pour être mis en œuvre avec la plateforme **Know & Decide** (Data Discover + Data Management + Data Report) comme moteur technique, l'expert terrain apportant les règles métier spécifiques M365 (matrice inactifs, critères d'éligibilité BAL/archive/OneDrive, profils usage).

---

## Licence
Usage privé — portfolio professionnel. Contenu non destiné à être reproduit ou distribué sans accord préalable.
