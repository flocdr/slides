# Introduction à Power BI – Vers un reporting robuste

## Objectif
- Message central : Power BI n'est pas "un Excel avec des jolis graphiques" — sa vraie valeur est dans l'automatisation de la préparation des données (Power Query) et dans la diffusion automatisée des rapports (Power BI Service). Un reporting réussi est un reporting qui tourne seul, sans intervention manuelle récurrente sur les données d'entrée.
- Ce que l'audience doit retenir / faire ensuite :
  - Comprendre les 3 vues de Power BI Desktop (données, modèle, rapport) et leur équivalent dans la logique Excel/tableaux croisés dynamiques.
  - Savoir que la qualité d'un rapport se joue d'abord dans Power Query (préparation/transformation), pas dans la mise en forme visuelle.
  - Connaître les bases de la publication (workspace, Power BI Service, licences) pour ne pas se limiter à un usage "Desktop solo".
  - Repartir avec une checklist de bonnes pratiques "robustesse" et un glossaire de référence.

## Audience & contexte
- Public : équipes internes (support digital, entreprise de construction), profils techniques/fonctionnels variés mais tous utilisateurs réguliers d'Excel.
- Niveau de connaissance : découverte totale de Power BI. Bonne maîtrise d'Excel (formules, tableaux croisés dynamiques) à utiliser comme point d'appui pédagogique.
- Format : formation de 4h, en présentiel, avec démonstrations live sur machine (l'animateur manipule Power BI Desktop en direct). Les slides servent de fil conducteur minimal, la majorité du temps se passe en démo commentée.
- Lecture autoportante nécessaire : oui, via un mode "complet" (voir Exigences techniques) — pendant la session l'animateur utilise le mode "léger", les absents/relecteurs peuvent basculer en mode "complet" pour un contenu plus détaillé.

## Ton & style
- Registre : professionnel mais accessible — sérieux sur le fond, formulations directes, pas de jargon gratuit. Quelques touches d'humour léger possibles.
- Niveau technique : volontairement plus poussé que les tutoriels "grand public" — on vise des réflexes d'entreprise (robustesse, automatisation, gouvernance), pas juste "comment faire un graphique".
- Contraintes visuelles : thème "lean" partagé, couleur d'accent personnalisée `#004e8c` (bleu corporate). Pas de logo pour l'instant.

## Exigences techniques spécifiques (pour slide-builder)

### Toggle "Light / Complet"
En plus du switcher de langue (FR/EN/ES) et du dark mode déjà prévus par `template.html`, ce deck nécessite un **troisième toggle "Light / Complet"** dans la `.deck-controls` toolbar :
- Piloté par un attribut `data-detail="light"|"full"` sur `<html>`, persisté en `localStorage` (même mécanisme que `data-lang`/`data-theme`).
- En mode `light` (par défaut, utilisé pendant la présentation live) : seuls les éléments essentiels sont visibles.
- En mode `full` : des blocs de contenu complémentaire (`.detail` ou équivalent) apparaissent — explications approfondies, exemples additionnels, nuances — pour permettre une relecture autonome.
- Ce mécanisme doit être ajouté à `shared/theme/lean.css` (classes/règles d'affichage) et `shared/theme/template.html` (bouton toolbar + script de persistance), pour être réutilisable par les futurs decks — pas une solution ad hoc à ce deck.
- Chaque slide de contenu (sections 2 à 8) doit prévoir au moins un bloc `.detail` avec le complément d'information correspondant.

## Plan

### 1. Titre / Introduction
- Slide(s) : 1
- Message clé : poser le cadre — "Introduction à Power BI : viser la robustesse, pas juste le graphique"
- Contenu : titre, sous-titre (objectif de la session, durée 4h), nom/rôle de l'animateur
- Notes orateur : présenter rapidement le déroulé (théorie courte + démo live tout au long)

### 2. À quoi sert Power BI — l'objectif de robustesse
- Slide(s) : 2
- Message clé : un bon reporting tourne seul ; le but n'est pas de "faire un graphique" mais de mettre en place un pipeline fiable qui ne demande (presque) plus d'intervention manuelle sur les données d'entrée.
- Contenu :
  - Positionnement : Power BI = outil de BI self-service (collecter, transformer, modéliser, visualiser, diffuser).
  - Contraste : "reporting artisanal" (copier-coller Excel, mise à jour manuelle, envoi de fichiers par mail) vs "reporting robuste" (sources connectées, actualisation automatique, diffusion via le cloud).
  - .detail : exemples concrets de "dérives" classiques (fichier Excel modifié à la main chaque mois, formules cassées, version différente envoyée à chacun) et ce que Power BI permet d'éviter.
- Notes orateur : insister sur le fait que cette formation va donner les clés pour viser ce niveau de robustesse, pas juste l'aspect visuel.

### 3. La vraie valeur ajoutée : Power Query + diffusion
- Slide(s) : 2
- Message clé : la création de valeur de Power BI se joue à deux endroits — (1) Power Query pour le pipeline de préparation des données, et (2) la publication sur Power BI Service pour la diffusion automatisée. Se contenter d'ouvrir Power BI Desktop, cliquer sur "Actualiser" et renvoyer le fichier par mail, c'est passer à côté de l'essentiel.
- Contenu :
  - Power Query = "l'usine" qui transforme des données brutes/sales en données prêtes à l'emploi, de façon répétable (étapes enregistrées, rejouables à chaque actualisation).
  - Power BI Service (Online) = le "guichet" qui diffuse automatiquement le rapport à jour à la bonne audience, sans manipulation manuelle de fichiers.
  - .detail : schéma mental "Sources → Power Query (transformation) → Modèle → Rapport → Power BI Service (diffusion)" ; comparaison avec le cycle "copier-coller mensuel dans Excel + envoi par mail".
- Notes orateur : c'est le message le plus important de toute la session — à répéter/illustrer si besoin pendant la démo.

### 4. Vue tabulaire (Power Query / Data view)
- Slide(s) : 2
- Message clé : la vue tabulaire, c'est l'équivalent d'un tableau Excel bien structuré — sauf que chaque transformation est une étape documentée et rejouable, pas une manipulation manuelle perdue.
- Contenu :
  - Présentation de la vue "Données" / éditeur Power Query : table de données, panneau des étapes appliquées.
  - Parallèle Excel : une table structurée + des opérations qu'on ferait "à la main" (filtrer, renommer, scinder une colonne, changer un type) deviennent des étapes Power Query réutilisables et automatiques.
  - .detail : exemples de transformations courantes (changement de type, suppression de colonnes, fusion de requêtes = équivalent RECHERCHEV/jointure, regroupement = équivalent TCD).
- Notes orateur : démo live — ouvrir une source, appliquer 2-3 transformations simples, montrer le panneau des étapes.

### 5. Vue relation (Model view)
- Slide(s) : 2
- Message clé : la vue relations, c'est le modèle de données — l'équivalent de relier plusieurs tableaux Excel par des RECHERCHEV, mais en mieux : des relations explicites, réutilisables par tous les visuels du rapport.
- Contenu :
  - Présentation de la vue "Modèle" : tables, relations, cardinalités.
  - Parallèle Excel : RECHERCHEV/INDEX répétés dans chaque feuille vs une relation définie une fois pour toutes.
  - Notion de modèle en étoile (table de faits + tables de dimensions) introduite simplement.
  - .detail : pourquoi un modèle en étoile est plus robuste/performant qu'un modèle "à plat" (un seul grand tableau), notion de granularité.
- Notes orateur : démo live — créer/vérifier une relation entre deux tables, montrer l'effet sur un visuel.

### 6. Vue rapport (Report view)
- Slide(s) : 2
- Message clé : tout ce qu'on voit dans la vue rapport n'est, fondamentalement, qu'un tableau croisé dynamique présenté différemment — graphique, carte, carte de KPI, etc. sont des TCD avec un habillage visuel différent.
- Contenu :
  - Présentation de la vue "Rapport" : visuels, champs (lignes/colonnes/valeurs = équivalent zones d'un TCD), filtres/segments = équivalent filtres de TCD.
  - Parallèle Excel : un graphique croisé dynamique vs un visuel Power BI — même logique sous-jacente (agrégation de données selon des axes), présentation différente.
  - .detail : notion d'interactivité entre visuels (un clic filtre les autres visuels), ce qu'un TCD classique ne fait pas nativement.
- Notes orateur : démo live — construire 1-2 visuels simples à partir du modèle préparé, montrer l'interactivité croisée.

### 7. Publier et partager : workspaces et licences
- Slide(s) : 2
- Message clé : un rapport qui reste sur le poste de l'auteur n'a pas de valeur collective — la publication sur Power BI Service (workspaces, partage, actualisation planifiée) est ce qui transforme un fichier personnel en outil d'équipe.
- Contenu :
  - Power BI Service (Online) : où sont publiés les rapports, notion de workspace (espace de travail partagé), d'app (vue consolidée pour les utilisateurs finaux).
  - Actualisation planifiée (refresh) — et mention rapide de la passerelle de données (gateway) pour les sources on-premise, condition pour avoir un reporting "qui tourne seul".
  - Aperçu des licences : Pro, Premium Per User (PPU), Premium/Fabric — qui peut consulter, qui peut publier, dans quels cas chaque licence est nécessaire.
  - .detail : différences plus fines entre les licences (capacité, limites de partage, fonctionnalités avancées), cas d'usage typiques par taille d'organisation.
- Notes orateur : pas de démo obligatoire ici (dépend des accès disponibles le jour J) — peut rester côté slides/explications orales.

### 8. Synthèse — Checklist de robustesse
- Slide(s) : 1-2
- Message clé : récapitulatif des réflexes "robustesse" à adopter systématiquement.
- Contenu (check-list) :
  - Connecter directement les sources (pas de copier-coller manuel de données).
  - Faire toutes les transformations dans Power Query, pas en retouchant manuellement les données sources ou dans Excel.
  - Construire un modèle de données propre (relations, modèle en étoile) plutôt qu'un tableau unique "à plat".
  - Publier et planifier l'actualisation plutôt que renvoyer des fichiers par mail.
  - Documenter les étapes de transformation (noms clairs) pour que le rapport reste maintenable par d'autres.
  - .detail : pour chaque point, un exemple de "mauvaise pratique" correspondante et son impact (temps perdu, erreurs, versions divergentes).
- Notes orateur : clôture de la session — vérifier que chaque point fait écho à une démo vue plus tôt.

### 9. Glossaire (FR/EN/ES)
- Slide(s) : 2-4 (groupé par thème)
- Message clé : glossaire de référence pour la relecture autonome, groupé par thème.
- Contenu : termes et courtes définitions, organisés par thème, avec traduction FR/EN/ES quand pertinent (les noms propres/produits restent identiques dans les 3 langues — voir section dédiée) :
  - **Données / Pipeline** : source de données, requête, étape de transformation, actualisation (refresh), dataflow.
  - **Modélisation** : modèle de données, relation, cardinalité, table de faits, table de dimension, modèle en étoile, mesure, colonne calculée, DAX.
  - **Visualisation / Rapport** : visuel, segment (slicer), page de rapport, interactivité croisée (cross-filtering), favoris (bookmarks).
  - **Publication / Partage** : workspace (espace de travail), app, Power BI Service, passerelle de données (gateway), licence Pro/PPU/Premium.
- Notes orateur : slide(s) de référence, à ne pas nécessairement projeter en détail pendant la session live — surtout utile en mode "complet" pour la relecture.

## Assets
- Captures d'écran de Power BI Desktop (vues données/modèle/rapport) — à créer lors de la préparation finale, idéalement issues du même jeu de données que la démo live.
- Aucun logo entreprise pour l'instant.

## Termes à ne pas traduire (FR/EN/ES)
- Power BI, Power BI Desktop, Power BI Service — noms de produits
- Power Query — nom de fonctionnalité/produit
- DAX — langage (acronyme)
- M — langage Power Query (mentionné si besoin, à clarifier dans le contexte pour éviter l'ambiguïté avec la lettre)
- Power Pivot — nom de fonctionnalité Excel
- Workspace, Dataflow, Gateway — termes techniques Power BI couramment utilisés tels quels même en FR
- Pro, Premium Per User (PPU), Premium, Fabric — noms de licences/offres
- DAX, RECHERCHEV/VLOOKUP — RECHERCHEV reste tel quel en FR, VLOOKUP en EN/ES (adapter selon la langue car ce sont des noms de fonctions Excel localisés)

## Conclusion / Call to action
- Dernière slide = checklist de robustesse (section 8), pas de section "prochaines étapes" supplémentaire pour l'instant.
