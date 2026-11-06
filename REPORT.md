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

