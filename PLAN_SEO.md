# Plan SEO — marine-dujardin-psychologue.fr

Objectif : mieux positionner le site sur les recherches locales (« psychologue Nice », « psychologue
Garibaldi », « thérapie des schémas Nice », « psychologue périnatalité Nice »…).

Cocher les cases (`- [x]`) au fur et à mesure.

---

## Constat initial (2026-09-28)

Le contenu est bon (riche, précis, bien localisé). Les points faibles sont techniques :

| Problème | Impact |
| --- | --- |
| Pas de `<title>` ni de `<meta description>` | Critique : Google invente le titre affiché |
| Pas de `lang="fr"` sur `<html>` | Moyen |
| 5 `<h1>` et 5 `<main>` sur une seule page | Moyen : hiérarchie brouillée |
| Le `<h1>` d'accueil ne contient ni « psychologue » ni « Nice » | Fort |
| Pas de `robots.txt`, `sitemap.xml`, ni URL canonique | Moyen |
| Pas de données structurées Schema.org | Fort pour le SEO local |
| Pas de favicon ni de balises Open Graph | Faible SEO, visible au partage (WhatsApp…) |
| Rendu via React + Babel chargés depuis unpkg (~3 Mo pour Babel) | Fort : lenteur, Core Web Vitals |
| Onglets « Mon approche » (ACT, psychocorporel, psycho positive) retirés du DOM tant qu'on ne clique pas | Fort : Google ne voit probablement que « Thérapie des schémas » |
| Navigation en `<button>` au lieu de liens `<a href>` | Faible (bloquant si multi-pages) |
| `portrait.jpg` 1200×1600, 270 Ko, sans `srcset` | Faible |

---

## Phase 1 — Quick wins techniques (≈ 1 h, design inchangé)

- [x] Ajouter `<html lang="fr">`
- [x] Ajouter `<title>` dans le `<head>` statique (pas dans `<helmet>`)
  - Proposition : *Marine Dujardin — Psychologue clinicienne à Nice (Garibaldi)*
- [x] Ajouter `<meta name="description">`
  - Proposition : *Psychologue clinicienne et psychothérapeute à Nice, près de la place Garibaldi. Anxiété, trauma, deuil, périnatalité. Séances au cabinet ou en visio.*
- [x] Ajouter `<link rel="canonical" href="https://marine-dujardin-psychologue.fr/">`
- [x] Ajouter un favicon (`favicon.svg`, `favicon-32.png`, `apple-touch-icon.png`, pastille « MD »)
- [x] Ajouter les balises Open Graph / Twitter (titre, description, photo)
- [x] Créer `robots.txt`
- [x] Créer `sitemap.xml`
- [x] Ajouter un bloc JSON-LD Schema.org `Psychologist` / `MedicalBusiness` (nom, adresse, téléphone, horaires, géolocalisation, tarif, `areaServed: Nice`, `sameAs`)
- [x] Corriger la hiérarchie des titres
  - [x] Un seul `<h1>` contenant « Psychologue à Nice » (le slogan peut rester visuellement)
    - Fait via un sur-titre visible « Psychologue clinicienne à Nice » au-dessus du slogan
  - [x] Les autres `<h1>` deviennent des `<h2>`
  - [x] Les `<main>` deviennent des `<section>` (un seul `<main>` englobant)
- [x] Garder les 4 panneaux « Mon approche » dans le HTML (masqués en CSS plutôt que retirés du DOM)
- [x] Transformer la navigation en `<a href="#apropos">…</a>`

## Phase 2 — Performance (≈ ½ journée)

- [ ] **Décision** : garder le runtime `support.js` ou passer en HTML/CSS/JS pur ?
  - Pour : gros gain de vitesse (plus de React ni de Babel au chargement)
  - Contre : perte de l'édition via l'outil d'origine (sélecteur de couleur, option tarifs)
- [ ] Si oui : réécrire l'interactivité (surlignage de la navigation au scroll + onglets) en JS natif
- [ ] Convertir le portrait en WebP/AVIF, en deux tailles (`srcset`)
- [ ] Ajouter `width` / `height` et `fetchpriority="high"` sur le portrait principal
- [ ] Auto-héberger les polices (ou garder Google Fonts avec `display=swap`)
- [ ] (Optionnel) Remplacer l'iframe Google Maps par une image statique cliquable
- [ ] Mesurer avant/après avec PageSpeed Insights (mobile)
  - Score avant : ___ / Score après : ___

## Phase 3 — Contenu et structure

- [ ] **Décision** : passer à plusieurs pages ? Qui rédige (brouillons par Claude, validation par Marine) ?
- [ ] Pages dédiées par motif / spécialité (une recherche ciblée par page)
  - [ ] `/psychologue-anxiete-nice`
  - [ ] `/psychotraumatisme-nice`
  - [ ] `/deuil`
  - [ ] `/perinatalite-parentalite-nice` (vraie spécialité, peu concurrencée)
  - [ ] `/therapie-des-schemas-nice` (idem)
  - [ ] `/teleconsultation`
- [ ] Section FAQ avec balisage `FAQPage`
  - [ ] Les séances sont-elles remboursées ?
  - [ ] Psychologue, psychiatre, psychothérapeute : quelle différence ?
  - [ ] Combien de séances faut-il prévoir ?
  - [ ] Recevez-vous les adolescents ?
- [ ] Page « Mentions légales »
- [ ] Mettre à jour `sitemap.xml` avec les nouvelles pages
- [ ] (Plus tard) Quelques articles de fond, par exemple « Qu'est-ce que la thérapie des schémas ? »

## Phase 4 — Hors site (SEO local : souvent le plus gros levier)

- [ ] **Fiche Google Business Profile** (priorité n°1 pour apparaître dans le bloc carte)
  - [ ] Créer ou revendiquer la fiche
  - [ ] Bonne catégorie (« Psychologue »)
  - [ ] Horaires, téléphone, lien vers le site
  - [ ] Photos du cabinet
  - [ ] Encourager les avis, dans le respect du code de déontologie
- [ ] Cohérence NAP (nom, adresse, téléphone strictement identiques partout)
  - [ ] Doctolib
  - [ ] Psychologue.net
  - [ ] Annuaire santé RPPS
  - [ ] PagesJaunes
  - [ ] Association Mes Petits Pois (demander un lien vers le site)
- [ ] Google Search Console
  - [ ] Vérifier le domaine
  - [ ] Soumettre le sitemap
  - [ ] Demander l'indexation de la page d'accueil
- [ ] (Bonus) Bing Webmaster Tools

---

## Questions ouvertes

- [ ] Garder le format actuel (`support.js`) ou passer en HTML pur ?
- [ ] Une fiche Google Business Profile et un profil Doctolib existent-ils déjà ?
- [ ] Marine est-elle prête à rédiger (ou relire) du contenu pour les pages par motif et la FAQ ?
- [x] Les horaires affichés (samedi 9h–18h) sont-ils exacts ? Oui, confirmés (lundi–samedi 9h–18h). À reprendre tels quels dans la fiche Google.
- [x] Code postal : 06300 (corrigé partout sur le site). Utiliser 06300 sur toutes les fiches (cohérence NAP).
- [ ] Autres profils à ajouter dans `sameAs` (Psychologue.net, annuaire RPPS…) ? Seul Doctolib y figure pour l'instant.

## Ordre recommandé

1. Phase 1 + fiche Google + Search Console (peu d'effort, l'essentiel du gain)
2. Phase 2
3. Phase 3, selon le temps disponible
