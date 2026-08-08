# Du bootstrap de paires au bootstrap de sujets — genèse et explication à trois vues

Status: **pedagogical case study / historical reconstruction with explicit evidence boundary**

## 0. Ce qui est historiquement vérifiable

Le protocole v0.2.2 actuellement versionné cite trois fondations :

1. R. M. Bolle, N. K. Ratha, S. Pankanti, *Error Analysis of Pattern Recognition Systems — The Subsets Bootstrap*, CVIU 93(1), 2004.
2. N. Poh, S. Bengio, *Estimating the Confidence Interval of Expected Performance Curve in Biometric Authentication Using Joint Bootstrap*, ICASSP 2007.
3. ISO/IEC 19795-1:2021 comme cadre général de test et reporting de performance biométrique, sans prétendre que la norme prescrit la construction pondérée utilisée ici.

La bibliographie a elle-même fait l'objet d'une correction : le papier Bolle pertinent pour le **subsets bootstrap** est celui de 2004. Le papier *The relation between the ROC curve and the CMC* de 2005 est distinct et ne doit pas être présenté comme la source du subsets bootstrap.

La reconstruction disponible de la discussion indique qu'une proposition initiale de correction avait bien identifié le besoin de resampler au niveau des identités/sujets, mais qu'une formulation intermédiaire supposait implicitement qu'après tirage de deux identités il existait toujours une paire correspondante à inclure. Cette intuition est naturelle pour une matrice complète de comparaisons, mais elle n'est pas valide pour LFW DevTest, qui n'observe qu'un **graphe sparse de 1000 paires**.

La trace verbatim de la toute première proposition externe n'est pas conservée ici ; ce document ne prétend donc pas attribuer mot pour mot à un reviewer ou à Claude une liste exacte de trois références. Il conserve ce qui est vérifiable dans le protocole et la trajectoire méthodologique réellement retenue.

---

# Vue 1 — PhD / biométrie / statistique

## 1. Le défaut initial

L'analyse historique resamplait les **indices de paires** genuine et impostor. Or plusieurs trials LFW partagent une identité. Les observations ne sont donc pas indépendantes au niveau paire.

Le bootstrap pair-level traite implicitement une partie de cette dépendance comme si elle n'existait pas. L'intervalle obtenu peut alors mal représenter l'incertitude de l'estimand biométrique.

Le problème n'est pas que la moyenne des scores soit fausse. Le problème est l'**unité de resampling utilisée pour quantifier l'incertitude**.

## 2. Pourquoi ne pas simplement faire un bootstrap de sujets sur une matrice complète ?

Parce que Study 0 est défini sur le protocole LFW DevTest observé : 500 genuine + 500 impostor edges, 963 sujets distincts.

Une matrice complète des 963 identités représenterait un autre échantillon de scores, donc potentiellement un autre estimand. Synthétiser ces comparaisons pour corriger l'incertitude changerait le protocole au lieu de le corriger.

La contrainte devient donc :

> resampler les sujets, mais ne jamais créer une edge absente du protocole observé.

## 3. Construction retenue

Pour un replicate bootstrap, on tire N=963 subject slots avec remise. Si le sujet i apparaît `m_i` fois :

- une edge genuine observée `(i,i)` reçoit le poids `m_i` ;
- une edge impostor observée `(i,j)` reçoit le poids `m_i * m_j`.

Ces poids représentent exactement combien de copies de l'edge observée existeraient si l'on matérialisait les slots tirés, tout en restant sur le support sparse du protocole.

Le protocole appelle cette construction :

> **protocol-preserving weighted subject-slot bootstrap adapted to the sparse symmetric LFW pair graph**.

Elle est inspirée des principes de resampling par sujets/subsets en biométrie, mais n'est pas présentée comme la reproduction verbatim d'un algorithme publié.

## 4. Pourquoi `m_i` pour genuine et `m_i m_j` pour impostor ?

Une genuine edge `(i,i)` correspond à un sujet comparé à lui-même dans le trial observé. Si le sujet i apparaît trois fois dans les slots bootstrap, on dispose conceptuellement de trois occurrences de ce trial : poids 3.

Une impostor edge `(i,j)` relie deux sujets différents. Si i apparaît deux fois et j trois fois, chaque occurrence de i peut se combiner avec chaque occurrence de j : `2 × 3 = 6` occurrences conceptuelles de **cette edge observée**.

Ce raisonnement ne crée aucune nouvelle edge `(i,k)` qui n'existait pas dans LFW DevTest.

---

# Vue 2 — ingénierie / système

## 5. Le problème vu comme un problème de modélisation

Le bug statistique ressemble à un problème classique d'ingénierie système : on avait choisi la mauvaise frontière d'indépendance.

Le fichier contient 1000 lignes, mais **1000 lignes ne signifient pas 1000 unités indépendantes**.

Plusieurs lignes dépendent du même objet réel : l'identité biométrique.

Si on ignore cette structure, le logiciel peut exécuter parfaitement le calcul demandé tout en donnant une confiance mal calibrée.

## 6. Pourquoi le graphe sparse est important industriellement

Une solution facile aurait été de reconstruire toutes les comparaisons possibles entre sujets. Mais cela aurait répondu à une question différente.

En ingénierie, c'est l'équivalent de changer le banc de test pour rendre une métrique plus facile à calculer : le nouvel environnement peut être intéressant, mais il ne valide plus exactement la claim initiale.

Le choix retenu garde donc deux invariants :

1. **on change l'unité statistique** pour refléter la dépendance réelle ;
2. **on ne change pas le protocole de données observées**.

C'est ce que signifie ici `protocol-preserving`.

## 7. Pourquoi utiliser des poids plutôt que dupliquer physiquement les lignes ?

Parce que les deux représentations sont mathématiquement équivalentes si l'implémentation est correcte.

Au lieu de copier six fois une même impostor edge, on conserve une ligne et on lui donne `weight=6`.

C'est moins coûteux, plus auditable, et plus facile à relier au pair_id original.

Mais l'équivalence entre duplication explicite et pondération doit être testée sur de petits fixtures calculables à la main. L'optimisation informatique ne doit pas introduire une nouvelle sémantique statistique.

---

# Vue 3 — Autrement dit

## 8. Imagine trois personnes : Alice, Bob et Chloé

Supposons que notre petit protocole n'ait observé que ces comparaisons :

```text
Alice ↔ Alice     genuine
Bob   ↔ Bob       genuine
Alice ↔ Bob       impostor
Bob   ↔ Chloé     impostor
```

Important : nous **n'avons jamais mesuré** `Alice ↔ Chloé`.

Maintenant nous voulons simuler ce qui se passerait si notre échantillon de personnes avait été légèrement différent.

On tire trois noms avec remise, par exemple :

```text
Alice, Alice, Bob
```

Donc :

```text
m_Alice = 2
m_Bob   = 1
m_Chloé = 0
```

### Genuine

La comparaison `Alice ↔ Alice` compte deux fois, parce qu'Alice a été tirée deux fois :

```text
poids = 2
```

`Bob ↔ Bob` compte une fois :

```text
poids = 1
```

### Impostor

`Alice ↔ Bob` peut être formée avec chacune des deux occurrences d'Alice et l'unique occurrence de Bob :

```text
2 × 1 = 2
```

Donc poids 2.

`Bob ↔ Chloé` vaut :

```text
1 × 0 = 0
```

Chloé n'a pas été tirée, donc cette comparaison disparaît dans ce replicate.

### Et Alice ↔ Chloé ?

On pourrait être tenté de dire :

> « Puisqu'on a tiré des personnes, créons toutes les paires entre elles. »

Mais **Alice ↔ Chloé n'existe pas dans nos données**.

On ne connaît pas son score biométrique.

L'inventer reviendrait à fabriquer une observation.

C'est la raison essentielle du graphe sparse :

> on change combien de fois les observations existantes comptent, mais on n'invente jamais une observation qui n'a pas été faite.

## 9. Pourquoi c'est mieux que tirer directement les quatre lignes ?

Parce que si Alice apparaît dans plusieurs lignes, ces lignes sont liées par Alice.

Tirer les lignes indépendamment pourrait faire comme si :

```text
Alice ↔ Alice
```

et

```text
Alice ↔ Bob
```

n'avaient rien en commun.

Le bootstrap par sujets conserve cette dépendance : quand Alice est très présente dans un replicate, **toutes les observations existantes qui dépendent d'Alice sont affectées ensemble**.

C'est exactement ce que nous cherchons à simuler.

---

# 10. La genèse méthodologique comme objet pédagogique

L'intérêt de cet épisode n'est pas seulement la formule finale.

La trajectoire elle-même est instructive :

```text
bootstrap par paires historique
        ↓
review : les paires partagent des identités
        ↓
intuition : resampler les identités
        ↓
proposition intermédiaire trop proche d'une matrice complète
        ↓
constat : LFW DevTest est un graphe sparse
        ↓
correction : subject slots + poids sur les seules edges observées
        ↓
correction bibliographique : Bolle 2004 subsets bootstrap, pas ROC/CMC 2005
        ↓
preregistration avant reanalyse
        ↓
coverage simulation avant ouverture des résultats historiques
```

Le point pédagogique central est qu'une bonne critique initiale peut être **directionnellement correcte sans que sa première implémentation soit encore la bonne**.

Le reviewer a aidé à déplacer l'unité de raisonnement de la paire vers le sujet. Le travail suivant a consisté à adapter cette idée à la structure exacte du protocole LFW sans introduire de données inexistantes.

---

# 11. Ce que les trois vues doivent avoir en commun

Les trois vues racontent exactement le même objet :

- même unité de resampling ;
- mêmes edges observées ;
- mêmes poids `m_i` / `m_i m_j` ;
- même interdiction de synthétiser les paires absentes ;
- mêmes limites scientifiques.

La vue `Autrement dit` ne remplace donc pas la définition scientifique. Elle fournit seulement un modèle mental suffisamment simple pour revenir ensuite au formalisme sans le réciter aveuglément.