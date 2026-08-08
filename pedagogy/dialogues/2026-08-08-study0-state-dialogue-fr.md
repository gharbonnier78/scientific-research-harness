# Dialogue pédagogique verbatim — état scientifique de Study 0

Status: **verbatim learning trace / non-authoritative / non-blocking**

This file preserves the user question and assistant answer as they were produced in conversation. Typos, phrasing, doubts, emphasis, and progression are intentionally retained. It is not a cleaned-up article and must not be used as scientific evidence or runner policy.

---

## User

Avant de continuer refaisons un point sur ce qu on cherchait a demontrer et oú on est actuellement " en francais dans le texte

## Assistant

Oui. Le plus important est de **revenir à la question scientifique initiale**, parce que toute la mécanique récente — bootstrap, coverage, chronicle, benchmark, SeedSequence — n’est qu’un moyen de rendre la réponse crédible.

## 1. Ce qu’on cherchait à démontrer au départ

L’expérience Study 0 partait d’une question assez simple :

**Peut-on réduire un embedding facial gelé de 512 dimensions à 128 dimensions, avec une projection apprise, sans dégrader de manière importante la capacité de vérification biométrique ?**

Plus précisément, on comparait quatre représentations :

- embedding brut 512D ;
- projection aléatoire 512→128 ;
- PCA 512→128 entraînée uniquement sur TRAIN ;
- projection linéaire 512→128 apprise avec une loss contrastive de type Siamese.

L’intérêt n’était pas simplement de montrer que « le réseau apprend quelque chose ».

Le pari intéressant était plutôt :

> une projection 128D apprise pourrait conserver suffisamment de l’information utile à la vérification pour devenir une représentation comprimée crédible face au 512D original.

Avec derrière une perspective d’ingénierie : stockage de grandes galeries, transferts, calculs 1:N, edge, etc.

Mais **Study 0 n’était volontairement pas encore une démonstration industrielle**. Pas de population nationale, pas de validation 1:N à grande échelle, pas de capteurs variés, pas de fairness/demographics, pas de latence industrielle.

C’était d’abord une preuve méthodologique contrôlée.

---

## 2. Quelle était la claim réellement testée

La claim importante n’était pas :

> « 128D est meilleur que 512D ».

Ni même :

> « le Siamese gagne en moyenne ».

On avait choisi une question beaucoup plus exigeante :

**la représentation 128D apprise est-elle non-inférieure au 512D de référence en FNMR, à un operating point FMR fixé ?**

Avec :

\[
\Delta_{\mathrm{FNMR}}
=
\mathrm{FNMR}_{128}
-
\mathrm{FNMR}_{512}
\]

et une marge de non-infériorité :

\[
\delta = 0.03
\]

Donc, en simplifiant :

- Δ < 0 : 128D paraît meilleur ;
- Δ légèrement > 0 : un peu moins bon, mais potentiellement acceptable ;
- pour conclure à la non-infériorité, il faut que **l’incertitude entière**, pas seulement la moyenne, reste compatible avec la marge +0,03.

C’est pour cela que l’ancien résultat :

> moyenne Δ ≈ +0,0228

n’était **pas suffisant** pour dire « non-inférieur ».

L’UCB dépassait la marge. Le gate avait donc correctement échoué.

Et c’est très important : **nous n’avons jamais transformé ce résultat défavorable en succès narratif**.

---

## 3. Puis on a découvert un problème plus profond

En auditant la statistique, on a découvert que l’ancien bootstrap était fait **au niveau des paires**.

Or LFW DevTest contient 1000 paires, mais ces paires ne sont pas indépendantes : des identités apparaissent dans plusieurs paires.

La vraie unité de dépendance est donc beaucoup plus proche du **sujet** que de la paire.

Autrement dit, notre intervalle de confiance pouvait être faux même si tout le code du bootstrap fonctionnait parfaitement.

C’est l’erratum **E-STAT-001**.

À ce moment-là, la question scientifique est devenue :

> **avant de réinterpréter le résultat 512→128, sommes-nous capables d’estimer correctement son incertitude compte tenu de la dépendance entre identités ?**

Et c’est là que Study 0 v0.2.2 a commencé.

---

## 4. Ce qu’on a corrigé méthodologiquement

Le nouveau bootstrap est un **subject-slot bootstrap pondéré** adapté au graphe sparse réellement observable dans LFW.

Nous ne fabriquons surtout pas une matrice complète de paires 963×963.

On garde uniquement les 1000 edges réellement observés.

Pour un tirage bootstrap des sujets avec multiplicité \(m_i\) :

- edge genuine \((i,i)\) : poids \(m_i\) ;
- edge impostor \((i,j)\) : poids \(m_i m_j\).

C’est une manière de reproduire le resampling des sujets **sans inventer de comparaisons biométriques inexistantes**.

Le source mapping a maintenant été matérialisé exactement :

- 1000 paires ;
- 500 genuine ;
- 500 impostor ;
- 963 sujets ;
- sources LFW versionnées et hashées ;
- subject map pseudonymisée ;
- les scores historiques Study 0 n’ont pas été ouverts pendant cette correction.

PR #4 est désormais effectivement présente sur `main`, ce que le reviewer a confirmé indépendamment.

---

## 5. Mais corriger un estimateur ne suffit pas

C’est probablement le point le plus important de tout ce travail.

Nous aurions pu écrire le nouveau bootstrap, le lancer sur les anciens scores et regarder si la conclusion devenait plus favorable.

Nous avons choisi de **ne pas le faire**.

Pourquoi ?

Parce qu’un estimateur statistique peut sembler raisonnable et être faux.

Donc avant de regarder le résultat que nous voulons finalement interpréter, nous demandons :

> **dans des mondes simulés où la vérité est connue, cet estimateur produit-il réellement des intervalles ayant la couverture qu’il prétend avoir ?**

C’est le **coverage gate**.

Cinq régimes ont été figés, dont :

- null indépendant ;
- null avec dépendance sujet ;
- non-infériorité +0,015 ;
- frontière +0,03 ;
- régime au-delà de la marge +0,05.

Et pour chaque régime, on contrôle séparément :

- ΔFNMR représentation ;
- FNMR opérationnel ;
- FMR opérationnel.

Pas de moyenne globale qui pourrait masquer un échec.

---

## 6. Où on en est exactement aujourd’hui

### Scientifiquement

Nous **n’avons toujours pas de nouveau résultat corrigé de Study 0**.

C’est volontaire.

Donc actuellement :

**E-STAT-001 : OPEN**  
**G2 : FAIL**  
**Study 1 : BLOCKED**

Et nous ne savons toujours pas si la projection Siamese 128D satisfait ou non la claim de non-infériorité après correction statistique.

C’est une réponse importante : **la question scientifique biométrique reste ouverte**.

---

## 7. Ce qui est désormais solide

En revanche, beaucoup de fondations sont maintenant considérablement plus fortes qu’au début.

Nous avons :

- le bon graphe sparse et les bonnes identités ;
- le mapping exact des 1000 paires ;
- les sources et hashes ;
- le bootstrap sujet implémenté ;
- les règles de seuil, ties et sentinel explicitement figées ;
- les comportements degenerates explicitement bloquants ;
- les seeds et checkpoints définis ;
- les scénarios de coverage preregistrés ;
- les métriques et critères de réussite figés ;
- la séparation représentation / opérationnel ;
- l’interdiction de recalibrer TEST dans le chemin opérationnel ;
- l’interdiction de regarder les anciens scores avant validation de l’estimateur.

C’est donc beaucoup plus qu’une simple correction de code.

---

## 8. Pourquoi sommes-nous maintenant en train de parler de coût CPU ?

Parce qu’en préparant **le test de validité de notre estimateur**, nous avons découvert que le test lui-même était coûteux.

Le premier checkpoint exige :

\[
5\ scénarios
\times
2000\ datasets
\times
10000\ bootstrap
\]

soit **100 millions de replicates internes**, avec deux chemins pour certaines métriques.

Nous avons alors fait quelque chose de méthodologiquement important :

**nous n’avons pas réduit discrètement le nombre de simulations pour que ça passe.**

Nous avons bloqué le coverage gate avec :

`CHRON-20260808-001`.

PR #5 formalise ce comportement : le harness n’altère pas le résultat scientifique, mais empêche l’exécution du coverage tant que cette difficulté de faisabilité n’est pas traitée. PR #5 reste aujourd’hui ouverte, Draft et non mergée.

---

## 9. Ce qu’a montré le benchmark

L’hypothèse initiale était :

> `weighted_threshold_at_fmr` semble O(K²), donc probablement le hotspot.

Le benchmark a montré que cette intuition était incomplète.

À l’operating point réel :

\[
FMR=0.01
\]

la boucle de threshold sort très tôt.

Le vrai coût mesuré est davantage réparti entre :

- génération des multiplicities ;
- construction des `edge_weights` ;
- puis le reste du bootstrap.

Sur GitHub Actions, on obtient environ :

**1,18 ms par replicate combiné**,

soit environ :

**11,8 s par dataset de 10 000 replicates**,

**6,6 h mono-worker par scénario au checkpoint 2000**,

et environ

**33 h mono-worker pour les cinq scénarios**.

PR #6 existe précisément pour documenter cette faisabilité **sans calculer de coverage et sans lire les résultats historiques**. Elle reste ouverte, Draft, basée sur la branche de PR #5, avec HEAD `020cf91f…`.

---

## 10. Ce que le benchmark a changé dans notre décision

C’est une petite mais vraie mise à jour scientifique/ingénierie.

Avant mesure :

> optimiser le threshold.

Après mesure :

> ce n’est probablement pas là qu’est le meilleur gain.

Le plan devient :

**1. paralléliser les datasets indépendants**,  
**2. vectoriser `edge_weights`**,  
**3. éventuellement mutualiser certains tirages représentation/opérationnel**,  
**4. seulement ensuite optimiser le threshold si le profilage le justifie.**

Mais le point 1 a lui-même révélé une nouvelle exigence :

**il faut garantir l’indépendance et la reproductibilité des sous-flux RNG.**

Le contrat fige actuellement `root_seed: 20260807` et `PCG64`, mais ne fige pas encore une hiérarchie de sous-flux.

D’où le prochain travail :

\[
SeedSequence(root)
\rightarrow
scenario
\rightarrow
dataset
\rightarrow
\{data,\ bootstrap\}
\]

avec replay déterministe d’un dataset isolé et identité serial/parallel.

---

# 11. Donc, en une phrase : où en sommes-nous ?

Nous sommes **entre la correction méthodologique et l’expérience corrigée**.

Nous avons pratiquement terminé la réponse à :

> **« Comment doit-on correctement estimer l’incertitude de Study 0 ? »**

mais nous devons encore démontrer que cet estimateur se comporte correctement en coverage.

Et **seulement ensuite** nous pourrons enfin revenir à la question originale :

> **« La projection Siamese 512→128 conserve-t-elle réellement suffisamment de performance biométrique pour être considérée non-inférieure au 512D dans ce cadre expérimental ? »**

---

## 12. La séquence restante

Je la résumerais ainsi :

**Méthode bootstrap correcte**  
→ presque stabilisée

**Faisabilité du coverage**  
→ benchmarkée, optimisation nécessaire

**SeedSequence + parallélisation + équivalence**  
→ prochaine étape

**Coverage 2k → 4k → 10k**  
→ pas encore exécuté

**Coverage PASS ?**

- non → méthode/amendement, G2 reste FAIL ;
- oui → ouverture contrôlée du `test_pair_scores.csv`.

Puis seulement :

**replay Study 0 corrigé**

→ nouvelle CI/UCB ΔFNMR

→ comparaison bootstrap paire historique vs bootstrap sujet corrigé

→ revue indépendante

→ décision sur E-STAT-001 / G2

→ éventuellement Study 1.

---

Et je pense qu’il y a un point de fond à ne pas perdre : **on n’est plus seulement en train de chercher un résultat sur la compression 512→128**.

Study 0 est progressivement devenu aussi un cas réel de démonstration de notre façon de faire de la science :

> une hypothèse peut être intéressante, un résultat peut sembler favorable, et pourtant la bonne décision scientifique peut être de retarder sa publication, invalider sa propre statistique, reconstruire l’estimateur, tester l’estimateur avant de regarder le résultat, puis même tester la faisabilité du test avant de l’exécuter.

C’est précisément pourquoi le `scientific-research-harness` est né de cette étude, plutôt que d’un exercice abstrait de framework design.
