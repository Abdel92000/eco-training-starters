# Backlog heavy-shop

## Contexte du projet

Le projet represente une boutique catalogue avec listes, recherche, detail produit, panier et checkout simplifie.

## User story 1

- Contexte: En tant que visiteur de la boutique, je veux que les promotions ne se rechargent pas automatiquement toutes les 5 secondes, afin de reduire les requetes reseau inutiles et la consommation energetique cote client.
- Objectif: Supprimer le polling automatique sur /api/promotions et le remplacer par un chargement unique a l'ouverture de la page.
- Bonne pratique d eco-conception ciblee: Eviter les requetes reseau non declenchees par une action utilisateur (RGESN — limiter les echanges de donnees inutiles).
- KPI associe: Nombre de requetes vers /api/promotions declenchees en 30 secondes d'inactivite (objectif : 1 seule requete au lieu de 6).
- Repo ou ecran concerne: frontend/src/ShopApp.tsx — composant HomePage, useEffect avec setInterval ligne 56.
- Critere de reussite: Aucune requete vers /api/promotions n'apparait apres le chargement initial dans l'onglet Network des DevTools.
- Niveau de priorite: haute

## User story 2

- Contexte: En tant que visiteur consultant la page de recherche, je veux que les resultats ne se rechargent pas a chaque caractere tape, afin de limiter le nombre de requetes envoyees au backend et reduire la charge serveur.
- Objectif: Ajouter un debounce d'au moins 300ms sur l'input de recherche avant de declencher l'appel a /api/search.
- Bonne pratique d eco-conception ciblee: Limiter la frequence des requetes declenchees par les interactions utilisateur (RGESN — optimiser les appels reseau).
- KPI associe: Nombre de requetes vers /api/search declenchees lors de la saisie d'un mot de 7 caracteres (objectif : 1 requete au lieu de 7).
- Repo ou ecran concerne: frontend/src/ShopApp.tsx — composant SearchPage, useEffect sur [query, category].
- Critere de reussite: La saisie de "cordage" ne declenche qu'une seule requete apres la fin de la frappe. Mesurable via l'onglet Network des DevTools.
- Niveau de priorite: haute

## User story 3

- Contexte: En tant que visiteur de la boutique, je veux que les donnees produit ne contiennent pas le meme texte repete 4 fois, afin de reduire le poids des reponses de l API.
- Objectif: Supprimer le champ duplicateMarketingCopy qui repete 4 fois la meme phrase dans chaque produit.
- Bonne pratique d eco-conception ciblee: Reduire la taille des donnees transferees entre le serveur et le client (RGESN — alleger les payloads API).
- KPI associe: Taille de la reponse /api/products (objectif : reduire d au moins 15% le poids du payload).
- Repo ou ecran concerne: data/products.json — champ duplicateMarketingCopy / endpoint GET /api/products.
- Critere de reussite: La reponse /api/products passe sous 15 KB contre 16 KB en baseline. Mesurable via l onglet Network DevTools colonne Size.
- Niveau de priorite: moyenne

## Plan d action sur 6 mois

| Mois | Action                                                            | Effort | Resultat attendu                                                   |
| ---- | ----------------------------------------------------------------- | ------ | ------------------------------------------------------------------ |
| M1   | US1 - Suppression du polling /api/promotions                      | Faible | 0 requete automatique sur l accueil                                |
| M2   | US2 - Debounce sur la recherche                                   | Faible | 1 requete par recherche au lieu de 7                               |
| M3   | US3 - Suppression du texte duplique dans les produits             | Faible | /api/products sous 15 KB                                           |
| M4   | Optimisation des images du catalogue (thumbnails, format, taille) | Moyen  | Catalogue sous 30 requetes                                         |
| M5   | Mise en cache des assets et des donnees backend                   | Moyen  | Plus de Cache-Control no-store ni de readFileSync a chaque requete |
| M6   | Nouvelle mesure EcoIndex et bilan                                 | Faible | Note A sur toutes les pages                                        |
