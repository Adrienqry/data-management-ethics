# CM 2 lab — Tue 24 Nov, 3:10 – 3:50

The former 1-hour CTP 1 is now the hands-on lab at the end of CM 2.

1. **Get READY (15 min):** `docker compose up -d` in `shopfast-kit`, activate the venv, start OpenRefine, run `python check_setup.py`.
2. **ShopFast data inventory (25 min, pairs):** fill `data_inventory_template.csv` for the 7 datasets (description, owner/steward/custodian, personal data, sensitivity, row count, 2+ issues). Use `explore_shopfast.sql` to query the database.
3. Push it to your project repo as `data_inventory_<group>.csv` — reused in CTP 3 and CTP 4.

## Partie 2 — Inventaire finalisé le 24 novembre 2026

Le livrable est **[data_inventory_Adrien_Queroy.csv](data_inventory_Adrien_Queroy.csv)**. Il contient les 7 jeux de données, une description par jeu, le domaine métier, les fonctions responsables proposées, la présence de données personnelles, la sensibilité, le nombre de lignes et au moins deux constats par jeu.

### Informations à valider

- **Nom du livrable** : `data_inventory_Adrien_Queroy.csv`, avec le suffixe choisi pendant l’exercice.
- **Owner, steward et custodian** : les fonctions du CSV sont des propositions pour l'entreprise fictive ShopFast, pas des responsabilités nominatives confirmées. À valider avec la répartition retenue par le groupe. L'owner décide des règles métier ; le steward suit la qualité et les définitions ; le custodian exploite les systèmes.
- **Fréquences de mise à jour** : non documentées dans les sources examinées. `To be confirmed` est conservé pour les 7 jeux ; aucune fréquence n'a été inventée.
- **Classement de `products`** : `Internal` est une proposition pour le fichier complet, qui inclut les fournisseurs et le statut d'activation. Les noms et prix publics seuls pourraient être classés `Public` selon la grille du cours.

### Synthèse des données

Les effectifs ci-dessous proviennent des CSV du dossier `shopfast-kit/data/`. Ils comptent les enregistrements, y compris les doublons.

| Jeu de données | Lignes | Données personnelles | Sensibilité retenue | Principaux constats |
|---|---:|:---:|---|---|
| `customers` | 1241 | Y | Restricted | Codes pays `FR`/`fr` ; formats de naissance différents ; email contenant `@@`. |
| `products` | 52 | N | Internal | SKU dupliqué ; catégories non normalisées ; un prix manquant et un prix négatif. |
| `orders` | 5207 | Y | Confidential | 7 identifiants dupliqués sur 14 lignes ; 14 références client absentes ; 11 dates manquantes ; statuts non standard. |
| `order_items` | 9193 | Y | Confidential | 8 quantités égales à zéro ou négatives ; 4 références produit absentes. |
| `payments` | 4902 | Y | Confidential | 6 montants différents du total de commande ; 4 paiements antérieurs à leur commande. |
| `support_tickets` | 422 | Y | Restricted | 53 tickets avec un motif d'IBAN non masqué ; 62 notes avec une date de naissance ; incohérence sujet/message observée. |
| `marketing_campaign_export` | 600 | Y | Confidential | Consentement et preuve associés absents de l'export ; date d'extraction absente. |

### Méthode et limites des constats

- Les CSV complets ont été examinés : valeurs manquantes, doublons, catégories, montants, dates et correspondance des identifiants entre tables. Les cinq premières lignes de chaque table ont aussi été affichées dans PostgreSQL pendant l'exercice.
- Les effectifs des 7 jeux ont été recomptés sur les CSV. L'auto-test exécuté dans le terminal a également confirmé 1241 clients dans PostgreSQL. Le script `explore_shopfast.sql`, fourni avec le lab, contient la requête pour recompter les 7 tables dans la base.
- Les contrôles temporels utilisent comme référence le **24 novembre 2026**. Les 5 commandes futures mentionnées dans le CSV le sont par rapport à cette date.
- Les 6 écarts de montant et les 4 écarts de date entre `payments` et `orders` sont calculés uniquement pour les commandes dont l'identifiant est unique. Ce sont des incohérences à investiguer : les erreurs peuvent provenir de `orders`. Un écart de montant doit aussi être confronté aux règles métier, notamment aux éventuels paiements partiels.
- Un champ `card_last4` vide n'est pas automatiquement une erreur. Son caractère obligatoire dépend du moyen de paiement et des règles métier.
- Les 53 motifs d'IBAN sont détectés par leur forme, sans validation de leur clé de contrôle. La présence d'un IBAN dans un message justifie `Restricted` selon la grille du cours. Les dates de naissance présentes dans les notes constituent un autre point de vigilance sur le texte libre.
- Pour `customers`, le niveau `Restricted` correspond à la présence conjointe de dates de naissance et d'adresses, selon la grille du cours.
- `orders`, `order_items` et `payments` sont rattachables à des clients par leurs identifiants. L'absence de nom dans ces tables ne suffit donc pas à les considérer comme anonymes.
- Les **600 contacts marketing ont tous `marketing_consent=true` dans la source actuelle**. Aucun doublon d'identifiant ou d'email normalisé n'a été trouvé dans cet export. Les constats portent sur la traçabilité : le fichier seul ne permet pas de vérifier le consentement ni de savoir à quelle date il reflétait la source. Cela ne démontre pas l'envoi d'une campagne sans consentement.
- Les données ShopFast sont fictives, comme indiqué dans le README du kit. Les constats de sensibilité appliquent la grille pédagogique du support, pages 25–26.

### Vérifications finales et suite

Le livrable a été relu comme un CSV : 12 colonnes, 7 lignes de données, aucun champ vide, effectifs conformes aux fichiers source et au moins deux constats par jeu. Les valeurs à confirmer sont explicitement signalées.

La partie 1 a également été réalisée : PostgreSQL démarré, environnement Python 3.12 activé, auto-test `READY` et interface OpenRefine ouverte, d'après les résultats partagés dans le terminal.

L'inventaire servira de référence pour les CTP 3 et 4. Les exercices suivants traiteront la correction des problèmes recensés ici.

