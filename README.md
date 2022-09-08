# subdomain-Recon


--------------------wordlist----------------

https://github.com/six2dez/OneListForAll

https://gist.github.com/jhaddix/f64c97d0863a78454e44c2f7119c2a6a

https://github.com/danielmiessler/SecLists

assetnote Manually Generated Wordlists

best-dns-wordlist.txt, 2m-subdomains.txt

---------------------------Collecting subdomain----------------------------

=>subdomain enum from bunch of resource

amass with api key in config.ini

$ amass enum -d example.com -passive -o amass_subdomain.txt

subdinder with api key in provided-config.yalm

$ subfinder -d example.com -silent -t 100 -nc -all > subfinder_subdomain.txt

assetfinder the tool made by tomnomnom

$ assetfinder --subs-only example.com | tee assetfinder_subdomain.txt

------------------bruteforcing for subdomain--------------------

$ puredns burteforce /path/to/subdomain.txt example.com -w puredns_bf_domain.txt -r resolver.txt

----------------------collecting subdomain in one wordlist--------------------

$ cat amass_subdomain.txt subfinder_subdomain.txt assetfinder_subdomain.txt puredns_bf_subdomain.txt | sort -u | anew collected_subdomain.txt

-------------------permutation subdomain---------------

$ gotator -sub collected_subdomain.txt -perm /path/to/permutation_wordlist.txt -depth 1 -numbers 10 -mindup -adv -md -silent > subs_to_resolve.txt

-------------------resolving the permutation wordlist-----------

$ puredns resolve subs_to_resolve.txt -r resolvers.txt --write valid_alt_domain.txt

----------------------------creating a wordlist----------------------

$ cat collected_subdomain.txt valid_alt_domain.txt | sort -u | tee subdomain.txt

------------------------------collecting url-----------------------------

$ cat subdomain.txt | gau --blacklist png,jpg,jpeg,img,svg,mp3,mp4,woff,woff1,woff2,css | tee gau_url.txt

----------------------------------spidering----------------------------------

$ cat subdomain.txt | hakrawler -t 1 -d 3 | tee crawled.txt

for js

cat crawled.txt | grep example.com | grep ".js$"

------------------------------------using httpx----------------------------

$ cat gau_url.txt | httpx -sc -td -server -ip -cname -json -o httpx.json -mc 200 -x POST GET TRACE OPTIONS

--------------------------------searching alive subdomain---------------------

$ cat subdomain.txt | httprobe | tee alive.txt

------------------------------------using aquaton--------------------------

$ cat subdomain.txt | aquatone

-------------------------------------port scaning--------------------------

for below 50 subdomain

$ nmap -sC -sV -sT -T2 -iL subdomain.txt -oN nmap.txt -vv

large list of subdomain

___subdomain to ip

$ cat subdomains.txt | xargs -n1 host | grep "has address" | cut -d" " -f4 | sort -u > ips.txt

$ masscan -iL ips.txt -p0-65535 --rate=10000 -oL scan.txt

----------------------directory burteforcing------------------

$ ffuf -w wordlist.txt -u https://example.com/FUZZ
