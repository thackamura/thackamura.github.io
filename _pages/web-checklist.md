---
layout: archive
title: "Web Checklist"
permalink: /web-checklist/
author_profile: true
redirect_from:

---

{% include base_path %}
<br/>

Phase de reconnaissance
======
* Obtenir l'ASN pour trouver les plages IP avec [bgp.he.net](https://bgp.he.net/)
* Examiner les acquisitions avec [crunchbase.com](https://www.crunchbase.com/)
<br/>
<br/>

* Recenser les sous-domaines via OSINT (subfinder, assetfinder, amass)
* Recenser les sous-domaines via Brute Force (gobuster dns -d DOMAINE -t 50 -w /usr/share/spiderfoot/discts/subdomains.txt)
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
* Leak ([dehashed.com](https://www.dehashed.com/), [pwndb](https://github.com/davidtavarez/pwndb), [breachdirectory.com](https://breachdirectory.org/))
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

