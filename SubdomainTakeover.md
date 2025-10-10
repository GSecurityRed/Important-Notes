- sequestro de dominio
- pode se utilizar com subdomain e depois subhunter

- falha ocorre por falta de limpeza de DNS e subdomains ja excluidos ou modificados etc
- se da mais por conta do CNAME, caso esteja exposto e o subdomain exlcuido e alguma hospedagem conhecida (por exemplo github pages ou aws) podemos simplesmente tentar criar o subdomain, bucket, diretorio etc com o mesmo nome, porém aplicando um novo CNAME, que seja nosso. Dessa forma toda vez que alguem for acessar a URL do subdomain da empresa ou pessoa, ela será redirecionada para uma hospedagem nossa.
