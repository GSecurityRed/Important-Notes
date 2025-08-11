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

---

## 🧠 Scripts NSE

- `nmap --script help` → Lista scripts disponíveis.
- `nmap --script=default 192.168.0.1` → Executa os scripts padrão.
- `nmap --script=vuln 192.168.0.1` → Executa scripts de verificação de vulnerabilidades.

---

## 🎯 Técnicas de Evasão

- `nmap -D RND:10 192.168.0.1` → Usa IPs falsos para camuflar o scan (Decoy).
- `nmap -S <spoofed-IP> 192.168.0.1` → Spoof de IP de origem.
- `nmap -f 192.168.0.1` → Fragmenta pacotes (bypass de firewalls).

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

## ✅ Dica Final

- Combine opções para um scan poderoso:  
  `nmap -sS -sV -O -T4 -p- 192.168.0.1`

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



---


