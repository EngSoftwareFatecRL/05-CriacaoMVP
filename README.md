# 📝 Roteiro Implementação do MVP — Connexa (Etapa 3)

## 🎯 Objetivo da Atividade

Executar as tarefas técnicas planejadas na Sprint (Etapa 2) para desenvolver uma primeira versão funcional (MVP) do produto **Connexa**. A atividade se concentrará na implementação prática utilizando **Node.js** para o back-end, **SQLite** como banco de dados e **HTML/CSS/JavaScript** para o front-end, com o auxílio do **GitHub Copilot** para acelerar o desenvolvimento.

-----

## 🔄 Continuidade do Cenário Fictício

**Produto:** *Connexa*
**Etapa Anterior (Etapa 2):** As User Stories mais prioritárias foram selecionadas para a primeira Sprint e detalhadas em tarefas técnicas.
**Etapa Atual (Etapa 3):** O time irá codificar e implementar as tarefas da Sprint, transformando o planejamento em um produto tangível. O foco é entregar uma funcionalidade completa, do banco de dados à interface do usuário.

-----

## 🛠️ Ferramentas e Tecnologias

  * **Editor de Código:** Visual Studio Code (VS Code)
  * **Assistente de IA:** GitHub Copilot / Copilot Chat
  * **Back-end:** Node.js com a biblioteca Express
  * **Banco de Dados:** SQLite (simples, baseado em arquivo, ideal para prototipagem)
  * **Front-end:** HTML, CSS e JavaScript puros

-----

## ⚙️ Parte 1: Configuração do Ambiente de Desenvolvimento

Antes de iniciar a codificação, é essencial preparar o ambiente de trabalho.

### 1.1. Pré-requisitos

Certifique-se de que todos os integrantes do grupo tenham o **Node.js** e o **VS Code** instalados em suas máquinas.

### 1.2. Configurando o GitHub Copilot no VS Code

O Copilot será seu parceiro de programação, ajudando a gerar código, sugerir soluções e acelerar tarefas repetitivas.

1.  **Instalar a Extensão:**
      * Abra o VS Code.
      * Vá para a aba de **Extensões** (ícone de blocos no menu lateral ou `Ctrl+Shift+X`).
      * Pesquise por `GitHub Copilot` e clique em **Instalar** na extensão oficial da Microsoft/GitHub.
      * Instale também a extensão `GitHub Copilot Chat` para ter acesso à interface de chat.
2.  **Autorizar o Acesso:**
      * Após a instalação, um ícone de login aparecerá na barra de status inferior do VS Code.
      * Clique nele e siga as instruções para autenticar com sua conta do GitHub que possui acesso ao Copilot.

### 1.3. Estrutura Inicial do Projeto

1.  Crie uma pasta principal para o projeto: `connexa-mvp`.
2.  Abra esta pasta no VS Code.
3.  Abra um terminal integrado (`Ctrl+'`) e inicie um projeto Node.js:
    ```bash
    npm init -y
    ```
4.  Instale as dependências necessárias:
      * **Express:** Para criar o servidor web.
      * **SQLite3:** Para interagir com o banco de dados SQLite.
      * **sqlite:** Um wrapper que facilita o uso do SQLite com `async/await`.
    <!-- end list -->
    ```bash
    npm install express sqlite sqlite3
    ```

-----

## 🤖 Parte 2: Usando o GitHub Copilot como Agente de Desenvolvimento

O Copilot Chat (`Ctrl+Shift+I`) no VS Code atua como um "agente" que entende o contexto do seu projeto. Você pode pedir para ele criar arquivos, explicar código ou gerar trechos complexos a partir de uma descrição.

**Boas práticas para criar prompts no modo agente (`@workspace`):**

  * **Dê Contexto:** Comece explicando o objetivo. Em vez de "crie um endpoint", diga "Estou criando uma API para um sistema de grupos de estudo chamado Connexa. Crie um endpoint para cadastro de usuários".
  * **Seja Específico:** Detalhe a estrutura de dados, os nomes dos campos, as validações necessárias e o comportamento esperado (ex: "o endpoint deve receber um JSON com nome, email e senha e salvar na tabela 'usuarios'").
  * **Itere:** Use o primeiro resultado como ponto de partida. Peça para o Copilot refinar, adicionar tratamento de erros, ou explicar uma parte do código que ele gerou.

-----

## 🚀 Parte 3: Implementando a Primeira Funcionalidade (Exemplo: Cadastro de Usuário)

Vamos usar uma das tarefas da Sprint para ilustrar o fluxo de trabalho completo.

### 3.1. Camada de Dados: Criando a Tabela com SQLite

O SQLite não requer um servidor. O banco de dados será um simples arquivo no seu projeto (ex: `connexa.db`).

#### Exemplo de Prompt para o GitHub Copilot Chat:

Abra o chat do Copilot e use o seguinte prompt para criar um script de inicialização do banco de dados.

```prompt
Usando as bibliotecas 'sqlite' e 'sqlite3' para Node.js, crie um script chamado 'database.js' que inicializa o banco de dados. Este script deve criar um arquivo de banco de dados chamado 'connexa.db' e, se não existir, criar uma tabela 'usuarios' com os seguintes campos: id (chave primária, autoincremento), nome (texto, não nulo), email (texto, único, não nulo), curso (texto), semestre (inteiro) e senha (texto, não nulo).
```

O Copilot deve gerar um código similar a este para seu arquivo `database.js`:

```javascript
// database.js
const sqlite3 = require('sqlite3').verbose();
const { open } = require('sqlite');

async function setup() {
  const db = await open({
    filename: './connexa.db',
    driver: sqlite3.Database
  });

  await db.exec(`
    CREATE TABLE IF NOT EXISTS usuarios (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      nome TEXT NOT NULL,
      email TEXT NOT NULL UNIQUE,
      curso TEXT,
      semestre INTEGER,
      senha TEXT NOT NULL
    );
  `);

  console.log('Banco de dados e tabela de usuários criados com sucesso.');
  return db;
}

setup();
```

Execute o script uma vez para criar o arquivo do banco de dados: `node database.js`.

### 3.2. Camada de Lógica: Criando o Endpoint no Back-end

Agora, vamos criar o servidor web e o endpoint de cadastro.

#### Exemplo de Prompt para o GitHub Copilot Chat:

```prompt
Estou usando Express.js. Crie um servidor básico em um arquivo 'server.js'. Adicione um endpoint POST em '/api/usuarios/cadastro'. Este endpoint deve:
1. Receber no corpo da requisição um JSON com: nomeCompleto, email, curso, semestre e senha.
2. Conectar ao banco de dados SQLite 'connexa.db'.
3. Inserir os dados recebidos na tabela 'usuarios'.
4. Retornar uma resposta de sucesso (status 201) com uma mensagem se o cadastro for bem-sucedido.
5. Retornar uma resposta de erro (status 400) se algum campo obrigatório estiver faltando.
6. Retornar um erro (status 500) se houver um problema no banco de dados, como um e-mail duplicado.
Adicione o middleware 'express.json()' para processar o corpo da requisição.
```

O Copilot deve gerar um código para o arquivo `server.js` parecido com:

```javascript
// server.js
const express = require('express');
const { open } = require('sqlite');
const sqlite3 = require('sqlite3');

const app = express();
app.use(express.json()); // Middleware para parsear JSON

let db;

// Conecta ao banco de dados
(async () => {
    db = await open({
        filename: './connexa.db',
        driver: sqlite3.Database
    });
})();

// Endpoint de cadastro
app.post('/api/usuarios/cadastro', async (req, res) => {
    const { nomeCompleto, email, curso, semestre, senha } = req.body;

    if (!nomeCompleto || !email || !senha) {
        return res.status(400).json({ error: 'Nome, e-mail e senha são obrigatórios.' });
    }

    try {
        const result = await db.run(
            'INSERT INTO usuarios (nome, email, curso, semestre, senha) VALUES (?, ?, ?, ?, ?)',
            [nomeCompleto, email, curso, semestre, senha]
        );
        res.status(201).json({ message: 'Usuário cadastrado com sucesso!', userId: result.lastID });
    } catch (error) {
        if (error.code === 'SQLITE_CONSTRAINT') {
            return res.status(400).json({ error: 'O e-mail informado já está em uso.' });
        }
        res.status(500).json({ error: 'Erro ao cadastrar usuário.', details: error.message });
    }
});

const PORT = 3000;
app.listen(PORT, () => {
    console.log(`Servidor rodando na porta ${PORT}`);
});
```

### 3.3. Camada de Apresentação: Criando o Formulário no Front-end

Por fim, crie a interface que o usuário utilizará.

#### Exemplo de Prompt para o GitHub Copilot Chat:

```prompt
Crie um arquivo HTML chamado 'index.html'. Ele deve conter um formulário de cadastro para a plataforma Connexa com os campos: "Nome Completo", "E-mail Institucional", "Curso", "Semestre" e "Senha".
Adicione um script JavaScript que:
1. Intercepte o evento de submit do formulário.
2. Capture os valores dos campos.
3. Envie uma requisição POST para o endpoint '/api/usuarios/cadastro' usando a API Fetch.
4. Exiba uma mensagem de sucesso ou erro em um parágrafo abaixo do formulário com base na resposta do servidor.
Adicione um estilo CSS básico para centralizar o formulário e dar uma aparência limpa.
```

O Copilot irá gerar o `index.html` com HTML, CSS e JavaScript integrados:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF--8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cadastro - Connexa</title>
    <style>
        body { font-family: sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #f0f2f5; }
        .container { padding: 2rem; background: white; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
        form { display: flex; flex-direction: column; gap: 1rem; }
        input { padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; }
        button { padding: 0.7rem; background-color: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
        #message { margin-top: 1rem; text-align: center; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Crie sua Conta na Connexa</h2>
        <form id="cadastroForm">
            <input type="text" id="nome" placeholder="Nome Completo" required>
            <input type="email" id="email" placeholder="E-mail Institucional" required>
            <input type="text" id="curso" placeholder="Curso">
            <input type="number" id="semestre" placeholder="Semestre">
            <input type="password" id="senha" placeholder="Senha" required>
            <button type="submit">Cadastrar</button>
        </form>
        <p id="message"></p>
    </div>

    <script>
        document.getElementById('cadastroForm').addEventListener('submit', async function(event) {
            event.preventDefault(); // Impede o recarregamento da página

            const nome = document.getElementById('nome').value;
            const email = document.getElementById('email').value;
            const curso = document.getElementById('curso').value;
            const semestre = document.getElementById('semestre').value;
            const senha = document.getElementById('senha').value;
            const messageEl = document.getElementById('message');

            try {
                const response = await fetch('/api/usuarios/cadastro', {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        nomeCompleto: nome,
                        email: email,
                        curso: curso,
                        semestre: parseInt(semestre),
                        senha: senha
                    })
                });

                const data = await response.json();

                if (response.ok) {
                    messageEl.textContent = data.message;
                    messageEl.style.color = 'green';
                } else {
                    messageEl.textContent = 'Erro: ' + data.error;
                    messageEl.style.color = 'red';
                }
            } catch (error) {
                messageEl.textContent = 'Erro de conexão com o servidor.';
                messageEl.style.color = 'red';
            }
        });
    </script>
</body>
</html>
```

-----

## ✅ Critérios de Entrega (Via Microsoft Teams)

A entrega da atividade deverá ser feita na plataforma Microsoft Teams, contendo os seguintes itens:

1.  **Prompt do Back-end:** O texto exato do prompt que seu grupo utilizou no GitHub Copilot Chat para gerar o endpoint de criação de uma das funcionalidades (ex: cadastro de usuário, criação de grupo, etc.).

2.  **Prompt do Front-end:** O texto exato do prompt utilizado para gerar a interface HTML/CSS/JavaScript correspondente à funcionalidade do back-end.

-----

## 📖 Referências e Recursos Complementares

  * **Node.js:**
      * [Documentação Oficial do Node.js](https://nodejs.org/en/docs/)
  * **Express.js:**
      * [Site Oficial do Express](https://expressjs.com/pt-br/)
  * **SQLite:**
      * [Documentação da biblioteca `sqlite`](https://www.google.com/search?q=%5Bhttps://www.npmjs.com/package/sqlite%5D\(https://www.npmjs.com/package/sqlite\))
  * **GitHub Copilot:**
      * [Documentação do GitHub Copilot Chat](https://docs.github.com/pt/copilot/github-copilot-chat/using-github-copilot-chat-in-your-ide)
  * **Fetch API (para o front-end):**
      * [Guia da MDN sobre Fetch API](https://developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API/Using_Fetch)
