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

## Retour d'Aurelien (2026-10-08)

Conseils reçus par e-mail, repris dans les phases ci-dessous (repérés par « (Aurelien) ») :

| Conseil | Où dans le plan | État |
| --- | --- | --- |
| Publier une version HTML statique sans `support.js` : la page était téléchargée deux fois, le script bloquait le `<head>`, chargeait React depuis unpkg et redessinait toute la page. Les robots qui n'exécutent pas le JavaScript (Bing en partie, GPTBot, ClaudeBot, PerplexityBot) voyaient un menu cassé | Phase 2 | Fait (PR #6) |
| Ajouter un fichier `llms.txt` pour décrire le site aux IA | Phase 2 bis | À faire |
| Mentions légales et politique de confidentialité, obligatoires pour un site professionnel (loi LCEN) | Phase 3 | Fait (`/mentions-legales/`, `/confidentialite/`) |
| Déclarer le site dans Google Search Console et soumettre le sitemap | Phase 4 | En cours |
| Créer ou relier une fiche Google Business Profile, essentielle pour « psychologue Nice » | Phase 4 | À faire |

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

- [x] **Décision** : garder le runtime `support.js` ou passer en HTML/CSS/JS pur ? → HTML/CSS/JS pur (Aurelien)
  - Règle aussi : double téléchargement de la page, script bloquant dans le `<head>`, menu cassé pour les robots sans JavaScript (Bing, robots des IA)
  - Pour : gros gain de vitesse (plus de React ni de Babel au chargement)
  - Contre : perte de l'édition via l'outil d'origine (sélecteur de couleur, option tarifs)
- [x] Si oui : réécrire l'interactivité (surlignage de la navigation au scroll + onglets) en JS natif (Aurelien)
- [x] Convertir le portrait en WebP/AVIF, en deux tailles (`srcset`)
- [x] Ajouter `width` / `height` et `fetchpriority="high"` sur le portrait principal
- [x] Auto-héberger les polices (ou garder Google Fonts avec `display=swap`) → Google Fonts conservé, déjà en `display=swap`
- [ ] (Optionnel) Remplacer l'iframe Google Maps par une image statique cliquable — non fait : l'iframe est déjà en `loading="lazy"` (chargée seulement près du bas de page), et une image statique Google demande une clé API
- [ ] Mesurer avant/après avec PageSpeed Insights (mobile)
  - Score avant : ___ / Score après : ___

## Phase 2 bis — Référencement par les IA (ChatGPT, Claude, Perplexity…)

- [ ] Créer `/llms.txt` : présentation courte du cabinet (qui, où, pour quoi, comment prendre rendez-vous) au format Markdown, avec les liens vers les pages du site (Aurelien)
- [ ] Le mettre à jour quand de nouvelles pages sont créées (phase 3)
- [x] `robots.txt` n'exclut aucun robot (GPTBot, ClaudeBot, PerplexityBot compris)

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
- [x] Page « Mentions légales » — obligatoire (loi LCEN) : identité, n° RPPS, adresse, contact, hébergeur (GitHub Pages) (Aurelien)
- [x] Page « Politique de confidentialité » — obligatoire : données collectées (aucun formulaire ; prise de rendez-vous via Doctolib), carte Google Maps et polices Google intégrées, droits RGPD (Aurelien)
- [x] Lien vers ces deux pages dans le pied de page
- [ ] Après héberger les polices et charger la carte au clic : retirer Google Fonts et Google Maps de la politique de confidentialité
- [ ] Mettre à jour `sitemap.xml` avec les nouvelles pages
- [ ] (Plus tard) Quelques articles de fond, par exemple « Qu'est-ce que la thérapie des schémas ? »

## Phase 4 — Hors site (SEO local : souvent le plus gros levier)

- [ ] **Fiche Google Business Profile** (priorité n°1 pour apparaître dans le bloc carte) (Aurelien)
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
- [ ] Google Search Console (Aurelien)
  - [ ] Vérifier le domaine
  - [ ] Soumettre le sitemap (Aurelien)
  - [ ] Demander l'indexation de la page d'accueil
- [ ] (Bonus) Bing Webmaster Tools

---

## Questions ouvertes

- [x] Garder le format actuel (`support.js`) ou passer en HTML pur ? → HTML pur
- [ ] Une fiche Google Business Profile et un profil Doctolib existent-ils déjà ?
- [ ] Marine est-elle prête à rédiger (ou relire) du contenu pour les pages par motif et la FAQ ?
- [x] Les horaires affichés (samedi 9h–18h) sont-ils exacts ? Oui, confirmés (lundi–samedi 9h–18h). À reprendre tels quels dans la fiche Google.
- [x] Code postal : 06300 (corrigé partout sur le site). Utiliser 06300 sur toutes les fiches (cohérence NAP).
- [ ] Autres profils à ajouter dans `sameAs` (Psychologue.net, annuaire RPPS…) ? Seul Doctolib y figure pour l'instant.

## Ordre recommandé

1. Phase 1 + fiche Google + Search Console (peu d'effort, l'essentiel du gain)
2. Phase 2
3. Mentions légales + politique de confidentialité (obligation légale, à ne pas repousser) et `llms.txt`
4. Reste de la phase 3, selon le temps disponible
