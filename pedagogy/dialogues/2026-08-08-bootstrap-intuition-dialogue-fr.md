# Dialogue pédagogique verbatim — rendre le bootstrap intuitif

Status: **verbatim learning trace / non-authoritative / non-blocking**

This file preserves the conversational learning path as produced: questions, doubts, clarifications, and answers. It is intentionally not normalized into textbook prose.

---

## User

Excellent, regarde combien de pages d explications du as generé, tres instructif, il fait que tu les gardes tel qul. Sans dout devrait on a chaque etape mineur ou majeur (et l indiquer dans le harness) un etat d etape, svec toujours ces trois vues qui nous caracterise pour que ce soit a la fois comprehensibke, serieux rigoureux et precis : phd, ing, 12y -- seulement si ca vaut le.cout -- ici je pense qucon est proche d une explication universelle car on a deja des niveaux (pkoi je fais cette projection reductive pour un industriel mais en conservant l  aspect phd biometrique) on est proche et sans dout suffisant. La ou il faut systematiquement amplifier (mais peut etee ecrit comme sous chapitre "autrement dit" n allons pas polluer un expert avec ce qui sont des "evidences, trivial..." dans ses priors/croyance. Je prends le chaptire 4 incomprehensible pour moi du moins -- rappel9ns aussi la genese de cette idees, des prositions de claude bootstrap avec 3 sources je crois que tu as revise et corrige.

## Assistant

Oui — et j’ai matérialisé ça dans le harness, sur une branche dédiée, pas sur `main`.

J’ai créé **PR #1** du nouveau repo :  
https://github.com/gharbonnier78/scientific-research-harness/pull/1

Elle contient trois choses importantes.

- `pedagogy/STEP_STATE_SPEC.md` : formalise le principe d’**état d’étape** après un changement réellement significatif de compréhension, d’évidence, d’incertitude ou de décision. Pas après chaque commit. Trois vues sont envisagées : **PhD/scientifique**, **ingénierie**, et **Autrement dit / 12 ans** seulement si cette troisième vue enlève un vrai obstacle conceptuel.
- `pedagogy/state-snapshots/siamese-study0-status-2026-08-08-fr.md` : j’ai conservé **tel quel** le long état d’étape que je viens de te donner. Il est explicitement marqué comme snapshot à ne pas réécrire silencieusement.
- `pedagogy/case-studies/bootstrap-from-pairs-to-subject-slots-fr.md` : premier vrai exemple des trois vues, centré sur ton chapitre 4.

Et pour le chapitre 4, oui, je pense qu’on a trouvé ce qui manquait. Le problème n’est pas de savoir lire \(m_i\) et \(m_i m_j\), mais de construire le bon modèle mental.

L’`Autrement dit` utilise trois personnes : Alice, Bob, Chloé. On suppose qu’on n’a observé que :

```text
Alice ↔ Alice     genuine
Bob   ↔ Bob       genuine
Alice ↔ Bob       impostor
Bob   ↔ Chloé     impostor
```

Puis le bootstrap tire :

```text
Alice, Alice, Bob
```

Donc :

```text
m_Alice = 2
m_Bob   = 1
m_Chloé = 0
```

Alors `Alice ↔ Alice` compte 2 fois, `Bob ↔ Bob` 1 fois, `Alice ↔ Bob` compte \(2×1=2\) fois, et `Bob ↔ Chloé` disparaît puisque Chloé n’a pas été tirée.

Mais surtout : **on ne crée jamais `Alice ↔ Chloé`**, parce que cette comparaison n’a jamais été observée. Voilà, je pense, l’intuition qui rend enfin le chapitre 4 accessible :

> on modifie combien de fois les observations existantes comptent, mais on n’invente jamais une observation qui n’a pas été faite.

Et le pourquoi du \(m_i m_j\) devient presque évident : si Alice apparaît deux fois et Bob trois fois, les deux occurrences d’Alice peuvent chacune être associées aux trois occurrences de Bob, donc l’edge observée Alice–Bob représente six occurrences dans ce replicate.

### La genèse du bootstrap

Ton souvenir des « trois sources » est bien fondé dans la **version normative finale**. La spec actuelle cite : Bolle–Ratha–Pankanti 2004 sur le subsets bootstrap, Poh–Bengio 2007 sur le joint bootstrap biométrique, et ISO/IEC 19795-1:2021 comme cadre général de performance biométrique.

Et il faut effectivement conserver la trajectoire, car elle est instructive :

**bootstrap historique au niveau paire**  
→ critique : plusieurs paires partagent la même identité  
→ intuition correcte : il faut remonter au sujet  
→ proposition intermédiaire : « tirer les identités puis prendre les paires correspondantes »  
→ problème : cette formulation suppose implicitement une matrice complète  
→ constat : LFW DevTest est un **graphe sparse** de seulement 1000 edges observées  
→ correction : subject slots + \(m_i\) / \(m_i m_j\) sur les seules edges existantes  
→ correction bibliographique : **Bolle 2004 subsets bootstrap**, et non le papier ROC/CMC de 2005  
→ preregistration avant résultat  
→ coverage avant réouverture des scores historiques.

C’est une excellente démonstration d’un autre principe du harness :

> **une critique peut être directionnellement juste sans que sa première formulation opérationnelle soit encore correcte.**

Le reviewer/Claude nous a aidés à identifier que l’unité « paire » était mauvaise. Ensuite, il a fallu **réadapter cette bonne idée à la topologie exacte des données**.

La seule chose que je refuse de reconstruire artificiellement : je n’ai pas retrouvé la trace verbatim de la toute première réponse de Claude permettant d’affirmer que **sa liste originale** était exactement ces trois références. Je peux confirmer que ce sont les trois fondations de la spec finale après nos vérifications/corrections. J’ai mis cette limite explicitement dans le document.

Je pense qu’on tient maintenant quelque chose d’assez fort pour le harness : **l’état d’étape n’est pas un résumé périodique ; c’est une photographie versionnée d’un changement de compréhension**. Et parfois, comme ici, l’explication universelle peut déjà contenir naturellement les niveaux PhD et ingénieur ; le niveau `Autrement dit` n’est ajouté que là où il débloque réellement la compréhension.

---

## User

Le chapitre 5 n est pas non plus tres intuitif pour un non statisticien

## Assistant

Oui. Le chapitre 5 — **« corriger un estimateur ne suffit pas »** — suppose déjà plusieurs intuitions de statisticien : *estimateur*, *intervalle*, *coverage*, *vérité connue par simulation*. Pour un non-statisticien, le saut est trop brutal.

Je garderais la version actuelle comme vue scientifique/ingénieur, mais j’ajouterais juste dessous un vrai **« Autrement dit »** de ce type :

### Autrement dit — pourquoi tester le test ?

Imaginons qu’on construise une balance pour peser des objets.

On vient de découvrir que notre ancienne balance avait un défaut. On la corrige.

La tentation serait alors de prendre immédiatement l’objet qui nous intéresse et de dire :

> « Maintenant, pesons-le avec la nouvelle balance. »

Mais il reste une question essentielle :

> **Comment savons-nous que la balance corrigée mesure correctement ?**

Pour le vérifier, on commence par lui donner des objets dont on connaît déjà exactement le poids.

Par exemple :

```text
objet A : 1 kg
objet B : 2 kg
objet C : 3 kg
```

Si la nouvelle balance nous donne régulièrement quelque chose de cohérent avec ces poids connus, on commence à lui faire confiance.

Si elle se trompe souvent, ce n’est pas l’objet qui est en cause : **c’est encore notre méthode de mesure**.

C’est exactement ce qu’on fait ici avec le bootstrap corrigé.

Nous créons artificiellement beaucoup de petits mondes biométriques où, contrairement au vrai Study 0, **nous connaissons la réponse correcte à l’avance**.

Par exemple, nous décidons lors de la simulation que la vraie différence entre 128D et 512D vaut :

```text
ΔFNMR = 0
```

ou :

```text
ΔFNMR = +0,015
```

ou exactement la limite que nous avons fixée :

```text
ΔFNMR = +0,03
```

Ensuite, nous faisons comme si nous ne connaissions plus cette réponse.

Nous donnons seulement les données simulées à notre bootstrap et nous lui demandons :

> « Quel intervalle d’incertitude proposes-tu ? »

Puis nous regardons si la vraie valeur que nous avions cachée se trouve bien dans cet intervalle.

Si notre méthode annonce un **intervalle à 95 %**, alors sur un très grand nombre de mondes simulés, la vraie valeur devrait se trouver dans cet intervalle environ 95 fois sur 100.

C’est cela qu’on appelle ici la **coverage**.

Donc :

> **nous ne testons pas encore si le 128D est bon.  
> Nous testons d’abord si notre manière de mesurer l’incertitude autour du 128D est digne de confiance.**

Et c’est précisément pour cette raison que nous refusons encore d’ouvrir les anciens scores Study 0.

Si nous regardions d’abord le résultat réel, puis commencions à modifier notre méthode statistique jusqu’à ce qu’elle donne une réponse satisfaisante, nous risquerions de fabriquer une méthode adaptée au résultat que nous avons déjà vu.

La séquence correcte est donc :

```text
1. construire la nouvelle méthode
2. la tester sur des problèmes dont on connaît la réponse
3. vérifier qu'elle se comporte comme annoncé
4. seulement ensuite l'utiliser sur le vrai problème
```

Je pense que c’est même une intuition assez universelle : **avant de croire une mesure inconnue, on calibre l’instrument sur des cas connus**.

Et cela rend le chapitre 5 beaucoup plus naturel après le chapitre 4 :

**chapitre 4 : avons-nous construit la bonne balance ?**  
**chapitre 5 : avons-nous vérifié que cette nouvelle balance mesure effectivement correctement ?**
