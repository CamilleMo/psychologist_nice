# Cabinet Psychologue

Site vitrine du cabinet de psychologie.

## Site en ligne

https://marine-dujardin-psychologue.fr

Publié via GitHub Pages depuis la branche `main` (racine du dépôt). Chaque push sur `main` redéploie le site automatiquement.

Le domaine personnalisé est configuré via le fichier `CNAME` à la racine du dépôt — ne pas le supprimer, sinon le domaine se débranche.

## Fichiers

| Fichier | Rôle |
| --- | --- |
| `index.html` | La page du site |
| `support.js` | Runtime chargé par la page |
| `image-slot.js` | Composant `<image-slot>` (pas encore utilisé) |
| `.nojekyll` | Désactive le traitement Jekyll sur GitHub Pages |
| `CNAME` | Nom de domaine personnalisé (`marine-dujardin-psychologue.fr`) |

## Aperçu local

```sh
python3 -m http.server 8000
```

Puis ouvrir http://localhost:8000

## À compléter

- Numéro de téléphone (actuellement un placeholder)
- Numéro RPPS
