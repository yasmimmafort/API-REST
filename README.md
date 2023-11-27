# API-REST
## 1. Instalação do Node.js
Aqui estão as etapas para instalar o Node.js no Linux:

1. Atualizar o sistema
Antes de começar a instalação, é uma boa prática atualizar os pacotes do sistema:

```
sudo apt update
sudo apt upgrade
```

2. Instalar o Node.js usando o Node Version Manager (NVM)
O NVM é uma ferramenta que permite instalar e gerenciar várias versões do Node.js. Execute os seguintes comandos para instalar o NVM:

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Feche e reabra o terminal ou execute o seguinte comando para aplicar as alterações:

```
source ~/.bashrc
```

3. Instalar uma versão específica do Node.js
Agora que o NVM está instalado, você pode usar o seguinte comando para instalar o Node.js. Iremos instalar a versão mais recente a v20.10.0

```
nvm install v20.10.0
```
Defina a versão instalada como padrão (opcional):

```
nvm use v20.10.0
nvm alias default v20.10.0
```

4. Verificar a instalação
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
1. Crie um novo projeto Node.js
Se você ainda não tiver um projeto Node.js, crie um novo diretório para o seu projeto e navegue até ele no terminal:

```
mkdir <nome do seu projeto>
cd <nome do seu projeto>
```
2. Inicie um novo projeto Node.js
Execute o seguinte comando para iniciar um novo projeto Node.js. Isso criará um arquivo package.json com as configurações padrão:

```
npm init -y
```
3. Instale o Fastify
Agora, instale o Fastify usando o seguinte comando:

```
npm install fastify --save
```
Este comando instalará o Fastify e adicionará a dependência ao seu arquivo package.json.

4. Crie um arquivo de exemplo
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

5. Execute o aplicativo de exemplo
Execute o aplicativo de exemplo com o seguinte comando:

```
node index.js
```
Abra o navegador e visite http://localhost:3333. Você deve ver a mensagem "hello world", indicando que o Fastify está funcionando corretamente.
