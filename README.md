# API-REST
## 1. Instalação do Node.js
Aqui estão as etapas para instalar o Node.js no Linux:

### 1. Atualizar o sistema
Antes de começar a instalação, é uma boa prática atualizar os pacotes do sistema:

```
sudo apt update
sudo apt upgrade
```

### 2. Instalar o Node.js usando o Node Version Manager (NVM)
O NVM é uma ferramenta que permite instalar e gerenciar várias versões do Node.js. Execute os seguintes comandos para instalar o NVM:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Feche e reabra o terminal ou execute o seguinte comando para aplicar as alterações:

```
source ~/.bashrc
```

### 3. Instalar uma versão específica do Node.js
Agora que o NVM está instalado, você pode usar o seguinte comando para instalar o Node.js. Iremos instalar a versão mais recente a v20.10.0

```
nvm install v20.10.0
```
Defina a versão instalada como padrão (opcional):

```
nvm use v20.10.0
nvm alias default v20.10.0
```

### 4. Verificar a instalação
Para garantir que o Node.js e o npm foram instalados corretamente, execute os seguintes comandos:

```
node -v
npm -v
```
Se tudo estiver configurado corretamente, você verá as versões instaladas do Node.js e do npm.

Agora você está pronto para começar a desenvolver com o Node.js no seu sistema Linux!

# Instalação do Fastify no Node.js
Este guia fornece instruções passo a passo sobre como instalar o framework Fastify no Node.js em um sistema operacional Linux.
## 2. Instalação do Fastify
### 1. Crie um novo projeto Node.js
Se você ainda não tiver um projeto Node.js, crie um novo diretório para o seu projeto e navegue até ele no terminal:

```
mkdir <nome do seu projeto>
cd <nome do seu projeto>
```
### 2. Inicie um novo projeto Node.js
Execute o seguinte comando para iniciar um novo projeto Node.js. Isso criará um arquivo package.json com as configurações padrão:

```
npm init -y
```
### 3. Instale o Fastify
Agora, instale o Fastify usando o seguinte comando:

```
npm install fastify --save
```
Este comando instalará o Fastify e adicionará a dependência ao seu arquivo package.json.

### 4. Crie um arquivo de exemplo
Crie um arquivo de exemplo para verificar se o Fastify foi instalado corretamente. Por exemplo, crie um arquivo chamado index.js e adicione o seguinte código:

```javascript
const fastify = require('fastify')();

fastify.get('/', async (request, reply) => {
  return { 'hello world' };
});

const start = async () => {
  try {
    await fastify.listen(3333);
    console.log('Servidor Fastify iniciado em http://localhost:3333');
  } catch (err) {
    console.error(err);
    process.exit(1);
  }
};

start();
```

### 5. Execute o aplicativo de exemplo
Execute o aplicativo de exemplo com o seguinte comando:

```
node index.js
```
Abra o navegador e visite http://localhost:3333. Você deve ver a mensagem "hello world", indicando que o Fastify está funcionando corretamente.

# 3. Criando um Banco de Dados Local
Esse código define uma classe simples para manipular uma coleção de resumos, permitindo a criação, listagem, atualização e exclusão de resumos. Lembre-se de que essa implementação é bastante básica e pode ser estendida para atender a requisitos mais complexos, como persistência de dados em um banco de dados externo.

### 1. Importação de Módulo
```javascript
import { randomUUID } from "node:crypto";
```

Esta linha importa a função randomUUID do módulo node:crypto. Essa função é usada para gerar identificadores exclusivos.

### 2. Definição da Classe Database
```javascript
export class Database {
  #summarizations = new Map();
  // ...
}
```
A classe Database é exportada para ser utilizada em outros módulos.
#summarizations é uma propriedade privada da classe e é uma instância de Map que será usada para armazenar os resumos. Cada resumo é associado a um identificador exclusivo.

### 3. Método create
```javascript
create(summarization) {
  const summarizationId = randomUUID();

  this.#summarizations.set(summarizationId, summarization);
}
```
O método create recebe um objeto summarization como parâmetro.
randomUUID() é chamado para gerar um identificador único.
O resumo é então adicionado ao Map usando o identificador como chave.

### 4. Método list
```javascript
list(search) {
  return Array.from(this.#summarizations.entries())
    .map((summarizationArray) => {
      const id = summarizationArray[0];
      const data = summarizationArray[1];

      return {
        id,
        ...data,
      };
    })
    .filter((summarization) => {
      if (search) {
        return summarization.title.includes(search);
      }

      return true;
    });
}
```
O método list retorna uma array de resumos, transformados em objetos contendo o id e os dados do resumo.
A filtragem é opcional e é feita com base no parâmetro search. Se fornecido, apenas os resumos que contêm a string de pesquisa no título serão incluídos.

### 6. Método update
```javascript
update(id, summarization) {
  this.#summarizations.set(id, summarization);
}
```
O método update recebe um identificador e um novo objeto de resumo.
O resumo correspondente ao identificador é substituído pelo novo resumo.

### 7. Método delete
```javascript
delete(id) {
  this.#summarizations.delete(id);
}
```
O método delete recebe um identificador e remove o resumo correspondente do Map.
Esse código define uma classe simples para manipular uma coleção de resumos, permitindo a criação, listagem, atualização e exclusão de resumos. Lembre-se de que essa implementação é bastante básica e pode ser estendida para atender a requisitos mais complexos, como persistência de dados em um banco de dados externo.

# Criando a API

Este código define um servidor Fastify com quatro rotas principais para criar, listar, atualizar e excluir resumos. Ele utiliza a classe Database para manter os resumos em memória.

### 1. Importação de Módulos:
```javascript
import Fastify from "fastify";
import { Database } from "./local-database.js";
```
O código utiliza o framework Fastify para criar um servidor web.
A classe Database é importada do arquivo local-database.js. Presumo que este arquivo contenha a implementação da classe Database que revisamos anteriormente.

### 2. Configuração do Servidor e Instância do Banco de Dados:
```javascript
const server = Fastify();
const database = new Database();
```
Uma instância do servidor Fastify é criada.
Uma instância da classe Database é criada para gerenciar resumos.

### 3. Rota POST para Criar Resumo:
```javascript
server.post("/summarization", (request, response) => {
  const { title, link, startAt, endAt } = request.body;
    
  database.create({
    title: title,
    link: link,
    startAt: startAt,
    endAt: endAt,
  })

  return response.status(201).send();
});
```
Quando uma solicitação POST é feita para /summarization, os dados do corpo da solicitação são extraídos (presumivelmente, esperando um formato JSON).
O método create da instância da classe Database é chamado para adicionar um novo resumo.
A resposta é enviada com um código de status 201 (Created).

### 4. Rota GET para Listar Resumos:
```javascript
server.get("/summarization", (request, response) => {
  const search = request.query.search;
  const summarizations = database.list(search);

  return response.status(200).send(summarizations);
});
```
Quando uma solicitação GET é feita para /summarization, a string de consulta search é extraída dos parâmetros da solicitação.
O método list da instância da classe Database é chamado para obter uma lista de resumos, opcionalmente filtrados pela pesquisa.
A resposta é enviada com um código de status 200 (OK) e a lista de resumos.

### 5. Rota PUT para Atualizar Resumo:
```javascript
server.put("/summarization/:id", (request, response) => {
  const summarizationId = request.params.id;
  const {title, link, startAt, endAt} = request.body;
 
  database.update(summarizationId, {
    title: title,
    link: link,
    startAt: startAt,
    endAt: endAt,
  });

  return response.status(204).send();
});
```
Quando uma solicitação PUT é feita para /summarization/:id, o identificador do resumo a ser atualizado é extraído dos parâmetros da solicitação.
Os dados do corpo da solicitação são utilizados para atualizar o resumo correspondente usando o método update da instância da classe Database.
A resposta é enviada com um código de status 204 (No Content).

### 6. Rota DELETE para Excluir Resumo:
```javascript
server.delete("/summarization/:id", (request, response) => {
  const summarizationId = request.params.id;
  database.delete(summarizationId)
  
  return response.status(200).send();
});
```
Quando uma solicitação DELETE é feita para /summarization/:id, o identificador do resumo a ser excluído é extraído dos parâmetros da solicitação.
O método delete da instância da classe Database é chamado para excluir o resumo correspondente.
A resposta é enviada com um código de status 200 (OK).

### 7.Inicialização do Servidor:
```javascript
server.listen({
    port: 3333
});
console.log("Server listening on port: 3333");
```
O servidor é configurado para escutar na porta 3333.
Uma mensagem é exibida no console indicando que o servidor está ouvindo.

