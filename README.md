<h1 align="center">
  <br>
  <a href="https://github.com/stanycruz/projeto-nodejs-rest"><img src="https://raw.githubusercontent.com/voodootikigod/logo.js/master/js.png" alt="JavaScript Vanilla" width="200"></a>
  <br>
  Agenda Petshop API
  <br>
</h1>

<h4 align="center">API REST de agenda de petshop construída em <a href="http://nodejs.org" target="_blank">Node.js</a> com <a href="https://expressjs.com/" target="_blank">Express</a> e <a href="https://www.mysql.com/" target="_blank">MySQL</a>.</h4>

<p align="center">
  <a href="https://img.shields.io/badge/license-ISC-blue.svg">
    <img src="https://img.shields.io/badge/license-ISC-blue.svg" alt="License: ISC">
  </a>
  <a href="https://nodejs.org/">
    <img src="https://img.shields.io/badge/node-%3E%3D14.0.0-339933?logo=node.js&logoColor=white" alt="Node >= 14">
  </a>
</p>

<p align="center">
  <a href="#key-features">Key Features</a> •
  <a href="#how-to-use">How To Use</a> •
  <a href="#download">Download</a> •
  <a href="#credits">Credits</a> •
  <a href="#related">Related</a> •
  <a href="#license">License</a>
</p>

## Key Features

- **CRUD de Atendimentos**: endpoints REST para criar, listar, buscar por id, atualizar parcialmente e excluir atendimentos.
- **Validações**:
  - **cliente**: mínimo de 5 caracteres.
  - **data**: deve ser maior ou igual à data/hora atual (aceita formato DD/MM/YYYY, armazenada como datetime).
- **Criação automática de tabela**: cria a tabela `Atendimentos` no MySQL se não existir.
- **Autoload de controllers**: utiliza `consign` para carregar rotas automaticamente.
- **Body parsing**: suporte a `application/json` e `application/x-www-form-urlencoded`.
- **Configuração simples**: servidor inicia em `http://localhost:3000`.

## How To Use

Para clonar e executar esta API, você precisará do [Git](https://git-scm.com), [Node.js](https://nodejs.org/en/download/) e do [MySQL](https://www.mysql.com/) instalados.

```bash
# Clone o repositório
git clone https://github.com/stanycruz/projeto-nodejs-rest

# Entre no diretório
cd projeto-nodejs-rest

# Instale as dependências
npm install

# Crie o banco de dados (se ainda não existir)
# A API espera um banco chamado agenda-petshop
# Ajuste o usuário/senha em infrastructure/conexao.js se necessário
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS \`agenda-petshop\`;"

# Inicie a API (usa nodemon em desenvolvimento)
npm start

# A API ficará disponível em
# http://localhost:3000
```

> Nota
> As credenciais padrão do MySQL estão em `infrastructure/conexao.js` (host: `localhost`, porta: `3306`, usuário: `root`, senha: `admin`, database: `agenda-petshop`). Altere conforme seu ambiente.

## Endpoints

Base URL: `http://localhost:3000`

- **GET `/atendimentos`**: lista todos os atendimentos.
  - Exemplo:
    ```bash
    curl -s http://localhost:3000/atendimentos | jq
    ```

- **GET `/atendimentos/:id`**: busca um atendimento por id.
  - Exemplo:
    ```bash
    curl -s http://localhost:3000/atendimentos/1 | jq
    ```

- **POST `/atendimentos`**: cria um novo atendimento.
  - Campos aceitos (JSON):
    - `cliente` (string, obrigatório, mínimo 5)
    - `pet` (string, opcional)
    - `servico` (string, obrigatório)
    - `data` (string, obrigatório, formato `DD/MM/YYYY`)
    - `status` (string, obrigatório)
    - `observacoes` (string, opcional)
  - Exemplo:
    ```bash
    curl -s -X POST http://localhost:3000/atendimentos \
      -H 'Content-Type: application/json' \
      -d '{
            "cliente": "Maria Silva",
            "pet": "Rex",
            "servico": "Banho",
            "data": "31/12/2025",
            "status": "agendado",
            "observacoes": "Cliente prefere horário da tarde"
          }' | jq
    ```
  - Respostas:
    - `201 Created` com o atendimento enviado (datas normalizadas não são retornadas).
    - `400 Bad Request` com lista de erros de validação ou erro do banco.

- **PATCH `/atendimentos/:id`**: atualiza parcialmente um atendimento.
  - Observação: se enviar `data`, use o formato `DD/MM/YYYY`.
  - Exemplo (atualizar `status`):
    ```bash
    curl -s -X PATCH http://localhost:3000/atendimentos/1 \
      -H 'Content-Type: application/json' \
      -d '{ "status": "concluido" }' | jq
    ```
  - Respostas: `200 OK` com os campos atualizados e o `id`, ou `400 Bad Request`.

- **DELETE `/atendimentos/:id`**: exclui um atendimento.
  - Exemplo:
    ```bash
    curl -s -X DELETE http://localhost:3000/atendimentos/1 | jq
    ```
  - Respostas: `200 OK` com `{ id }`, ou `400 Bad Request` em caso de erro.

## Estrutura do projeto

```
config/
  customExpress.js         # Configuração do Express, body-parser e autoload via consign
controllers/
  atendimentos.js          # Definição das rotas da API
infrastructure/
  conexao.js               # Configuração da conexão MySQL
  tabelas.js               # Criação automática da tabela Atendimentos
models/
  atendimentos.js          # Regras de negócio, validações e acesso ao banco
index.js                   # Ponto de entrada do servidor (porta 3000)
```

## Download

Este projeto é distribuído como código-fonte. Para obter a última versão, **clone o repositório**:

```bash
git clone https://github.com/stanycruz/projeto-nodejs-rest
```

## Scripts

- **start**: inicia o servidor com `nodemon` (hot-reload em desenvolvimento).

```bash
npm start
```

## Credits

Esta API utiliza os seguintes pacotes open source:

- [Express](https://expressjs.com/)
- [Node.js](https://nodejs.org/)
- [consign](https://www.npmjs.com/package/consign)
- [body-parser](https://www.npmjs.com/package/body-parser)
- [mysql](https://www.npmjs.com/package/mysql)
- [moment](https://momentjs.com/)

## Related

- Documentação do [MySQL](https://dev.mysql.com/doc/)
- Guia do [Express](https://expressjs.com/pt-br/)

## License

ISC

---

> GitHub [@stanycruz](https://github.com/stanycruz)
