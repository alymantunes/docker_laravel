
# Projeto Laravel com Docker e Nginx

Este projeto configura um ambiente de desenvolvimento para uma aplicação Laravel usando Docker, Nginx, PHP-FPM e MariaDB.

## Estrutura do Projeto

A estrutura de pastas e arquivos do projeto é a seguinte:

```
docker/
├── nginx/
│   ├── conf.d/
│   │   └── default.conf
│   └── Dockerfile
├── laravel-app/
└── docker-compose.yaml
```

- **docker/nginx/conf.d/default.conf**: Arquivo de configuração do Nginx.
- **docker/nginx/Dockerfile**: Dockerfile para construir a imagem do Nginx.
- **laravel-app/**: Diretório onde a aplicação Laravel está localizada.
- **docker-compose.yaml**: Arquivo de configuração do Docker Compose.

## Pré-requisitos

Antes de começar, certifique-se de ter o seguinte software instalado:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Configuração

1. **Clone o repositório:**

   ```bash
   git clone https://seu-repositorio-url.git
   cd seu-repositorio
   ```

2. **Configurar as variáveis de ambiente:**

   Certifique-se de que o arquivo `.env` esteja configurado corretamente dentro do diretório `laravel-app/`. Se necessário, crie um novo arquivo `.env`:

   ```bash
   cp laravel-app/.env.example laravel-app/.env
   ```

   Atualize as variáveis de ambiente conforme necessário.

3. **Construir os containers Docker:**

   No diretório raiz do projeto, execute:

   ```bash
   docker-compose up -d --build
   ```

   Este comando irá construir e iniciar os containers em segundo plano.

4. **Instalar as dependências do Laravel:**

   Acesse o container da aplicação Laravel e instale as dependências:

   ```bash
   docker-compose exec app composer install
   ```

5. **Gerar a chave da aplicação Laravel:**

   Ainda dentro do container da aplicação, execute:

   ```bash
   docker-compose exec app php artisan key:generate
   ```

6. **Migrar o banco de dados:**

   Execute as migrações para configurar o banco de dados:

   ```bash
   docker-compose exec app php artisan migrate
   ```

## Acessando a Aplicação

Após a configuração, a aplicação estará disponível em `http://localhost`. Se você configurou outra porta, ajuste a URL conforme necessário.

## Comandos Úteis

- **Parar os containers:**

  ```bash
  docker-compose down
  ```

- **Ver logs dos containers:**

  ```bash
  docker-compose logs -f
  ```

- **Acessar o container do Laravel:**

  ```bash
  docker-compose exec app bash
  ```

## Problemas Comuns

- Se você encontrar problemas de permissão, tente rodar os comandos com `sudo`.
- Certifique-se de que a versão do Docker e Docker Compose é compatível com o projeto.

## Contribuição

Sinta-se à vontade para abrir um Pull Request se quiser contribuir com melhorias ou correções.

## Licença

Este projeto está licenciado sob os termos da [MIT License](LICENSE).
