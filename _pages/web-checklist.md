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