## Projeto Prático - Configuração e Otimização do Servidor NGINX

### Desafio

Configurar e otimizar um servidor NGINX para atuar como servidor web, proxy reverso e gateway de API. O projeto visa otimizar o desempenho, implementar HTTPS, configurar regras de proxy reverso e gerenciar servidores web seguros.

### Etapas do Projeto

#### 1. Configuração Básica

**Objetivo:** configurar o NGINX para atuar como servidor web para um site ou aplicativo.

#### Passos:

**a. Instalar o Nginx:**

```
sudo apt update
sudo apt install nginx
```

- Verificar se o Nginx está em execução:

```
sudo systemctl status nginx
```

<img src="/images/status.png"> <br>

- Inicializar o Nginx:

Executar esse comando caso o status atual do serviço seja `inactive (dead)`.

```
sudo systemctl start nginx
```

- Testar o servidor rodando na porta 8080:

```
http://localhost:8080/
```

<img src="/images/welcome.png"> <br>

**b. Configurar um Servidor Web Básico:**

- Criar o arquivo de configuração padrão para o novo servidor:

```
sudo nano /etc/nginx/sites-enabled/default.conf
```

- Criar o conteúdo para servir um site básico:

```
server {
    listen 80; # Porta em que o servidor irá escutar

    server_name localhost; # Nome do servidor

    location / {
        root /var/www/my_server_nginx; # Diretório raiz do servidor
        index index.html; # Arquivo padrão de índice
    }
}
```

- Verificar se há erros de sintaxe na configuração do servidor:

```
sudo nginx -t
```

- Recarregar a configuração do servidor:

```
sudo nginx -s reload
```

- Adicionar o conteúdo HTML desejado no arquivo `index.html`:

```
cd /var/www/
sudo mkdir my_server_nginx
cd my_server_nginx/
sudo nano index.html
```

- Testar o novo servidor rodando na porta 80:

```
http://localhost/
```

<img src="/images/novo_servidor.png"> <br>

