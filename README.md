# Econometrics: Final Report: Hugo Gesland – BIDM (EDHEC x UTC)

## Analyse des Rendements d'Actifs Financiers et Modélisation Économétrique :
### Étude des Actions d'IBM et "Adobe" via le Modèle CAPM et APT

### Introduction
Dans ce projet économétrique, nous explorons les rendements des actions d'IBM et de Adobe en utilisant deux modèles économétriques fondamentaux en finance : le modèle CAPM (Capital Asset Pricing Model) et le modèle APT (Arbitrage Pricing Theory). Ces modèles permettent de comprendre la relation entre les rendements des actions et les variables économiques telles que les taux d'intérêt, l'inflation, la masse monétaire, etc. L'objectif est de quantifier les risques systémiques et d'évaluer la rentabilité des actions dans un contexte économique donné.  
Ce projet a pour but de nous familiariser avec les techniques économétriques nécessaires à l'analyse empirique en finance, tout en appliquant des méthodes telles que la régression linéaire, la régression quantile et l'analyse en composantes principales (PCA).

---

## 1. Données

### Source des Données et choix des variables :
Les données utilisées pour ce projet proviennent du cours d’econometrics ainsi que du site Investing.com, une plateforme reconnue pour ses séries temporelles financières. Nous avons utilisé des séries mensuelles des rendements des actions de IBM et Adobe, ainsi que des données économiques macroéconomiques pertinentes telles que :
- **S&P500** (indice boursier),
- **CPI** (indice des prix à la consommation),
- **M1SUPPLY** (masse monétaire),
- **USTB3M et USTB10Y** (rendements des bons du Trésor à 3 mois et 10 ans),
- **CCREDIT** (crédit à la consommation),
- **INDPRO** (production industrielle),
- **BMINUSA** (écart de crédit).

### Choix des Variables : IBM et Adobe
Les entreprises IBM et Adobe ont été sélectionnées pour ce projet en raison de leur ancienneté et de la disponibilité de données historiques fiables, remontant à 1986. Nous avons collecté les prix mensuels des actions de ces entreprises depuis cette date, ce qui correspondait aux exigences du dataset initial fourni dans le cours, et permettait de garantir la cohérence et la robustesse des analyses économétriques.

De plus, le choix d'entreprises américaines simplifie l'accès aux données. En effet, nous pouvions remplacer Microsoft et Ford par IBM et Adobe conservant donc la structure et la logique initiales du dataset du cours tout en personnalisant l'analyse pour ce projet. Toutes les variables nécessaires, telles que les indices de marché (S&P500), les taux d'intérêt (USTB3M, USTB10Y) et d'autres indicateurs macroéconomiques, étaient disponibles sur le cours d’Econometrics.

### Préparation des Données :
Les données ont été nettoyées et transformées pour répondre aux besoins du modèle économétrique :
- **Rendements Logarithmiques** : Nous avons calculé les rendements continuellement composés (logarithmiques) à l’aide de la fonction `diff(log(...))` en R, pour chaque série temporelle des actions et des indices économiques.
- **Rendements Excédentaires** : Pour estimer les rendements excédentaires par rapport au taux sans risque, nous avons utilisé les rendements des bons du Trésor (USTB3M) pour déduire le taux sans risque des rendements des actifs.

### Fréquence Mensuelle des Données :
Les données pour l’analyse des rendements des actions d'IBM et de "Adobe", ainsi que des variables économiques associées, telles que l'indice des prix à la consommation (CPI), la masse monétaire (M1SUPPLY) et les rendements des bons du Trésor ont été utilisées à une fréquence mensuelle. Cette fréquence a été choisie parce qu'elle permet d'observer les fluctuations économiques à un rythme suffisamment détaillé tout en évitant une volatilité excessive qui pourrait résulter d'une fréquence quotidienne. Les rendements mensuels sont également pertinents dans le cadre d'une analyse de portefeuille où l’on cherche à observer des tendances et des relations macroéconomiques.

#### Logique et pertinence des données mensuelles :
Les variables économiques évoluent généralement à une fréquence mensuelle, ce qui permet de mieux capturer les tendances macroéconomiques sans être influencées par les fluctuations quotidiennes du marché. Les événements importants, comme les publications d'indices ou les données sur l'emploi, ont souvent un impact visible sur une période d’un à deux mois, justifiant l’utilisation de données mensuelles. Une telle fréquence permet d’observer les tendances tout en évitant les bruits des données quotidiennes. De plus, les modèles comme le CAPM et l'APT sont souvent appliqués sur des séries mensuelles, offrant un bon compromis entre précision et disponibilité des données.

---

## 2. Modèle CAPM :

Le modèle CAPM (Capital Asset Pricing Model) est utilisé pour estimer les rendements excédentaires des actions d'IBM et de "Adobe" en fonction des rendements excédentaires du marché. La formule du modèle CAPM est donnée par :

$$ E(R_i) = R_f + \beta_i \left(E(R_M) - R_f\right) $$

Où :  
- \(E(R_i)\) : Rendement attendu de l'actif \(i\),  
- \(R_f\) : Taux sans risque (par exemple, rendement des bons du Trésor),  
- \(\beta_i\) : Sensibilité du rendement de l'actif \(i\) aux fluctuations du marché,  
- \(E(R_M)\) : Rendement attendu du marché,  
- \(E(R_M) - R_f\) : Prime de risque du marché.

### Régression CAPM :
Nous avons utilisé la régression linéaire pour estimer les coefficients \(\beta_i\) pour IBM et Adobe en régressant les rendements excédentaires des actions sur les rendements excédentaires du marché (S&P500). Nous avons utilisé la commande `lm()` en R pour cette régression, qui donne une estimation de la relation entre les rendements de l'actif et du marché.

---

## 3. Régression Quantile :
Pour approfondir l'analyse, nous avons appliqué la régression quantile, qui permet d'estimer les paramètres du modèle CAPM à différents quantiles des rendements, afin d'explorer les rendements dans les queues de la distribution. Cela permet de mieux comprendre le comportement des actions en période de forte volatilité ou de marché en crise. La commande `rq()` du package `quantreg` en R a été utilisée pour estimer ces régressions.

### Interprétation de la régression quantile simple :
Le coefficient de `rsandp` pour \( \tau = 0.5 \) (médiane) est 0.95975, ce qui confirme que la relation entre les rendements excédentaires d'IBM et du marché reste significative à la médiane.  
Cela montre que la sensibilité d'IBM aux fluctuations du marché est relativement constante, même à la médiane des rendements excédentaires.

### Interprétation de la régression Quantile sur plusieurs quantiles :
Les résultats pour différents quantiles (\(\tau\)) montrent que le bêta de IBM varie de 0.92 à 0.97 en fonction du quantile, avec un bêta plus faible dans les quantiles inférieurs (\(\tau = 0.1\)) et plus élevé dans les quantiles supérieurs (\(\tau = 0.9\)).

Cela indique que IBM est plus sensible aux rendements du marché pendant les marchés haussiers (quantiles supérieurs) et moins sensible pendant les périodes de rendements faibles ou négatifs (marchés baissiers).

---

## 4. Modèle APT (Arbitrage Pricing Theory) :

### Régression linéaire du modèle APT (pour IBM)
Modèle :

**Résumé des résultats** :  
- **\( R^2 \)** ajusté : 0.3084  
- **F-statistique** : 29.4, p-value : < 2.2e-16 (indiquant que le modèle est globalement significatif)

**Interprétation** :  
- **rsandp (rendement excédentaire du marché)** : Le coefficient de `rsandp` est 0.961 avec une p-value < 2e-16, ce qui montre que le rendement du marché (S&P500) a un impact significatif et positif sur le rendement excédentaire d'IBM. Ce résultat est robuste, ce qui conforte l’idée que les variations du marché expliquent en grande partie les fluctuations du rendement d'IBM.  
- **dspread (écart de crédit)** : Le coefficient de `dspread` est -0.0098, avec une p-value de 0.0951 (niveau de significativité de 10%). Cela suggère que les variations des spreads de crédit influencent légèrement le rendement d'IBM, mais cette relation n'est pas aussi forte que celle du marché.  
- **rterm (écart de taux d'intérêt)** : Le coefficient de `rterm` est 0.02628 avec une p-value de 0.0559 (au seuil de 5%), ce qui indique que la courbe des taux a un impact marginal sur les rendements excédentaires d'IBM.

---

## 5. Test de significativité des coefficients (t-tests)

Le t-test montre que `rsandp` est extrêmement significatif (p-value < 2e-16), mais que des variables comme `dcredit`, `dinflation`, et `dmoney` ne sont pas significativement associées au rendement excédentaire d'IBM. Cela signifie que ces variables ont peu d'impact direct sur les rendements d'IBM, selon ce modèle.

---

## Conclusion
Ce projet a permis d'explorer l’application des modèles économétriques en finance, en particulier le CAPM et l'APT, pour analyser les rendements des actions d'IBM et de Adobe. Nous avons observé que les rendements de ces actions sont significativement influencés par les fluctuations du marché et certains facteurs économiques comme l'inflation et les spreads de crédit. Les résultats de la régression quantile ont montré que les risques extrêmes sont plus importants pour IBM, ce qui souligne l'importance de cette méthode pour une analyse plus fine des rendements dans les queues de la distribution.

Le modèle APT a mis en évidence l'impact de certains facteurs macroéconomiques, mais a aussi montré que d'autres variables comme la masse monétaire ne jouent pas un rôle significatif dans les rendements des actions analysées. En conclusion, cette étude met en lumière les limites des modèles linéaires classiques pour capturer les dynamiques complexes des rendements financiers et souligne l'intérêt des approches complémentaires comme la régression quantile. Ces résultats fournissent une base solide pour approfondir l’analyse des risques financiers, notamment en intégrant des facteurs supplémentaires ou en explorant des horizons temporels différents. 

À l’avenir, une extension possible serait d'inclure des variables liées aux événements géopolitiques ou aux innovations technologiques, qui peuvent également avoir une influence significative sur les rendements boursiers. De plus, l'intégration d'une analyse de la volatilité ou de mesures de risque comme la Value at Risk (VaR) pourrait compléter l'évaluation des rendements et des risques associés aux investissements dans des actions comme celles d'IBM et Adobe. Cette approche plus robuste permettrait de mieux appréhender les scénarios extrêmes et de renforcer la gestion des risques dans des portefeuilles d'investissement.

---

## Sources :
1. **Investing.com** : Source des données financières utilisées pour ce projet.  
2. **R Documentation** : Documentation des packages R utilisés dans l’analyse (quantreg, lmtest, plm, moments, etc.).  
3. **Support de cours d’économétrie sur Blackboard**
