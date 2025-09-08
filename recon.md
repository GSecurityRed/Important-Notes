# 🔎 Recon - Anotações de Enumeração e Coleta de Informações

Este documento reúne comandos e técnicas úteis para **reconhecimento de aplicações web e infraestrutura**, com foco em:

- Descoberta de subdomínios  
- Coleta de endpoints  
- Fuzzing de parâmetros  
- Descoberta de diretórios  
- Fingerprinting de tecnologias  
- Ferramentas auxiliares

Ideal para uso em CTFs, pentests ou estudos de segurança ofensiva.

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

## Cliente RPC

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
- showmount -e 10.129.202.5 -> Mostrar ações NFS disponíveis </br>
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

<img width="1295" height="872" alt="image" src="https://github.com/user-attachments/assets/81626df3-236f-49c1-bbe1-4feb6486b9f3" />

<img width="1312" height="708" alt="image" src="https://github.com/user-attachments/assets/4c6b3f60-cfe1-42f6-ac16-6d2bcbc01871" />
Configurações perigosas
<img width="1298" height="411" alt="image" src="https://github.com/user-attachments/assets/7cf5470c-146e-4563-833c-b5cfbe5c7157" />



---
## 🧰 Extras e Ferramentas Úteis

Ferramenta para buscar segredos e chaves de API expostas:

🔐 TruffleHog: busca por credenciais, secrets e chaves de API em históricos de código, repositórios Git, arquivos e strings expostas.


