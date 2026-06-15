---
name: slide-builder
description: Génère un deck de slides HTML (Reveal.js, thème "lean" partagé) à partir d'un fichier brief.md produit par le skill slide-interview. À utiliser quand l'utilisateur veut construire, générer ou mettre à jour le HTML/les assets d'une présentation.
---

# Slide Builder

Transforme un `brief.md` (plan + contenu validés) en une présentation
Reveal.js statique, prête à être ouverte dans un navigateur ou exportée en
PDF.

## Pré-requis

- Un `<sous-projet>/brief.md` existe (sinon, proposer d'utiliser d'abord le
  skill `slide-interview`).

## Étapes

1. **Lire le brief** (`<sous-projet>/brief.md`) en entier pour avoir le plan,
   le ton, les contraintes visuelles et le contenu de chaque slide.

2. **Préparer la sortie** :
   - Créer `<sous-projet>/slides/` s'il n'existe pas.
   - Créer `<sous-projet>/slides/assets/` pour les images/screenshots
     spécifiques au deck.
   - Copier `shared/theme/template.html` vers
     `<sous-projet>/slides/index.html` comme point de départ (chemins
     relatifs vers `../../shared/theme/lean.css` à vérifier selon la
     profondeur du dossier). Ce template contient déjà la barre
     `.deck-controls` (switcher FR/EN/ES + mode sombre) et la logique JS
     associée — **toujours la conserver**, ne pas la recréer.

3. **Générer les sections** : une `<section>` par slide du plan.
   - **Slide de titre** : `<section class="title center">` avec `<h1>` +
     `.subtitle`.
   - **Séparateurs de partie** : `<section class="section center">` avec
     un `<h2>`.
   - **Contenu** : `<section>` standard. Utiliser `<span class="eyebrow">`
     pour rappeler la section/le fil rouge, `<h2>` pour le titre de slide,
     puis le contenu (liste, paragraphe, image, code).
   - **Chiffres clés / comparaisons** : utiliser les classes utilitaires
     `.grid.cols-2` / `.grid.cols-3` + `.card`, `.stat` / `.stat-label`
     (voir le template pour des exemples).
   - **Code / DAX / M / requêtes** : `<pre><code class="language-xxx"
     data-trim>`. Le plugin `highlight` est déjà chargé.
   - **Citations** : `<blockquote>`.
   - **Notes orateur** : `<aside class="notes">...</aside>` à l'intérieur
     de la section, repris depuis "Notes orateur" du brief.

4. **Couleur d'accent / branding** : si le brief précise des contraintes de
   couleur, surcharger `--accent` (et au besoin `--bg`, `--text`) dans un
   `<style>` en tête de `index.html`, après le lien vers `lean.css`. Sinon
   garder la valeur par défaut du thème (`#4f46e5`).

5. **Assets** :
   - Si le brief référence des captures d'écran/images existantes, les
     copier dans `slides/assets/` et les référencer en relatif
     (`assets/nom.png`).
   - Si une image est nécessaire mais non fournie, créer un placeholder
     SVG simple (rectangle + libellé) plutôt que de laisser un `<img>`
     cassé, et le signaler à l'utilisateur dans le résumé final.
   - Pour des graphiques de données, préférer un SVG inline simple ou
     Chart.js via CDN (`https://cdn.jsdelivr.net/npm/chart.js`) plutôt que
     des images statiques, sauf si l'utilisateur fournit déjà l'image.

6. **Vérifier** : ouvrir `index.html` (ou lancer un serveur statique léger,
   ex. `python3 -m http.server`) et vérifier visuellement au moins la
   slide de titre, une slide de contenu, et une slide avec composant
   (grid/card ou code) avant de conclure.

## Traduction (FR / EN / ES) et mode sombre

Le brief est rédigé en français ; chaque deck doit être livré traduit en
anglais et en espagnol, avec un switcher (voir `.deck-controls` dans le
template) et un mode sombre. Le mécanisme est géré par
`shared/theme/lean.css` (variables `[data-theme="dark"]` + règles
`[data-lang="..."]`) — ne pas réinventer un autre système.

- **Pour chaque élément de texte traduisible** (titres, paragraphes,
  listes, `eyebrow`, `stat-label`, sous-titres...), générer **trois
  versions sœurs** avec `lang="fr"`, `lang="en"`, `lang="es"` (voir les
  exemples dans `template.html`). Un seul jeu de balises est affiché à la
  fois selon `data-lang` sur `<html>`.
- **Traduire fidèlement** le sens, pas mot à mot ; adapter les formats de
  nombres/dates à la locale (ex. `1,5 %` en FR/ES vs `1.5%` en EN — ce qui
  diffère selon l'usage choisi pour ES) et les idiomes.
- **Ne pas traduire** : noms propres/marques, termes techniques figés
  (Power BI, DAX, M, Power Query, noms de mesures/colonnes...), et le
  contenu du code (`<pre><code>`) — ces éléments restent uniques, sans
  attribut `lang`, donc toujours visibles.
- **Notes orateur** (`<aside class="notes">`) restent en français
  uniquement (pas de variantes par langue).
- **Couleurs** : ne jamais coder une couleur en dur dans le HTML/CSS d'un
  deck — toujours utiliser les variables (`var(--bg)`, `var(--text)`,
  `var(--accent)`, etc.) pour que le mode sombre fonctionne automatiquement.
  Pour les images/screenshots fournis par l'utilisateur (souvent en mode
  clair), prévoir un léger cadre/fond clair (`.card`/`.bg-alt`) pour qu'ils
  restent lisibles en mode sombre plutôt que de les modifier.
- **Vérification** : tester la slide de titre et au moins une slide de
  contenu dans les 3 langues ET dans les 2 thèmes (clair/sombre) avant de
  conclure.

## Principes de design ("lean")

- **Une idée par slide.** Si une slide du brief contient trop d'éléments,
  proposer de la scinder en plusieurs slides plutôt que de surcharger.
- **Texte court** : titres percutants, listes à puces courtes (pas de
  phrases complètes si un fragment suffit), pas de paragraphes denses.
- **Une seule couleur d'accent**, utilisée avec parcimonie (titres de
  section, chiffres clés, liens, puces).
- **Beaucoup d'espace blanc** : ne pas chercher à remplir la slide, le
  thème `lean.css` a déjà des marges généreuses — ne pas les réduire.
- **Cohérence** : réutiliser les classes utilitaires du thème (`eyebrow`,
  `grid`, `card`, `stat`) plutôt que d'introduire des styles ad hoc par
  slide. Si un besoin récurrent n'est pas couvert par `shared/theme/lean.css`,
  l'ajouter au thème partagé (pas en inline) pour que les futurs decks en
  profitent aussi.

## Export PDF

Reveal.js permet l'export PDF via le navigateur : ouvrir
`index.html?print-pdf` et imprimer en PDF (sans marges, fond activé).

## Sortie

Résumer à l'utilisateur : chemin du deck généré, nombre de slides, assets
manquants/à fournir, et comment l'ouvrir (chemin local + rappel
`?print-pdf` pour l'export).
