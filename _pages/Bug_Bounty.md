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


Etape 1 - Acquisitions
======
Tout d'abord, nous devons savoir quelles autres sociétés sont détenues par la société principale.
<br/>L'une des possibilités consiste à consulter le site [crunchbase.com](https://www.crunchbase.com), à rechercher l'entreprise principale et à cliquer sur la section "acquisitions".

Etape 2 - ASNs
======
Un numéro de système autonome (ASN) est un numéro unique attribué à un système autonome (AS) par l'Internet Assigned Numbers Authority (IANA).
<br/>Un AS est constitué de blocs d'adresses IP qui ont une politique distincte d'accès aux réseaux externes et sont administrés par une seule organisation mais peuvent être constitués de plusieurs opérateurs.
<br/>Il est intéressant de savoir si l'entreprise a attribué un ASN pour trouver ses plages IP. Il sera intéressant d'effectuer un test de vulnérabilité contre tous les hôtes à l'intérieur de la portée et de rechercher les domaines à l'intérieur de ces IP.
<br/>Vous pouvez effectuer une recherche par nom de société, par IP ou par domaine sur [bgp.he.net](https://bgp.he.net/).
<br/>Autres sources : [asnlookup.com](http://asnlookup.com/) [ipv4info.com](http://ipv4info.com/) [amass](https://github.com/OWASP/Amass)

<br/>

Domaines
======
Nous connaissons toutes les entreprises du périmètre et leurs actifs, il est temps de trouver les domaines du périmètre.
<br/>Veuillez noter que dans les techniques suivantes, vous pouvez également trouver des sous-domaines et que cette information ne doit pas être sous-estimée.
<br/>Tout d'abord, vous devez rechercher le ou les domaines principaux de chaque entreprise. Par exemple, pour Orange, ce sera orange.com.


Etape 1 - Reverse DNS
======
Comme vous avez trouvé toutes les plages d'adresses IP des domaines, vous pouvez essayer d'effectuer des recherches DNS inversées sur ces adresses IP pour trouver d'autres domaines dans le champ d'application. Essayez d'utiliser un serveur DNS de la victime ou un serveur DNS bien connu (1.1.1.1, 8.8.8.8).

* dnsrecon -d facebook.com -r IP/Masque

Autres : [ptrarchive.com](http://ptrarchive.com/)


Etape 2 - Reverse Whois
======
Dans un whois, vous pouvez trouver beaucoup d'informations intéressantes comme le nom de l'organisation, l'adresse, les emails, les numéros de téléphone... Mais ce qui est encore plus intéressant, c'est que vous pouvez trouver plus d'actifs liés à la société si vous effectuez des recherches inversées dans le whois par l'un de ces champs (par exemple d'autres registres whois où le même email apparaît).
<br/>Vous pouvez utiliser des outils en ligne tels que :

[viewdns.info](https://viewdns.info/reversewhois/), [domaineye.com](https://domaineye.com/reverse-whois), [reversewhois.io](https://www.reversewhois.io/)