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
# Informações do kernel e sistema
cat /proc/version
uname -a
lsb_release -a
cat /etc/os-release

# Variáveis de ambiente
echo $PATH
env
set
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



```

### 🔐 Permissões Sudo
Verifique permissões do usuário:

```bash
sudo -l
```

### 📦 Pacotes, Jobs e Reutilização
Verificar pacotes instalados (Linux):

```bash
dpkg -l
```

### Checar histórico de comandos:

```bash
cat ~/.bash_history
# Ou, no Windows:
Get-Content (Get-PSReadlineOption).HistorySavePath
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





