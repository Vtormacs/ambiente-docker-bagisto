# Dockerização do Bagisto

O objetivo principal deste repositório é fornecer um ambiente de trabalho com todas as dependências necessárias para o Bagisto. Neste repositório, incluímos os seguintes serviços:

- PHP-FPM
- Nginx
- MySQL
- Redis
- PHPMyAdmin
- Elasticsearch
- Kibana
- Mailpit

## Versão do Bagisto Suportada

Atualmente, todos esses serviços estão incluídos para atender às dependências da seguinte versão do Bagisto:

**Versão do Bagisto:** v2.3.6 e superior.

No entanto, pode haver alguns casos específicos em que ajustes sejam necessários. Recomendamos revisar o arquivo `Dockerfile` ou `docker-compose.yml` para quaisquer modificações necessárias.

> [!IMPORTANT]
> Se você estiver usando a versão master, existe a possibilidade de que o script de configuração atual neste repositório esteja configurado para o **Bagisto dev-master**. Os arquivos `.env` localizados na pasta `.configs` estão alinhados com esta versão. Se você planeja modificar o script ou alterar a versão do Bagisto, por favor, certifique-se de que suas alterações permaneçam compatíveis com a versão atualizada.

## Requisitos do Sistema

- Os requisitos de sistema/servidor do Bagisto são mencionados [aqui](https://devdocs.bagisto.com/getting-started/before-you-start.html#system-requirements). Usando o Docker, esses requisitos serão atendidos pelas imagens Docker do PHP-FPM e Nginx, e nossa aplicação rodará em uma arquitetura de múltiplas camadas.

- Instale a versão mais recente do Docker e do Docker Compose se ainda não estiverem instalados. O Docker suporta os sistemas operacionais Linux, MacOS e Windows. Clique em [Docker](https://docs.docker.com/install/) e [Docker Compose](https://docs.docker.com/compose/install/) para encontrar o guia de instalação deles.

## Instalação

- Este é um repositório simples, sem configurações complexas. Apenas atualize o arquivo `docker-compose.yml` se necessário, e você estará pronto!

- Ajuste seus serviços conforme necessário. Por exemplo, a maioria dos usuários de Linux tem um UID de 1000. Se o seu UID for diferente, certifique-se de atualizá-lo de acordo com sua máquina host.


```yml
  services:
    php-fpm:
      build:
        args:
          container_project_path: /var/www/html/
          uid: 1000 # adicione seu UID aqui
          user: $USER
        context: .
        dockerfile: ./Dockerfile
      image: php-fpm
      ports:
        - "5173:5173" # Porta do servidor de desenvolvimento Vite
      volumes:
        - ./workspace/:/var/www/html/

    nginx:
      image: nginx:latest
      ports:
        - "80:80" # ajuste sua porta aqui, se quiser alterar
      volumes:
        - ./workspace/:/var/www/html/
        - ./.configs/nginx/nginx.conf:/etc/nginx/conf.d/default.conf
      depends_on:
        - php-fpm
  ```

- Neste repositório, o foco inicial foi atender a todos os requisitos do projeto. Seja seu projeto novo ou pré-existente, você pode facilmente copiá-lo e colá-lo no diretório de trabalho designado. Se você não tem certeza por onde começar, um script shell foi fornecido para agilizar o processo de configuração para você. Para instalar e configurar tudo, basta executar:

  ```sh
  sh setup.sh
  ```

## Após a instalação

- Para fazer login como administrador.

  ```text
  http(s)://your_server_endpoint/admin/login

  Email: admin@example.com
  Password: admin123
  ```

- Para fazer login como cliente. Você pode se registrar diretamente como cliente e depois fazer o login.

  ```text
  http(s):/seu_endpoint_do_servidor/customer/register
  ```

## Já é um especialista em Docker?

- Você pode usar este repositório como seu ambiente de trabalho. Para construir seu contêiner, simplesmente execute o seguinte comando:

  ```sh
  docker-compose build
  ```

- Após a construção, você pode executar o contêiner com:

  ```sh
  docker-compose up -d
  ```

- Agora, você pode acessar o shell do contêiner e instalar o  [Bagisto](https://github.com/bagisto/bagisto).
