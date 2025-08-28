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

## NFS (Network File System)

Network File System (NFS) é um sistema de arquivos de rede desenvolvido pela Sun Microsystems e tem a mesma finalidade que o SMB. Seu objetivo é acessar sistemas de arquivos através de uma rede como se fossem locais. No entanto, ele usa um protocolo totalmente diferente.

Ao rastrear o NFS, as portas TCP 111 e 2049 são essenciais

- sudo nmap 10.129.14.128 -p111,2049 -sV -sC
- nmap --script nfs* 10.129.14.128 -sV -p111,2049 -> O rpcinfo do script NSE recupera uma lista de todos os serviços RPC em execução no momento, seus nomes e descrições e as portas que eles usam.
- ls -l mnt/nfs/ -> Listar conteúdo com nomes de usuário e nomes de grupos
- ls -n mnt/nfs/ -> Listar conteúdo com UIDs e GUIDs

 [!bash!]$ mkdir target-NFS </br>
 [!bash!]$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock </br>
 [!bash!]$ cd target-NFS </br>
 [!bash!]$ tree .


---

## 🧰 Extras e Ferramentas Úteis

Ferramenta para buscar segredos e chaves de API expostas:

🔐 TruffleHog: busca por credenciais, secrets e chaves de API em históricos de código, repositórios Git, arquivos e strings expostas.


