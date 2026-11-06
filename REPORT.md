# Comptes rendus DataOps

**Nom :** Mariem Tanabene

## TP1

**Q1.** Le fichier `requirements.txt` garde la liste des bibliothèques et de leurs versions. Avec une seule commande, n'importe qui peut refaire le même environnement sur son PC.

**Q2.** Le rapport affiche 8 lignes au lieu de 5. Le script croit que `tunisia`, `MOROCCO` et ` Tunisia` (avec un espace) sont des pays différents.

**Q3.**

| order_id | Problème | Dimension |
|---|---|---|
| 1004 | quantité négative | Validité |
| 1008 | client manquant | Complétude |
| 1013 | date impossible (2026-13-45) | Validité |
| 1010 | commande en double | Unicité |
| 1006, 1016, 1023 | pays écrit de plusieurs façons | Cohérence |
| 1019 | prix de 4500 au lieu de 45 | Exactitude |

**Q4.** Le total est trop haut (surtout à cause du prix de 4500 et du doublon). Mais on ne peut pas le savoir sans recalculer, car certaines erreurs l'augmentent et d'autres le diminuent.

**Q5.** Avantages des fonctions : on peut tester chaque étape seule, et on trouve plus vite d'où vient un problème.

**Q6.** Le bon total est 7655.90. L'ancien script donnait 12771.10, donc 5115.20 de trop, soit environ 67 % d'erreur.

**Q7.** La commande 1019 (prix de 4500). Elle a été rejetée par la règle 4, `invalid_unit_price`.

**Q8.** On garde les lignes rejetées pour savoir ce qui a été écarté et pourquoi, et pour pouvoir les corriger plus tard. Si on les supprime, on perd des données sans le savoir.

**Q9.** Avec `> -5`, la quantité -2 n'est plus rejetée et le test `test_negative_quantity_is_rejected` échoue. Quand j'ai remis `> 0`, les 5 tests sont repassés.

**Q10.** Un test unitaire vérifie que mon code est correct. Un data check vérifie que les données du jour sont bonnes.

**Q11.**

| | Ancien script | Pipeline |
|---|---|---|
| Jour 2 : erreur ? | Non | Oui : `PIPELINE STOPPED`, 100 % de lignes rejetées |
| Jour 2 : total | 270200.0 (faux) | Rien n'est publié |
| Jour 3 : erreur ? | Oui, il plante : `KeyError: 'unit_price'` | `PIPELINE STOPPED: Missing columns: ['unit_price']` |

**Q12.** Jour 2 : les prix sont multipliés par 100 (449 devient 44900). Jour 3 : la colonne `unit_price` s'appelle maintenant `price`.

**Q13.** Le jour 2 est le plus dangereux : le script ne plante pas et donne un total faux qui a l'air normal. Le jour 3, il plante, donc on voit le problème.

**Q14.** Un planificateur (cron, Airflow), un outil de CI comme GitHub Actions, ou un outil de monitoring. Ils lisent le code de sortie pour savoir si le job a réussi et envoyer une alerte si besoin.

## TP2

**Q1.** Non. Tant que je travaillais sur la branche, `main` n'avait pas la règle 6. Elle est arrivée sur `main` au moment du `git merge --no-ff feature/country-rule`. C'est utile parce que le rapport de la nuit tourne depuis `main` : je peux essayer ma règle sur la branche sans risquer de casser le rapport.

**Q2.** Avec une pull request, quelqu'un d'autre peut relire mon code avant la fusion, et les tests se lancent tout seuls. Un `git merge` sur mon PC ne fait ni l'un ni l'autre.

**Q3.** `md5: 99df2450a60018e29f0dd952cad2eef2.dir`. C'est l'empreinte du contenu du dossier `data/raw`. Si je change un seul caractère dans un CSV, l'empreinte devient complètement différente, et DVC sait que les données ont changé.

**Q4.** C'est le fichier `data/.gitignore`. Il contient `/raw`, donc Git ne regarde plus ce dossier.

**Q5.** Ils portent le nom de leur empreinte md5. Deux versions du même fichier ont donc deux noms différents et ne s'écrasent jamais.

**Q6.** Le fichier `raw.dvc` ne garde que l'empreinte des données. Quand les données changent, seule l'empreinte change, donc deux lignes suffisent. Mais il faut quand même commiter, sinon cette version des données n'est liée à aucun commit et je ne pourrai pas la retrouver.

**Q7.** Non, le code n'a pas bougé, seules les données ont changé. Le total est passé de 7655.90 (data-v1) à 8835.40 (data-v2).

**Q8.** `git checkout data-v1 -- data/raw.dvc` a seulement remis l'ancien petit fichier pointeur. `dvc checkout` a ensuite remis les vrais CSV de la v1 dans `data/raw`.

**Q9.** Le pointeur dit v1, mais les fichiers sur le disque sont encore ceux de la v2. Je l'ai testé : le pipeline donne toujours 8835.40. C'est dangereux parce qu'il n'y a aucune erreur, et on peut donner un mauvais chiffre en croyant avoir l'ancien.

**Q10.** `git clone` lui a donné le code, l'historique et les fichiers pointeurs, mais pas les CSV. `dvc pull` lui a donné les vrais CSV, pris dans le remote.

**Q11.** Il aurait le pointeur de la v2, mais les fichiers ne seraient pas dans le remote. `dvc pull` ne les trouverait pas, donc il n'aurait pas les nouvelles données et ne pourrait pas obtenir 8835.40.

### Historique Git

```
* 2ad71bf (HEAD -> main) Describe the report as a DVC stage
* 1c52d41 (tag: data-v2) Data v2: late orders for 2026-10-05
* 26b4e40 Add the DVC remote
* e20b0d1 (tag: data-v1) Track raw data with DVC
*   7623aa9 Merge the country rule
|\
| * 655fcb2 (feature/country-rule) Reject orders from countries the shop does not sell to
|/
* ffee020 Finish TP1, add DVC to requirements
* aa02ba7 (tag: tp1, origin/main) Add CI workflow for testing and pipeline execution
* b987bfb Add pipeline, data checks and unit tests
* f8940b0 Empty project with requirements
```
