"Vulnerability Disclosure Program" "VDP" site:uk -bughunt -hackerone -intigriti -bugcrowd -yeswehack

https://dorks.faisalahmed.me/


nmap -sU -p 53 --script=dns-nsid 10.129.2.48 -> ver~soa dns do servidor alvo pode ser vuln de exposição de dados sensiveis etc


subfinder -all -d alvo.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/2025/CVE-2025-0133.yaml
