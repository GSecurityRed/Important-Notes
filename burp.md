# 🐞 Burp Suite - Anotações Rápidas

Anotações essenciais para uso do Burp Suite em pentests, CTFs e testes de segurança web.

---

## 🔁 Interceptação & Proxy

- **Intercept is On/Off** → Controla o tráfego interceptado manualmente.
- **Proxy → Intercept → Forward** → Envia requisição interceptada ao destino.
- **Proxy → Options → Match & Replace** → Edita conteúdo automaticamente (ex: headers).

---

## 🔍 Repeater

- **Send** → Reenvia a requisição manualmente com ou sem alterações.
- **Ctrl + R (envia pra Repeater)** → Atalho para testar manipulações em endpoints.

---

## 🔬 Intruder

- **Sniper** → Testa um campo de cada vez.
- **Battering Ram** → Mesma carga em todos os campos.
- **Cluster Bomb** → Combina múltiplas cargas em múltiplos campos.
- **Pitchfork** → Testa cargas lado a lado (linha a linha).

---

## 📦 Decoder

- **Decode/Encode** → Codifica/decodifica strings (Base64, URL, Hex, etc).
- **Hash** → Gera hashes (MD5, SHA1, etc).

---

## 🎯 Comparador

- **Load→Compare** → Compara diferenças entre duas respostas (por exemplo, antes/depois de bypass).

---

## 🧠 Extensões Úteis (BApp Store)

- **Autorize** → Detecta problemas de controle de acesso.
- **ActiveScan++** → Aumenta escaneamento automático.
- **Logger++** → Registra todas as requisições e respostas.
- **Param Miner** → Descobre parâmetros ocultos.

---

## 💡 Dicas Práticas

- Use **FoxProxy / Burp CA Cert** no navegador para interceptar HTTPS.
- Combine **Intruder + Repeater** para ataques de brute-force + validação manual.
- Utilize **Hotkeys** para acelerar testes (ex: Ctrl + I para enviar ao Intruder).

---

> ⚠️ Use Burp apenas com autorização. Ferramenta poderosa = responsabilidade dobrada.




### Pra usar o proxychains como proxy também junto com o burp: 

- nano /etc/proxychains4.conf
- descomenta o quiet mode
- comenta os proxys tor
- e adciona o http 127.0.0.1 8080
- ligue o burp e habilite o intercept on
- no terminal: proxychains4 curl IP
- *Aparecerá a requisição no seu burp*

---


### Pra usar com Metasploit

- msfconsole
- msf6 > use auxiliary/scanner/http/robots_txt
- msf6 auxiliary(scanner/http/robots_txt) > set PROXIES **HTTP:127.0.0.1:8080**
- msf6 auxiliary(scanner/http/robots_txt) > set RHOST SERVER_IP
- msf6 auxiliary(scanner/http/robots_txt) > set RPORT PORT
- msf6 auxiliary(scanner/http/robots_txt) > run
- *Aparecerá a requisição no seu burp*









