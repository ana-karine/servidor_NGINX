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

- Criar o arquivo de configuração `default.conf` para o novo servidor:

```
sudo nano /etc/nginx/sites-enabled/default.conf
```

- Criar o conteúdo do arquivo `default.conf`:

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

#### 2. Proxy Reverso

**Objetivo:** configurar regras de proxy reverso para direcionar o tráfego da porta 8080 para a porta 80.

#### Passos:

**a. Criação de uma Configuração de Proxy Reverso:**

- Entrar no arquivo de configuração padrão do Nginx:

```
sudo nano /etc/nginx/sites-enabled/default
```

- Adicionar a configuração de proxy reverso:

```
server {
        listen 8080 default_server;
        listen [::]:8080 default_server;

        root /var/www/html;
        index index.nginx-debian.html;

        server_name _;

        location / {
                proxy_pass http://localhost:80; # Proxy reverso para o servidor na porta 80
        }
}
```

- Verificar se há erros de sintaxe e recarregar o servidor:

```
sudo nginx -t
sudo nginx -s reload
```

- Testar o novo servidor rodando na porta 8080:

```
http://localhost:8080/
```

<img src="/images/proxy.png"> <br>

#### 3. API Gateway / Microsserviços

**Objetivo:** configurar regras de proxy reverso para direcionar o tráfego para diferentes serviços ou aplicativos.

#### Passos:

**a. Configurar um servidor ou site específico:**

- Criar o arquivo de configuração `microservicos.conf`:

```
sudo nano /etc/nginx/sites-enabled/microservicos.conf
```

- Criar o conteúdo para `microservicos.conf`:

```
server {
    listen 8001;
    server_name localhost;
 
    location / {
        root /var/www/my_server_nginx/servico1;
        index index.html;
    }
}

server {
    listen 8002;
    server_name localhost;

    location / {
        root /var/www/my_server_nginx/servico2;
        index index.html;
    }
}
```

- Criar os diretórios `servico1` e `servico2`:

```
sudo mkdir /var/www/my_server_nginx/servico1 /var/www/my_server_nginx/servico2
```

- Adicionar o conteúdo HTML desejado nos arquivos `index.html`:

```
cd /var/www/my_server_nginx/servico1
sudo nano index.html
```

```
cd /var/www/my_server_nginx/servico2
sudo nano index.html
```

**b. Configurar o servidor web (Nginx) para atuar como um gateway reverso:**

- Entrar no arquivo de configuração padrão do Nginx:

```
sudo nano /etc/nginx/sites-enabled/default
```

- Adicionar a configuração de gateway reverso:

```
server {
        listen 8080 default_server;
        listen [::]:8080 default_server;

        root /var/www/html;
        index index.nginx-debian.html;

        server_name _;

        location / {
                proxy_pass http://localhost:80; # Proxy reverso para o servidor na porta 80
        }

        location /servico1 {
                proxy_pass http://localhost:8001/;
        }

        location /servico2 {
                proxy_pass http://localhost:8002/;
        }
}
```

- Verificar se há erros de sintaxe e recarregar o servidor:

```
sudo nginx -t
sudo nginx -s reload
```

- Testar os novos serviços rodando na porta 8080:

```
http://localhost:8080/servico1
```

<img src="/images/servico1.png"> <br>


```
http://localhost:8080/servico2
```

<img src="/images/servico2.png"> <br>

