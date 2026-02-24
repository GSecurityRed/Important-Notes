- nuclei -u https://dotdev.shopify.com/password -tags config,exposure -header "User-Agent: bug bounty name ted64x"
- nuclei -l targets.txt -tags rce,sqli,xss,ssrf,lfi
- nuclei -l urls.txt -dast -automatic-scan
- nuclei -l targets.txt -tags grafana
- nuclei -l targets.txt -severity high,critical -c 40 -rl 100
- nuclei -l targets.txt -severity high,critical -c 40 -rl 100
- subfinder -all -d target.com -recursive | httpx | nuclei -nmhe -t ./nuclei-templates/http/cves/2025/CVE-2025-0133.yaml
- nuclei -u IP_DA_MAQUINA -t windows,smb,rdp,iis,exchange,ntlm
- nuclei -u https://target.com -exclude-severity info

```
nuclei -u "https://login-accpt.portofantwerpbruges.com/poam/XUI/?service=test" \
-dast \
-dast-server \
-tags xss \
-fuzz \
-headless
````


# Limit to 10 requests per second
nuclei -u https://target.com -rate-limit 10

# Limit concurrent templates (less aggressive)
nuclei -u https://target.com -bulk-size 10 -concurrency 5

-rate-limit 10: Max 10 requests/second
-bulk-size 10: Process 10 templates at once
-concurrency 5: Max 5 parallel connections
