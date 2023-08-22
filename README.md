# readme
Repositório para armazenar o readme do trabalho da AWS

#MYSQL
Criar uma instância do mysql no lightsail
Acessar a instância para pegar os dados de conexão, procurando pelos labels:
User name
Password
O host estará em Endpoint

Foi criada uma base (wordpress) neste mysql para instalar instâncias do wordpress

#WORDPRESS
Criar duas instâncias do wordpress no ligthsail.
As instâncias foram acessadas por ssh.
Acessar /opt/bitnami/wordpress/wp-config.php e configurar os dados do mysql criado acima.
Acessar a área adm de um wordpress (a primeira vez será necessário configurar).
IP/admin ou IP/wp-amin
A senha que foi configurada para nossa equipe está em um doc do drive do Teddy.
Fazer uma alteração de informação em um IP e acessar com o outro para ver se a mudança surtiu efeito nos dois (mostrando que os dois apontam para um mesmo banco)

#LOAD BALANCER
No lightsail, em networks, criar um load balancer.
Apontar para as duas instâncias do wordpress
