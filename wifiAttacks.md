- adaptador wireless ou placa de rede que vem no notebook para testes
- iwconfig

### processo para habilitar modo monitor de seu adaptador wireless
- airmon-ng check
- airmon-ng check kill
- airmon-ng start wlan0

obs: modo monitor é necessario para listar todas as redes wifi próximas

### listar redes wifi disponiveis perto

- airodump-ng wlan [nome de sua interface wireless]

- BSSID é o endereço MAC dos roteadores
- PWR é o nivel de intensidade das redes listadas
- Beacons são os chamados envios do roteador
- Data são os dados que se da pra interceptar
- MB conectividade
- ENC tipo de criptografia (obs: visto em algumas aulas que a criptografia WEP que é bem antiga e legada)
- ESSID nome da rede

