# **Exercícios Docker - Docker Desktop**
---
###### 🟢 Fácil
---

### Exercício 1: Rodando um container básico com Nginx

Abra o terminal do Docker Desktop.

Execute o comando para que se posaa baixar e rodar a imagem, como a do Nginx por exemplo:

````bash
docker run -d -p 8080:80 nginx

````
![Image](https://github.com/user-attachments/assets/ea6780c5-4317-4e7c-9cd6-bf7b20c9ffa6)

*`-d`* faz o container rodar em segundo plano (detached).
*`-p`* 8080:80 mapeia a porta 80 do container para a porta 8080 do seu host. 

No navegador, acessando http://localhost:8080 
![Image](https://github.com/user-attachments/assets/b0d2779e-fc3f-4a2d-9063-728d9e06f22e)
Por enquanto exibirá a página padrão do Nginx.

Então como proposto no exercicio para usar a landing page do TailwindCSS, podemos criar um arquivo HTML simples:

````bash 
echo '<!DOCTYPE html><html lang="pt"><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width, initial-scale=1.0"><link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet"><title>My Tailwind Page</title></head><body class="bg-gray-100"><h1 class="text-4xl text-center mt-10">Hello, TailwindCSS!</h1></body></html>' > index.html
````
![Image](https://github.com/user-attachments/assets/3f3d515e-b608-4ebe-9188-8ff375eca80f)

Crie um Dockerfile para usar esse arquivo HTML:


````bash
nano Dockerfile
`````
![Image](https://github.com/user-attachments/assets/cd8c78df-b610-4544-8ccf-cc1372bc7f5b)

### Dockerfile
```bash
FROM nginx
COPY index.html /usr/share/nginx/html/index.html
````
![Image](https://github.com/user-attachments/assets/3609565d-2d28-44e2-8ad4-d50a84ae167a)
Construa a imagem:

````bash
docker build -t my-tailwind-nginx .
`````
![Image](https://github.com/user-attachments/assets/4808884d-fcb9-4f59-bd2b-a8a07d2d2209)

Acesse novamente http://localhost:8080 para ver sua página com TailwindCSS.

![Image](https://github.com/user-attachments/assets/7033a314-48e4-4ccf-9497-0a6845477187)

Agora com a edição localmente de um arquivo index.html, onde foi utilizadando o exemplo landing page https://github.com/tailwindtoolbox/Landing-Page/blob/master/index.html

![Image](https://github.com/user-attachments/assets/3cdb8c90-26c6-410b-a7e9-24e1d08e743c)
 
 Após a criação desse arquivo usando o exemplo acima, podemos então copiar para dentro do container:
 
 ```bash
docker cp "/mnt/d/ESTUDOS/LINUX/docker/index.html" bbf920ad7653:/usr/share/nginx/html/index.html
```

eu consigo verificar se o arquivo foi copiado corretamente com:

````bash
docker exec -it bbf920ad7653 ls /usr/share/nginx/html/
````

![Image](https://github.com/user-attachments/assets/382307fd-5968-4167-a87e-76776b8d7570)

logo verifico a porta http://localhost:8080
 e tenho a seguinte saida:

 ![Image](https://github.com/user-attachments/assets/ea2518a1-cc1a-4349-ab52-a63dd2f1a95e)

 ![Image](https://github.com/user-attachments/assets/79920306-1e5e-448c-b620-d6f12bb87efa)

 ![Image](https://github.com/user-attachments/assets/c3e159d6-4ac8-45cb-9121-524b68cd5cea)

#### Esse site foi usado como exemplo de teste dentro dos exercicos propostos.

### Exercício 2: Criando e rodando um container interativo com Ubuntu

para iniciar um container Ubuntu interativo:

````bash
docker run -it ubuntu
`````
![Image](https://github.com/user-attachments/assets/25afc933-f928-4df0-827f-46eaaad421cd)

*`-it`* permite que eu interaja com o terminal do container.

Dentro do container, você pode executar comandos. Por exemplo, para atualizar o sistema e instalar o curl:

````bash
apt-get update
apt-get install -y curl
`````
![Image](https://github.com/user-attachments/assets/bda05eb9-4ae7-421b-b144-392799f572ae)

Para sair do container:

**`exit`**

![Image](https://github.com/user-attachments/assets/f81a8b6c-8bfe-4109-b925-ed722e4fc112)

### Exercício 3: Listando e removendo containers
Listando todos os containers:

````bash
docker ps -a
`````
![Image](https://github.com/user-attachments/assets/2141273c-5014-4247-ad67-22799f011423)

Para *``parar``* um container em execução, use o comando:

````bash
docker stop container_id
`````
Substitua *``container_id``* pelo ID do container que você deseja parar.

Remover um container específico use:

````bash
docker rm container_id
`````

### Exercício 4: Criando um Dockerfile para uma aplicação simples em Python (Flask)
Criando agora um diretório para a aplicação:

````bash
mkdir my-flask-app

cd my-flask-app
`````

![Image](https://github.com/user-attachments/assets/7a40d3a2-9ce9-4029-9874-a312d2b6f290)

Crie um arquivo chamado app.py com o seguinte conteúdo:

````bash
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Olá, mundo! Esta é uma aplicação Flask rodando no Docker."

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
`````
![Image](https://github.com/user-attachments/assets/3792b999-d25a-47e0-b4d8-789bbd103dab)

**Crie um arquivo requirements.txt com o seguinte conteúdo:**

````bash
Flask
`````
**Crie um Dockerfile com o seguinte conteúdo:**

````bash
# Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

CMD ["python", "app.py"]
`````

![Image](https://github.com/user-attachments/assets/fd58cad4-0cc6-4f7f-86da-86614190bd21)

**Construa a imagem:**

5. Construa a imagem:

```bash
docker build -t my-flask-app .
```
![Image](https://github.com/user-attachments/assets/63416006-bafe-435e-b56b-b6c1c15f57ba)

6. Execute o container:

```bash
docker run -d -p 5000:5000 my-flask-app
```

![Image](https://github.com/user-attachments/assets/bfec06e0-8936-4dbb-af5b-a0674a87a116)

7. Acesse a aplicação no navegador em: [http://localhost:5000](http://localhost:5000)

![Image](https://github.com/user-attachments/assets/55c32924-4f9c-4e7b-bc83-7933db514505)

---
##### 🟡 Médio
---

### Exercício 5: Criando e utilizando volumes para persistência de dados com MySQL

Instalar PHP e Dependências

```bash
sudo apt update && sudo apt install -y php-cli php-mbstring php-xml php-bcmath php-curl unzip curl git composer php-mysql
```
![Image](https://github.com/user-attachments/assets/fd451f99-c0fd-4d9f-ae8a-cbdf35de8cd1)

Instalar Node.js e npm

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Criar e Rodar o Container MySQL com Volume Persistente

```bash
docker volume create mysql_data

docker run --name mysql-breeze -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=laravel -e MYSQL_USER=laravel -e MYSQL_PASSWORD=secret -p 3306:3306 -v mysql_data:/var/lib/mysql -d mysql:8
```
![Image](https://github.com/user-attachments/assets/9b9f5257-b587-4996-a7a7-5c2830f7c576)

Criar um Novo Projeto Laravel

```bash
composer create-project laravel/laravel laravel-breeze && cd laravel-breeze
```
![Image](https://github.com/user-attachments/assets/524cc503-6b5b-4b96-8c22-59d912b0cdd4)

Configurar o Banco de Dados (.env)

```bash
sudo nano .env
```
Adicione ou edite as seguintes linhas:
```ini
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```
![Image](https://github.com/user-attachments/assets/09abf2f6-8f73-4c27-8a65-658f7e4e3c4b)

Instalar e Configurar Laravel Breeze

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
```

Instalar e Rodar o Vit

```bash
npm install
npm run dev
```

Rodar o Servidor Laravel

```bash
php artisan serve
```
Acesse: [http://127.0.0.1:8000](http://127.0.0.1:8000)

![Image](https://github.com/user-attachments/assets/e51f3a7c-1984-4eb8-b2c9-74f2d0ae857d)

Testar Persistência do Banco de Dados
```bash
docker rm -f mysql-breeze

docker run --name mysql-breeze -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=laravel -e MYSQL_USER=laravel -e MYSQL_PASSWORD=secret -p 3306:3306 -v mysql_data:/var/lib/mysql -d mysql:8

docker exec -it mysql-breeze mysql -u laravel -p
```
Dentro do MySQL:

```sql
USE laravel;
SELECT * FROM users;
````

## Exercício 6: Criando e rodando um container multi-stage


### Passos para otimizar uma aplicação Go com multi-stage build:

 Criação do Dockerfile para uma aplicação Go que utiliza multi-stage build:
   ```dockerfile

   FROM golang:1.18 AS builder

   WORKDIR /app
   COPY . .
   RUN go mod tidy
   RUN go build -o app


   FROM alpine:latest

   WORKDIR /app
   COPY --from=builder /app/app .

   CMD ["./app"]
   ```
   - `FROM golang:1.18 AS builder`: usa a imagem do Go para compilar o código.
   - `COPY . .`: copia os arquivos do código fonte para o contêiner.
   - `RUN go build -o app`: compila o código Go.
   - `FROM alpine:latest`: usa uma imagem mais leve (Alpine) para a imagem final.
   - `COPY --from=builder /app/app .`: copia o binário compilado da etapa anterior.
   - `CMD ["./app"]`: define o comando padrão para rodar a aplicação.

2. **Construir e rodar o container**:
   Após criar o Dockerfile, você pode construir e rodar o container:
   ![Image](https://github.com/user-attachments/assets/680cf08d-2c58-4822-a5d9-2a30ea87e1d7)
   ```bash
   docker build -t go-app .
   docker run -p 8080:8080 go-app
   ```
   - `docker build -t go-app .`: constrói a imagem com a tag `go-app`.
   - `docker run -p 8080:8080 go-app`: executa o container e mapeia a porta 8080 para acessar a API.
![Image](https://github.com/user-attachments/assets/6e84ac09-43fa-4fe6-bc5e-de5426d806a5)

#### Exercício 7: Construindo uma rede Docker para comunicação entre containers

1. **Criar uma rede para que os containers possam se comunicar:

   ```bash
   docker network create mean-network
   ```

2. **Rodar o container MongoDB e Node.js**:
   Em seguida, você roda os containers do MongoDB e Node.js conectados à rede `mean-network`:
   ```bash
   docker run --network mean-network --name mongodb -d mongo
   docker run --network mean-network -p 3000:3000 --name node-app -d node:14
   ```
![Image](https://github.com/user-attachments/assets/727f4d51-d1c6-4a5f-92d6-d4aaa7ea3aca)

3. **Conectar MongoDB e Node.js**:
   No código Node.js, você pode se conectar ao MongoDB usando o nome do container como host:
   ```javascript
   const mongoose = require('mongoose');
   mongoose.connect('mongodb://mongodb:27017/todos');
   ```
   - `mongodb`: é o nome do container do MongoDB, que será resolvido na rede Docker.
