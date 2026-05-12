# 📘 Correction – TD Progressif : Arbres de Décision

> **Note du professeur :** Ce document est une correction détaillée, pensée pour quelqu'un qui fait ce type d'exercice pour la première fois. Chaque étape est expliquée pas à pas. Lisez attentivement les explications **avant** de regarder le résultat final.

---

## Exercice 1 : Calcul de l'entropie

### Dataset de référence

| Age   | Revenu | Achat |
|-------|--------|-------|
| Jeune | Bas    | Non   |
| Jeune | Haut   | Oui   |
| Moyen | Haut   | Oui   |
| Agé   | Bas    | Non   |
| Agé   | Haut   | Oui   |

> **Total : 5 exemples**

---

### Question 1 — Calculer la proportion de chaque classe

> 💡 **Comprendre la question :** On regarde uniquement la colonne **Achat** (c'est notre variable cible, celle qu'on veut prédire). On compte combien de fois chaque valeur apparaît, puis on divise par le total.

**Comptage :**
- **Oui** apparaît 3 fois (lignes 2, 3, 5)
- **Non** apparaît 2 fois (lignes 1, 4)
- **Total :** 5 exemples

**Proportions :**

$$p(\text{Oui}) = \frac{3}{5} = 0{,}6$$

$$p(\text{Non}) = \frac{2}{5} = 0{,}4$$

---

### Question 2 — Calculer l'entropie globale du dataset

> 💡 **Comprendre la question :** L'entropie mesure le **désordre** dans les données. Si toutes les lignes appartiennent à la même classe → entropie = 0 (parfaitement ordonné). Si les classes sont parfaitement mélangées → entropie = 1 (maximum de désordre).

**Formule :**

$$H(S) = -\sum_{i=1}^{c} p_i \log_2(p_i)$$

**Application :**

$$H(S) = -(0{,}6 \times \log_2(0{,}6)) - (0{,}4 \times \log_2(0{,}4))$$

**Calcul des logarithmes :**
- $\log_2(0{,}6) \approx -0{,}737$
- $\log_2(0{,}4) \approx -1{,}322$

$$H(S) = -(0{,}6 \times (-0{,}737)) - (0{,}4 \times (-1{,}322))$$

$$H(S) = 0{,}4422 + 0{,}5288$$

$$\boxed{H(S) \approx 0{,}971}$$

> **Interprétation :** Une entropie proche de 1 signifie que le dataset est assez désordonné : les classes Oui et Non sont mélangées.

---

### Question 3 — Calculer l'entropie après division selon la variable **Revenu**

> 💡 **Comprendre la question :** On va "couper" le dataset en deux groupes selon la valeur de **Revenu** (Bas ou Haut), puis calculer l'entropie dans chacun de ces groupes.

**Groupe 1 — Revenu = Bas :**

| Age   | Revenu | Achat |
|-------|--------|-------|
| Jeune | Bas    | **Non** |
| Agé   | Bas    | **Non** |

→ 2 exemples, 0 Oui, 2 Non

$$p(\text{Oui}) = 0 \quad ; \quad p(\text{Non}) = 1$$

$$H(\text{Bas}) = -(0 \times \log_2(0)) - (1 \times \log_2(1)) = 0$$

> *Par convention, $0 \times \log_2(0) = 0$*

✅ Entropie = **0** → Ce groupe est **pur** (tout le monde dit Non).

---

**Groupe 2 — Revenu = Haut :**

| Age   | Revenu | Achat |
|-------|--------|-------|
| Jeune | Haut   | **Oui** |
| Moyen | Haut   | **Oui** |
| Agé   | Haut   | **Oui** |

→ 3 exemples, 3 Oui, 0 Non

$$p(\text{Oui}) = 1 \quad ; \quad p(\text{Non}) = 0$$

$$H(\text{Haut}) = 0$$

✅ Entropie = **0** → Ce groupe est **pur** (tout le monde dit Oui).

---

**Entropie pondérée après division par Revenu :**

$$H_{\text{Revenu}} = \frac{|S_{\text{Bas}}|}{|S|} \times H(\text{Bas}) + \frac{|S_{\text{Haut}}|}{|S|} \times H(\text{Haut})$$

$$H_{\text{Revenu}} = \frac{2}{5} \times 0 + \frac{3}{5} \times 0 = \boxed{0}$$

---

### Question 4 — Calculer le gain d'information

> 💡 **Comprendre la question :** Le gain d'information mesure **combien on réduit le désordre** en utilisant une variable pour diviser les données. Plus le gain est élevé, meilleure est cette variable pour créer un nœud dans l'arbre.

**Formule :**

$$\text{Gain}(S, A) = H(S) - \sum_v \frac{|S_v|}{|S|} H(S_v)$$

**Pour Revenu :**

$$\text{Gain}(S, \text{Revenu}) = 0{,}971 - 0 = \boxed{0{,}971}$$

---

**Pour Age (calcul comparatif) :**

- **Jeune** : {Non, Oui} → H = 1
- **Moyen** : {Oui} → H = 0
- **Agé** : {Non, Oui} → H = 1

$$H_{\text{Age}} = \frac{2}{5} \times 1 + \frac{1}{5} \times 0 + \frac{2}{5} \times 1 = 0{,}8$$

$$\text{Gain}(S, \text{Age}) = 0{,}971 - 0{,}8 = \boxed{0{,}171}$$

---

### Question 5 — Quelle variable choisir comme racine ?

> 💡 **Règle :** On choisit toujours la variable avec le **gain d'information le plus élevé** comme racine de l'arbre.

| Variable | Gain d'information |
|----------|--------------------|
| Revenu   | **0,971** ✅       |
| Age      | 0,171              |

**→ On choisit `Revenu` comme nœud racine**, car elle divise parfaitement le dataset (entropie = 0 dans les deux sous-groupes).

---

## Exercice 2 : Construction de l'arbre

### 1. Variable racine

Comme calculé à l'exercice 1 : **Revenu** est la racine.

### 2. Les branches

La variable **Revenu** a deux valeurs possibles → deux branches :
- Branche gauche : `Revenu = Bas`
- Branche droite : `Revenu = Haut`

### 3. Les feuilles

- `Revenu = Bas` → tous les exemples donnent **Non** → feuille : **Non**
- `Revenu = Haut` → tous les exemples donnent **Oui** → feuille : **Oui**

> Pas besoin de subdiviser davantage car les groupes sont déjà **purs** (entropie = 0).

### Arbre obtenu

```
              [Revenu ?]
             /          \
         Bas              Haut
          |                 |
       [Non] ✅          [Oui] ✅
```

> **Lecture de l'arbre :** Si une personne a un revenu **Bas** → elle n'achètera **pas**. Si son revenu est **Haut** → elle **achètera**.

---

## Exercice 3 : Implémentation Python

> 💡 **Note :** Cet exercice consiste à **exécuter le code fourni** et à comprendre ce que fait chaque étape. Voici les explications ligne par ligne.

### Étape 1 — Importation des bibliothèques

```python
import numpy as np                         # Pour les calculs mathématiques
import pandas as pd                        # Pour manipuler les données en tableau
from sklearn.tree import DecisionTreeClassifier  # L'algorithme de l'arbre de décision
from sklearn.tree import plot_tree         # Pour dessiner l'arbre
from sklearn.model_selection import train_test_split  # Pour séparer train/test
import matplotlib.pyplot as plt            # Pour afficher les graphiques
```

> Chaque `import` charge une bibliothèque externe. Sans ces lignes, les fonctions utilisées ensuite ne sont pas disponibles.

---

### Étape 2 — Chargement du dataset Iris

```python
from sklearn.datasets import load_iris
iris = load_iris()
X = iris.data    # Les 4 caractéristiques (longueur/largeur des pétales et sépales)
y = iris.target  # Les classes (0=setosa, 1=versicolor, 2=virginica)
```

> Le dataset **Iris** est un jeu de données classique en data science : 150 fleurs décrites par 4 mesures, réparties en 3 espèces.

---

### Étape 3 — Séparation train/test

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)
```

> - `test_size=0.3` : 30% des données pour tester, 70% pour entraîner.
> - `random_state=42` : fixe le "hasard" pour que tout le monde obtienne le même résultat.

**Pourquoi séparer ?** Pour ne pas évaluer le modèle sur les mêmes données qui ont servi à l'entraîner. Ce serait comme donner les réponses à un élève avant l'examen.

---

### Étape 4 — Entraînement du modèle

```python
model = DecisionTreeClassifier(max_depth=3)
model.fit(X_train, y_train)
```

> - `max_depth=3` : on limite l'arbre à 3 niveaux de profondeur.
> - `fit()` : c'est ici que le modèle "apprend" à partir des données d'entraînement.

---

## Exercice 4 : Visualisation de l'arbre

### Code

```python
plt.figure(figsize=(10, 6))
plot_tree(model,
          feature_names=iris.feature_names,
          class_names=iris.target_names,
          filled=True)
plt.show()
```

> `filled=True` colorie les nœuds selon la classe majoritaire → plus facile à lire.

---

### Question 1 — Quelle variable apparaît à la racine ?

> 💡 La variable racine est celle qui a le **meilleur gain d'information** sur le dataset Iris.

**Réponse :** La variable **`petal length (cm)`** (longueur du pétale) apparaît à la racine de l'arbre. C'est la mesure qui sépare le mieux les trois espèces d'Iris.

---

### Question 2 — Combien de niveaux contient l'arbre ?

**Réponse :** L'arbre contient **3 niveaux** (profondeur = 3), car on a fixé `max_depth=3`. Cela correspond à 3 questions successives maximum avant d'arriver à une décision finale.

---

### Question 3 — Quelles classes sont les plus faciles à séparer ?

**Réponse :** La classe **Iris setosa** est la plus facile à séparer. Dès le premier nœud (racine), elle est entièrement isolée des deux autres espèces grâce à la longueur du pétale. Les classes **versicolor** et **virginica** se ressemblent davantage et nécessitent des nœuds supplémentaires pour être distinguées.

---

## Exercice 5 : Influence de la profondeur

### Rappel du code

```python
DecisionTreeClassifier(max_depth=1)   # Arbre très simple
DecisionTreeClassifier(max_depth=3)   # Arbre équilibré
DecisionTreeClassifier(max_depth=10)  # Arbre très complexe
```

---

### Question 1 — Comparer les arbres obtenus

| Profondeur | Description | Comportement |
|------------|-------------|--------------|
| `max_depth=1` | 1 seul nœud de décision, 2 feuilles | Très simple, souvent insuffisant |
| `max_depth=3` | Arbre équilibré, 3 niveaux | Bon compromis précision/complexité |
| `max_depth=10` | Arbre très détaillé, nombreux nœuds | Risque de mémoriser les données |

---

### Question 2 — Que se passe-t-il si l'arbre est trop profond ?

**Réponse :** Si l'arbre est trop profond, il finit par créer **des règles très spécifiques pour chaque exemple d'entraînement**, y compris pour les données atypiques (le "bruit"). Il devient excellent sur les données d'entraînement, mais très mauvais sur de nouvelles données.

> Imaginez un élève qui mémorise toutes les questions d'un ancien examen sans comprendre le cours : il réussit cet examen, mais échoue à tout nouvel examen.

---

### Question 3 — Expliquer le phénomène de surapprentissage

**Réponse :** Le **surapprentissage** (ou **overfitting**) se produit quand un modèle apprend **trop bien** les données d'entraînement, au point d'en mémoriser les détails et le bruit, plutôt que d'apprendre les tendances générales.

**Conséquences :**
- **Précision sur train ≈ 100%** → le modèle "connaît" toutes les réponses
- **Précision sur test ≪ 100%** → le modèle généralise très mal

**Solution :** Limiter la profondeur (`max_depth`), ou utiliser des techniques comme l'élagage (pruning) ou la validation croisée.

---

## Exercice 6 : Importance des variables

### Code et exécution

```python
importances = model.feature_importances_
for i, v in enumerate(importances):
    print(i, v)
```

**Résultat typique sur Iris (max_depth=3) :**

| Index | Variable | Importance |
|-------|----------|------------|
| 0 | sepal length (cm) | ≈ 0.00 |
| 1 | sepal width (cm)  | ≈ 0.00 |
| 2 | petal length (cm) | ≈ 0.57 |
| 3 | petal width (cm)  | ≈ 0.43 |

---

### Question 1 — Quelle variable est la plus importante ?

**Réponse :** La variable **`petal length (cm)`** (longueur du pétale, index 2) est la plus importante, avec une valeur d'importance d'environ **0,57** (57%).

---

### Question 2 — Comment interpréter cette importance ?

**Réponse :** `feature_importances_` indique **quelle proportion de la réduction totale d'impureté** (entropie) est attribuée à chaque variable. 

- Une importance élevée signifie que cette variable a été utilisée souvent dans l'arbre et qu'elle a fortement contribué à séparer les classes.
- Une importance proche de 0 signifie que la variable n'a pas été utile (ou n'a pas été utilisée du tout).

> 📌 **La somme de toutes les importances = 1** (elles représentent des proportions).

---

## Exercice 7 : Application réelle

> 💡 **Objectif :** Trouver un exemple concret de problème réel qu'un arbre de décision pourrait résoudre.

### Exemple choisi : Prédiction de réussite académique

---

### Variables (les caractéristiques d'entrée)

| Variable | Type | Exemple |
|----------|------|---------|
| Heures de travail par semaine | Numérique | 5, 10, 20 |
| Taux d'absence | Numérique (%) | 0%, 15%, 40% |
| Note du dernier contrôle | Numérique | 8/20, 14/20 |
| Participation en cours | Catégorielle | Faible / Moyenne / Forte |
| Accès à internet | Binaire | Oui / Non |

---

### Variable cible

$$\text{Résultat} \in \{\text{Réussite}, \text{Échec}\}$$

---

### Structure possible de l'arbre

```
           [Heures de travail > 10h/semaine ?]
                    /              \
                 Oui                Non
                  |                  |
      [Note dernier contrôle > 12 ?]  [Échec] ❌
            /          \
          Oui            Non
           |               |
       [Réussite] ✅    [Absence < 20% ?]
                             /       \
                           Oui        Non
                            |           |
                        [Réussite] ✅ [Échec] ❌
```

---

### Justification du choix

Les arbres de décision sont particulièrement adaptés à ce problème car :
- Les règles obtenues sont **interprétables** (on peut expliquer pourquoi un étudiant est prédit en échec).
- Les données scolaires contiennent souvent des variables **mixtes** (numériques et catégorielles), ce que les arbres gèrent bien.
- Cela permet aux enseignants ou établissements d'identifier des **facteurs de risque** tôt dans l'année.

---

## 📝 Résumé général

| Concept | Définition rapide |
|---------|------------------|
| **Entropie** | Mesure du désordre dans un ensemble de données |
| **Gain d'information** | Réduction de l'entropie apportée par une variable |
| **Nœud racine** | Variable avec le gain d'information le plus élevé |
| **Feuille** | Nœud terminal = décision finale |
| **max_depth** | Paramètre qui limite la profondeur de l'arbre |
| **Surapprentissage** | Le modèle mémorise au lieu d'apprendre |
| **feature_importances_** | Mesure la contribution de chaque variable à l'arbre |

---

*Correction rédigée à des fins pédagogiques – Licence 3 Informatique*
