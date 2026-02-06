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

```md

Enumeração de sistema e ambiente Linux — comandos úteis para reconhecimento inicial, CTFs e pentests.

# Coleta informações do kernel e arquitetura do sistema
uname -a

# Mostra a versão do kernel e do compilador usado
cat /proc/version

# Identifica a distribuição Linux
cat /etc/os-release
lsb_release -a

# Lista usuários locais do sistema
cat /etc/passwd

# Exibe hashes de senha (se houver permissão)
cat /etc/shadow

# Identifica usuários que possuem shell válida
grep "sh$" /etc/passwd

# Lista shells disponíveis no sistema
cat /etc/shells

# Mostra histórico de comandos do usuário atual
history
cat ~/.bash_history

# Exibe variáveis de ambiente + PATH abuse (Adicionando . ao PATH de um usuário adiciona seu diretório de trabalho atual à lista. Por exemplo, se pudermos modificar o caminho de um usuário, poderemos substituir um binário comum, como ls com um script malicioso, como um shell reverso.)
env
set
echo $PATH
htb_student@NIX02:~$ PATH=.:${PATH}
htb_student@NIX02:~$ export PATH
htb_student@NIX02:~$ echo $PATH
htb_student@NIX02:~$ touch ls
htb_student@NIX02:~$ echo 'echo "PATH ABUSE!!"' > ls
htb_student@NIX02:~$ chmod +x ls
htb_student@NIX02:~$ ls
PATH ABUSE!!

# Verifica privilégios sudo do usuário atual
sudo -l

# Mostra a versão do sudo instalada
sudo -V

# Tenta escalar privilégios diretamente para root (se permitido)
sudo su

# Lista processos em execução pertencentes ao root
ps aux | grep root

# Mostra interfaces de rede e endereços IP
ip a
ifconfig

# Exibe tabela de rotas
route

# Lista hosts descobertos via ARP
arp -a

# Lista pacotes instalados no sistema
dpkg -l

# Gera uma lista limpa dos pacotes instalados e salva em arquivo
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee installed_pkgs.list

# Compara pacotes instalados com binários do GTFOBins
for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d'); do
  if grep -q "$i" installed_pkgs.list; then
    echo "Check GTFOBins for: $i"
  fi
done

# Procura arquivos de configuração no sistema
find / -type f \( -name "*.conf" -o -name "*.config" \) -exec ls -l {} \; 2>/dev/null
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null

# Procura scripts shell fora de diretórios comuns
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"

# Procura arquivos de histórico criados por scripts ou programas
find / -type f \( -name "*_hist" -o -name "*_history" \) -exec ls -l {} \; 2>/dev/null

# Lista todos os arquivos ocultos do sistema
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null

# Lista todos os diretórios ocultos do sistema
find / -type d -name ".*" -ls 2>/dev/null

# Procura por flags comuns em CTFs
find / -name flag.txt 2>/dev/null
grep -Rni 'HTB{' / 2>/dev/null

# Lista tarefas agendadas diárias
ls -la /etc/cron.daily/

# Mostra discos e partições montadas
lsblk

# Mostra sistemas de arquivos montados automaticamente
cat /etc/fstab

# Lista diretórios temporários e permissões
ls -l /tmp /var/tmp /dev/shm

# Procura diretórios graváveis por qualquer usuário
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null

# Procura arquivos graváveis por qualquer usuário
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null

# Credenciais do banco de dados MySQL nos arquivos de configuração do WordPress
grep 'DB_USER\|DB_PASSWORD' wp-config.php

# É a principal maneira de acessar informações do processo e pode ser usada para visualizar e modificar as configurações do kernel
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"

# Procura binários com bit SUID ativo (possível privilege escalation)
find / -perm -u=s -type f 2>/dev/null

# Listar os programas compilados na forma de binário
ls -l /bin /usr/bin/ /usr/sbin/

# Lista último horário de login de cada usuário
lastlog

# Ver se mais alguém está atualmente logado no sistema conosco
w

# Ferramentas automatizadas de enumeração
linpeas.sh
linenum.sh
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





