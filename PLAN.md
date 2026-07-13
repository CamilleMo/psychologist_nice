# Plan : brancher le nom de domaine personnalisé

Objectif : servir le site sur le domaine de la cliente (ex. `camilleperrin-psy.fr`) au lieu de
`https://camillemo.github.io/psychologist_nice/`.

Dans tout ce document, remplacer `EXEMPLE.fr` par le vrai domaine.

**Ordre important** : vérifier le domaine (étape 1) *avant* de le brancher (étape 3). Cela évite
qu'un autre compte GitHub puisse revendiquer le domaine.

---

## Décision préalable : apex ou www ?

| Choix | URL finale | DNS nécessaire |
| --- | --- | --- |
| **Apex** (recommandé) | `https://EXEMPLE.fr` | 4 A + 4 AAAA sur la racine, + CNAME pour `www` |
| **Sous-domaine** | `https://www.EXEMPLE.fr` | 1 seul CNAME |

L'apex est plus court et plus naturel pour un cabinet. GitHub redirige automatiquement `www` vers
l'apex (et inversement) si les deux sont configurés — d'où le CNAME `www` en bonus ci-dessous.

---

## Étape 1 — Vérifier le domaine (GitHub + DNS)

But : prouver à GitHub qu'on possède le domaine.

1. GitHub → photo de profil → **Settings** → **Pages** → **Add a domain**.
   (Niveau *compte*, pas niveau dépôt.)
2. Saisir `EXEMPLE.fr`. GitHub affiche un enregistrement TXT à créer, de la forme :

   | Type | Nom | Valeur |
   | --- | --- | --- |
   | TXT | `_github-pages-challenge-CamilleMo` | *(code fourni par GitHub)* |

3. Créer ce TXT chez le registrar.
4. Attendre la propagation, vérifier :

   ```sh
   dig +short TXT _github-pages-challenge-CamilleMo.EXEMPLE.fr
   ```

5. Cliquer **Verify** sur GitHub.

---

## Étape 2 — Configurer le DNS (chez le registrar)

Enregistrements à créer sur la zone de `EXEMPLE.fr`.

### 2a. Apex (`EXEMPLE.fr`) — 4 enregistrements A

| Type | Nom | Valeur |
| --- | --- | --- |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |

### 2b. Apex — 4 enregistrements AAAA (IPv6, recommandé)

| Type | Nom | Valeur |
| --- | --- | --- |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |

> Si le registrar propose un `ALIAS` ou `ANAME` sur l'apex, on peut l'utiliser à la place des A/AAAA
> et le faire pointer vers `camillemo.github.io`.

### 2c. `www` — 1 CNAME

| Type | Nom | Valeur |
| --- | --- | --- |
| CNAME | `www` | `camillemo.github.io.` |

> La cible est `camillemo.github.io` — **sans** le nom du dépôt.

### 2d. Supprimer les enregistrements en conflit

Retirer tout A / AAAA / CNAME existant sur `@` et `www` qui pointerait ailleurs (page de parking du
registrar, ancien hébergeur…). Un CNAME ne peut pas coexister avec d'autres enregistrements sur le
même nom.

### 2e. Vérifier la propagation

```sh
dig +short EXEMPLE.fr           # doit renvoyer les 4 IP 185.199.x.153
dig +short www.EXEMPLE.fr       # doit renvoyer camillemo.github.io
```

Compter de quelques minutes à 24 h selon le TTL.

---

## Étape 3 — Brancher le domaine sur le dépôt (GitHub)

Une fois le DNS propagé :

```sh
gh api -X PUT repos/CamilleMo/psychologist_nice/pages -f cname=EXEMPLE.fr
```

Cela écrit aussi un fichier `CNAME` à la racine du dépôt. Le récupérer en local :

```sh
git pull
```

> Ce fichier `CNAME` doit rester dans le dépôt : s'il disparaît, le domaine se débranche.

Équivalent en interface : dépôt → **Settings** → **Pages** → **Custom domain**.

---

## Étape 4 — Forcer le HTTPS

GitHub provisionne un certificat Let's Encrypt automatiquement (peut prendre jusqu'à ~24 h après
que le DNS est correct).

```sh
gh api -X PUT repos/CamilleMo/psychologist_nice/pages -F https_enforced=true
```

Si la case « Enforce HTTPS » est grisée dans l'interface, c'est que le certificat n'est pas encore
émis : attendre, puis réessayer.

---

## Étape 5 — Vérifier

```sh
gh api repos/CamilleMo/psychologist_nice/pages --jq '{cname, status, https_enforced}'

curl -sI https://EXEMPLE.fr          | head -1   # attendu : HTTP/2 200
curl -sI http://EXEMPLE.fr           | head -1   # attendu : redirection 301 vers HTTPS
curl -sI https://www.EXEMPLE.fr      | head -1   # attendu : redirection 301 vers l'apex
```

Puis ouvrir `https://EXEMPLE.fr` dans un navigateur : cadenas présent, page affichée correctement
(le rendu dépend de `support.js`, donc à contrôler visuellement).

---

## Étape 6 — Mettre à jour le README

Remplacer l'URL `camillemo.github.io/psychologist_nice` par `https://EXEMPLE.fr` dans `README.md`.

---

## Pannes courantes

| Symptôme | Cause probable |
| --- | --- |
| 404 sur le domaine | DNS pas encore propagé, ou fichier `CNAME` absent du dépôt |
| « Domain does not resolve to the GitHub Pages server » | A/AAAA manquants ou incorrects |
| Case HTTPS grisée | Certificat pas encore émis — attendre |
| Le domaine se débranche tout seul | Le fichier `CNAME` a été supprimé par un push |
| Page blanche mais HTTP 200 | Problème de rendu côté `support.js`, pas de DNS |

---

## Références

- IP et cible CNAME vérifiées le 2026-07-13 sur la doc GitHub :
  <https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site>
