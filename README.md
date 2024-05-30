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

**b. Verificar se o Nginx está em execução:**

```
sudo systemctl status nginx
```

<img src="/images/status.png"> <br>

**c. Inicializar o Nginx:**

Executar esse passo, caso o status atual do serviço seja `inactive (dead)`.

```
sudo systemctl start nginx
```

**d. Testar o servidor rodando na porta 8080:**

```
http://localhost:8080/
```

<img src="/images/welcome.png">