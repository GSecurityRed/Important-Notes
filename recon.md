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
```
Também verifique manualmente:
- Código-fonte da página (Ctrl + U)
- Informações do certificado HTTPS (cadeado)
- robots.txt: http://target.com/robots.txt
- Detecção de tecnologias com extensão ou CLI do Wappalyzer

## 🧰 Extras e Ferramentas Úteis

Ferramenta para buscar segredos e chaves de API expostas:

🔐 TruffleHog: busca por credenciais, secrets e chaves de API em históricos de código, repositórios Git, arquivos e strings expostas.


