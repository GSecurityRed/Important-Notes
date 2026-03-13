### Hashing passwords

As funções de hash são projetadas para trabalhar em one direction. Isso significa que não deve ser possível descobrir qual foi a senha original baseada apenas no hash. Quando os atacantes tentam fazer isso, é chamado password cracking. Técnicas comuns são de usar rainbow tables, para executar dictionary attacks, e normalmente como último recurso, para executar brute-force attacks.

- echo -n senha1 | md5sum
- echo -n senha1 | sha256sum


### Salt

O salt é um valor aleatório que é adicionado à senha antes de gerar o hash. Ele serve para tornar o processo de password cracking muito mais difícil.
