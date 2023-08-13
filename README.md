# namecheap_mikrotik_ddns
MikroTik DDNS updater script for Namecheap

Thanks to:
* https://www.ipify.org
* https://github.com/MattDietz/mattdietz.net/blob/master/posts/namecheap-dyndns-with-a-mikrotik.md
* https://www.namecheap.com/support/knowledgebase/article.aspx/29/11/how-to-dynamically-update-the-hosts-ip-with-an-http-request/
* MikroTik scripts are created/edited on your work station and sftp to the router. https://youtu.be/QdKLe1TBZPU 

MikroTik script:
```
# namecheap-update.rsc
# Set local variables. If you want to update subdomain.example.com, put in "subdomain" under subdomain and "example.com" in the domain variable. If you want to update just example.com with no subdomain, enter in "@" for the subdomain.

:local password "43ddc42212c24feba2285ece449f39a8"
:local subdomain "woodfern"
:local domain "wd6awp.us"

:global dyndnsForce
:global previousIP

:log info "NameCheap DDNS update started"

# Get Public IP
/tool fetch url="https://api.ipify.org" mode=http dst-path=pubIP.txt
:local currentIP [ /file get pubIP.txt contents ]

:log info "Current Public IP is $currentIP"
:log info "Previous Public IP is $previousIP"

# Remove the # on next line to force an update every single time - useful for debugging, but you could end up getting blacklisted by DynDNS!
#:set dyndnsForce true

# Determine if dyndns update is needed
:if (($currentIP != $previousIP) || ($dyndnsForce = true)) do={
    :set dyndnsForce false
    :set previousIP $currentIP
    :local url1 "http://dynamicdns.park-your-domain.com/update\3Fhost=$subdomain&domain=$domain&password=$password&ip=$currentIP"
    /tool fetch url=($url1) mode=http
    :log info "DNS Successfully Updated"
} else={
    :log info ("No DNS update needed")
}
```
