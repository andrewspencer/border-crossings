---
layout: post
title: "Softshake compte rendu"
date: 2010-10-20
---

J'étais lundi à SoftShake, une conférence réunissant des courants différents mais assez complémentaires de l'informatique.  En parallèle il y avait des présentations Java, Agile, iPhone et Incubateur, cette dernière catégorisation regroupant tout ce qui méritait d'être présenté mais qui ne tombait pas sous l'égide de l'un des autres sillons [NdT : je suppose que c'est comme ça qu'ils disent "track" à l'Académie Française].

Il y avait des sujets intéressants dans toutes les sessions, mais j'ai surtout fréquenté la salle Incubateur, avec une petite interlude Java.  J'ai laissé de côté la session Agile, avec regret, et le sillon iPhone, sans regret.  (J'ai déjà acquis les bases en Agilité, et je ne travaille pas avec iOS.)

Voici un petit compte rendu de deux des présentations qui m'ont le plus marqué.
<!--more-->
<h3>Dominic Williams : Le développement durable</h3>

J'avais déjà vu et aimé la présentation de Dominic sur le développement hédoniste à XP Day Paris en 2009.  J'avais donc de bons espoir pour celle-ci, d'autant plus que le développement durable (dans les deux sens du jeu de mots) est un sujet qui me tient à coeur.  Je n'ai pas été deçu : c'était limpide, bien étoffé, réfléchi et qui donne à réfléchir.

Dominic a commencé par un tour d'horizon du bilan écologique et social de notre économie et société actuelles : réchauffement climatique, pollution, inégalités entre les sexes et entre les hommes et femmes, maladies dues au stress professionnel...  Ensuite il est passé au bilan de l'industrie informatique : génération des déchets toxiques, consommation d'énergie, etc.  L'empreinte du secteur, quoique non prépondérante, n'est pas non plus négligeable.  Il nous a dessiné les tendances, qui vont vers l'économie d'énergie (<a href="http://www.greenit.fr">Green IT</a>) et l'élimination des substances toxiques (rendue obligatoire en Europe par RoHS il y a deux ans déjà).

Ensuite -- et c'est la partie la plus intéressante -- il a donné quelques actions que nous pouvons prendre, individuellement et en tant que secteur, pour réduire notre empreinte et notre bilan social.  Individuellement, nous pouvons :
<ul>
	<li>Conserver et réutiliser le vieux matériel (exemple : mettre un vieil écran peu performant dans une salle serveurs où il sera rarement utilisé)</li>

	<li>Paramétrer correctement les mises en veille</li>

	<li>Choisir du matériel peu gourmand, c'est-à-dire non surdimensionné vis-à-vis du besoin réel
Etre regardant sur les datacentres, exiger un reporting de son fournisseur</li>

	<li>Se renseigner sur la "note" environnementale des fabricants de matériel et des fournisseurs de services (Greenpeace fournit des classements)</li>

	<li>Utiliser des technologies qui exigent moins de puissance (par exemple, JEE demande du matériel plus puissant que PHP+MySQL... mais c'est pas pour autant que vous me retrouverez à faire du PHP).  Le cloud computing peut aider sur ce point dans la mesure où il aide à mutualiser les infrastructures.</li>

</ul>


En regardant plus largement notre travail, Dominic a pu ensuite faire un peu de pub pour l'Agilité.  Si on peut améliorer l'efficacité de l'équipe, on va générer plus rapidement de la valeur métier -- d'ailleurs c'est un des objectifs premiers des processus Agiles -- et donc notre empreinte écologique, pour un résultat équivalent, sera moindre.  Pour générer rapidement de la valeur métier, il faut faire du développement itératif et incrémental.

Un autre objectif de l'Agilité est de respecter les développeurs et autres acteurs en tant qu'êtres humains.  Dominic a parlé du <a href="http://en.wikipedia.org/wiki/Hawthorne_effect">Hawthorne Effect</a>, un phénomène qui a été constaté comme quoi le fait de s'intéresser au leur processus métier augmente la productivité des travailleurs.  L'Agilité implique davantage les développeurs dans leur propre processus, mais également veut se mettre davantage à l'écoute du métier du client.

Dominic a ensuite suggéré de ne pas perdre l'art de l'optimisation : faire attention aux goulets d'étrangement ; être conscient de la performance des algorithmes que l'on utilisé (il est tentant de traiter la mémoire et le CPU comme illimités) ; et plus largement, comprendre le fonctionnement de la mémoire, de l'ordinateur, du réseau, afin de ne pas gaspiller leurs ressources.  

On peut tirer le bilan écologique et social de son produit.  Par exemple, les logiciels anti-spam ont un excellent bilan ecologique si l'on tient compte des ressources qu'ils permettent d'économiser, qui autrement seraient dépensées à traiter ledit spam.  (D'autres exemples sur <a href="http://smart2020.org/">SMART2020</a>.)

Sur l'amélioration des aspects sociaux de la vie professionnel, plusieurs suggestions, dont j'ai retenu la notion de "manager son manager" et la piste de la structure co-opérative (en France, il s'agit du statut de <a href="http://fr.wikipedia.org/wiki/Soci%C3%A9t%C3%A9_coop%C3%A9rative_de_production">SCOP</a>).

Pour finir sur une note d'espoir, Dominic a fait valeur que l'informatique a le potentiel d'aider les autres secteurs à réduire leur empreinte, en les aidant à automatiser, à être économe... 

<h3>Vaadin</h3>

<a href="http://vaadin.com/">Vaadin</a> est un framework qui permet de faire des applications Web, en écrivant du code Java, et uniquement du Java, avec une API d'un style similaire à Swing (en fait l'API semble plus lisible que celle de Swing).  <a href="http://twitter.com/joonaslehtinen">Joonas Lehtinen</a> nous a fait une rapide présentation de l'historique du projet et de son principe de fonctionnement.  Il s'agit de "RIA thin client", c'est-à-dire que la plupart des choses se passent côté serveur, et sur le client vous avez les widgets de présentation qui ont des canaux de communication avec les objets sur le serveur.  Chaque widget est représentée par deux classes : une classe écrite en Java qui sera compilée en bytecode et déployée sur le serveur, qui est celle avec laquelle on code, et une classe déployée sur le navigateur, écrite aussi en Java mais traduite en Javascript par <a href="http://code.google.com/webtoolkit/">GWT</a>.  Tout cela est une boîte noire pour le développeur, et on code (presque) comme si on codait une application s'exécutant entièrement sur le serveur.

Une conséquence de ce modèle est qu'on a accès à l'ensemble des bibliothèques JSE et JEE, et non pas un petit sous-ensemble comme c'est le cas avec GWT.  Une autre conséquence est que la charge sur le serveur est plus importante que pour une application où beaucoup des fonctionnalités s'exécutent en Javascript sur le navigateur.  (<a href="http://vaadin.com/blog/-/blogs/server-side-ria-scalability">Voici une présentation sur la question</a>.)

Une bonne moitié de la présentation a été une démonstration de codage en temps réel d'une petite application web avec la librairie Vaadin.  C'est toujours fascinant de regarder quelqu'un d'autre coder en direct, surtout quand c'est un expert, et j'ai beaucoup aimé cette démo.  De plus, cela illustre très concrètement la limpidité de l'API Vaadin.

Alors j'ai bien envie d'essayer ce framework, d'autant plus que l'écosystème semble assez riche.  Il y aurait une rubrique "démo" sur le site où l'on peut jouer avec des mini-apps et étudier leur code source.  Ils ont un wizard de création de thème, comme le <a href="http://jqueryui.com/themeroller/">ThemeRoller de JQuery UI</a>, où on peut personnaliser l'un des thèmes par défaut et ensuite télécharger une définition de ce thème personnalisé.  On peut créer ses propres widgets, et il existe déjà un catalogue de composants fournis par des tiers, qui sont publiés sur un repository Maven.  Il y a un outil VaadinTestBench dérivé de Selenium avec l'ajout de capabilités pertinentes à Vaadin...  Et apparamment le framework est même déployable sur Google App Engine.

Voilà : une excellente conférence, et je remercie vivement les organisateurs et les présentateurs de l'énorme travail qu'ils ont accompli.
