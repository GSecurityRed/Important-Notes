# Meterpreter

O payload Meterpreter é um tipo específico de carga útil multifacetada e extensível que usa DLL injection para garantir que a conexão com o host da vítima seja estável e difícil de detectar usando verificações simples e possa ser configurada para ser persistente em reinicializações ou alterações no sistema. Além disso, o Meterpreter reside inteiramente na memória do host remoto e não deixa rastros no disco rígido, dificultando sua detecção com técnicas forenses convencionais. <br> <br>
Ele pode nos ajudar a encontrar várias técnicas de escalonamento de privilégios, técnicas de evasão de AV, pesquisas adicionais sobre vulnerabilidades, fornecer acesso persistente, pivô, etc.

https://www.rapid7.com/blog/post/2015/03/25/stageless-meterpreter-payloads/

# Gerais

- help
- sysinfo
- getuid
- pwd
- ls
- cd
- download
- upload
- shell
- ps
- migrate
- background ou bg
- caso use o comando shell e queira voltar para o meterpreter usar o "exit"
- CTRL + L

# Privsec
- run post/multi/recon/local_exploit_suggester 
