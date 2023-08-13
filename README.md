# namecheap_mikrotik_ddns
MikroTik DDNS updater script for Namecheap

Thanks to:
* https://www.ipify.org
* https://github.com/MattDietz/mattdietz.net/blob/master/posts/namecheap-dyndns-with-a-mikrotik.md
* https://www.namecheap.com/support/knowledgebase/article.aspx/29/11/how-to-dynamically-update-the-hosts-ip-with-an-http-request/

MikroTik script:
```
# namecheap-update.rsc
# Set local variables. If you want to update subdomain.example.com, put in "subdomain" under subdomain and "example.com" in the domain variable. If you want to update just example.com with no subdomain, enter in "@" for the subdomain.

:local password "<password_from namecheep>"
:local subdomain "<optional_subdomain>"
:local domain "<domain.tld>"

:log info "Update started"

# Get Public IP
/tool fetch url="https://api.ipify.org" mode=http dst-path=pubIP.txt
:local currentIP [ /file get pubIP.txt contents ]
:log info "Current Public IP is:$currentIP"

# Post to Namecheep
:local url1 "http://dynamicdns.park-your-domain.com/update\3Fhost=$subdomain&domain=$domain&password=$password&ip=$currentIP"
/tool fetch url=($url1) mode=http
:log info "DNS Successfully Updated"%
```

MilroTik scripts are created on your local work station and sftp to the router. 
* https://youtu.be/QdKLe1TBZPU
