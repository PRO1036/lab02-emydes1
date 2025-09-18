Lab 02 - Plastic waste
================
Émy Descarreaux
16-09-2025

## Chargement des packages et des données

``` r
library(tidyverse) 
```

``` r
plastic_waste <- read_csv("data/plastic-waste.csv")
```

Commençons par filtrer les données pour retirer le point représenté par
Trinité et Tobago (TTO) qui est un outlier.

``` r
plastic_waste <- plastic_waste %>%
  filter(plastic_waste_per_cap < 3.5)
```

## Exercices

### Exercise 1

``` r
ggplot(plastic_waste,aes(x=plastic_waste_per_cap))+
  geom_histogram(binwidth = 0.1)+
  facet_wrap(~continent)
```

![](lab-02_files/figure-gfm/plastic-waste-continent-1.png)<!-- -->

### Exercise 2

``` r
ggplot(plastic_waste,aes(x=plastic_waste_per_cap,colour=continent, fill=continent))+
  geom_density(alpha=0.4)
```

![](lab-02_files/figure-gfm/plastic-waste-density-1.png)<!-- -->

les réglages de la couleur (colour et fill) se retrouvent dans aes
puisqu’on associe la couleur et le remplissage à la donnée continent.
Pour ce qui est du réglage pour la transparence (alpha), il se trouve
dans geom_dendity puisqu’on veut modifier un aspect de la géométrie du
graphique entier. \### Exercise 3

Boxplot:

``` r
ggplot(plastic_waste,aes(x=continent,y=plastic_waste_per_cap))+
  geom_boxplot()
```

![](lab-02_files/figure-gfm/plastic-waste-boxplot-1.png)<!-- -->

Violin plot:

``` r
ggplot(plastic_waste,aes(x=continent,y=plastic_waste_per_cap))+
  geom_violin()
```

![](lab-02_files/figure-gfm/plastic-waste-violin-1.png)<!-- -->

Les violons permettent de voir la distribution des données de façon plus
détaillée ce qui n’est pas possible avec les boxplot.

### Exercise 4

``` r
ggplot(plastic_waste,aes(x=plastic_waste_per_cap,y=mismanaged_plastic_waste_per_cap,colour =continent ))+
  geom_point()
```

![](lab-02_files/figure-gfm/plastic-waste-mismanaged-1.png)<!-- -->

Il semble y avoir une tendance positive puisque plus la quantité de
déchets produits par habitant augmente, plus la quantité de déchets non
gérés est grande. De plus, la majorité des pays ne produisent pas
beaucoup de déchets par habitants et n’ont pas beaucoup de déchets non
gérés car une grande partie des points se trouve en bas et à gauche du
graphique. Plus précisément, on remaque que les continents plus
développés comme l’Europe et L’Amérique du Nord ont tendance à produire
plus de déchets par habitants mais ceux-ci sont assez bien gérés.
L’Europe semble gérer plus ses déchets que l’Amérique du Nord puisque
les points sont plus bas dans le graphique. En revanche, l’Afrique
semble avoir une moins bonne gestion des déchets puisque lorsque la
quantité de déchets par habitant augmente, les déchets non gérés
augmentent.L’Asie, l’Océanie et l’Amérique du Sud semblent plus
éparpillés, ainsi certains de leurs pays gèrent très bien les déchets
alors que d’autres les gèrent très mal puisqu’il y a des points dans le
bas et très haut dans le graphique.

### Exercise 5

``` r
ggplot(plastic_waste, aes(x=total_pop,y=plastic_waste_per_cap,colour=continent))+
  geom_point()
```

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/plastic-waste-population-total-1.png)<!-- -->

``` r
ggplot(plastic_waste, aes(x=coastal_pop,y=plastic_waste_per_cap,colour=continent))+
  geom_point()
```

![](lab-02_files/figure-gfm/plastic-waste-population-coastal-1.png)<!-- -->

Il ne semble pas vraiment y avoir une relation plus forte pour une des
paires de variables. Les deux graphiques sont assez difficiles à
interpréter puisque les valeurs extrèmes de population font en sorte que
le reste des données sont concentrées à gauche des graphiques.Ainsi, on
ne peut pas affirmer que les déchets sont plus produits par les
habitants côtiers que le reste de la population. \## Conclusion

Recréez la visualisation:

``` r
plastic_waste_coastal <- plastic_waste %>% 
  mutate(coastal_pop_prop = coastal_pop / total_pop) %>%
  filter(plastic_waste_per_cap < 3)

ggplot(plastic_waste_coastal,aes(x=coastal_pop_prop,y=plastic_waste_per_cap, colour=continent))+
  geom_point()+
  geom_smooth(method="loess",se=TRUE, colour="black")+
  labs(title = "Quantité de déchets plastiques Vs Proportion de la population côtière",subtitle = "Selon le continent",x="Proportion de la population côtière (Coastal/total population)",y="Nombre de déchets plastiques par habitant")
```

    ## `geom_smooth()` using formula = 'y ~ x'

    ## Warning: Removed 10 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 10 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](lab-02_files/figure-gfm/recreate-viz-1.png)<!-- -->

La courbe noire est la courbe tendance du nombre de déchets plastiques
par habitants selon la proportion de la population côtière. Ainsi, on
remarque que le nomdre de déchets augmente légèrement lorsque la
proportion de la population côtière augmente.On remarque aussi que les
pays produisant le plus de déchets par habitants selon la proportion de
population côtière sont en Amérique du Nord, en Asie et en Europe.
