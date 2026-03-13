### Hashing passwords

As funções de hash são projetadas para trabalhar em one direction. Isso significa que não deve ser possível descobrir qual foi a senha original baseada apenas no hash. Quando os atacantes tentam fazer isso, é chamado password cracking. Técnicas comuns são de usar rainbow tables, para executar dictionary attacks, e normalmente como último recurso, para executar brute-force attacks.

- echo -n senha1 | md5sum
- echo -n senha1 | sha256sum


### Salt

O salt é um valor aleatório que é adicionado à senha antes de gerar o hash. Ele serve para tornar o processo de password cracking muito mais difícil.

### Identificação de formatos de hash

- https://openwall.info/wiki/john/sample-hashes
- https://pentestmonkey.net/cheat-sheet/john-the-ripper-hash-formats
- hashid -j 193069ceb0461e1d40d216e32c79c704



### John The Ripper 

É uma ferramenta de teste de penetração bem conhecida usada para quebrar senhas através de vários ataques, incluindo força bruta e dicionário

- Single crack mode é uma técnica de cracking baseada em regras que é mais útil ao segmentar credenciais Linux. Ele gera candidatos a senha com base no nome de usuário da vítima, nome do diretório inicial e valores GECOS (nome completo, número de quarto, número de telefone, etc.). Essas strings são executadas contra um grande conjunto de regras que aplicam modificações comuns de strings vistas em senhas (por exemplo, um usuário cujo nome real é Bob Smithpode usar Smith1como sua senha):
```
Gust4vo@htb[/htb]$ john --single passwd

Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
[...SNIP...]        (r0lf)     
1g 0:00:00:00 DONE 1/3 (2025-04-10 07:47) 12.50g/s 5400p/s 5400c/s 5400C/s NAITSABESFL0R..rSebastiannaitsabeSr
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

- Wordlist mode é usado para quebrar senhas com um ataque de dicionário, o que significa que ele tenta todas as senhas em uma lista de palavras fornecida contra o hash de senha. A sintaxe básica para o comando é a seguinte:

```
Gust4vo@htb[/htb]$ john --wordlist=<wordlist_file> <hash_file>
```

- Incremental mode é um modo de quebra de senha poderoso, estilo força bruta, que gera senhas candidatas com base em um modelo estatístico (cadeias de Markov). Ele é projetado para testar todas as combinações de caracteres definidas por um conjunto de caracteres específico, priorizando senhas mais prováveis com base em dados de treinamento. Este modo é o mais exaustivo, mas também o mais demorado.  Ele gera suposições de senha de forma dinâmica e não depende de uma lista de palavras predefinida, em contraste com o modo wordlist.

```
Gust4vo@htb[/htb]$ john --incremental <hash_file>
```

