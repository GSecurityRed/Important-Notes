# CURL

![image](https://github.com/user-attachments/assets/496335fc-692e-49e7-bff8-0fc819cab474)

## O que é CURL?

O comando `curl` é uma ferramenta de linha de comando usada para transferir dados de ou para servidores usando protocolos como HTTP, HTTPS, FTP, entre outros. Ela é útil para testar APIs, baixar arquivos e automatizar requisições web.

---

## Comandos importantes

- `curl <IP>`  
  Faz uma requisição HTTP para o endereço IP informado.

- `curl -O <IP>`  
  Faz uma requisição HTTP para o endereço IP informado e salva em um arquivo do seu diretório.

- `curl -v <IP>`  
  Faz uma requisição HTTP para o endereço IP informando detalhes da conexão (modo detalhado).

- `curl -vvv <IP>`  
  Faz uma requisição HTTP para o endereço IP com informações muito detalhadas.

- `curl -k <IP>`  
  Faz uma requisição HTTPS para o endereço IP informado **sem verificar o certificado**, forçando a entrega da requisição mesmo que o certificado seja inválido ou suspeito.

- `curl -I <IP>`  
  Mostra apenas os **cabeçalhos** da resposta, **sem o corpo**.

- `curl -i <IP>`  
  Mostra os **cabeçalhos da resposta junto com o corpo** (conteúdo da página).

- `curl -H <IP>`  
  Define um determinado **cabeçalho HTTP** a ser passado.

- `curl -L <IP>`  
  Segue o fluxo da requisição em caso de redirecionamento (códigos 3xx como 302).

- `curl -b <IP>`  
  Passa um **cookie de sessão**.

- `curl -A <IP>`  
  Define um **User-Agent** personalizado.

- `curl -s <IP>`  
  Modo "silencioso", filtra determinadas saídas da requisição, como mensagens de erro.

- `curl -X POST <IP> -d`  
  Faz uma requisição HTTP POST e utiliza `-d` para enviar parâmetros.

- `curl -X POST <IP> | jq`  
  Faz uma requisição HTTP POST e utiliza o utilitário `jq` para manipular a resposta JSON.


- `curl -X POST -d '{"search":"flag"}' -b 'PHPSESSID=1f5a2pmrjjo579htvnpqu3g7s5' -H 'Content-Type: application/json' http://83.136.251.68:58479/search.php`
  
    Faz uma requisição HTTP POST utilizando o -d para passar o parametro a ser procurado pelo back-end (search.php), com -b para passar um cookie de sessão válido (dessa forma sem precisar passar alguma credencial) e o -H para passar o cabeçalho de Content-Type      especificando que o back-end formate em JSON

- `curl http://94.237.50.250:49189/search.php?search=flag -H 'Authorization: Basic YWRtaW46YWRtaW4='`
  
   Faz uma requisição HTTP POST utilizando o -H para passar o cabeçalho Authorization para bypassar as credenciais.

- `curl ipinfo.io/<IP>`
  
   Faz uma requisição HTTP para o site ipinfo.io, retornando a origem do IP.

 - `curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'`
   
   Faz uma requisição HTTP POST para a API (api.php) para criar na tabela "city" do uma cidade chamada HTB_City e usando o -H para especificar o JSON.

---

