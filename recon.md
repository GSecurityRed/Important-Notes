# 🔎 Recon - Anotações de Enumeração e Coleta de Informações


---

## 🌐 Enumeração de Subdomínios

Comandos para descobrir subdomínios e coletar informações HTTP básicas:

```bash
subfinder -d example.com -all -recursive| httpx -sc -lc -title -tech-detect -threads 100 -timeout 10

subfinder -d saltlabs.com -all -recursive -silent \
  | waybackurls \
  | sort -u \
  | httpx -mc 200 -title -tech-detect -threads 100 -timeout 10

dnsx -resp -a -cname -o resolved.txt
```

## 🔁 Coleta de Endpoints

Geração e coleta de endpoints com base em subdomínios:
```bash
cat subdomains.txt | httprobe | tee -a host.txt

cat host.txt | waybackurls | tee -a endpoint.txt
```
## 🧪 Fuzzing e Testes de XSS
Testes simples e automatizados de possíveis vulnerabilidades XSS:

```bash

cat endpoint.txt \
  | qsreplace '"onerror=alert(1)"' \
  | httpx -status-code

cat endpoint.txt \
  | qsreplace '"><img src=x onerror=alert(1)>' \
  | tee -a xss_fuzz.txt

cat xss_fuzz.txt | freq | tee -a possible_xss.txt

subfinder -all -d target.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/2025/CVE-2025-0133.yaml
nuclei -l listadehost.txt -tags xss -o nuclei-xss.txt
nuclei -u target.com -t cves -p http://127.0.0.1:8080 -v
nuclei -u target.com -severity high,critical
```
## 📂 Força Bruta e Descoberta de Diretórios
Comandos para fuzzing de diretórios e caminhos em aplicações web:

```bash
gobuster dir -u http://test.php.vulnweb.com -w wordlist.txt

ffuf -u http://test.php.vulnweb.com/FUZZ -w wordlist.txt

dirb http://test.php.vulnweb.com
```

## 🕵️ Enumeração Web – Comandos Diversos

Footprinting e fingerprinting de tecnologias, serviços e estrutura:

```bash
nmap -p 80,443 --script http-headers,http-title,http-enum cursos.rizomasur.org
gobuster dir -u http://target.com -w wordlist.txt
gobuster dns -d target.com -w wordlist.txt
subfinder -d target.com
whatweb http://target.com
dirb http://target.com
nikto -h http://target.com
curl -IL http://target.com
nmap -sV -p- target.com
nmap --script vuln target.com
dig any inlanefreight.com
gobuster dir   -u https:/teste   -w /usr/share/wordlists/dirb/common.txt   -t 50   -H 'Host: app.homolog.itamaraty.local'   -H 'Cookie: ASP.NET_SessionId=whvgzv2plsyzvnglugu2kxyc; outro_cookie=valor'   -k -x html,php,js,aspx,bat,txt,zip
nuclei -u IP_DA_MAQUINA -as
nuclei -u IP_DA_MAQUINA -t windows,smb,rdp,iis,exchange,ntlm
```
## SSL certificate do site principal

Muitas vezes, esse certificado inclui mais do que apenas um subdomínio, o que significa que o certificado é usado para vários domínios, e estes provavelmente ainda estão ativos.

<img width="1267" height="305" alt="image" src="https://github.com/user-attachments/assets/1fc15c87-00e5-4722-bd69-059c8bf57f91" />

Outra fonte para encontrar mais subdomínios é a [crt.sh](https://crt.sh/). 

---
## 📂 FTP (File Transfer Protocol)

O File Transfer Protocol (FTP) é um dos protocolos mais antigos da Internet. O FTP é executado dentro da camada de aplicação da pilha de protocolos TCP/IP.

- `fpt ip`  Também pode funcionar como "anonymous FTP", onde qualquer um acessa sem senha (usado para disponibilizar arquivos públicos).
- `ftp> get Important\ Notes.txt` Baixar algo
- `ftp> put testupload.txt` Com o PUT comando, podemos fazer upload de arquivos na pasta atual para o servidor FTP.
- `find / -type f -name ftp* 2>/dev/null | grep scripts` Todos os scripts NSE de FTP
 
---
## 📂 SMB (Server Message Block)

SMB é um protocolo cliente-servidor que regula o acesso a arquivos e diretórios inteiros e outros recursos de rede, como impressoras, roteadores ou interfaces lançadas para a rede. A troca de informações entre diferentes processos do sistema também pode ser feita com base no protocolo SMB. 
- Existe uma implementação alternativa do servidor SMB chamada Samba, que é desenvolvida para sistemas operacionais baseados em Unix. Samba implementa o Sistema Comum de Arquivos da Internet (CIFS) protocolo de rede. CIFS é um dialeto do SMB, o que significa que é uma implementação específica do protocolo SMB originalmente criado pela Microsoft. Isso permite que o Samba se comunique efetivamente com sistemas Windows mais recentes. Por isso, é frequentemente chamado de SMB/CIFS. 
- Quando os comandos SMB são transmitidos pelo Samba para um serviço NetBIOS mais antigo, as conexões normalmente ocorrem por portas TCP 137, 138, e 139. Em contraste, o CIFS opera pela porta TCP 445 exclusivamente.

- `smbclient //IP/ -U "user"` -> logar com um user especifico
- smbclient //10.129.202.41/devshare/ -U 'alex%lol123!mD'
- `smbclient //IP/dir` -> Acessar um pasta em especifico
- `cat /etc/samba/smb.conf | grep -v "#\|\;"` -> Ler arquivo de configuração SAMBA
- `sudo systemctl restart smbd` -> Reiniciar SAMBA apos modificar arquivo de configuração
- `smbclient -N -L //10.129.14.128` -> -L exibe uma lista dos compartilhamentos do servidor e o -N é chamado "null session", que é o acesso anonymous sem entrada de usuários existentes ou senhas.
- `get arquivo.txt` -> Baixar um arquivo para sua maquina
- `!ls / !cat / entre outros` -> Com o "!" pode-se usar comando do sistema local de sua maquina sem interromper a sessão.
- `smbstatus` -> Exibe informações de conexão, versão, etc.
- `sudo nmap 10.129.14.128 -sV -sC -p139,445` -> Pegando serviços com nmap
- `rpcclient -U "" 10.129.14.128` -> O rpcclient é uma ferramenta de linha de comando usada para interagir com serviços RPC (Remote Procedure Call) do Windows, especialmente com o SMB/CIFS.
- Tools para enumerar SMB como samrdump.py, SMBMap, CrackMapExec e enum4linux-ng (https://github.com/cddmp/enum4linux-ng)



---

### Cliente RPC

O rpcclient nos oferece muitas solicitações diferentes com as quais podemos executar funções específicas no servidor SMB para obter informações. Uma lista completa de todas essas funções pode ser encontrada no página de manual do cliente rpc.

- `rpcclient -U "" IP`

<img width="992" height="487" alt="image" src="https://github.com/user-attachments/assets/d3d2ee8b-f9df-4452-a397-1525770e3c82" />

---

## 📂 NFS (Network File System)

Network File System (NFS) é um sistema de arquivos de rede desenvolvido pela Sun Microsystems e tem a mesma finalidade que o SMB. Seu objetivo é acessar sistemas de arquivos através de uma rede como se fossem locais. No entanto, ele usa um protocolo totalmente diferente.

Ao rastrear o NFS, as portas TCP 111 e 2049 são essenciais

- sudo nmap 10.129.14.128 -p111,2049 -sV -sC
- nmap --script nfs* 10.129.14.128 -sV -p111,2049 -> O rpcinfo do script NSE recupera uma lista de todos os serviços RPC em execução no momento, seus nomes e descrições e as portas que eles usam.
- ls -l mnt/nfs/ -> Listar conteúdo com nomes de usuário e nomes de grupos
- ls -n mnt/nfs/ -> Listar conteúdo com UIDs e GUIDs 
- showmount -e 10.129.202.5 -> Mostrar diretorios compartilhados disponiveis com everyone </br>
### Montagem compartilhamento NFS </br>
 - mkdir target-NFS </br>
 - sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock </br>
 - cd target-NFS </br>
 - tree . </br>
### Desmontando </br>
 - cd .. </br>
 - sudo umount ./target-NFS </br>
---
## 📂 DNS

<img width="983" height="592" alt="image" src="https://github.com/user-attachments/assets/40804917-f354-4119-ae6f-d2f5fc8988ce" />

Diferente DNS records são usados para consultas DNS, todas com tarefas diversas. Além disso, existem entradas separadas para diferentes funções, pois podemos configurar servidores de e-mail e outros servidores para um domínio.

<img width="1032" height="712" alt="image" src="https://github.com/user-attachments/assets/69a1d90b-0518-4280-89b4-5fc30c2d15db" />

- dig soa www.inlanefreight.com
- dig AAAA www.inlanefreight.com
- dig any inlanefreight.htb @10.129.14.128
- dig CH TXT version.bind 10.129.120.85 -> Version Query
- dig axfr inlanefreight.htb @10.129.14.128
- dig axfr internal.inlanefreight.htb @10.129.14.128
- dnsenum --dnsserver 10.129.149.49 --enum -p 0 -s 0 -o subdomains.txt -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb   -> brute force de servidores dns

### Todos os servidores DNS funcionam com três tipos diferentes de arquivos de configuração:

- arquivos de configuração DNS local
- arquivos de zona
- arquivos de resolução de nome reverso

O servidor DNS Bind9 é muito frequentemente usado em distribuições baseadas em Linux. Seu arquivo de configuração local (named.conf) é dividido aproximadamente em duas seções: primeiro, a seção de opções para configurações gerais e, segundo, as entradas de zona para os domínios individuais. Os arquivos de configuração local geralmente são:

- named.conf.local
- named.conf.options
- named.conf.log

- cat /etc/bind/named.conf.local
- cat /etc/bind/db.domain.com
- cat /etc/bind/db.10.129.14

### DNS TOOLS

<img width="1008" height="905" alt="image" src="https://github.com/user-attachments/assets/49eb6f12-c9c7-4e9a-b46e-0bf91be5cf0a" />

---


## 📂 SMTP

O Simple Mail Transfer Protocol (SMTP) é um protocolo para envio de e-mails em uma rede IP. Ele pode ser usado entre um cliente de e-mail e um servidor de e-mail de saída ou entre dois servidores SMTP. O SMTP funciona sem criptografia, sem medidas adicionais, e transmite todos os comandos, dados ou informações de autenticação em texto simples. Para evitar a leitura não autorizada de dados, o SMTP é usado em conjunto com a criptografia SSL/TLS. Sob certas circunstâncias, um servidor usa uma porta diferente da porta TCP padrão 25 para a conexão criptografada, por exemplo, porta TCP 465.

Para interagir com o servidor SMTP, podemos usar o telnet ferramenta para inicializar uma conexão TCP com o servidor SMTP

- telnet 10.129.14.128 25
- O comando VRFY pode ser usado para enumerar usuários existentes no sistema. No entanto, isso nem sempre funciona. Dependendo de como o servidor SMTP está configurado, o servidor SMTP pode apresentar problemas code 252 e confirmar a existência de um usuário que não existe no sistema.
- https://serversmtp.com/smtp-error/ -> Uma lista de todos os códigos de resposta SMTP
- Da pra enviar o e-mail pelo proprio terminal via SMTP.
- Os scripts Nmap padrão incluem smtp-commands, que usa o EHLO comando para listar todos os comandos possíveis que podem ser executados no servidor SMTP de destino. sudo nmap 10.129.14.128 -sC -sV -p25
- sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
- O comando VRFY no SMTP serve para perguntar ao servidor se um determinado usuário/endereço de e-mail existe.
- o comando STARTTLS serve para inicializar uma sessão criptografada (TLS/SSL)
- smtp-user-enum -M VRFY -U agoraVAI.txt -t 10.129.124.140 -v -w 15
---

## 📂 POP3 / IMAP

Ao contrário do Post Office Protocol (POP3), o IMAP permite o gerenciamento on-line de e-mails diretamente no servidor e suporta estruturas de pastas. O POP3, por outro lado, não possui a mesma funcionalidade do IMAP e fornece apenas listagem, recuperação e exclusão de e-mails como funções no servidor de e-mail.

- Por padrão, portas 110 e 995 são usados para POP3 e portas 143 e 993 são usados para IMAP.  Os portos mais altos (993 e 995) use TLS/SSL para criptografar a comunicação entre o cliente e o servidor.
- nmap 10.129.123.200 -sV -sC -p 110,993,995,143
- No entanto, vejamos a lista de comandos e vejamos como podemos interagir e nos comunicar diretamente com IMAP e POP3 usando a linha de comando.:
- openssl s_client -connect 10.129.123.200:pop3s
- "A" OU ALGUMA OUTRA VOGAL É NECESSARIO PARA OS COMANDOS DENTRO DO IMAP
- A LIST "" *  (lista tudo primeiro)
- A SELECT DIR_ESPECIFICO (se tiver algum conteudo disponivel aparecera algo como EXISTS 1 OU MAIS)
- DENTRO DO SELECT DO DIR ESPECIFICO DAR UM: A SEARCH ALL (irá pegar o ID daquele conteudo que tem no dir, e assim vc pode rodar o comando abaixo para vizualizar o conteudo)
- A FETCH ID_ESPECIFICO BODY[TEXT]  
- A FETCH ID_ESPECIFICO BODY[HEADER]
- /usr/share/nmap/scripts/imap-brute.nse
- /usr/share/nmap/scripts/imap-capabilities.nse
- /usr/share/nmap/scripts/imap-ntlm-info.nse

<img width="1295" height="872" alt="image" src="https://github.com/user-attachments/assets/81626df3-236f-49c1-bbe1-4feb6486b9f3" />

<img width="1312" height="708" alt="image" src="https://github.com/user-attachments/assets/4c6b3f60-cfe1-42f6-ac16-6d2bcbc01871" />
Configurações perigosas
<img width="1298" height="411" alt="image" src="https://github.com/user-attachments/assets/7cf5470c-146e-4563-833c-b5cfbe5c7157" />

---
## 📂 SNMP

O SNMP (Simple Network Management Protocol) é um protocolo padrão usado para monitoramento e gerenciamento de dispositivos de rede. Ele permite que administradores acompanhem o estado e o desempenho de equipamentos como roteadores, switches, servidores, impressoras, câmeras IP, entre outros.. Além disso, este protocolo também pode ser usado para lidar com tarefas de configuração e alterar configurações remotamente.

- SNMPv1 e v2c: simples, mas com segurança fraca (senhas em texto claro).
- SNMPv3: adiciona criptografia e autenticação, tornando o protocolo mais seguro para redes críticas.
- MIB = catálogo organizado das informações.
- OID = “código de barras” que identifica cada informação dentro da MIB.
- Community strings: Funciona como uma senha simples que controla o acesso ao dispositivo via SNMP. Basicamente é o que a gente precisa pra se autenticar

### Portas principais do SNMP

- UDP 161 → usada pelo gerente SNMP (NMS) para consultar ou configurar o agente SNMP no dispositivo. </br>
Exemplo: coletar informações de CPU, interfaces, etc.</br>

- UDP 162 → usada pelos agentes SNMP para enviar Traps ou Notifications (alertas) para o gerente.</br>
Exemplo: quando uma interface de rede cai, o agente dispara uma mensagem automática para o sistema de monitoramento.

### Recon SNMP

- Para fazer footprinting do SNMP, podemos usar ferramentas como snmpwalk, onesixtyone, e braa.
- Snmpwalk é usado para consultar os OIDs com suas informações.
- Onesixtyone pode ser usado para forçar as Community strings, pois elas podem ser nomeadas arbitrariamente pelo administrador. Como essas strings de comunidade podem ser vinculadas a qualquer fonte, identificar as strings de comunidade existentes pode levar algum tempo.
- snmpwalk -v2c -c public 10.129.14.128
- sudo apt install onesixtyone
- onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128
- Depois de conhecermos uma string comunitária, podemos usá-la braa para forçar brutamente os OIDs individuais e enumerar as informações por trás deles.
- sudo apt install braa
- braa <community string>@<IP>:.1.3.6.*   # Syntax
- braa public@10.129.14.128:.1.3.6.*   ou  braa "backup@10.129.55.151:.*"  (o nome antes do arroba varia dependendo de usuario e o resultado do onesixtyone)
---

## 📂 MySQL

- É um SGBD (Sistema Gerenciador de Banco de Dados) relacional
- O banco de dados é controlado usando o Linguagem de banco de dados SQL
- Um dos melhores exemplos de uso de banco de dados é o CMS WordPress. O WordPress armazena todas as postagens, nomes de usuário e senhas criadas em seu próprio banco de dados, que só pode ser acessado pelo host local.
- Esses bancos de dados geralmente são armazenados em um único arquivo com a extensão file .sql, por exemplo, como wordpress.sql.
- Modelo relacional → organiza dados em tabelas (linhas e colunas).
- Dados confidenciais, como senhas, podem ser armazenados em texto simples pelo MySQL; no entanto, eles geralmente são criptografados previamente pelos scripts PHP usando métodos seguros, como Criptografia unidirecional.
- O aplicativo web informa ao usuário se ocorrer algum erro durante o processamento, podendo nos fornecer informações pra um possivel SQL injection.

### Recon MySQL

- Normalmente, o servidor MySQL é executado na porta TCP 3306, e podemos escanear esta porta com Nmap para obter informações mais detalhadas.
- Os bancos de dados mais importantes para o servidor MySQL são os system schema (sys) e information schema (information_schema). O esquema do sistema contém tabelas, informações e metadados necessários para o gerenciamento.

```
- nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
- Interação com o servidor MySQL: mysql -u root -pP4SSw0rd -h 10.129.14.128 (o -p realmente tem que ser junto da senha)
- show databases;	Mostrar todas as bases de dados.
- select version(); Versão
- use <database>;	Selecione um dos bancos de dados existentes.
- select version();
- show tables; Mostrar todas as tabelas disponíveis na base de dados seleccionada.
- show columns from <table>; Mostrar todas as colunas na tabela seleccionada.
- select * from <table> Mostrar tudo na tabela desejada.
- select * from <table> where <column> = "<string>";	Procurar por necessário string na tabela desejada.
- select host, unique_users from host_summary;
````
---

## 📂 MSSQL

- Microsoft SQL (MSSQL) é o sistema de gerenciamento de banco de dados relacional baseado em SQL da Microsoft. Ao contrário do MySQL, que discutimos na última seção, o MSSQL é de código fechado e foi inicialmente escrito para ser executado em sistemas operacionais Windows.
- SQL Server Management Studio (SSMS) vem como um recurso que pode ser instalado com o pacote de instalação do MSSQL ou pode ser baixado e instalado separadamente.
- Ele não existe apenas no servidor que hospeda o banco de dados. Isso significa que podemos nos deparar com um sistema vulnerável com SSMS com credenciais salvas que nos permitem nos conectar ao banco de dados. A imagem abaixo mostra o SSMS em ação.
  
<img width="1269" height="920" alt="image" src="https://github.com/user-attachments/assets/335ec8ed-e72e-4aa7-a6d0-3cdfe5c02e8a" />

- Muitos outros clientes podem ser usados para acessar um banco de dados em execução no MSSQL: / mssql-cli /	SQL Server PowerShell	/ HeidiSQL / SQLPro /	Mssqlclient.py do Impacket
- Dos clientes MSSQL listados acima, os pentesters podem achar o mssqlclient.py do Impacket o mais útil devido ao projeto Impacket da SecureAuthCorp estar presente em muitas distribuições de pentesting na instalação. Para descobrir se e onde o cliente está localizado em nosso host, podemos usar o seguinte comando: locate mssqlclient.

  <img width="1312" height="640" alt="image" src="https://github.com/user-attachments/assets/3cf25999-4756-4e0a-bc73-a611bb90b543" />


### Configurações perigosas

- Clientes MSSQL que não usam criptografia para se conectar ao servidor MSSQL
- O uso de certificados autoassinados quando a criptografia está sendo usada. É possível falsificar certificados autoassinados
- O uso de named pipes
- Credenciais padrões ou fracas.

### Recon MSSQL

-  O NMAP possui scripts mssql padrão que podem ser usados para direcionar a porta tcp padrão 1433 que o MSSQL escuta.
- nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
- Também podemos usar o Metasploit para executar um scanner auxiliar chamado mssql_ping que escaneará o serviço MSSQL e fornecerá informações úteis em nosso processo de pegada. Imagem abaixo:
  <img width="1294" height="631" alt="image" src="https://github.com/user-attachments/assets/3e81747e-2d24-4e1e-a82c-033bfd7b07c7" />

- Conectando-se com Mssqlclient.py:  impacket-mssqlclient user@IP -windows-auth
- Sem -windows-auth → você autentica com login e senha do SQL Server.
- Com -windows-auth → você autentica com credenciais Windows/AD, podendo até abusar de hash/tickets em pentests.
- enum_db → lista as db
- SELECT @@VERSION; → versão
- enum_users → lista usuários do SQL Server.
- enum_logins → lista logins de instância.
---

## Oracle TNS

O Oracle Transparent Network Substrate (TNS) server é um protocolo de comunicação que facilita a comunicação entre bancos de dados Oracle e aplicativos através de redes.

- Por padrão, o ouvinte escuta as conexões de entrada na porta TCP/1521
- Os arquivos de configuração do Oracle TNS são chamados tnsnames.ora e listener.ora e normalmente estão localizados no $ORACLE_HOME/network/admin diretório.
- O Oracle 9 tem uma senha padrão, CHANGE_ON_INSTALL
- Ferramenta de ataque ao banco de dados Oracle (ODAT) é uma ferramenta de teste de penetração de código aberto escrita em Python e projetada para enumerar e explorar vulnerabilidades em bancos de dados Oracle. Ele pode ser usado para identificar e explorar várias falhas de segurança em bancos de dados Oracle, incluindo injeção de SQL, execução remota de código e escalonamento de privilégios.
- nmap -p1521 -sV 10.129.204.235 --open
- nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
- sudo apt install odat
- odat all -s 10.129.204.235
- podemos usar a ferramenta sqlplus para se conectar ao banco de dados Oracle e interagir com ele.
- sqlplus scott/tiger@10.129.204.235/XE
- Se você se deparar com o seguinte erro sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory, por favor execute o abaixo,
- sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
- select table_name from all_tables;
- SELECT username FROM all_users;
- select * from user_role_privs;
- sqlplus scott/tiger@10.129.204.235/XE as sysdba -> tentar escalar user normal logado para root
- SELECT name, password FROM sys.user$;  -> ver todos os hashes de senha de usuario


Podemos seguir muitas abordagens quando tivermos acesso a um banco de dados Oracle. Depende muito das informações que temos e de toda a configuração. No entanto, não podemos adicionar novos usuários nem fazer modificações. A partir deste ponto, poderíamos recuperar os hashes de senha do sys.user$ e tente quebrá-los offline. A consulta para isso seria semelhante à seguinte:

- select name, password from sys.user$;

Outra opção é fazer upload de um shell da web para o destino. No entanto, isso requer que o servidor execute um servidor web, e precisamos saber a localização exata do diretório raiz do servidor web. No entanto, se soubermos com que tipo de sistema estamos lidando, podemos tentar os caminhos padrão, que são: </br>
Linux	/var/www/html </br>
Windows	C:\inetpub\wwwroot </br>

- echo "Oracle File Upload Test" > testing.txt
- ./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
- curl -X GET http://10.129.204.235/testing.txt
---

## 📂 IPMI

Interface de gerenciamento de plataforma inteligente (IPMI) é um conjunto de especificações padronizadas para sistemas de gerenciamento de host baseados em hardware usados para gerenciamento e monitoramento de sistemas. Ele atua como um subsistema autônomo e funciona independentemente do BIOS, CPU, firmware e sistema operacional subjacente do host. </br>
O IPMI é normalmente usado de três maneiras:
- Antes que o sistema operacional seja inicializado para modificar as configurações do BIOS
- Quando o host estiver totalmente desligado
- Acesso a um host após uma falha do sistema </br>

O IPMI se comunica pela porta 623 UDP e os sistemas que usam o protocolo IPMI são chamados de Controladores de Gerenciamento de Baseboard (BMCs).

- Se pudermos acessar um BMC durante uma avaliação, obteremos acesso total à placa-mãe host e poderemos monitorar, reiniciar, desligar ou até mesmo reinstalar o sistema operacional host.
- nmap -sU --script ipmi-version -p 623 ilo.inlanfreight.local
- use auxiliary/scanner/ipmi/ipmi_version 
- senha padrões como root:calvin / Administrator:randomized 8-character string consisting of numbers and uppercase letters / ADMIN:ADMIN podem ser encontradas as vezes.
- caso senhas padrões não funcionem, ainda se da para usar a uma falha no protocolo RAKP no IPMI 2.0
- use auxiliary/scanner/ipmi/ipmi_dumphashes
- hashcat -m 7300 ipmi.txt /usr/share/wordlists/rockyou.txt.gz -a 0 --status
- hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u
- hashcat

## 🧰 Extras e Ferramentas Úteis

Ferramenta para buscar segredos e chaves de API expostas:

🔐 TruffleHog: busca por credenciais, secrets e chaves de API em históricos de código, repositórios Git, arquivos e strings expostas.


