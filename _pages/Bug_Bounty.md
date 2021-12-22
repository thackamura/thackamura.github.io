---
layout: archive
title: "Bug Bounty"
permalink: /Bug_Bounty/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}
<br/>

Qu'est ce que la reconnaissance?
======
La reconnaissance est la première étape essentielle de la liste de contrôle du pentester avant la recherche de vulnérabilités. 
Il s'agit essentiellement d'énumérer les sous-domaines (si le champ d'application est un nom de domaine avec un caractère générique), de vérifier les ports ouverts, de rechercher les hôtes actifs et de trouver les technologies utilisées par l'entreprise.

Reconnaissance active vs reconnaissance passive
======
La reconnaissance active a une approche très directe de la cible. Il s'agit en fait de mettre votre capuche de hacker et de prendre le risque d'une détection précoce tout en sondant le système à la recherche de faiblesses. C'est la méthode la plus rapide et elle peut éventuellement conduire à la découverte de vulnérabilités potentielles sans trop creuser (oui, vous pouvez attraper et ouvrir un panneau d'administration dans la nature).
<br/>
<br/>La reconnaissance passive consiste à recueillir autant d'informations que possible sur la cible sans avoir à interagir avec elle. Cela peut se faire en cherchant dans les moteurs de recherche Google des liens utiles ou même dans les médias sociaux des informations utiles sur les employés (à ne pas confondre avec le harcèlement).

<br/>
  
Découverte des actifs
======
On vous a dit que tout ce qui appartient à une certaine société se trouve dans le périmètre, et vous voulez savoir ce que cette société possède réellement.
Le but de cette phase est d'obtenir toutes les sociétés appartenant à la société principale, puis tous les actifs de ces sociétés. Pour ce faire, nous allons :

* Trouver les acquisitions de la société principale, cela nous donnera les sociétés à l'intérieur du périmètre.
* Trouver l'ASN (s'il y en a un) de chaque société, ce qui nous donnera les plages d'IP appartenant à chaque société.
* Utilisez les recherches inversées de whois pour trouver d'autres entrées (noms d'organisation, domaines...) liées à la première (ceci peut être fait de manière récursive).
* Utiliser d'autres techniques comme shodan et sslfilters pour rechercher d'autres actifs.


Publications
======
  <ul>{% for post in site.publications %}
    {% include archive-single-Cheetsheat.html %}
  {% endfor %}</ul>
  
Talks
======
  <ul>{% for post in site.talks %}
    {% include archive-single-talk-Cheetsheat.html %}
  {% endfor %}</ul>
  
Teaching
======
  <ul>{% for post in site.teaching %}
    {% include archive-single-Cheetsheat.html %}
  {% endfor %}</ul>
  
Service and leadership
======
* Currently signed in to 43 different slack teams
