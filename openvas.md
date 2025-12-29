## Instalação

- sudo apt-get update && apt-get -y full-upgrade
- sudo apt-get install gvm && openvas
- gvm-setup (Isso iniciará o processo de configuração e levará até 30 minutos.)

  <img width="1049" height="574" alt="image" src="https://github.com/user-attachments/assets/f5c748f0-d276-4321-af6f-eba041d7f8b8" />
  
- gvm-start (Finalmente, podemos iniciar o OpenVas:)

## Bug normal resolver

- se der o erro:
Job for ospd-openvas.service failed because the control process exited with error code.
See "systemctl status ospd-openvas.service" and "journalctl -xeu ospd-openvas.service" for details.

- executar esses comandos:

- 1- sudo chmod 666 /var/log/gvm/openvas.log
- 2- sudo chown _gvm:_gvm /var/log/gvm/openvas.log
- 3- sudo gvm-stop
- 4- sudo gvm-start

## Trocar senha

-sudo runuser -u _gvm -- gvmd --user=admin --new-password='pohadesenha'
