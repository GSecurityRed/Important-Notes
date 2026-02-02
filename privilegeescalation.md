# 🧬 Privilege Escalation - Anotações de Elevação de Privilégios

Este documento contém referências, comandos e dicas para **elevação de privilégios em ambientes Linux e Windows**, seja em CTFs, pentests ou laboratórios de estudo.

---

## 📜 Scripts de Enumeração (Linux / Windows)

Ferramentas automatizadas para levantamento de informações que possam levar à escalada de privilégios:

- 🔍 [PEASS-ng (LinPEAS / WinPEAS)](https://github.com/peass-ng/PEASS-ng)
- 🐧 [LinEnum](https://github.com/rebootuser/LinEnum)
- 🧪 [linuxprivchecker](https://github.com/sleventyeleven/linuxprivchecker)
- 🪟 [Seatbelt (Windows)](https://github.com/GhostPack/Seatbelt)
- 🪟 [JAWS - Windows Privilege Escalation](https://github.com/411Hall/JAWS)

> ⚠️ **Atenção:** Esses scripts executam diversos comandos potencialmente detectáveis por antivírus ou sistemas de monitoramento. Em ambientes sensíveis ou reais, considere fazer **enumeração manual** para reduzir o "ruído".

---

## 🛠️ Dicas e Técnicas Gerais

Comandos e abordagens manuais úteis para escalada de privilégios.

### 🔑 Acesso via chave SSH do root

Verifique permissões da chave privada do root:

```bash
ls -ld /root/.ssh/id_rsa
```
Se tiver leitura, copie o conteúdo, salve como id_rsa localmente e:

```bash
chmod 600 id_rsa
ssh root@<IP> -i id_rsa
```
### 🔎 Enumeração de Sistema e Ambiente Linux

```bash

linpeas.sh
linenum.sh
cat /proc/version
uname -a
lsb_release -a
ifconfig
ip a
cat /etc/os-release
find / -perm -u=s -type f 2>/dev/null
ps aux | grep root
history
cat /etc/passwd
cat /etc/shadow
cat ~/.bash_history
sudo -l (Listar privilégios do usuário)
sudo su
dpkg -l
ls -la /etc/cron.daily/
lsblk
echo $PATH
env
set
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null   (Encontre diretórios graváveis)
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null   (Encontre arquivos graváveis)




```
### 🔎 Enumeração de Sistema e Ambiente Windows

```bash
sysinfo
getuid
whoami /priv
getsystem
ps
getpid e depois migrate id_de_outro_processo_para_persisntecia
bypass de UAC
hashdump
Get-Content (Get-PSReadlineOption).HistorySavePath
```
### Meterpreter / Metasploit

```bash
run killav
screenshot
run vnc
keyscan_start
keyscan_dump
search -d C:/Users -f .pdf
download -r caminho
upload arquivo caminho
use multi/recon/local_exploit_suggester
```


## 🌐 Sites e Recursos Úteis

- 🧠 [HackTricks](https://book.hacktricks.xyz/)  
  Guia abrangente sobre pentest, enumeração, privilege escalation, bypasses e muito mais.

- 📂 [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)  
  Repositório com payloads organizados por categoria (XSS, LFI, privilege escalation, etc).

- 🔍 [GTFOBins](https://gtfobins.github.io/)  
  Abusa de binários comuns do Linux para executar comandos com escalada de privilégios.

- 🧬 [LOLBAS](https://lolbas-project.github.io/)  
  Coleta de binários nativos do Windows que podem ser abusados para execução arbitrária.

- 💣 [Exploit-DB (searchsploit)](https://www.exploit-db.com/)  
  Banco de dados de exploits — também acessível via CLI com o comando `searchsploit`.





