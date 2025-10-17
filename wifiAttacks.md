- adaptador wireless ou placa de rede que vem no notebook para testes
- iwconfig

### processo para habilitar modo monitor de seu adaptador wireless
- airmon-ng check
- airmon-ng check kill
- airmon-ng start wlan0

obs: modo monitor é necessario para listar todas as redes wifi próximas

### listar redes wifi disponiveis perto

- airodump-ng wlan [nome de sua interface wireless]

