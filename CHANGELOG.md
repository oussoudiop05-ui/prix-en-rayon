# Journal des versions — Prix en rayon

Chaque version : nom, date, session Claude, description des modifications.

## v11 — « Prédictions d'achats » (19 nov. 2026)
Session : https://claude.ai/code/session_01EvniZ57G7WZk9pTAk6Ycce

**Nouveautés**
- Nouvel onglet **Prédire** (5ᵉ onglet dans la barre de navigation).
- **Modèle de fréquence** : calcule l'intervalle moyen entre chaque achat par produit (nom + magasin), avec une **moyenne mobile pondérée** (les achats récents comptent plus).
- **Filtre variance** : les achats très irréguliers (cadeaux, dépenses exceptionnelles) sont automatiquement exclus via un coefficient de variation > 1.
- **Horizons sélectionnables** : 3 jours / 7 jours / 14 jours (sélection par groupe de boutons).
- **Minimum 3 achats** par produit pour apparaître dans les prédictions.
- **Score de confiance** (●●● / ●●○ / ●○○) : calculé d'après la régularité des intervalles.
- **Urgence visuelle** : carte rouge = en retard, carte accentuée = à faire bientôt, grisé = dans les prochains jours.
- **Bouton 🛒 Panier** par suggestion : ajoute le produit directement au panier si un prix correspondant existe.
- Indicateur ✓ pour les produits déjà dans le panier.
- Mise à jour automatique à chaque changement de données ou de navigation sur l'onglet.

**Modèle statistique**
- Regroupement par `(nom normalisé + magasin)`.
- Intervalles calculés entre achats consécutifs (en jours).
- Moyenne pondérée : poids = position dans la liste (plus récent = poids plus élevé).
- Date estimée du prochain achat = dernière date + intervalle moyen pondéré.
- Confiance = `max(0, (1 − CV) × 100)` où CV = écart-type / moyenne (CV > 1 → exclusion).

## v10 — « Export JSON direct » (19 nov. 2026)
Session : https://claude.ai/code/session_01EvniZ57G7WZk9pTAk6Ycce

**Nouveautés**
- Export JSON : téléchargement natif via Blob URL (fonctionne sur mobile sans passer par l'interface Claude).
- Bouton « 📋 Copier le JSON » pour copier tout le JSON dans le presse-papiers et le coller dans un fichier ou un chat.
- Fallback automatique : si le téléchargement échoue, le JSON s'affiche dans une zone de texte pour copier-coller.
- Format du fichier : `prix-en-rayon-sauvegarde-AAAA-MM-JJ.json` contenant prix, achats et panier.

**Usage**
- Dans ⚙️ Réglages → « ⬇ Sauvegarde JSON » : télécharge le fichier directement.
- Ou « 📋 Copier le JSON » : copier dans le presse-papiers, puis envoyer dans le chat pour analyse ou modification.

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
