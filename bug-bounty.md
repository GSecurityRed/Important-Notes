"Vulnerability Disclosure Program" "VDP" site:uk -bughunt -hackerone -intigriti -bugcrowd -yeswehack

https://dorks.faisalahmed.me/


nmap -sU -p 53 --script=dns-nsid 10.129.2.48 -> ver~soa dns do servidor alvo pode ser vuln de exposição de dados sensiveis etc


subfinder -all -d alvo.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/2025/CVE-2025-0133.yaml


nmap 10.129.4.84 -sV -D RND:2 -Pn -sU -sS -g53 --max-retries 2 --script-args http.useragent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36" -p-

nuclei -t cves/ -u http://alvo.com -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
