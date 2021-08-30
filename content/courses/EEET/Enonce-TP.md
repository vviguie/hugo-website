---
title: "TP : le modèle NEDUM-2D"
date: '2021-01-01'
type: book
weight: 25
toc: true
---

TP de modélisation

<!--more-->
{{< icon name="clock" pack="fas" >}} 3h


## Fichiers du TP
{{< icon name="download" pack="fas" >}} [fichiers du TP](http://www.centre-cired.fr/wp-content/uploads/2021/08/TP_EEET.zip)

Le dossier comprend 4 fichiers:
- `TP Modele urbain NEDUM.pdf` : énoncé du TP
- `Script_analyse.py` : fonctions Python principales
- `Slides_NEDUM.pdf` : présentation du modèle NEDUM
- `output_NEDUM.csv` : données pour le TP

## Introduction
Dans ce TP, nous allons utiliser le modèle NEDUM-2D pour étudier des stratégies 2010-2030 de transition post-carbone pour l’agglomération parisienne s’appuyant sur plusieurs politiques :

- Le passage d’une tarification “par  zones” des transports en commun à un tarif unique pour l’ensemble des zones. Cela correspond principalement à une baisse des tarifs de transport en commun, notamment pour les localisations périphériques.
- La mise en place d’une ceinture verte en 2010, qui correspond à l’interdiction de construction dans la zone située en dehors des limites de l’agglomération en 2010.
- L’interdiction de toute nouvelle construction de bâtiments résidentiels (par rapport à 2010) dans les zones inondables de l’Ile-de-France.

Ces politiques sont supposées avoir des effets sur les déplacements domicile-travail, donc sur les émissions de gaz à effet de serre, mais aussi sur l’étalement urbain, les prix des loyers et/ou l’exposition aux risques d’inondation. Pour étudier les impacts de ces politiques, on utilise le modèle NEDUM-2D calibré sur l’Ile-de-France entre 1900 et 2008 (voir document de présentation). Le modèle est utilisé dans sa version monocentrique, où l’on suppose que tous les habitants font des déplacements domicile-travail vers le centre de l’agglomération (défini comme étant la station de métro Châtelet).

Par ailleurs, on définit un scénario d’évolution des variables socio-économiques utilisées en entrée de NEDUM-2D entre 2010 et 2030. Ce scénario de référence est défini comme étant le scénario « Central hypothesis for 2030 » dans le tableau ci-dessous (les hypothèses basses et hautes servent à des études de sensibilité des résultats qui ne sont pas demandées dans ce TP). On compare les résultats en sortie du modèle pour ces entrées en appliquant ou non les politiques précédemment citées.

{{< figure src="/table.png" title="Scenarios" >}}

## Description des données
Le fichier ‘NEDUM_output.csv’ contient les valeurs de 4 variables de sortie pour des simulations de NEDUM-2D pour 5 différents scénarios (scénario de référence pour les variables + activation ou non des politiques). Chaque variable se présente sous la forme d’un vecteur avec les valeurs pour chaque point de grille de l’espace utilisé pour l’application de NEDUM-2D à l’agglomération parisienne (6541 “pixels” de 1km x 1km).

Les scénarios sont :
- 'simul_2008_etat_initial' : simulation de l’état initial pour l’année 2008.
- 'simul_2030_sc1_BAU' : scénario 1 à l’horizon 2030, “Business as Usual”.  
- 'simul_2030_sc2_TU' : scénario 2, mise en place d’une tarification unique pour les transports en commun (fin du système de zones).
- 'simul_2030_sc3_ZI' : scénario 3, interdiction des constructions nouvelles dans les zones inondables.
- 'simul_2030_sc4_CV : scénario 4, mise en place d’une ceinture verte.
- 'simul_2030_sc5_mix' : scénario 5, mise en place simultanée d’une ceinture verte et d’une tarification unique pour les transports en commun.

Pour chacun de ces états simulés, 4 variables sont données dans le fichier :

- 'loyer' : loyer mensuel par m2
- 'logement' : densité de construction de logements, en nombre de m2 construits par nombre de km2 de terrain.
- 'taille' : taille des logements
- 'emissions' : émissions de gaz à effet de serre dues aux déplacement domicile-travail pour un ménage vivant dans le carreau de la grille (en geqCO2).

En plus de ces variables, le tableau contient les coordonnées X et Y de chaque carreau de grille, dans un repère ad hoc centré sur le point choisi comme étant le centre de l’agglomération et ayant pour unité le km.

Enfin, une variable ‘coeff_land’ donne la part de la surface du pixel qui est urbanisable (on enlève ainsi les parcs, lacs, zones protégées, etc.), ainsi qu’une variable ‘part_inond’ qui donne la part de la surface urbanisable qui est située en zone inondable. Ces deux variables ont été estimées à l’aide de la base de donnée européennes Corinne Land Cover et avec un logiciel de SIG.

## Questions
### Effet de la tarification des transports en commun
   1. Décrire l’effet attendu théoriquement d’un passage d’une tarification des transports en commun par zone où le prix de l’abonnement augmente lorsqu’on s’éloigne du centre à un tarif unique (effet sur les modes de transport et effet sur la structure de la ville).
   2. La densité de population (en nombre de ménages / km2) dans chaque carré de grille s’obtient en divisant la quantité de logement construit par la taille moyenne des logements pour un ménage. Tracer une carte de la comparaison de la densité de population pour le scénario 1 (BAU) et le scénario 2 (Tarif unique). Idem pour la construction (en multipliant par ‘coeff_land ‘). Quel est l’impact du tarif unique sur l’étalement urbain ?  Est-ce l’effet attendu ?
   3. Que prédit NEDUM-2D sur l’impact de la tarification unique des transports sur les émissions de gaz à effet de serre liées au déplacement domicile-travail ? Pourquoi ?
   4. Tracer une carte de la différence des loyers entre scénario BAU et scénario avec tarif unique. Discuter de l’équité d’une telle mesure.

### Interdiction de construire dans les zones inondables
   1. En théorie, interdire de construire dans les zones inondables permet de limiter le nombre de logements exposés aux inondations. Quantifier cet effet dans NEDUM-2D en comparant le nombre de logements en zone inondable dans le scénario 3 par rapport au scénario BAU (sc1).
   2. Tracer une carte de la différence de loyers due à l’interdiction de construire dans les zones inondables. Comment expliquez-vous ces résultats ?
### Effet de la ceinture verte
   1. Calculer la surface totale urbanisée dans le scénario avec ceinture verte (sc4) et le comparer au scénario de référence (sc1). Idem pour les émissions de gaz à effet de serre. Conclure sur les avantages de la ceinture verte.
   2. Estimer le nombre de personnes vivant en zone inondable et comparer au scénario de référence. Expliquer les résultats obtenus. Idem pour le loyer au centre de l’agglomération (le maximum des loyers).
### Mix de politiques
   1. Comparer les résultats pour les émissions de gaz à effet de serre, le loyer au centre de Paris, la surface urbanisée et le nombre de logements en zone inondable pour le scénario avec ceinture verte + tarif unique + interdiction de construire dans les zones inondables (sc5) et pour le scénario BAU.
   2. Remplir un tableau avec les impacts (+ ou -, éventuellement négligeable) de ces trois politiques lorsqu’elles sont mises en place séparément ou ensemble, sur les différents objectifs que l’on se fixe (prix des logements dans Paris, émissions de gaz à effet de serre, limitation des risques liés aux inondations, étalement urbain / protection des espaces naturels et agricoles).
   3. Conclure.

### Discussion
   1. Au vu du travail de ce TP, qu’apporte un modèle comme NEDUM-2D par rapport au modèle théorique de l’économie urbaine ?
   2. Quels sont les principaux messages en termes de politiques publiques que l’on pourrait déduire d’un tel travail de modélisation ?
   3. Ci-dessous est présentée la carte du projet de Grand Paris Express. Le modèle tel que l’on vous l’a présenté est-il adapté à l’étude des impacts de ce projet ? Pourquoi ?



{{< figure src="GPE.png">}}

## Script

```python

#!/usr/bin/env python2
# -*- coding: utf-8 -*-
"""Exemple de script pour l'analyse des résultats de NEDUM"""

import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

import geopandas
import contextily as ctx
from shapely.geometry import Point #pour faire des cartes


##################################
#%% Importation des données
##################################

outNedum = pd.read_csv('output_NEDUM.csv', sep = ',')

# transformatio en geodataframe (pour afficher comme une carte)
geometry = [Point(xy) for xy in zip(outNedum['X'], outNedum['Y'])]
outNedum_gdf = geopandas.GeoDataFrame(outNedum, geometry=geometry)
# on fixe le bon système de coordonnées
# CRS = coordinate reference system (cf. https://en.wikipedia.org/wiki/Spatial_reference_system)
# le CRS 2154 c'est le système de référence de l'IGN pour les cartes autour de Paris
outNedum_gdf.crs = "EPSG:2154"
# on convertit dans le système de coordonnées utilisé par Google Map
outNedum_gdf=outNedum_gdf.to_crs(epsg=3857)

##################################
#%% Exemples de courbes et de cartes
##################################

#%% 1. Courbe des loyers en fonction de la distance

#pour fermer les autres figures
plt.close('all')

#coordonnées du centre de Paris
X_centre_Paris=653750
Y_centre_Paris=6862350
outNedum['distance_centre'] = np.sqrt((outNedum['X']-X_centre_Paris) ** 2 + (outNedum['Y']-Y_centre_Paris) ** 2)/1000
plt.scatter(outNedum['distance_centre'],outNedum['simul_2030_sc1_BAU_loyer'])

#%% 2. Carte de la construction 

#pour fermer les autres figures
plt.close('all')

#'-logement' est en nombre de m2 construits par nombre de km2 au sol
# Attention, pour avoir le Coefficient d'Occupation des Sols, il faut diviser par 10^6 
plt.figure()
plt.scatter(outNedum['X'], outNedum['Y'],20, outNedum['simul_2030_sc3_ZI_logement'] * outNedum['coeff_land']) #construction
plt.colorbar()

#%% 3. Pour tracer une carte avec un fond de carte 

# fonction qui trace une carte
def carto(variable=outNedum['coeff_land'],X=outNedum['X'],Y=outNedum['Y']):   
    # creation d'un geodataframe (pour afficher comme une carte)
    # 1. on cree les infos spatiales
    geometry = [Point(xy) for xy in zip(X, Y)]
    # 2. on cree le dataframe a transformer en geodataframe
    data_frame_temp=pd.concat([X,Y,pd.Series(variable)], axis=1)
    # 3. on le convertit en geodataframe
    geodata_frame_temp = geopandas.GeoDataFrame(data_frame_temp, geometry=geometry)
    
    # on fixe le bon système de coordonnées
    # CRS = coordinate reference system (cf. https://en.wikipedia.org/wiki/Spatial_reference_system)
    # le CRS 2154 c'est le système de référence de l'IGN pour les cartes autour de Paris
    geodata_frame_temp.crs = "EPSG:2154"
    # on convertit dans le système de coordonnées utilisé par Google Map et Open Street Map (nécessaire pour ajouter automatiquement un fond de crte)
    geodata_frame_temp=geodata_frame_temp.to_crs(epsg=3857)
    
    # on trace la carte
    ax=geodata_frame_temp.plot(column=geodata_frame_temp.columns[2],
              alpha=0.5, 
              edgecolor="None",
              legend=True,
              marker='o',
              s=20)
    # on ajoute un fond de carte: le fond Stamen Toner (https://wiki.openstreetmap.org/wiki/Stamen)
    ctx.add_basemap(ax,
                source=ctx.providers.Stamen.Toner,
                alpha=0.8
                )
    # ax.set_axis_off() 
    return ax

#pour fermer les autres figures
plt.close('all')

carto(outNedum['simul_2030_sc3_ZI_logement'] * outNedum['coeff_land'])

#%% 4. Si jamais on veut enregistrer la dernière figure affichée

plt.savefig("nom_de_la_figure.png", dpi=200)   


##################################
#%% Calcul de la densité de population
##################################

# Attention, il faut multiplier par 'coeff_land' qui représente la part du pixel qui est urbanisable
sc1_densite_pop = outNedum['simul_2030_sc1_BAU_logement'] * outNedum['coeff_land'] / outNedum['simul_2030_sc1_BAU_taille'] #en ménages / km2

##################################
#%% Calcul des émissions totales de la ville
##################################

#Les variables 'emissions' donnent la quantité d'émissions pour un ménage vivant dans le carreau (en gCO2 / an)
sc1_emission_tot = np.sum( sc1_densite_pop * outNedum['simul_2030_sc1_BAU_emissions']) / np.sum(sc1_densite_pop) /1000 #en moyenne par ménage en kgCO2

##################################
#%% Calcul de la surface urbanisée
##################################

borne_urba = 100000 #en dessous de cette densité de construction on considère qu'on n'est plus en ville 
sc1_surface_urba = np.sum(outNedum['coeff_land']*(outNedum['simul_2030_sc1_BAU_logement']>borne_urb   1.) #en km2

##################################
#%% Scénario 3: zone inondable
##################################

#'part_inond' représente la part de la surface constructible qui est située en zone inondable
carto(outNedum['part_inond'])

```

