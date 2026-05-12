# 📘 Correction – TD Progressif : Arbres de Décision (Dataset modifié)

> **Note du professeur :** Ce document utilise un dataset légèrement modifié par rapport au TD original. Une ligne a été ajoutée (`Moyen | Bas | Oui`) pour que les calculs soient plus réalistes et plus intéressants : le premier découpage ne donnera **pas** une entropie nulle immédiatement, ce qui est beaucoup plus représentatif de la vraie vie.

---

## Dataset modifié

| # | Age   | Revenu | Achat |
|---|-------|--------|-------|
| 1 | Jeune | Bas    | Non   |
| 2 | Jeune | Haut   | Oui   |
| 3 | Moyen | Haut   | Oui   |
| 4 | Agé   | Bas    | Non   |
| 5 | Agé   | Haut   | Oui   |
| 6 | Moyen | Bas    | **Oui** ← nouvelle ligne |

> **Total : 6 exemples** (contre 5 dans le TD original)

---

## Exercice 1 : Calcul de l'entropie

---

### Question 1 — Calculer la proportion de chaque classe

> 💡 On regarde uniquement la colonne **Achat**. On compte combien de fois chaque valeur apparaît, puis on divise par le total.

**Comptage :**
- **Oui** apparaît **4 fois** (lignes 2, 3, 5, 6)
- **Non** apparaît **2 fois** (lignes 1, 4)
- **Total :** 6 exemples

**Proportions :**

$$p(\text{Oui}) = \frac{4}{6} = \frac{2}{3} \approx 0{,}667$$

$$p(\text{Non}) = \frac{2}{6} = \frac{1}{3} \approx 0{,}333$$

---

### Question 2 — Calculer l'entropie globale du dataset

> 💡 L'entropie mesure le **désordre** dans les données. Plus les classes sont mélangées, plus l'entropie est élevée.

**Formule :**

$$H(S) = -\sum_{i=1}^{c} p_i \log_2(p_i)$$

**Application :**

$$H(S) = -\left(\frac{2}{3} \times \log_2\left(\frac{2}{3}\right)\right) - \left(\frac{1}{3} \times \log_2\left(\frac{1}{3}\right)\right)$$

**Calcul des logarithmes (rappel : $\log_2(x) = \frac{\ln(x)}{\ln(2)}$) :**
- $\log_2\!\left(\frac{2}{3}\right) = \log_2(0{,}667) \approx -0{,}585$
- $\log_2\!\left(\frac{1}{3}\right) = \log_2(0{,}333) \approx -1{,}585$

$$H(S) = -\left(\frac{2}{3} \times (-0{,}585)\right) - \left(\frac{1}{3} \times (-1{,}585)\right)$$

$$H(S) = 0{,}390 + 0{,}528$$

$$\boxed{H(S) \approx 0{,}918}$$

> **Interprétation :** Une entropie de 0,918 (proche du maximum de 1) indique que le dataset est assez désordonné : les classes sont mélangées mais pas encore à 50/50.

---

### Question 3 — Calculer l'entropie après division selon la variable **Revenu**

> 💡 On coupe le dataset en deux groupes selon la valeur de **Revenu** (Bas ou Haut), puis on calcule l'entropie dans chaque groupe séparément.

---

**Groupe 1 — Revenu = Bas :**

| # | Age   | Revenu | Achat |
|---|-------|--------|-------|
| 1 | Jeune | Bas    | Non   |
| 4 | Agé   | Bas    | Non   |
| 6 | Moyen | Bas    | Oui   |

→ 3 exemples : **1 Oui**, **2 Non**

$$p(\text{Oui}) = \frac{1}{3} \approx 0{,}333 \quad ; \quad p(\text{Non}) = \frac{2}{3} \approx 0{,}667$$

$$H(\text{Bas}) = -\left(\frac{1}{3} \times (-1{,}585)\right) - \left(\frac{2}{3} \times (-0{,}585)\right) = 0{,}528 + 0{,}390$$

$$\boxed{H(\text{Bas}) \approx 0{,}918}$$

> ⚠️ **Ce groupe n'est PAS pur !** Il contient un mélange de Oui et de Non, d'où une entropie non nulle. C'est exactement ce qui rend cet exercice plus réaliste que le TD original.

---

**Groupe 2 — Revenu = Haut :**

| # | Age   | Revenu | Achat |
|---|-------|--------|-------|
| 2 | Jeune | Haut   | Oui   |
| 3 | Moyen | Haut   | Oui   |
| 5 | Agé   | Haut   | Oui   |

→ 3 exemples : **3 Oui**, **0 Non**

$$p(\text{Oui}) = 1 \quad ; \quad p(\text{Non}) = 0$$

$$H(\text{Haut}) = -(1 \times \log_2(1)) - (0 \times \log_2(0)) = 0$$

✅ Ce groupe est **pur** (tout le monde achète).

---

**Entropie pondérée après division par Revenu :**

$$H_{\text{Revenu}} = \frac{|S_{\text{Bas}}|}{|S|} \times H(\text{Bas}) + \frac{|S_{\text{Haut}}|}{|S|} \times H(\text{Haut})$$

$$H_{\text{Revenu}} = \frac{3}{6} \times 0{,}918 + \frac{3}{6} \times 0$$

$$\boxed{H_{\text{Revenu}} = 0{,}459}$$

---

### Question 4 — Calculer le gain d'information

> 💡 Le gain mesure combien on réduit le désordre en utilisant une variable donnée. On compare le résultat pour **Revenu** et pour **Age**.

---

**Gain pour Revenu :**

$$\text{Gain}(S, \text{Revenu}) = H(S) - H_{\text{Revenu}} = 0{,}918 - 0{,}459$$

$$\boxed{\text{Gain}(S, \text{Revenu}) = 0{,}459}$$

---

**Gain pour Age (calcul comparatif) :**

Les trois sous-groupes selon Age :

| Age   | Lignes | Oui | Non | H        |
|-------|--------|-----|-----|----------|
| Jeune | 1, 2   |  1  |  1  | 1,000    |
| Moyen | 3, 6   |  2  |  0  | 0,000    |
| Agé   | 4, 5   |  1  |  1  | 1,000    |

$$H_{\text{Age}} = \frac{2}{6} \times 1{,}000 + \frac{2}{6} \times 0{,}000 + \frac{2}{6} \times 1{,}000$$

$$H_{\text{Age}} = \frac{1}{3} + 0 + \frac{1}{3} = \frac{2}{3} \approx 0{,}667$$

$$\text{Gain}(S, \text{Age}) = 0{,}918 - 0{,}667$$

$$\boxed{\text{Gain}(S, \text{Age}) = 0{,}251}$$

---

### Question 5 — Quelle variable choisir comme racine ?

**Tableau comparatif des gains :**

| Variable | Gain d'information |
|----------|--------------------|
| Revenu   | **0,459** ✅       |
| Age      | 0,251              |

**→ On choisit `Revenu` comme nœud racine** car son gain d'information est le plus élevé.

> Contrairement au TD original, le gain n'est plus 1 : la variable Revenu est utile, mais elle ne suffit **pas** à elle seule à tout classifier. Il faudra un deuxième niveau dans l'arbre.

---

## Exercice 2 : Construction de l'arbre

### 1. Variable racine

**Revenu** (gain = 0,459).

---

### 2. Les branches du premier niveau

- Branche gauche : `Revenu = Bas` → groupe **impur** → il faut continuer à subdiviser
- Branche droite : `Revenu = Haut` → groupe **pur** → feuille : **Oui**

---

### 3. Subdivision du groupe `Revenu = Bas`

Ce groupe contient :

| # | Age   | Achat |
|---|-------|-------|
| 1 | Jeune | Non   |
| 4 | Agé   | Non   |
| 6 | Moyen | Oui   |

On applique maintenant la variable **Age** sur ce sous-groupe :

| Age   | Résultat |
|-------|----------|
| Jeune | Non      |
| Agé   | Non      |
| Moyen | Oui      |

Chaque sous-groupe est **pur** → on obtient directement des feuilles :
- `Age = Jeune` → **Non**
- `Age = Agé` → **Non**
- `Age = Moyen` → **Oui**

---

### 4. Les feuilles

| Chemin                          | Décision |
|---------------------------------|----------|
| Revenu = Haut                   | **Oui** ✅ |
| Revenu = Bas ET Age = Jeune     | **Non** ❌ |
| Revenu = Bas ET Age = Agé       | **Non** ❌ |
| Revenu = Bas ET Age = Moyen     | **Oui** ✅ |

---

### Arbre obtenu

```
                    [Revenu ?]
                   /           \
               Bas               Haut
                |                  |
            [Age ?]             [Oui] ✅
           /   |   \
       Jeune Moyen  Agé
         |     |      |
       [Non] [Oui]  [Non]
        ❌    ✅      ❌
```

> **Lecture :** L'arbre a maintenant **2 niveaux**. Un revenu Haut suffit à prédire l'achat. Un revenu Bas nécessite une deuxième question sur l'âge pour trancher.

---

## Exercice 3 : Implémentation Python

> Ces exercices utilisent le dataset **Iris** de scikit-learn, indépendamment du dataset manuel ci-dessus.

### Étape 1 — Importation des bibliothèques

```python
import numpy as np                              # Calculs mathématiques
import pandas as pd                             # Manipulation de tableaux de données
from sklearn.tree import DecisionTreeClassifier # Algorithme arbre de décision
from sklearn.tree import plot_tree              # Visualisation de l'arbre
from sklearn.model_selection import train_test_split  # Séparation train/test
import matplotlib.pyplot as plt                 # Affichage des graphiques
```

---

### Étape 2 — Chargement du dataset Iris

```python
from sklearn.datasets import load_iris
iris = load_iris()
X = iris.data    # 4 mesures : longueur/largeur des pétales et sépales
y = iris.target  # 3 classes : setosa (0), versicolor (1), virginica (2)
```

---

### Étape 3 — Séparation train/test

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)
```

> - `test_size=0.3` : 30% des données pour tester, 70% pour entraîner
> - `random_state=42` : fixe le hasard pour des résultats reproductibles

---

### Étape 4 — Entraînement

```python
model = DecisionTreeClassifier(max_depth=3)
model.fit(X_train, y_train)
```

---

## Exercice 4 : Visualisation de l'arbre

### Question 1 — Quelle variable apparaît à la racine ?

**Réponse :** La variable **`petal length (cm)`** (longueur du pétale) apparaît à la racine. C'est la mesure qui discrimine le mieux les trois espèces d'Iris selon le critère du gain d'information.

---

### Question 2 — Combien de niveaux contient l'arbre ?

**Réponse :** **3 niveaux**, car `max_depth=3`. L'arbre peut poser au maximum 3 questions successives avant de donner une réponse finale.

---

### Question 3 — Quelles classes sont les plus faciles à séparer ?

**Réponse :** **Iris setosa** est parfaitement isolée dès le premier nœud (racine) grâce à la longueur du pétale. Les espèces **versicolor** et **virginica** sont plus proches et nécessitent des nœuds supplémentaires pour être distinguées.

---

## Exercice 5 : Influence de la profondeur

### Question 1 — Comparer les arbres obtenus

| Profondeur   | Structure           | Comportement typique               |
|--------------|---------------------|------------------------------------|
| `max_depth=1`  | 1 nœud, 2 feuilles  | Trop simple, souvent insuffisant   |
| `max_depth=3`  | Plusieurs niveaux   | Bon équilibre précision/lisibilité |
| `max_depth=10` | Très ramifié        | Risque élevé de surapprentissage   |

---

### Question 2 — Que se passe-t-il si l'arbre est trop profond ?

**Réponse :** Un arbre trop profond crée des règles ultra-spécifiques pour chaque exemple d'entraînement, y compris les cas atypiques (le "bruit"). Il devient très précis sur les données d'entraînement, mais très mauvais sur de nouvelles données qu'il n'a jamais vues.

> Comme un étudiant qui **mémorise les réponses de tous les anciens examens** sans comprendre le cours : il réussit les anciens examens, mais échoue dès que le sujet change légèrement.

---

### Question 3 — Expliquer le phénomène de surapprentissage

**Réponse :** Le **surapprentissage** (ou **overfitting**) survient quand un modèle apprend les données d'entraînement **trop fidèlement**, en mémorisant même le bruit, au lieu d'en extraire les tendances générales.

| Indicateur | Arbre adapté | Arbre en surapprentissage |
|------------|-------------|---------------------------|
| Précision sur train | Bonne | Quasi 100% |
| Précision sur test  | Bonne | Mauvaise |
| Généralisation      | ✅ Oui | ❌ Non |

**Solutions :** Limiter `max_depth`, utiliser la validation croisée, ou appliquer l'élagage (pruning).

---

## Exercice 6 : Importance des variables

### Question 1 — Quelle variable est la plus importante ?

**Résultat typique sur Iris (max_depth=3) :**

| Index | Variable           | Importance approx. |
|-------|--------------------|--------------------|
| 0     | sepal length (cm)  | ≈ 0,00             |
| 1     | sepal width (cm)   | ≈ 0,00             |
| 2     | **petal length (cm)** | **≈ 0,57** ✅   |
| 3     | petal width (cm)   | ≈ 0,43             |

**Réponse :** La variable **`petal length (cm)`** est la plus importante (≈ 57% de la réduction d'impureté totale).

---

### Question 2 — Comment interpréter cette importance ?

**Réponse :** `feature_importances_` mesure **quelle proportion de la réduction totale d'entropie** chaque variable a apportée à l'ensemble de l'arbre.

- **Importance élevée** → la variable a été souvent utilisée et a fortement contribué à séparer les classes.
- **Importance ≈ 0** → la variable n'a pas été utile (ou n'a pas été choisie par l'algorithme).

> 📌 La somme de toutes les importances est toujours **égale à 1** (ce sont des proportions).

---

## Exercice 7 : Application réelle

### Problème choisi : Prédiction de réussite académique

---

### Variables d'entrée

| Variable                      | Type         | Exemples de valeurs     |
|-------------------------------|--------------|-------------------------|
| Heures de travail / semaine   | Numérique    | 5h, 10h, 20h            |
| Taux d'absence                | Numérique(%) | 0%, 15%, 40%            |
| Note du dernier contrôle      | Numérique    | 8/20, 14/20             |
| Participation en cours        | Catégorielle | Faible / Moyenne / Forte|
| Accès à Internet              | Binaire      | Oui / Non               |

---

### Variable cible

$$\text{Résultat} \in \{\text{Réussite},\ \text{Échec}\}$$

---

### Structure possible de l'arbre

```
        [Heures de travail > 10h/semaine ?]
                  /               \
               Oui                 Non
                |                   |
  [Note dernier contrôle > 12 ?]  [Échec] ❌
         /             \
       Oui              Non
        |                 |
  [Réussite] ✅    [Absence < 20% ?]
                        /        \
                      Oui         Non
                       |            |
                 [Réussite] ✅   [Échec] ❌
```

---

### Pourquoi les arbres de décision sont adaptés ici ?

- Les règles produites sont **interprétables** : un enseignant peut comprendre et expliquer pourquoi un étudiant est prédit en difficulté.
- Les données scolaires mélangent des variables **numériques** (notes, absences) et **catégorielles** (participation), ce que les arbres gèrent naturellement.
- Permet une **détection précoce** des étudiants à risque pour intervenir à temps.

---

## 📝 Résumé général

| Concept | Définition rapide |
|---------|-------------------|
| **Entropie** | Mesure du désordre dans un ensemble de données (entre 0 et 1) |
| **Gain d'information** | Réduction d'entropie apportée par une variable |
| **Nœud racine** | Variable avec le gain d'information le plus élevé |
| **Feuille pure** | Sous-groupe contenant une seule classe (entropie = 0) |
| **Feuille impure** | Sous-groupe encore mélangé → nécessite un niveau supplémentaire |
| **max_depth** | Paramètre qui limite la profondeur (et la complexité) de l'arbre |
| **Surapprentissage** | Le modèle mémorise au lieu d'apprendre des tendances générales |
| **feature_importances_** | Contribution de chaque variable à la réduction d'entropie totale |

---

> 🔑 **Leçon clé de ce TD modifié :** Dans la réalité, la première variable choisie (la racine) réduit rarement l'entropie à zéro. Il faut presque toujours **plusieurs niveaux** de questions pour arriver à des feuilles pures — c'est pourquoi les arbres ont de la profondeur.

---

*Correction rédigée à des fins pédagogiques – Licence 3 Informatique*
