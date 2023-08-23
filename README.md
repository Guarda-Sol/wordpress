Diretivas para configuração de 2 wordpress apontando para um mesmo banco de dados.

# MYSQL
Criar uma instância do mysql no lightsail
Acessar a instância para pegar os dados de conexão, procurando pelos labels:
User name
Password
O host estará em Endpoint

Foi criada uma base (wordpress) neste mysql para instalar instâncias do wordpress

# WORDPRESS
Criar duas instâncias do wordpress no ligthsail.
As instâncias foram acessadas por ssh.
Acessar /opt/bitnami/wordpress/wp-config.php e configurar os dados do mysql criado acima.
Acessar a área adm de um wordpress (a primeira vez será necessário configurar).
IP/admin ou IP/wp-amin
A senha que foi configurada para nossa equipe está em um doc do drive do Teddy.
Fazer uma alteração de informação em um IP e acessar com o outro para ver se a mudança surtiu efeito nos dois (mostrando que os dois apontam para um mesmo banco)

# LOAD BALANCER
No lightsail, em networks, criar um load balancer.
Apontar para as duas instâncias do wordpress

# CRIAMOS UMA PÁGINA NO WORDPRESS COM LEITURA DE DADOS EM UMA INSTÂNCIA DO POSTGRES
## PASSOS PARA ESTA CONFIGURAÇÃO
No diretório do tema do wordpress, criar um arquivo para fazer o procedimento desejado 
Criamos o arquivo bitnami/wordpress/wp-content/themes/twentytwentythree/devops/get.php com o conteúdo:
```
<?php
$dbHost = ';
$dbName = '';
$dbUser = '';
$dbPassword = '';

try {
    // Conectando ao banco de dados
    $pdo = new PDO("pgsql:host=$dbHost;dbname=$dbName", $dbUser, $dbPassword);
    $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    // Consulta SQL para buscar dados da tabela "comentarios"
    $sql = "SELECT texto FROM mydjangoapp_comentario WHERE usuario_id = 1 or usuario_id=5";

    $stmt = $pdo->query($sql);
    // Exibir os dados na tela
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo "Texto: " . $row['texto'] . "<br>";
    }
} catch (PDOException $e) {
    echo "Erro na conexão: " . $e->getMessage();
}
?>
```

Adicionamos o arquivo bitnami/wordpress/wp-content/themes/twentytwentythree/functions.php com o conteúdo:
```
function custom_template_shortcode() {
    ob_start();
    include get_template_directory() . '/devops/get.php';
    return ob_get_clean();
}
add_shortcode('custom_template', 'custom_template_shortcode');
```

# Habilitar biblioteca para postgres
No arquivo `/opt/bitnami/php/etc/ph
p.ini` habilitar a extensão: `extension=pdo_pgsql`

# Criar página para exibição dos dados
No wordpress, foi criada uma página e adicionado um shortcode referenciando o shortcode criado em functions.php: [custom_template] 
