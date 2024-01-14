# HTTP - Entendendo a Web por debaixo dos panos.
Para acompanhamento do curso, será utilizado um projeto(Back e Front) para realização de algumas práticas, visando um melhor entendimento do protocolo. Neste repósitório será armazenado os projetos ultilizados em suas versões finais, onde no codigo referente a API Rest ficaram comentadas as alterações realizadas.

### Bixando e inicializando o projeto:
+ Baixando backend
```
# baixa nosso backend
git clone https://github.com/alura-cursos/api-alurabooks.git

# entra na pasta do backend
cd api-alurabooks

# instala as dependências que estão listadas no arquivo package.json
npm install

# executa o backend e o disponibiliza através de um servidor no endereço http://localhost:8000
npm run start-auth

```
+ Baixando frontend
```
# baixa nosso frontend
git clone https://github.com/alura-cursos/curso-react-alurabooks.git

# entra na pasta do frontend
cd curso-react-alurabooks

# seleciona a versão correta
git checkout aula-5

# instala as dependências
npm install

# compila o frontend e o disponibiliza através de um servidor no endereço http://localhost:3000
npm start

```
### Usando Telnet
+ Instalação
```
sudo apt-get install telnet
```
+ uso
```
telnet localhost 8000

```

### Configurando HTTPS - Back
+ Gerando certificado
```
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.crt
```
+ Habilitando HTTPs no Back:
    + Importação da biblioteca https:
    ```
    const https = require("https")
    ```
    + Encapsulando inicialização do server para usar https:
    ```
    https.createServer({
  key: fs.readFileSync('server.key'),
  cert: fs.readFileSync('server.crt')}, server).listen(8000, () => {
   console.log("API disponível em https://localhost:8000")})
    ```

### Configurando HTTP/2
+ Gerar chave e certificado:
```
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout server.key -out server.crt
```
+ Adicionar pacote que dá suporte HTTP/2 no node:
```
npm install spdy
```
+ Gerar build da aplicação
```
npm run build
```
+ Adicionar arquivo server_http2.js contendo o conteúdo a seguir:
```
const spdy = require("spdy")
const express = require("express")
const fs = require("fs")

const app = express()

app.use(express.static("build"))

spdy.createServer(
  {
    key: fs.readFileSync("./server.key"),
    cert: fs.readFileSync("./server.crt")
  },
  app
).listen(3002, (err) => {
  if(err){
    throw new Error(err)
  }
  console.log("Listening on port 3002")
})
```
+ Para testar basta executar:
```
node server_http2.js
``` 