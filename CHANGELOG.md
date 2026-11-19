# Journal des versions — Prix en rayon

## v14 — « Navigation libre par mois & tickets groupés » (19 nov. 2026)
Session : https://claude.ai/code/session_01EvniZ57G7WZk9pTAk6Ycce

**Dépenses — navigation mensuelle libre**
- Suppression du filtre «30 jours» : il reste désormais 3 périodes uniquement — «Par mois», «Tout» et «📅 Dates».
- «Par mois» devient un vrai navigateur mensuel : on peut reculer librement vers n'importe quel mois passé, même s'il ne contient aucun achat. Un mois vide affiche «Aucune dépense ce mois-ci» sans rediriger automatiquement.
- La flèche ‹ (mois précédent) n'est plus verrouillée au premier mois ayant des achats : navigation libre sans plancher.
- Suppression de la bascule automatique sur «30 jours» (`spendAuto`) : elle ne se déclenchait plus que sur les mois vides et provoquait des comportements inattendus.

**Dépenses — affichage des tickets groupés (Leclerc 7 nov.)**
- Correction de l'affichage dans le calendrier des dépenses pour les achats enregistrés en format «panier» (via caisse).
- Avant : le détail du jour affichait des lignes vides car le code lisait `p.name` et `p.count`, absents du format groupé `{items:[]}`.
- Maintenant : si l'achat contient un tableau `items`, les articles sont affichés depuis `items[].name`, `items[].qty` et `items[].total`, avec un résumé «… et N articles» si le ticket dépasse 8 lignes.

## v13 — « Légende confiance & correctif navigation mois » (19 nov. 2026)
Session : https://claude.ai/code/session_01EvniZ57G7WZk9pTAk6Ycce

**Prédictions — légende inline**
- Une légende compacte s'affiche directement dans l'onglet Prédire sous le compteur de produits.
- Elle explique les 3 niveaux de confiance des pastilles : ●●● (régulier = fiable), ●●○ (assez régulier), ●○○ (irrégulier = indicatif).
- Deux indicateurs couleur : 🟢 dans les délais, 🔴 en retard.

**Dépenses — correctif navigation mensuelle (suite)**
- `buildBuckets()` utilisait `today(0)` pour calculer le début du mois au lieu de `agMonth` → KPIs, graphiques et calendrier affichaient toujours novembre même en naviguant vers octobre. Correction : `(agMonth || today(0).slice(0,7)) + '-01'`.
- La bascule automatique sur «30 jours» (`spendAuto`) ne se déclenche plus quand l'utilisateur navigue manuellement vers un autre mois.
- `spendAuto` est réinitialisé lors des clics ‹/› et lors du changement de période, pour débloquer la navigation libre.



Chaque version : nom, date, session Claude, description des modifications.

## v12 — « Timeline prédictions & correctif Dépenses » (19 nov. 2026)
Session : https://claude.ai/code/session_01EvniZ57G7WZk9pTAk6Ycce

**Correctif**
- **Dépenses — navigation mensuelle** : la fonction `inPeriod()` comparait toujours au mois courant (`today(0).slice(0,7)`) au lieu d'utiliser `agMonth`. Résultat : les achats des mois précédents n'apparaissaient pas quand on reculait avec la flèche ‹. Correction : `d.slice(0,7) === (agMonth || today(0).slice(0,7))`.

**Prédictions — refonte de l'affichage (Option A : timeline)**
- Remplacement de la vue en cartes par une **timeline horizontale à défilement** (scroll-snap).
- **3j / 7j** : une colonne par jour ; **14j** : colonnes groupées par semaine.
- Les articles **en retard** (date estimée dépassée) apparaissent dans une **ligne rouge fixe** en haut de la timeline, indépendamment de l'horizon sélectionné.
- Chaque suggestion reste cliquable pour ajouter au panier (bouton 🛒) ; indicateur ✓ si déjà dans le panier.
- Score de confiance (●●● / ●●○ / ●○○) et numéro de jour conservés sur chaque chip.

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
