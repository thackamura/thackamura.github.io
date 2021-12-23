---
layout: archive
title: "Web Checklist"
permalink: /web-checklist/
author_profile: true
redirect_from:

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

* [viewdns.info](https://viewdns.info/reversewhois/)
* [domaineye.com](https://domaineye.com/reverse-whois)
* [reversewhois.io](https://www.reversewhois.io/)

Autres : amass intel -d DOMAINE -whois

PS: Cette technique fonctionne de manière récursive


Etape 3 - Trackers
======
Si vous trouvez le même ID du même tracker dans 2 pages différentes, vous pouvez supposer que les deux pages sont gérées par la même équipe.
<br/>Par exemple, si vous voyez le même ID de Google Analytics ou le même ID d'Adsense sur plusieurs pages.
<br/>Il existe des pages qui vous permettent de faire des recherches par ces trackers et plus encore :

* [builtwith.com](https://builtwith.com/)
* [publicwww.com](https://publicwww.com/)
* [spyonweb.com](https://spyonweb.com/)


Etape 4 - Shodan
======
Comme vous le savez déjà, le nom de l'organisation propriétaire de l'espace IP. 
<br/>Vous pouvez rechercher cette donnée dans [shodan.io](https://www.shodan.io/) en utilisant : org : "EXEMPLE". 

Etape 5 - Google Dorks
======
Dans l'utilisation quotidienne, les moteurs de recherche comme Google, Bing, DuckDuckGo et Yahoo acceptent un terme de recherche (un mot), ou une chaîne de termes de recherche, et renvoient les résultats correspondants. 
<br/>Mais la plupart des moteurs de recherche sont programmés pour accepter des « filtres » ou des « opérateurs de préfixes » plus avancés.
<br/>Utilisation du filtre « site :  » pour trouver des sous domaines.

<br/>

Sous-domaines
======
Nous connaissons toutes les entreprises du périmètre, tous les actifs de chaque entreprise et tous les domaines liés à ces entreprises.
<br/>Il est temps de trouver tous les sous-domaines possibles de chaque domaine trouvé.

Etape 1 - OSINT
======
Le moyen le plus rapide d'obtenir un grand nombre de sous-domaines est de chercher dans des sources externes. 
<br/>Un très bon endroit pour rechercher des sous-domaines est [crt.sh](https://crt.sh/).

Autres : amass enum -d DOMAINE

Etape 2 - Brute force DNS
======
Essayons de trouver de nouveaux sous-domaines en faisant du brute force les serveurs DNS en utilisant les noms de sous-domaines possibles.
<br/>J’utilise personnellement gobuster.

* gobuster dns -d DOMAINE -t 50 -w /usr/share/spiderfoot/discts/subdomains.txt

<br/>

Recherche de serveurs Web
======
Nous avons trouvé toutes les entreprises et leurs actifs et nous connaissons les plages d'IP, les domaines et les sous-domaines à l'intérieur du périmètre. Il est temps de rechercher les serveurs web.
<br/>Dans les étapes précédentes, vous avez probablement déjà effectué une reconnaissance des IP et des domaines découverts, et vous avez donc peut-être déjà trouvé tous les serveurs Web possibles. Cependant, si ce n'est pas le cas, nous allons maintenant voir quelques astuces rapides pour rechercher des serveurs web à l'intérieur du périmètre.
<br/>Veuillez noter que cette recherche est orientée vers les applications web, vous devez également effectuer un scan des vulnérabilités et des ports (si le périmètre le permet).

Etape 1 - Masscan
======
Une méthode rapide pour découvrir les ports ouverts liés aux serveurs web est d’utilisé masscan.
* sudo masscan -p80,443,8000-8100,8443 IP/Masque

Etape 2 - Captures d'écran
======
Maintenant que vous avez découvert tous les serveurs web fonctionnant dans le périmètre (dans les IP de l'entreprise et dans tous les domaines et sous-domaines), vous ne savez probablement pas par où commencer. Alors, faisons simple et commençons par faire des captures d'écran de chacun d'entre eux.
<br/>En jetant un coup d'œil à la page principale de chacun d'entre eux, vous pourrez trouver des points d'accès étranges plus susceptibles d'être vulnérables.
<br/>Utilisation de [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness)

<br/>

Conclusion
======
A ce stade, vous avez déjà effectué toutes les énumérations de base. Oui, c'est basique parce que beaucoup d'autres énumérations peuvent être faites.
<br/>Donc vous avez déjà :

* Trouver toutes les entreprises dans le périmètre
* Trouver tous les actifs appartenant aux entreprises
* Trouver tous les domaines appartenant aux entreprises
* Trouver tous les sous-domaines des domaines 
* Trouver tous les serveurs web et en faire une capture d'écran 

<br/>Ensuite, c'est l'heure de la vraie chasse aux vulnérabilités. 
