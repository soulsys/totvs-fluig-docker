# TOTVS Fluig Docker

O objetivo desse repositório é ser um ponto de partida para criação de ambientes de 
desenvolvimento Fluig utilizando nossas [imagens docker](https://hub.docker.com/u/soulsys) das soluções TOTVS.

## Importante

- Esse repositório não possui qualquer relação com a TOTVS S/A
- As imagens **NÃO** devem ser usadas em ambientes de produção
- Utilize apenas como ambiente de desenvolvimento
- Ao utilizar você concorda com os termos da licença MIT

## Pré-requisitos

### Windows

- Habilitar o [WSL2](https://www.omgubuntu.co.uk/how-to-install-wsl2-on-windows-10)
- Instalar o [Docker Desktop](https://docs.docker.com/desktop/windows/install) ou configurar manualmente o docker na distribuição linux do WSL2

### Linux

- Instalar e configurar as últimas versões do docker e docker compose

### Mac

- Instalar o [Docker Desktop](https://docs.docker.com/desktop/mac/install/)

## Mémoria

- Os containers precisam de pelo ao menos 4 GB de RAM para execução sem erros
- Se estiver usando o Docker Desktop, habilite a memória na área de recursos

## Softwares recomendados

- [VS Code](https://code.visualstudio.com/download)
(instalar plugin [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker))
- [Node.js](https://nodejs.org/en/download/)
- [Azure Data Studio](https://docs.microsoft.com/pt-br/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15)

## Usando pela primeira vez

1. Defina as variáveis de ambiente no arquivo ***.env***. 
Todas as variáveis disponíveis estão documentadas no [docker hub](https://hub.docker.com/u/soulsys).

2. Mantenha a variável ***FLUIG_DISABLED*** com o valor *true*

4. Criação de uma rede para os containers

```bash
docker network create sample-docker-network
```

4. Download das imagens e criação dos containers

```bash
docker-compose up
```

5. Acesse a instância do SQL Server. Recomendamos utilizar o [Azure Data Studio](https://docs.microsoft.com/pt-br/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15).
- Servidor: localhost
- Porta: conforme a variável ***MSSQL_PORT***
- Usuário: sa
- Senha: conforme a variável ***SA_PASSWORD***

6. Crie o banco de dados do Fluig com o mesmo nome definido na variável ***FLUIG_DB_ALIAS***

```sql
CREATE DATABASE [fluig_db] COLLATE Latin1_General_CI_AS;
ALTER DATABASE [fluig_db] SET READ_COMMITTED_SNAPSHOT ON;
```

7. Pare os containers:

```bash
docker-compose stop
```
8. Mude o valor da variável ***FLUIG_DISABLED*** para *false*

9. Inicie novamente os containers:

```bash
docker-compose up
```

10. Acesse o Fluig considerando os valores definidos nas variáveis ***FLUIG_CONTAINER_NAME*** e ***FLUIG_PORT***. 
Exemplo: *http://mypc-name:7080*

## Dicas 💡

- Se você também for subir nossos containers do Protheus, remova o *mssql* e o *license* do *docker-compose* e 
configure as variáveis de ambiente para utilizar os mesmos containers do ERP. Nesse caso, crie o banco de dados do 
Fluig no mesmo container do SQL Server do ambiente Protheus.

- A TOTVS ainda não possui um plugin oficial do VS Code para o Fluig. Diante disso, recomendamos codificar no VS Code 
e utilizar o Eclipse para trabalhar com diagramas e fazer deploys no servidor.

- Por questões de performance, o *realtime* e o *indexer* rodam no mesmo container do Fluig

- Após o primeiro uso, sempre utilize o comando ***docker-compose up --no-recreate*** para não reinstalar o fluig 
a cada inicialização

- Instale o [Node.js](https://nodejs.org/en/download/) e utilize os scripts NPM para subir e parar os containers 
de forma visual através do VS Code

- Acesse a página das [imagens no Docker Hub](https://hub.docker.com/u/soulsys) para conhecer todas 
as variáveis de ambiente e scripts disponíveis.