lembrar apenas: sudo docker-compose -f docker-compose.yml --compatibility up -d

API significa Application Programming Interface (Interface de Programação de Aplicações), um conjunto de regras, protocolos e ferramentas que permite que diferentes softwares se comuniquem e troquem dados de forma segura e eficiente. Ela funciona como um intermediário que facilita a integração entre sistemas, possibilitando que um aplicativo utilize as funcionalidades de outro sem precisar conhecer seus detalhes internos.

atento aos esquemas óbvios de nomenclatura de URL:

- https://target-name.com/api/v1
- https://api.target-name.com/v1 
- https://target-name.com/docs
- https://dev.target-name.com/rest

Procure indicadores de API em nomes de diretórios como:
/api, /api/v1, /v1, /v2, /v3, /rest, /swagger, /swagger.json, /doc, /docs, /graphql, /graphiql, /altair, /playground

Além disso, subdomínios também podem ser indicadores de APIs da web:

- API.target-name.com
- uat.target-name.com
- dev.target-name.com
- teste.target-name.com

Outro indicador de APIs da web são os cabeçalhos de solicitação e resposta HTTP. O uso de JSON ou XML pode ser um bom indicador de que você descobriu uma API. Cabeçalhos de solicitação e resposta HTTP contendo "Tipo de conteúdo: application/json, application/xml"

- Além disso, fique atento às respostas HTTP que incluem instruções como:
{"mensagem": "Token de autorização ausente"}

- trufflehog
- https://web.archive.org/


### Recon API

- nmap -sV --script=http-enum <target> -p 80,443,8000,8080
- amass enum -active -d target-name.com | grep api
- gobuster dir -u targetaddress/ -w /usr/share/wordlists/api_list/common_apis_160 -x 200,202,301 -b 302

*Kiterunner* é uma excelente ferramenta que foi desenvolvida e lançada pela Assetnote. O Kiterunner é atualmente a melhor ferramenta disponível para descobrir endpoints e recursos de API. Embora ferramentas de força bruta de diretório como Gobuster/Dirbuster/ funcionem para descobrir caminhos de URL, elas normalmente dependem de solicitações HTTP GET padrão. O Kiterunner não apenas usará todos os métodos de solicitação HTTP comuns com APIs (GET, POST, PUT e DELETE), mas também imitará estruturas comuns de caminho de API.

#### Passo a passo para baixar:

- sudo apt install golang
- git clone https://github.com/assetnote/kiterunner
- cd kiterunner
- make build

# 🛡️ Disclaimer 

This repository was created **for educational and cybersecurity research purposes only**.  
The use of any information, scripts, or tools contained herein **is the sole responsibility of the user**.  
**I am not responsible for any misuse** or activity that violates local laws or third-party policies.


© [GSecurity]
- sudo mv dist/kr /usr/local/bin/
- kr -h


