---
name: slide-interview
description: Mène une interview structurée pour définir le contenu, le plan et les messages clés d'une présentation, puis écrit le résultat dans un fichier brief.md. À utiliser quand l'utilisateur veut démarrer une nouvelle présentation/deck, planifier des slides sur un sujet, ou définir le contenu d'un talk avant de générer le HTML.
---

# Slide Interview

Ce skill produit le document de référence (`brief.md`) qui servira ensuite
d'entrée au skill `slide-builder`. L'objectif est de clarifier le fond
(message, public, structure) AVANT de toucher au design ou au HTML.

## Étapes

1. **Identifier le sous-projet** (la thématique). C'est un dossier à la
   racine du repo (ex: `powerbi/`). S'il n'existe pas encore, le créer.
   S'il existe déjà un `brief.md`, demander si on part d'une nouvelle
   version ou si on l'enrichit.

2. **Mener l'interview**, par petites salves de questions (ne pas tout
   poser d'un coup). Utiliser `AskUserQuestion` pour les choix fermés
   (durée, ton, niveau d'audience) et des questions ouvertes en texte pour
   le contenu (messages clés, données, exemples).

   Points à couvrir :
   - **Objectif & message central** : qu'est-ce que le public doit retenir
     / faire après la présentation ?
   - **Audience** : qui, niveau de connaissance du sujet, enjeux/attentes.
   - **Contexte** : durée de présentation, format (réunion, conférence,
     atelier), si le doc sera aussi lu seul (besoin d'être autoportant).
   - **Plan / structure** : proposer un plan en sections à partir des
     réponses, le faire valider ou ajuster par l'utilisateur. Chaque
     section donnera un ou plusieurs slides.
   - **Contenu par slide** : pour chaque slide du plan, capturer le point
     clé, les éléments à montrer (texte, capture d'écran, chiffres,
     schéma, code/DAX, démo) et les notes/arguments pour le présentateur.
   - **Ton & style** : registre (formel/décontracté), niveau technique,
     humour ou non, contraintes de marque/couleurs si l'utilisateur en a.
   - **Assets disponibles** : captures d'écran, logos, fichiers de données,
     liens — noter où ils se trouvent ou s'ils doivent être créés/placeholder.
   - **Termes à ne pas traduire** : noms de produits/marques, termes
     techniques, noms de mesures/colonnes DAX, etc. (le deck final sera
     généré en FR/EN/ES avec switcher — ces termes resteront identiques
     dans les 3 langues).
   - **Call to action / conclusion** : ce qu'on veut comme dernière slide.

3. **Itérer** : reformuler le plan obtenu et demander une validation avant
   d'écrire le fichier final. Ne pas hésiter à proposer une structure par
   défaut (intro → contexte/problème → 2-4 sections de contenu →
   synthèse/recommandations → conclusion/CTA) que l'utilisateur peut
   accepter rapidement plutôt que de partir de zéro.

4. **Écrire `<sous-projet>/brief.md`** avec le gabarit ci-dessous.

## Gabarit de brief.md

```markdown
# <Titre de la présentation>

## Objectif
- Message central : ...
- Ce que l'audience doit retenir / faire ensuite : ...

## Audience & contexte
- Public : ...
- Niveau de connaissance : ...
- Format : (durée, type d'événement)
- Lecture autoportante nécessaire : oui/non

## Ton & style
- Registre : ...
- Niveau technique : ...
- Contraintes visuelles : (couleurs, logos, à éviter...)

## Plan

### 1. <Titre de section>
- Slide(s) : <nombre approximatif>
- Message clé : ...
- Contenu : (points, données, visuels à inclure)
- Notes orateur : ...

### 2. <Titre de section>
...

## Assets
- <description> — <emplacement ou "à créer">

## Termes à ne pas traduire (FR/EN/ES)
- <terme> — <raison>

## Conclusion / Call to action
- ...
```

## Sortie

À la fin, indiquer à l'utilisateur le chemin du `brief.md` créé et proposer
d'enchaîner avec le skill `slide-builder` pour générer le HTML.
