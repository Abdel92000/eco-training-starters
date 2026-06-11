# Backlog heavy-shop

## Contexte du projet

Le projet represente une boutique catalogue avec listes, recherche, detail produit, panier et checkout simplifie.

## Format attendu

Completer au minimum 3 user stories.
Remplacer chaque champ entre crochets par votre contenu.

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
- KPI associe: Nombre de requetes vers /api/search declenchees lors de la saisie d'un mot de 8 caracteres (objectif : 1 requete au lieu de 8).
- Repo ou ecran concerne: frontend/src/ShopApp.tsx — composant SearchPage, useEffect sur [query, category].
- Critere de reussite: La saisie de "cordage" ne declenche qu'une seule requete apres la fin de la frappe. Mesurable via l'onglet Network des DevTools.
- Niveau de priorite: haute

## User story 3

- Contexte: En tant que visiteur de la boutique, je veux que les donnees produit ne contiennent pas le meme texte repete 4 fois, afin de reduire le poids des reponses de l API.
- Objectif: Supprimer le champ duplicateMarketingCopy qui repete 4 fois la meme phrase dans chaque produit.
- Bonne pratique d eco-conception ciblee: Reduire la taille des donnees transferees entre le serveur et le client (RGESN — alleger les payloads API).
- KPI associe: Taille de la reponse /api/products (objectif : reduire d au moins 30% le poids du payload).
- Repo ou ecran concerne: data/products.json — champ duplicateMarketingCopy / endpoint GET /api/products.
- Critere de reussite: La reponse /api/products passe sous 15 KB contre 22 KB en baseline. Mesurable via l onglet Network DevTools colonne Size.
- Niveau de priorite: moyenne

## Notes

- Vous pouvez ajouter d autres user stories si necessaire.
- Le niveau de detail attendu doit permettre une priorisation exploitable.
