 
# Cahier des charges pour une solution de suivi et de gestion d’individus et d'échantillons

> *Document : Cahier des charges général
> Contexte : Projet européen Cross-DBio
> Autrice : Cynthia Borot - Parc National de la Vanoise*

- [Cahier des charges pour une solution de suivi et de gestion d’individus et d'échantillons](#cahier-des-charges-pour-une-solution-de-suivi-et-de-gestion-dindividus-et-déchantillons)
  - [Contexte et besoin](#contexte-et-besoin)
  - [Objectifs](#objectifs)
  - [Architecture](#architecture)
    - [Le choix GeoNature](#le-choix-geonature)
    - [Organisation des développements](#organisation-des-développements)
      - [Modifications du coeur](#modifications-du-coeur)
      - [Module individus](#module-individus)
      - [Module des échantillons](#module-des-échantillons)
      - [Interactions entre dénombrements de taxons](#interactions-entre-dénombrements-de-taxons)
      - [Portail de visualisation d'individus équipés d'émetteurs](#portail-de-visualisation-dindividus-équipés-démetteurs)
  - [Données concernées par le projet](#données-concernées-par-le-projet)
    - [La capture](#la-capture)
    - [Les prélèvements d’échantillons](#les-prélèvements-déchantillons)
    - [Les individus](#les-individus)
    - [Les marquages](#les-marquages)
    - [Les émetteurs/balises](#les-émetteursbalises)
    - [Biométrie](#biométrie)
    - [Les résultats d'analyse d'échantillons effectués en laboratoire](#les-résultats-danalyse-déchantillons-effectués-en-laboratoire)
    - [Observations humaines d'individus-marqués](#observations-humaines-dindividus-marqués)
    - [Observation via un émetteur GPS](#observation-via-un-émetteur-gps)
    - [Observation via piège photo](#observation-via-piège-photo)
  - [Modèle de données relationnel](#modèle-de-données-relationnel)
    - [Modèle de données complet](#modèle-de-données-complet)
      - [MCD de gestion des individus](#mcd-de-gestion-des-individus)
      - [MCD de gestion des échantillons](#mcd-de-gestion-des-échantillons)
  - [Mise en relation avec le besoin initial](#mise-en-relation-avec-le-besoin-initial)
    - [Données de capture](#données-de-capture)
    - [Données de prélèvements d’échantillons](#données-de-prélèvements-déchantillons)
    - [Données sur les individus](#données-sur-les-individus)
    - [Données de marquages](#données-de-marquages)
    - [Données sur les émetteurs/balises](#données-sur-les-émetteursbalises)
    - [Données de biométrie](#données-de-biométrie)
    - [Données sur les résultats d'analyses d'échantillons effectuées en laboratoire](#données-sur-les-résultats-danalyses-déchantillons-effectuées-en-laboratoire)
    - [Données d'observations humaines d'individus marqués](#données-dobservations-humaines-dindividus-marqués)
    - [Données d'observations via émetteur](#données-dobservations-via-émetteur)
    - [Données d'observation via piège photo](#données-dobservation-via-piège-photo)

## Contexte et besoin

Le Parc national de la Vanoise (PNV) et le Parc national du Grand Paradis (PNGP) partagent un patrimoine naturel exceptionnel, avec des milieux alpins similaires et des espèces emblématiques communes. Les populations animales, notamment le bouquetin des Alpes, se déplacent ponctuellement entre les deux territoires, ignorant les frontières administratives. Cette continuité écologique nécessite une approche transfrontalière de la gestion et du suivi de la biodiversité.

Les deux parcs ont développé, au fil des années, des protocoles de suivi rigoureux qui ont généré une masse considérable de données : plus de 1500 bouquetins capturés et marqués, des dizaines de milliers de données GPS issues du suivi télémétrique de différentes espèces (bouquetins, gypaètes barbus, aigles royaux, tétras-lyres), et une quantité croissante d'images issues des pièges photographiques. Ces données, d'une valeur scientifique inestimable, sont actuellement stockées dans des formats et sur des supports variés, rendant leur exploitation complexe et limitée.

De plus, l’avancée dans les technologies de deep learning ouvre des perspectives prometteuses pour le traitement et la valorisation des données issues de piège photo. Cependant, leur exploitation nécessite une restructuration préalable des données actuelles sous forme de base de données relationnelles selon des standards communs.

Cette harmonisation permettra non seulement d'optimiser l'exploitation des données existantes, mais aussi de faciliter le partage d'informations, via des outils simples d’usage et standardisés, entre les deux parcs puis avec la communauté scientifique.

## Objectifs

Ce projet vise à :

- Rationnaliser la récolte des données de suivi des individus marqués : suivi dynamique de population, veille sanitaire, données issues après post-traitement de pièges photo …
- Centraliser ces données
- Faciliter la réalisation de bilans et l’aide à la décision : optimiser le suivi en permettant aux agents une interrogation des données existantes (repérage des sites préférentiels, connaissance de la date des dernières observations)
- Permettre, via des exports et/ou des interrogations directes à la base de données, de mieux valoriser ces données sur des cartographies faciles à réaliser et adaptées aux différents besoins (animations scolaires, partenaires)
- Faciliter la récupération des données pour les futures analyses et travaux de recherche (CMR, réseaux sociaux, analyse de reproduction, analyse spatiale, taille des groupes etc.) avec nos partenaires et futurs collaborateurs
- Proposer une consultation grand public des déplacements des individus

Il remplacera au parc national de la Vanoise, dans un 1er temps :

- Le tableur « BDD bouquetins »
- La base de données « Bouquetins marqués »
- Le portail de gestion et de visualisation des données GPS « GPS 3 vallées » (plus fonctionnel).

## Architecture

### Le choix GeoNature

GeoNature est la plateforme de gestion de données naturalistes du parc national de la Vanoise.

Les observations occasionelles d'animaux marqués, donc d'individus, sont actuellement insérées dans la synthèse mais avec une perte d'information occasionnée par le fait que GeoNature ne puisse pas accueillir tous les éléments de l'observations.

Au regard du besoin du PNV, de son envie de faire bénéficier à d'autres le développement de nouvelles fonctionnalités et du souhait de l'équipe SI de s'impliquer et contribuer plus fortement au projet GeoNature, ce choix est une évidence.

Après divers échanges avec le Parc national des Cevennes et le Parc national des Ecrins, nous avons eu confirmation que notre projet pouvait s'inscrire dans l'écosystème GeoNature. Cela implique :

- d'élargir les réflexions pour des fonctionnalités servant l'ensemble de la communauté
- de respecter les méthodes de travail du projet
- de respecter les choix techniques

Les développements à venir s'appuieront donc sur le socle technique de Geonature :

- PostgreSQL / PostGIS
- Python 3 et dépendances Python nécessaires à l’application
- Flask (framework web Python)
- Apache
- Angular 15, Angular CLI, NodeJS
- Librairies javascript (Leaflet, ChartJS)
- Librairies CSS (Bootstrap, Material Design)

### Organisation des développements

Lors de plusieurs échanges avec les principaux mainteneurs de GeoNature, nous avons longement discuté le découpage de ces développments, coeur de GeoNature ou moduels à part. Voici dans les paragraphes suivants, l'arbitrage qui a été fait.

#### Modifications du coeur

Ces modifications seront relativements mineures si ce n'est, côté frontend, d'intégrer la possibilité de sélectionner un individus dans le dénombrement.

Nous aurons donc comme modifications à apporter :

1. L'intégration de la notion d'individus au niveau du dénombrement.
2. La possibilité dans la synthese de filtrer par individus
3. La possibilité de rattacher un échantillon à un relevé, une occurence ou un dénombrement
4. Ajout des notions d'interaction entre dénombrements

#### Module individus

cf CDC [Gestion des individus dans GeoNature](./../gestion-individus/cahier-des-charges/Cross-DBio-CDC_gestion-individus.md)

#### Module des échantillons

cf CDC [Gestion des échantillons dans GeoNature](./../gestion-echantillons/Cross-DBio_MCD_gestion-echantillons.md)

#### Interactions entre dénombrements de taxons

> [!CAUTION]
> En cours

cf CDC [Interactions entre dénombrements de taxons dans GeoNature](./../interactions/cahier-des-charges/Cross-DBio-CDC_interactions-denombrements.md)

#### Portail de visualisation d'individus équipés d'émetteurs

> [!CAUTION]
> A définir

## Données concernées par le projet

Les données relatives aux individus devront soit intégrer le MCD actuel de GeoNature, soit le respectant en suivant la même logique de conception, afin de stocker tous les types de données listées ci-après (cf état des lieux).

### La capture

- Nom de/des personnes ayant capturé
- Géolocalisation
- Lieu-dit
- Date de capture
- Type de capture
- Température
- Poids
- Rythme cardiaque
- Echographie
- Anesthésie / antidote :
  - Nombre de tests injection (=nb tirs)
  - Distance
  - Heure injection anesthésiant
  - Localisation injection
  - Produit anesthésiant
  - Posologie
  - Délais : Type de délais (perte posture, coucher, sommeil, maîtrise, réaction capture, relevé tête, déplacement) + unité + valeur
  - Heure injection antidote
  - Produit antidote
  - Observations

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données de capture](#données-de-capture)

### Les prélèvements d’échantillons

Les prélèvements sont réalisés lors de capture ou lors d’une observation d'indices de présence.

- Type de prélèvement (nomenclature : sang, carcasse, selles, urines, plumes, poils …)
- Date de prélèvement
- Identifiant

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données de prélèvements d’échantillons](#données-de-prélèvements-déchantillons)

### Les individus

La notion d'individus ne concerne que la faune. Ces données sont immuables dans le temps.

- Nom
- Identifiant unique
- Espèce
- Année de naissance
- Sexe (peut être optionnel selon l’espèce)
- Population / sous-population

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données sur les individus](#données-sur-les-individus)

### Les marquages

Les marquages sont réalisés sur l’animal lors des captures / recaptures. Un individu peut posséder plusieurs marquages

- Type de marquage : Collier, boucles, peinture, encoches …
- Caractéristiques du marquage (nomenclature : couleur, lettre)
- Localisation du marquage sur l’animal : oreille droite, oreille gauche, cou, carcasse, aile droite …
- Remarques

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données de marquages](#données-de-marquages)
  
### Les émetteurs/balises

Il est question des émetteurs installés sur l'animal. Ces données n'étant pas encore traitées au sein du PNV, cette liste de champs provient de réflexions avec le PNGP.

- Type d’émetteur : balise GPS, collier GPS, collier VHF …
- Informations techniques : (Marque, modèle, batterie …)
- Date de pose
- Géolocalisation lors de la pose
- Date de retrait
- Géolocalisation lors du retrait
- Référentiel géographique

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données sur les émetteurs/balises](#données-sur-les-émetteursbalises)
  
### Biométrie

Cela concerne les données de biométrie réalisées lors des captures et recaptures.

- Date des mesures
- Mesures :
  - Localisation de la mesure sur l’animal (nomenclature)
  - Mesure
  - Unité de mesure (nomenclature)

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données de biométrie](#données-de-biométrie)

### Les résultats d'analyse d'échantillons effectués en laboratoire

- Type d’analyse (nomenclature : sérologie, test gestation, génétique, coprologie, autopsie …)
- Date de réception au laboratoire
- Nom du laboratoire (nomenclature)
- Référence de dossier transmis par le laboratoire
- Mode de conservation de l’échantillon ou du cadavre entier (nomenclature)
- Commentaires

La liste suivante pourra évoluer dans le temps.

Sérologie :

- Virus recherchés :
  - Nom du virus (nomenclature)
  - Résultat
  
Tests de gestation :

- Type de dosage (nomenclature)
- Valeurs

Génétique :

- Données relatives à l’empreinte génétique
- Informations de filiation

Autopsie :

- Date mort présumée
- Etat cadavre (nomenclature)
- Etat physiologique (doublon avec observation visuelles ?)
- Statut reproducteur
- Synthèse lésionnelle + harmonisée
- Affection / virus / maladies :
  - Type
  - Résultat
- Causes mort (+ respi cause mort)
- Evolutions terminales
- Découvertes
- Matrices bact, Nb de matrices bact
- Bactério P
- Bactéries aéro 1 à 7, bactéries anaéréo 1 et 2
- Parasito directe
- Coproscopie
- VIRP / Myco/ Autre

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données sur les résultats d'analyses d'échantillons effectuées en laboratoire](#données-sur-les-résultats-danalyses-déchantillons-effectuées-en-laboratoire)

### Observations humaines d'individus-marqués

- Géolocalisation
- Identifiant observateur (identifiant Geonature)
- Etat physiologique
- Interaction entre individus
  - Suitée
  - Identifiants d’autres individus marqués
  - Nombre d’individus du groupe observé (dont les marqués)
    - Sexe
    - Classe d’âge
    - Nombre
  - Caractérisation de l’interaction entre 2 individus
- Autres observations :
  - Type observation (dyspnee, jetage, kerato, toux …)
  - Valeur (nomenclature)

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données d'observations humaines d'individus marqués]((#données-dobservations-humaines-dindividus-marqués))

### Observation via un émetteur GPS

- Identifiant de l'émetteur ou de la balise
- Individu
- Géolocalisation (point)
- Date + heure
- Température contre l'animal
- Autres informations dépendantes des émetteurs

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données d'observations via émetteur](#données-dobservations-via-émetteur)

### Observation via piège photo

- Identifiant du piège photo
- Espèce
- Dénombrement
- Géolocalisation
- Date + heure
- Méthode de détermination (humain ou algorithme)
- Autres informations d'observation

Pour comprendre comment ont été implémentés ces champs en BDD, cf le tableau de correspondance suivant : [Données d'observation via piège photo](#données-dobservation-via-piège-photo)

## Modèle de données relationnel

Dans la présentation du modèle de données ci-après, nous nous efforçons d'expliquer la généricité des choix opérés.

### Modèle de données complet

Le modèle de données, ci-dessous, ne reprends pas l'ensemble du modèle de GeoNature. Il ne présente que les nouvelles tables créées pour le projet ainsi que les tables principales du modèle actuel qui leurs sont liées (en vert) et sont nécessaires à la compréhension du modèle. Les champs créés dans des tables existantes sont surlignés en orange.

#### MCD de gestion des individus

*Schéma complet du MCD de gestion des individus* :

![Schéma complet du MCD de gestion des individus](./images/mcd_individus.png)

#### MCD de gestion des échantillons

*Schéma complet du MCD de gestion des échantillons* :

![Schéma complet du MCD de gestion des échantillons](./images/mcd_echantillons.png)

## Mise en relation avec le besoin initial

Via les tableaux de correspondance suivant, nous validons que l'ensemble des données (des champs) listées dans le paragraphe [Données concernées par le projet](#données-concernées-par-le-projet) trouvent leur emplacement dans le modèle de données proposé ci-dessus.

### Données de capture

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Nom de/des personnes ayant capturé | pr_occtax.t_releves_occtax | observers_txt | |
| Géolocalisation | pr_occtax.t_releves_occtax | geom_local, geom_4326 | |
| Lieu-dit | pr_occtax.t_releves_occtax | additional_data | `{[...],"locality": "xxx"}` |
| Date de capture | pr_occtax.t_releves_occtax | date_min, date_max, hour_min, hour_max, | |
| Type de capture | pr_occtax.t_releves_occtax | additional_data | `{[...],"capture_type": "000"}` |
| Température | pr_occtax.t_counting_occtax | additional_data | `{[...],"temperature": "000"}` |
| Poids | pr_occtax.cor_counting_occtax | additional_data | `{[...],"weight": "000"}` |
| Rythme cardiaque | pr_occtax.cor_counting_occtax | additional_data | `{[...],"heart_rate": "000"}` |
| Echographie | pr_occtax.cor_counting_occtax | additional_data | `{[...],"pregnant": "yes/no"}` |
| Anesthésie : Nombre de tirs, Distance tir, Commentaires | pr_occtax.cor_counting_occtax | additional_data | `{[...],"anesthesia_general": "xxx"}`|
| Anesthésie injection : Heure, localisation, nom produit, posologie | pr_occtax.cor_counting_occtax | additional_data | `{[...],"anesthesia_injection":"xxx""}` |
| Anesthésie réactions (x n): Type, unité, valeur | pr_occtax.cor_counting_occtax | additional_data | `{[...],"anesthesia_reaction""xxx"}` |
| Antidote : Heure injection, produit, posologie | pr_occtax.cor_counting_occtax | additional_data | `{[...],"antidote_injection":"xxx"}` |
| Commentaires  | pr_occtax.t_releve_occtax | comment | |
| Individus capturés | pr_occtax.cor_counting_occtax | id_individual | |

### Données de prélèvements d’échantillons

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Type de prélèvement | gn_commons.t_samples | id_nomenclature_sample_type | |
| Date de prélèvement | pr_occtax_t_releves_occtax | date_min | |
| Identifiant | gn_commons.t_samples | id_sample | |

### Données sur les individus

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Nom | gn_monitoring.t_individuals | individual_name | |
| Identifiant unique | gn_monitoring.t_individuals | id_individual | |
| Espèce | gn_monitoring.t_individuals | cd_nom | |
| Année de naissance | gn_monitoring.t_individuals | additional_data | `{[...],"birth_year": "000"}` |
| Sexe | gn_monitoring.t_individuals | id_nomenclature_sex | |
| Population / sous-population | gn_monitoring.t_individuals | additional_data | `{[...],"population": "xxx","subpopulation": "xxx"}` |

### Données de marquages

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Type de marquage | gn_individuals.t_individual_deployments | id_nomenclature_deployment_type | Le marquage fait partie des équipements déployés sur l'individu |
| Caractéristiques du marquage | gn_individuals.t_individual_deployments | marking_code ou id_tracking_device | |
| Localisation du marquage sur l’animal | gn_individuals.t_individual_deployments | id_nomenclature_deployment_location | |
| Remarques | gn_individuals.t_individual_deployments | comment | |
  
### Données sur les émetteurs/balises

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Type d’émetteur | gn_individuals.t_tracking_devices | id_nomenclature_device_type | Les émetteurs font parties du matériel de suivi des individus |
| Informations techniques : manufacturer, model, serial_number | gn_individuals.t_tracking_devices | comment | |
| Date de pose | gn_individuals.t_individual_deployments | install_date | |
| Géolocalisation lors de la pose | pr_occtax.t_releves_occtax | geom_local, geom_4326 | Géolocalisation de la première "observation de type capture" associée au déploiement du matériel |
| Date de retrait | gn_individuals.t_individual_deployments | removal_date |  |
| Géolocalisation du retrait | pr_occtax.t_releves_occtax | date_min | Géolocalisation de la dernière donnée d'observation |
  
### Données de biométrie

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Date des mesures | pr_occtax.t_releves_occtax | date_min |  |
| Mesures : Localisation sur l’animal, mesure, unité de mesure | pr_occtax.cor_counting_occtax | additional_data | `{[...],"measures":"xxx"}` |

### Données sur les résultats d'analyses d'échantillons effectuées en laboratoire

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Type d’analyse | gn_individuals.t_samples_analysis | id_nomenclature_analysis_type | |
| Date de réception au laboratoire | gn_individual.t_sample_analysis | sample_receipt_date | |
| Nom du laboratoire | gn_individuals.bib_laboratories | name | |
| Référence dues résultats transmis par le laboratoire | gn_individuals.t_sample_analysis | laboratory_analysis_ref | |
| Mode de conservation de l’échantillon | gn_common.t_samples | comment | |
| Remarques | gn_individuals.t_samples_analysis | comment | |

Les résultats d'analyses à proprement parlé seront stockés dans le champ `additional_data` de la table `gn_individual.t_sample_analysis`.

Exemple de structure du champ pour une analyse de type **Sérologie** :

```json
{
  "searched_virus": {
    "1": {   
      "name": "xxx",
      "result": "xxx",
    },
    "2": {
      "name": "xxx",
      "result": "xxx",
    },
    [...]
  }
}
```

Exemple de structure du champ pour une analyse de type **Test gestationnel** :

```json
{
  "gestational_test": {
    "1": {   
      "dosage_type": "xxx",
      "value": "xxx",
    },
    "2": {
      "dosage_type": "xxx",
      "value": "xxx",
    },
    [...]
  }
}
```

Pour les analyses de type **Génétique**, nous n'avons pas d'exemple à proposer.

Exemple de structure du champ pour une analyse de type **Autopsie** :

```json
{
  "autopsy": {
    "presumed_death_date": "000",
    "corpse_condition": "xxx",
    "physilogical_state": "xxx",
    "reproductive_status": "xxx",
    "lesion_synthesis": "xxx",
    "affection_virus_disease": {
     "1": { "type": "xxx","result": "xxx"},
     [...]
    },
    "death_cause": "xxx",
    "terminal_evolutions": "xxx",
    [...]
  }
}
```

### Données d'observations humaines d'individus marqués

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Géolocalisation | pr_occtax.t_releves_occtax | geom_local, geom_4326 | |
| Identifiant observateur | pr_occtax.t_releves_occtax | observateur_txt | |
| Etat physiologique | pr_occtax.t_occurrences_occtax | id_nomenclature_bio_condition | |
| Femelle suitée | pr_occtax.t_interaction_occtax | ensemble des champs | |
| Identifiants d’autres individus marqués observés ensembles | pr_occtax.cor_counting_occtax | id_individual | Les enregistrements de chaque individu feront référence au même id_occurence_occtax |
| Nombre d’individus du groupe observé (hors marqués) distingués par sexe, classe d’âge et nombre | pr_occtax.t_counticor_counting_occtaxng_occtax | id_nomenclature_life_stage, id_nomenclature_sex, id_nomenclature_type_count | Les enregistrements de chaque groupe feront référence au même id_occurence_occtax |
| Caractérisation de l’interaction entre 2 individus  | pr_occtax.t_interaction_occtax | ensemble des champs |  |
| Observations sanitaires : Type observation et valeur  | pr_occtax.cor_counting_occtax | additional_fields | `{[...],"health_observations":{"1":{"type": "xxx","value": "xxx"},"2":{"type": "xxx","value": "xxx"},[...]}}` |

### Données d'observations via émetteur

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Identifiant de l'émetteur ou de la balise | pr_occtax.t_releves_occtax | additional_fields | `{"id_tracking_device": "000"}` |
| Identifiant de l'émetteur ou de la balise | pr_occtax.cor_role_releves_occtax | id_role | Un role unique appelé "matériel" sera systématiquement associé à ces observations qui seront elles, associées à un jeu de données spécifique |
| Individu | pr_occtax.cor_counting_occtax | id_individual | |
| Géolocalisation (point) | pr_occtax.t_releves_occtax | geom_local, geom_4326 | |
| Date + heure | pr_occtax.t_releves_occtax | date_min, hour_min | |
| Température contre l'animal | pr_occtax.t_releves_occtax | comment | |
| Autres informations dépendantes des émetteurs | pr_occtax.t_releves_occtax | comment | |

### Données d'observation via piège photo

Les pièges photos ne remonteront pas seulement des informations sur les individus marqués mais sur toute espèce.

| Donnée initiale | Schéma / table | Champ | Explication |
| :---- | :---- | :---- | :---- |
| Identifiant du piège photo | pr_occtax.t_releves_occtax | additional_fields | `{"id_tracking_device": "000"}` |
| Espèce | pr_occtax.t_releves_occtax | cd_nom | |
| Individu | pr_occtax.cor_counting_occtax | id_individual | |
| Dénombrement | pr_occtax.cor_counting_occtax | tous les champs | |
| Géolocalisation | pr_occtax.t_releves_occtax | geom_local, geom_4326 | |
| Date + heure | pr_occtax.t_releves_occtax | date_min, hour_min | |
| Méthode de détermination (humain ou algorithme) | pr_occtax.t_occurences | id_nomenclature_determination_method | A renseigner différement si les photos ont été analysées par l'oeil humain ou via un algorithme |
| Autres informations d'observation | pr_occtax.t_releves_occtax | comment | |