# Journal des versions — Prix en rayon

Chaque version : nom, date, session Claude, description des modifications.

## v9 — « Un produit, plusieurs références » (8 nov. 2026)
Session : https://claude.ai/code/session_01EvniZ57G7WZk9pTAk6Ycce

**Nouveautés**
- Recherche : un produit = une carte groupée avec toutes ses références (magasin, prix, €/kg, quantité), la meilleure en premier.
- Fusion des doublons : bandeau « Doublons possibles » dans Rechercher, fusion, refus (« garder séparés ») et annulation.
- Achat global : saisir uniquement un montant (sans poids), avec choix facultatif des produits.
- « Enregistrer et mettre au panier » dans le formulaire d'ajout.
- Panier : choix de la référence (magasin) pour chaque produit.
- Dépenses : bascule automatique sur la période contenant des achats si « Ce mois » est vide.

**Corrections**
- Chargement infini (erreur de script) et affichage mobile (viewport).
- Gemini : modèles retirés remplacés par une détection automatique ; commandes du type « ajoute X à ma liste de courses » mieux comprises (liste de courses ≠ base de prix).
- Service worker v2 (réseau d'abord) : fin des anciennes versions en cache.
