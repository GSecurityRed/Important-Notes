# 🛠️ Nmap - Anotações Rápidas

Este repositório contém uma coleção de **comandos essenciais do Nmap** com explicações breves. Ideal para revisão, estudos e uso em CTFs ou pentests.

---

## 📦 Escaneamento Básico

- `nmap 192.168.0.1` → Escaneia as 1000 portas padrão de um host.
- `nmap -sP 192.168.0.0/24` → Descobre hosts ativos na rede (ping scan).
- `nmap -p 22,80,443 192.168.0.1` → Escaneia portas específicas.

---

## 🔍 Detecção de Serviços e Versões

- `nmap -sV 192.168.0.1` → Detecta versões dos serviços.
- `nmap -O 192.168.0.1` → Tenta identificar o sistema operacional.
- `nmap -A 192.168.0.1` → Ativa detecção de SO, versões, script scan e traceroute.
- `nmap -sC 192.168.0.10` → Serve para executar automaticamente os scripts considerados “default” do NSE (Nmap Scripting Engine).
  

---

## 🧠 Scripts NSE

- `nmap ip --script category` → Mecanismo de script Nmap.
- `nmap ip --s` → .
- `nmap --script ip` → .

---

## 🎯 Técnicas de Evasão

- `nmap -D RND:10 192.168.0.1` → Usa IPs falsos para camuflar o scan (Decoy).
- `D 192.168.32.101,192.168.32.102,192.168.32.103,192.168.32.23 192.168.32.1`
- `nmap -S <spoofed-IP> 192.168.0.1` → Spoof de IP de origem.
- `-n` -> Desativa a resolução de DNS.
- `nmap -f 192.168.0.1` → Fragmenta pacotes (bypass de firewalls).
- `--disable-arp-ping` desativa ping arp
- `sudo nmap 10.129.2.28 --source-port 53` -> Executa as varreduras a partir da porta de origem especificada, ajuda a te camuflar como uma mera requisição (ou -g53).
- `nmap -sU -p 53 --script=dns-nsid <IP_do_destino>` -> Descobrir a versão do servidor dns do alvo.
- `--max-retries`
- `--script-args http.useragent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36"`
- `nmap 10.129.38.5 -sS -Pn --source-port 53 -D RND:3 -sV -sU`

---

## 🧪 Scans Especiais

- `nmap -sS 192.168.0.1` → SYN scan (stealth).
- `nmap -sU 192.168.0.1` → Escaneia portas UDP.
- `nmap -sT 192.168.0.1` → TCP connect scan (menos furtivo).

---

## ⏱️ Performance & Otimização

- `nmap -T4 192.168.0.1` → Acelera o scan (modo agressivo).
- `nmap --min-rate 1000 192.168.0.1` → Define taxa mínima de pacotes.
- `nmap --max-retries 2 192.168.0.1` → Reduz tentativas de retry.

---

## 💾 Output

- `nmap -oN output.txt 192.168.0.1` → Salva saída em formato normal.
- `nmap -oX scan.xml 192.168.0.1` → Salva saída em XML.
- `nmap -oA scan 192.168.0.1` → Gera todos os formatos (`.nmap`, `.xml`, `.gnmap`).

---

## ✅

- Combine opções para um scan poderoso:  
  `nmap -sS -sV -O -T4 -p- 192.168.0.1`
  - `nmap -p 80,443 --script http-headers,http-title,http-enum cursos.rizomasur.org` da uma de gobuster massa
  - nmap -sV -sS -sC -O -D RND:2 -Pn IP
  - --all ports

---
  
## 🔍 Alcance da rede de varredura
  - `nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5` → </br>
    10.129.2.0/24	Alcance da rede alvo. </br>
    -sn	Desativa a varredura de portas. </br>
    -oA tnet	Armazena os resultados em todos os formatos, começando pelo nome 'tnet'. </br>


---
## Verificando as 10 principais portas TCP
  - `sudo nmap 10.129.2.28 --top-ports=10 `
## Verificando as 100 principais portas
  - `sudo nmap 10.129.2.28 -F`
## Exibe o motivo pelo qual uma porta está em um estado específico.
  - `--reason`
## Dica relatório html
  - xsltproc target.xml -o target.html
## Verifica todas as portas.
  - -p-
## Mostra o progresso da varredura a cada 5 segundos.
- --stats-every=5s	
---
## Se conecta a uma porta especifica usando o DNS para talvez saber a versão

- `ncat -nv --source-port 53 ip 50000`


