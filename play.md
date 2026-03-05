```
gobuster dir \
-u https://www.thinkglobalhealth.org/ \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt \
-b 400 \
-t 1 \
-k \
-x html,php,js,aspx,bat,txt,zip \
--exclude-length 93239 \
--delay 500ms \
-a "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36" \
| tee -a diretorios.txt

```

```
nmap -p 80,443 --script http-headers,http-title,http-enum cursos.rizomasur.org
```

- https://web.archive.org/cdx/search/cdx?url=*.teste.com/*&output=txt&fl=original&collapse=urlkey&page=/

```
subfinder -d saltlabs.com -all -recursive -silent \
  | waybackurls \
  | sort -u \
  | httpx -mc 200 -title -tech-detect -threads 100 -timeout 10
```

```
nuclei -u https://dotdev.shopify.com/password -tags config,exposure -header "User-Agent: bug bounty name ted64x"
```

```
nuclei -u "https://login-accpt.portofantwerpbruges.com/poam/XUI/?service=test" \
-dast \
-dast-server \
-tags xss \
-fuzz \
-headless
```

```
nuclei -u https://target.com -rate-limit 10
```
