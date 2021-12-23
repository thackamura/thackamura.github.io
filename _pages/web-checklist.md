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

* Recenser les sous-domaines via OSINT (subfinder, assetfinder, amass)
* Recenser les sous-domaines via Brute Force (gobuster dns -d DOMAINE -t 50 -w /usr/share/spiderfoot/discts/subdomains.txt)
* [shodan.io](https://www.shodan.io/)
* Transfert de zone (dig)
* Captures d'écran avec [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness)