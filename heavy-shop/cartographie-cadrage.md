# Cartographie de cadrage — heavy-shop

## Service réel de référence

**TennisFlash** est un service de collecte et livraison de raquettes de tennis pour le cordage à Paris.
Le parcours utilisateur principal est : commande en ligne → paiement Stripe → suivi de la demande.

**Catégorie retenue :** E-commerce / catalogue / réservation
**Repo miroir associé :** heavy-shop

Le rattachement est justifié par la présence d'un catalogue de prestations, d'un parcours de commande
et d'une page de paiement — structure identique à celle du heavy-shop.

---

## Périmètre du projet

Pages analysées sur le repo heavy-shop :

| Page | URL | Rôle |
|---|---|---|
| Accueil | / | Vitrine, carrousel promos, produits mis en avant |
| Catalogue | /products | Listing de tous les produits |
| Recherche | /search | Filtres par nom et catégorie |
| Fiche produit | /products/:id | Détail, galerie, stock, recommandations |
| Panier | /cart | Récapitulatif de la sélection |
| Checkout | /checkout | Finalisation de commande |

---

## Constats issus de l'ACV flash

Anti-patterns identifiés directement dans le code :

- Polling automatique toutes les 5s sur `/api/promotions` sans action utilisateur
- 4 appels API déclenchés simultanément au démarrage, y compris checkout et panier non visités
- Recherche qui envoie une requête à chaque frappe clavier sans délai
- 4 thumbnails identiques chargées sur chaque fiche produit (même SVG répété 4 fois)
- Champ `duplicateMarketingCopy` : même texte répété 4 fois dans chaque produit JSON
- Assets SVG de 1600×900px sans cache (`Cache-Control: no-store`)
- `readFileSync` exécuté à chaque requête côté backend, sans mise en cache

---

## Mesures EcoIndex — baseline

| Page | EcoIndex | Note | Requêtes | Taille (KB) | DOM |
|---|---|---|---|---|---|
| Accueil | 78.77 | B | 28 | 1785 | 109 |
| Catalogue | 71.98 | B | 56 | 1840 | 125 |
| Recherche | 78.12 | B | 28 | 1798 | 129 |

Le catalogue est la page la plus impactante : 56 requêtes contre 28 sur les autres pages.

---

## Objectifs du projet

- Réduire le nombre de requêtes réseau sur le catalogue de 56 à moins de 30
- Supprimer les requêtes non déclenchées par une action utilisateur
- Réduire le poids des assets images d'au moins 50% sur la fiche produit
- Alléger les payloads JSON en supprimant les données dupliquées
- Passer le score EcoIndex du catalogue en note A (score > 80)

---

## Indicateurs de suivi retenus

| Indicateur | Outil de mesure | Objectif cible |
|---|---|---|
| Score EcoIndex | Extension GreenIT | > 80 (note A) sur toutes les pages |
| Nombre de requêtes au chargement | DevTools Network | < 30 sur /products |
| Poids total des images | DevTools Network | < 500 KB sur fiche produit |
| Requêtes de polling | DevTools Network | 0 requête automatique après chargement |
| Taille payload /api/products | DevTools Network | < 15 KB |

---

## Contraintes

- Repo pédagogique : pas de déploiement en production, mesures en local uniquement
- Stack imposée : React + Vite (frontend), Express + TypeScript (backend)
- Durée du projet : 6 mois
- Ressource : 1 développeur
- Les optimisations ne doivent pas casser la logique fonctionnelle existante

---

## Lien avec TennisFlash

Chaque action identifiée sur heavy-shop est directement transposable :

| Problème heavy-shop | Équivalent TennisFlash |
|---|---|
| Polling promos toutes les 5s | Polling inutile sur disponibilités |
| Chargement checkout au démarrage | Chargement Stripe avant navigation |
| Images non optimisées | Photos de raquettes non redimensionnées |
| Payload produit verbeux | Payload commande avec données redondantes |
| Recherche sans debounce | Recherche de créneaux sans délai |
