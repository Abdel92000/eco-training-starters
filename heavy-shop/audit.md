# Audit éco-conception — heavy-shop

## US 1 — Suppression du polling /api/promotions

**Page :** Accueil `/`

| Indicateur                     | Avant | Après |
| ------------------------------ | ----- | ----- |
| Requêtes /api/promotions (30s) | 6     | 0     |
| EcoIndex                       | 78.77 | 93.99 |
| Note                           | B     | A     |
| GES (gCO2e)                    | 1.42  | 1.12  |
| Eau (cl)                       | 2.14  | 1.68  |

## US 2 — Debounce sur la recherche

**Page :** Recherche `/search`

| Indicateur                       | Avant | Après |
| -------------------------------- | ----- | ----- |
| Requêtes /api/search ("cordage") | 7     | 1     |
| EcoIndex                         | 78.12 | 78.37 |
| Note                             | B     | B     |
| GES (gCO2e)                      | 1.44  | 1.43  |
| Eau (cl)                         | 2.16  | 2.15  |

## US 3 — Suppression du texte marketing dupliqué

**Pages :** Catalogue `/products` et fiche produit

| Indicateur              | Avant   | Après   |
| ----------------------- | ------- | ------- |
| Poids /api/products     | 16.4 KB | 13.4 KB |
| Poids /api/products/:id | 1.9 KB  | 1.6 KB  |
