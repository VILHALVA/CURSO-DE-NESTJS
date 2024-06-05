# INSTRUÇÕES (GENERICAS)
## 01) CRIAÇÃO DE APLICAÇÕES BACKEND COM BASE DO NODE.JS
Neste tutorial, vamos passar pelos passos necessários para criar uma aplicação backend usando NestJS, que é um framework construído em cima do Node.js e TypeScript. Vamos cobrir desde a instalação até a estruturação de um projeto simples.

### 1. Instalar Node.js e NestJS CLI
Primeiro, precisamos ter o Node.js instalado. Você pode baixar o Node.js [aqui](https://nodejs.org/).

Depois de instalar o Node.js, instale o NestJS CLI globalmente:

```bash
npm install -g @nestjs/cli
```

### 2. Criar um Novo Projeto NestJS
Use o comando abaixo para criar um novo projeto:

```bash
nest new project-name
```

Substitua `project-name` pelo nome do seu projeto. O CLI perguntará qual gerenciador de pacotes você deseja usar (npm ou yarn). Escolha o que preferir.

### 3. Navegar até o Diretório do Projeto
```bash
cd project-name
```

### 4. Estrutura do Projeto
A estrutura inicial do projeto é a seguinte:

```
project-name/
├── dist/
├── node_modules/
├── src/
│   ├── app.controller.spec.ts
│   ├── app.controller.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── .eslintrc.js
├── .gitignore
├── .prettierrc
├── nest-cli.json
├── package.json
├── README.md
├── tsconfig.build.json
├── tsconfig.json
```

### 5. Iniciar a Aplicação
Para iniciar a aplicação em modo de desenvolvimento, use o comando:

```bash
npm run start:dev
```

### 6. Adicionar um Novo Módulo, Controlador e Serviço
Vamos adicionar um novo módulo chamado `cats`, junto com um controlador e um serviço.

#### Criar um Novo Módulo
```bash
nest generate module cats
```

#### Criar um Novo Controlador
```bash
nest generate controller cats
```

#### Criar um Novo Serviço
```bash
nest generate service cats
```

### 7. Implementação do Módulo Cats
#### Atualizar o Serviço (`cats.service.ts`)
```typescript
import { Injectable } from '@nestjs/common';

export interface Cat {
  id: number;
  name: string;
  age: number;
  breed: string;
}

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  findAll(): Cat[] {
    return this.cats;
  }

  findOne(id: number): Cat {
    return this.cats.find(cat => cat.id === id);
  }

  create(cat: Cat) {
    this.cats.push(cat);
  }
}
```

#### Atualizar o Controlador (`cats.controller.ts`)
```typescript
import { Controller, Get, Post, Param, Body } from '@nestjs/common';
import { CatsService, Cat } from './cats.service';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Get()
  findAll(): Cat[] {
    return this.catsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: number): Cat {
    return this.catsService.findOne(id);
  }

  @Post()
  create(@Body() createCatDto: Cat) {
    this.catsService.create(createCatDto);
  }
}
```

#### Atualizar o Módulo (`cats.module.ts`)
```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

#### Registrar o Módulo no Módulo Principal (`app.module.ts`)
```typescript
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

### 8. Testar a Aplicação
Execute a aplicação:

```bash
npm run start:dev
```

Use ferramentas como Postman ou cURL para testar as rotas.

#### Exemplo de Requisições
- **GET** `/cats`: Retorna todos os gatos.
- **GET** `/cats/:id`: Retorna um gato pelo ID.
- **POST** `/cats`: Cria um novo gato.

```json
// Exemplo de corpo para POST /cats
{
  "id": 1,
  "name": "Whiskers",
  "age": 2,
  "breed": "Persian"
}
```

### Conclusão
Você agora tem uma aplicação backend básica criada com NestJS, incluindo a criação de módulos, controladores e serviços. O NestJS fornece uma estrutura poderosa e escalável para o desenvolvimento de aplicações Node.js, permitindo a criação de aplicações robustas e bem organizadas.

## 02) ESTRUTURA INICIAL DE UM PROJETO NESTJS
A estrutura inicial de um projeto NestJS é organizada de forma a manter o código limpo e modular, facilitando a manutenção e a escalabilidade. Abaixo está a estrutura de diretórios e arquivos que você obterá após criar um novo projeto NestJS:

### Estrutura de Diretórios e Arquivos
```
project-name/
├── dist/
├── node_modules/
├── src/
│   ├── app.controller.spec.ts
│   ├── app.controller.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── .eslintrc.js
├── .gitignore
├── .prettierrc
├── nest-cli.json
├── package.json
├── README.md
├── tsconfig.build.json
├── tsconfig.json
```

### Descrição dos Diretórios e Arquivos
- **dist/**: Diretório onde os arquivos compilados serão armazenados. Este diretório é gerado após a compilação do TypeScript para JavaScript.
- **node_modules/**: Diretório onde as dependências do projeto são instaladas.
- **src/**: Diretório principal do código-fonte da aplicação.
  - **app.controller.spec.ts**: Arquivo de teste unitário para o `AppController`.
  - **app.controller.ts**: Arquivo do controlador principal que lida com as requisições HTTP.
  - **app.module.ts**: Módulo raiz da aplicação. É onde você importa os outros módulos e define a estrutura principal da aplicação.
  - **app.service.ts**: Serviço principal da aplicação, contendo a lógica de negócio.
  - **main.ts**: Arquivo de entrada da aplicação. Inicializa o NestJS e começa a escutar por requisições HTTP.
- **test/**: Diretório para testes end-to-end.
  - **app.e2e-spec.ts**: Arquivo de teste end-to-end.
  - **jest-e2e.json**: Configuração do Jest para testes end-to-end.
- **.eslintrc.js**: Configuração do ESLint, uma ferramenta de análise estática para identificar e corrigir problemas em JavaScript e TypeScript.
- **.gitignore**: Arquivo para especificar quais arquivos e diretórios devem ser ignorados pelo Git.
- **.prettierrc**: Configuração do Prettier, um formatador de código.
- **nest-cli.json**: Configuração do Nest CLI.
- **package.json**: Arquivo de configuração do projeto npm, contendo scripts, dependências e outras configurações do projeto.
- **README.md**: Arquivo de descrição do projeto.
- **tsconfig.build.json**: Configuração do TypeScript para o processo de build.
- **tsconfig.json**: Configuração principal do TypeScript.

### Explicação dos Arquivos Principais
#### app.controller.ts
```typescript
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }
}
```

- Define o `AppController`, que lida com as requisições HTTP. Neste exemplo, ele tem uma rota GET que retorna uma mensagem.

#### app.module.ts
```typescript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Module({
  imports: [],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

- Define o módulo principal da aplicação. Importa o `AppController` e o `AppService`.

#### app.service.ts
```typescript
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

- Define o `AppService`, que contém a lógica de negócio da aplicação. Neste exemplo, ele tem um método `getHello()` que retorna uma mensagem.

#### main.ts
```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

- É o ponto de entrada da aplicação. Cria e inicializa a aplicação NestJS e começa a escutar por requisições na porta 3000.

### Configuração do Projeto
Para iniciar a aplicação, use os comandos a seguir:

- **Iniciar em modo de desenvolvimento**:

  ```bash
  npm run start:dev
  ```

- **Build para produção**:

  ```bash
  npm run build
  ```

- **Iniciar a aplicação em produção**:

  ```bash
  npm run start:prod
  ```

### Conclusão
A estrutura inicial do NestJS é projetada para ser modular e escalável, facilitando a organização e manutenção do código. Compreender essa estrutura é crucial para desenvolver aplicações robustas e eficientes com NestJS. 

## 03) TRABALHANDO COM CONTROLLERS
Os controladores são responsáveis por lidar com as requisições HTTP e retornar respostas ao cliente. Eles atuam como intermediários entre o sistema de roteamento e a lógica de negócios, que geralmente reside nos serviços.

### Criando e Configurando Controladores
Vamos explorar como criar e configurar controladores no NestJS com exemplos práticos.

#### 1. Criar um Controlador
Usando o CLI do NestJS, você pode gerar um controlador com o seguinte comando:

```bash
nest generate controller cats
```

Isso criará um arquivo de controlador (`cats.controller.ts`) na pasta `src/cats/`.

#### 2. Estrutura de um Controlador
Um controlador básico em NestJS pode ser assim:

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Param, Body } from '@nestjs/common';
import { CatsService } from './cats.service';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Get()
  findAll() {
    return this.catsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.catsService.findOne(id);
  }

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```

#### 3. Serviços e DTOs
Vamos adicionar um serviço e um DTO para tornar o exemplo mais completo.

##### Serviço (`cats.service.ts`)
```typescript
// src/cats/cats.service.ts
import { Injectable } from '@nestjs/common';

export interface Cat {
  id: number;
  name: string;
  age: number;
  breed: string;
}

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  findAll(): Cat[] {
    return this.cats;
  }

  findOne(id: string): Cat {
    return this.cats.find(cat => cat.id === parseInt(id));
  }

  create(cat: Cat) {
    this.cats.push(cat);
  }
}
```

##### DTO (`create-cat.dto.ts`)
```typescript
// src/cats/dto/create-cat.dto.ts
export class CreateCatDto {
  readonly id: number;
  readonly name: string;
  readonly age: number;
  readonly breed: string;
}
```

#### 4. Atualizar o Módulo (`cats.module.ts`)
Certifique-se de que o módulo `cats` está configurado corretamente para incluir o controlador e o serviço.

```typescript
// src/cats/cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

#### 5. Registrar o Módulo Principal (`app.module.ts`)
Adicione o `CatsModule` ao módulo principal da aplicação.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

### Explicação dos Decoradores Usados
- **@Controller('cats')**: Define que essa classe é um controlador e mapeia as rotas que começam com `/cats`.
- **@Get()**: Mapeia a rota HTTP GET para o método findAll.
- **@Get(':id')**: Mapeia a rota HTTP GET com um parâmetro dinâmico `id` para o método findOne.
- **@Post()**: Mapeia a rota HTTP POST para o método create.
- **@Param('id')**: Extrai o parâmetro `id` da rota.
- **@Body()**: Extrai o corpo da requisição.

### Testando o Controlador
Inicie a aplicação com o comando:

```bash
npm run start:dev
```

Use ferramentas como Postman ou cURL para testar as rotas:

- **GET** `/cats`: Retorna todos os gatos.
- **GET** `/cats/:id`: Retorna um gato pelo ID.
- **POST** `/cats`: Cria um novo gato.

```json
// Exemplo de corpo para POST /cats
{
  "id": 1,
  "name": "Whiskers",
  "age": 2,
  "breed": "Persian"
}
```

### Resumo
Os controladores no NestJS são essenciais para definir como a aplicação lida com as requisições HTTP. Eles são responsáveis por receber as requisições, chamar os serviços necessários para tratar a lógica de negócio e retornar as respostas apropriadas. Com o uso de decoradores, a configuração das rotas torna-se simples e intuitiva. Este exemplo cobre a criação de um controlador básico, integração com serviços e uso de DTOs para gerenciar dados. 

## 04) TRABALHANDO COM PARÂMETROS EM ROTAS
Trabalhar com parâmetros em rotas no NestJS é uma tarefa comum ao desenvolver APIs RESTful. Os parâmetros de rota são frequentemente usados para identificar recursos específicos. Vamos explorar como usar diferentes tipos de parâmetros de rota, incluindo parâmetros de caminho (path), query parameters e corpo da requisição (body).

### Parâmetros de Caminho (Path Parameters)
Parâmetros de caminho são partes da URL que são dinâmicas e podem variar. No NestJS, você pode usar o decorador `@Param` para acessar esses parâmetros.

#### Exemplo de Controlador com Parâmetros de Caminho
```typescript
import { Controller, Get, Post, Param, Body, Put, Delete } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';
import { UpdateCatDto } from './dto/update-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Get()
  findAll() {
    return this.catsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.catsService.findOne(id);
  }

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return this.catsService.update(id, updateCatDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.catsService.remove(id);
  }
}
```

### Query Parameters
Query parameters são usados para passar dados adicionais na URL após o ponto de interrogação (`?`). Você pode acessá-los usando o decorador `@Query`.

#### Exemplo de Controlador com Query Parameters
```typescript
import { Controller, Get, Query } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Query() query: any) {
    const { limit, offset } = query;
    // Use os parâmetros limit e offset conforme necessário
    return `Limit: ${limit}, Offset: ${offset}`;
  }
}
```

Para fazer uma requisição GET com query parameters, você pode usar uma URL como:
```
http://localhost:3000/cats?limit=10&offset=20
```

### Corpo da Requisição (Request Body)
O corpo da requisição é usado principalmente em métodos POST e PUT para enviar dados ao servidor. O decorador `@Body` permite acessar esses dados.

#### Exemplo de Controlador com Corpo da Requisição
```typescript
import { Controller, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```

### Combinação de Parâmetros
Você pode combinar parâmetros de caminho, query parameters e corpo da requisição em um único método de controlador.

#### Exemplo de Controlador com Todos os Tipos de Parâmetros
```typescript
import { Controller, Get, Post, Put, Delete, Param, Query, Body } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';
import { UpdateCatDto } from './dto/update-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Get()
  findAll(@Query('limit') limit: string, @Query('offset') offset: string) {
    // Use os parâmetros limit e offset conforme necessário
    return `Limit: ${limit}, Offset: ${offset}`;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.catsService.findOne(id);
  }

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return this.catsService.update(id, updateCatDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.catsService.remove(id);
  }
}
```

### DTOs (Data Transfer Objects)
DTOs são usados para definir a estrutura dos dados que são transferidos entre o cliente e o servidor.

#### Exemplo de DTO
```typescript
// src/cats/dto/create-cat.dto.ts
export class CreateCatDto {
  readonly id: number;
  readonly name: string;
  readonly age: number;
  readonly breed: string;
}

// src/cats/dto/update-cat.dto.ts
export class UpdateCatDto {
  readonly name?: string;
  readonly age?: number;
  readonly breed?: string;
}
```

### Resumo
Os controladores no NestJS permitem trabalhar de forma eficiente com diferentes tipos de parâmetros em rotas, incluindo parâmetros de caminho, query parameters e corpo da requisição. Ao usar decoradores como `@Param`, `@Query` e `@Body`, você pode acessar e manipular facilmente esses parâmetros. Além disso, o uso de DTOs ajuda a definir e validar a estrutura dos dados que são enviados e recebidos pela aplicação.

## 05) TRABALHANDO COM OS DADOS DE PAYLOAD (HTTP POST)
No NestJS, os dados de payload são enviados no corpo da requisição e podem ser facilmente acessados e manipulados usando decoradores como `@Body`.

### 1. Configuração Inicial do Projeto
Antes de começar, certifique-se de ter um projeto NestJS configurado. Se ainda não tiver um, siga estas etapas:

```bash
nest new my-nest-project
cd my-nest-project
```

### 2. Criar um Módulo, Controlador e Serviço
Vamos criar um módulo chamado `cats`, junto com um controlador e um serviço.

```bash
nest generate module cats
nest generate controller cats
nest generate service cats
```

### 3. Definir DTOs (Data Transfer Objects)
DTOs são usados para definir a estrutura dos dados que serão transferidos. Vamos criar um DTO para o nosso exemplo.

#### Criar um DTO para Criar um Gato (`create-cat.dto.ts`)
```typescript
// src/cats/dto/create-cat.dto.ts
export class CreateCatDto {
  readonly name: string;
  readonly age: number;
  readonly breed: string;
}
```

### 4. Implementar o Serviço
O serviço conterá a lógica de negócio para gerenciar os gatos.

```typescript
// src/cats/cats.service.ts
import { Injectable } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';

export interface Cat {
  id: number;
  name: string;
  age: number;
  breed: string;
}

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];
  private idCounter = 1;

  create(createCatDto: CreateCatDto): Cat {
    const newCat = { id: this.idCounter++, ...createCatDto };
    this.cats.push(newCat);
    return newCat;
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

### 5. Implementar o Controlador
O controlador lida com as requisições HTTP e usa o serviço para realizar operações.

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }

  @Get()
  findAll() {
    return this.catsService.findAll();
  }
}
```

### 6. Atualizar o Módulo
Certifique-se de que o módulo `cats` está configurado corretamente para incluir o controlador e o serviço.

```typescript
// src/cats/cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

### 7. Registrar o Módulo Principal
Adicione o `CatsModule` ao módulo principal da aplicação.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

### 8. Testar a Aplicação
Inicie a aplicação:

```bash
npm run start:dev
```

Use uma ferramenta como Postman ou cURL para testar a rota POST.

#### Exemplo de Requisição POST
- **URL**: `http://localhost:3000/cats`
- **Método**: POST
- **Cabeçalhos**:
  - Content-Type: application/json
- **Body**:
  ```json
  {
    "name": "Whiskers",
    "age": 2,
    "breed": "Persian"
  }
  ```

#### Exemplo de Resposta
```json
{
  "id": 1,
  "name": "Whiskers",
  "age": 2,
  "breed": "Persian"
}
```

#### Exemplo de Requisição GET
- **URL**: `http://localhost:3000/cats`
- **Método**: GET

#### Exemplo de Resposta
```json
[
  {
    "id": 1,
    "name": "Whiskers",
    "age": 2,
    "breed": "Persian"
  }
]
```

### Validação de Dados
Para adicionar validação aos dados de entrada, você pode usar o `class-validator` e o `class-transformer`. Primeiro, instale os pacotes necessários:

```bash
npm install class-validator class-transformer
```

Atualize o DTO para adicionar validações:

```typescript
// src/cats/dto/create-cat.dto.ts
import { IsString, IsInt, IsPositive } from 'class-validator';

export class CreateCatDto {
  @IsString()
  readonly name: string;

  @IsInt()
  @IsPositive()
  readonly age: number;

  @IsString()
  readonly breed: string;
}
```

E habilite a validação global no seu arquivo `main.ts`:

```typescript
// src/main.ts
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

### Conclusão
Neste tutorial, você aprendeu a trabalhar com dados de payload em requisições HTTP POST no NestJS. Criamos um módulo, controlador, serviço e DTO para gerenciar os dados de um recurso `Cat`. Também adicionamos validação para garantir que os dados de entrada estejam no formato correto. Essa abordagem modular e organizada facilita a manutenção e escalabilidade da aplicação.

## 06) TRABALHANDO COM HTTP STATUS CODE
Trabalhar com códigos de status HTTP é uma prática essencial ao desenvolver APIs RESTful, pois esses códigos fornecem informações importantes sobre o resultado das requisições. No NestJS, você pode facilmente definir e manipular códigos de status HTTP usando decoradores e métodos do módulo `@nestjs/common`.

### Utilizando Códigos de Status HTTP
#### 1. Configurando os Controladores
Os controladores no NestJS podem usar o decorador `@HttpCode` para definir o código de status HTTP que será retornado. Você também pode usar o método `res.status()` do objeto de resposta se estiver usando a abordagem baseada em Express.

### Exemplos de Controladores com Diferentes Códigos de Status
#### 1. Utilizando `@HttpCode`
```typescript
import { Controller, Post, Body, HttpCode, HttpStatus } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  @HttpCode(HttpStatus.CREATED)
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```

No exemplo acima, o método `create` retorna um código de status 201 (Created) quando um novo gato é criado.

#### 2. Utilizando `res.status()`
Se você precisar de mais controle sobre a resposta HTTP, pode usar o método `res.status()` do objeto de resposta Express.

```typescript
import { Controller, Post, Body, Res } from '@nestjs/common';
import { Response } from 'express';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  create(@Body() createCatDto: CreateCatDto, @Res() res: Response) {
    const newCat = this.catsService.create(createCatDto);
    return res.status(201).json(newCat);
  }
}
```

Neste exemplo, o método `create` usa o objeto `res` para definir o código de status 201 e retornar o novo gato como JSON.

### Manipulando Erros com Exceções
Para lidar com erros, você pode lançar exceções que automaticamente configuram os códigos de status apropriados.

#### Exemplo com Exceções HTTP
```typescript
import { Controller, Get, Param, NotFoundException } from '@nestjs/common';
import { CatsService } from './cats.service';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Get(':id')
  findOne(@Param('id') id: string) {
    const cat = this.catsService.findOne(id);
    if (!cat) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    return cat;
  }
}
```

Neste exemplo, se o gato com o ID fornecido não for encontrado, uma exceção `NotFoundException` será lançada, retornando automaticamente um código de status 404 (Not Found).

### Usando Interceptors para Códigos de Status Personalizados
Interceptors podem ser usados para modificar a resposta antes de enviá-la ao cliente.

#### Exemplo de Interceptor
```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable()
export class TransformInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      map(data => ({
        statusCode: context.switchToHttp().getResponse().statusCode,
        data,
      })),
    );
  }
}
```

### Aplicando o Interceptor Globalmente
Para aplicar o interceptor globalmente, configure-o no arquivo `main.ts`.

```typescript
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { TransformInterceptor } from './transform.interceptor';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalInterceptors(new TransformInterceptor());
  await app.listen(3000);
}
bootstrap();
```

### Lista de Códigos de Status HTTP Comuns
- **200 OK**: Requisição bem-sucedida.
- **201 Created**: Recurso criado com sucesso.
- **204 No Content**: Requisição bem-sucedida, mas sem conteúdo a ser retornado.
- **400 Bad Request**: Requisição inválida.
- **401 Unauthorized**: Falta de autenticação.
- **403 Forbidden**: Acesso negado.
- **404 Not Found**: Recurso não encontrado.
- **500 Internal Server Error**: Erro interno no servidor.

### Resumo
Trabalhar com códigos de status HTTP no NestJS é direto e intuitivo. Usando decoradores como `@HttpCode`, métodos do objeto de resposta, e exceções integradas, você pode controlar como sua API responde às requisições de maneira eficaz. Interceptors oferecem uma maneira poderosa de modificar ou adicionar lógica antes de enviar a resposta ao cliente. Com estas práticas, sua API NestJS pode fornecer feedback apropriado aos clientes, melhorando a clareza e a usabilidade.

## 07) REQUISIÇÕES HTTP PATCH, PUT E DELETE
No desenvolvimento de APIs RESTful, é comum usar diferentes métodos HTTP para realizar operações de criação, leitura, atualização e exclusão de recursos. No NestJS, você pode facilmente implementar esses métodos usando decoradores e serviços. Vamos explorar como implementar as requisições HTTP PATCH, PUT e DELETE.

### Implementando Requisições PATCH, PUT e DELETE
#### 1. Configuração Inicial
Certifique-se de ter um projeto NestJS configurado e um módulo, controlador e serviço criados.

```bash
nest new my-nest-project
cd my-nest-project
nest generate module cats
nest generate controller cats
nest generate service cats
```

### 2. Definir DTOs
Para as operações de atualização e criação, você precisará de DTOs (Data Transfer Objects).

```typescript
// src/cats/dto/create-cat.dto.ts
export class CreateCatDto {
  readonly name: string;
  readonly age: number;
  readonly breed: string;
}

// src/cats/dto/update-cat.dto.ts
export class UpdateCatDto {
  readonly name?: string;
  readonly age?: number;
  readonly breed?: string;
}
```

### 3. Implementar o Serviço
O serviço conterá a lógica de negócio para gerenciar os gatos.

```typescript
// src/cats/cats.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { UpdateCatDto } from './dto/update-cat.dto';

export interface Cat {
  id: number;
  name: string;
  age: number;
  breed: string;
}

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];
  private idCounter = 1;

  create(createCatDto: CreateCatDto): Cat {
    const newCat = { id: this.idCounter++, ...createCatDto };
    this.cats.push(newCat);
    return newCat;
  }

  findAll(): Cat[] {
    return this.cats;
  }

  findOne(id: string): Cat {
    const cat = this.cats.find(cat => cat.id === parseInt(id));
    if (!cat) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    return cat;
  }

  update(id: string, updateCatDto: UpdateCatDto): Cat {
    const catIndex = this.cats.findIndex(cat => cat.id === parseInt(id));
    if (catIndex === -1) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    const updatedCat = { ...this.cats[catIndex], ...updateCatDto };
    this.cats[catIndex] = updatedCat;
    return updatedCat;
  }

  remove(id: string): void {
    const catIndex = this.cats.findIndex(cat => cat.id === parseInt(id));
    if (catIndex === -1) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    this.cats.splice(catIndex, 1);
  }
}
```

### 4. Implementar o Controlador
O controlador lidará com as requisições HTTP e usará o serviço para realizar as operações.

```typescript
// src/cats/cats.controller.ts
import { Controller, Get, Post, Put, Patch, Delete, Param, Body } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';
import { UpdateCatDto } from './dto/update-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }

  @Get()
  findAll() {
    return this.catsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.catsService.findOne(id);
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return this.catsService.update(id, updateCatDto);
  }

  @Patch(':id')
  partialUpdate(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return this.catsService.update(id, updateCatDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    this.catsService.remove(id);
    return { message: `Cat with ID ${id} deleted successfully` };
  }
}
```

### 5. Atualizar o Módulo
Certifique-se de que o módulo `cats` está configurado corretamente para incluir o controlador e o serviço.

```typescript
// src/cats/cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

### 6. Registrar o Módulo Principal
Adicione o `CatsModule` ao módulo principal da aplicação.

```typescript
// src/app.module.ts
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

### Testando a Aplicação
Inicie a aplicação:

```bash
npm run start:dev
```

Use uma ferramenta como Postman ou cURL para testar as diferentes rotas HTTP.

#### Exemplo de Requisição PUT
- **URL**: `http://localhost:3000/cats/1`
- **Método**: PUT
- **Cabeçalhos**:
  - Content-Type: application/json
- **Body**:
  ```json
  {
    "name": "Whiskers",
    "age": 3,
    "breed": "Siamese"
  }
  ```

#### Exemplo de Requisição PATCH
- **URL**: `http://localhost:3000/cats/1`
- **Método**: PATCH
- **Cabeçalhos**:
  - Content-Type: application/json
- **Body**:
  ```json
  {
    "age": 4
  }
  ```

#### Exemplo de Requisição DELETE
- **URL**: `http://localhost:3000/cats/1`
- **Método**: DELETE

### Resumo
Neste tutorial, você aprendeu a implementar as requisições HTTP PUT, PATCH e DELETE no NestJS. Estas operações são essenciais para criar APIs RESTful completas que permitem a manipulação de recursos de maneira eficiente. Utilizando decoradores e serviços, você pode facilmente lidar com diferentes tipos de requisições e manipular os dados de forma apropriada. 

## 08) CONHECENDO E CRIANDO UM SERVIÇO COM NESTJS
Um serviço no NestJS é uma classe que pode conter lógica de negócios, reutilizável em toda a aplicação. Essa lógica pode incluir operações de acesso a banco de dados, chamadas a APIs externas, manipulação de dados e muito mais. Vamos explorar como conhecer e criar um serviço no NestJS.

### Conhecendo Serviços no NestJS
#### Benefícios dos Serviços
- **Reutilização de Código**: A lógica de negócios pode ser centralizada em serviços e reutilizada em vários lugares da aplicação.
- **Testabilidade**: Serviços podem ser facilmente testados isoladamente, tornando a aplicação mais testável e robusta.
- **Separação de Responsabilidades**: Os serviços ajudam a manter a separação de responsabilidades, tornando o código mais organizado e fácil de manter.
- **Injeção de Dependência**: O NestJS oferece um sistema de injeção de dependência embutido, permitindo que os serviços sejam facilmente injetados em outros componentes, como controladores e módulos.

### Criando um Serviço no NestJS
Vamos criar um serviço simples para gerenciar gatos em nossa aplicação.

#### 1. Gerar o Serviço
Use o CLI do NestJS para gerar um novo serviço:

```bash
nest generate service cats
```

#### 2. Implementar a Lógica de Negócios
No arquivo `cats.service.ts`, implemente a lógica de negócios para gerenciar gatos. Isso pode incluir operações como criar, listar, atualizar e excluir gatos.

```typescript
// src/cats/cats.service.ts
import { Injectable } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';

export interface Cat {
  id: number;
  name: string;
  age: number;
  breed: string;
}

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];
  private idCounter = 1;

  create(createCatDto: CreateCatDto): Cat {
    const newCat = { id: this.idCounter++, ...createCatDto };
    this.cats.push(newCat);
    return newCat;
  }

  findAll(): Cat[] {
    return this.cats;
  }

  findOne(id: string): Cat {
    return this.cats.find(cat => cat.id === parseInt(id));
  }

  update(id: string, updateCatDto: Partial<CreateCatDto>): Cat {
    const catIndex = this.cats.findIndex(cat => cat.id === parseInt(id));
    if (catIndex === -1) {
      return null; // or throw an exception
    }
    const updatedCat = { ...this.cats[catIndex], ...updateCatDto };
    this.cats[catIndex] = updatedCat;
    return updatedCat;
  }

  remove(id: string): void {
    const catIndex = this.cats.findIndex(cat => cat.id === parseInt(id));
    if (catIndex === -1) {
      return; // or throw an exception
    }
    this.cats.splice(catIndex, 1);
  }
}
```

#### 3. Utilizando o Serviço
Você pode injetar e usar o serviço em qualquer outro componente do NestJS, como controladores, interceptores ou outros serviços.

```typescript
// src/app.controller.ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { CatsService, Cat } from './cats/cats.service';
import { CreateCatDto } from './cats/dto/create-cat.dto';

@Controller()
export class AppController {
  constructor(private readonly catsService: CatsService) {}

  @Post('cats')
  create(@Body() createCatDto: CreateCatDto): Cat {
    return this.catsService.create(createCatDto);
  }

  @Get('cats')
  findAll(): Cat[] {
    return this.catsService.findAll();
  }
}
```

### Conclusão
Os serviços são componentes fundamentais do NestJS, permitindo a organização e reutilização de lógica de negócios em toda a aplicação. Ao criar serviços, você pode manter uma arquitetura limpa e modular, facilitando a manutenção e o teste da sua aplicação. 

## 09) USANDO OS MÉTODOS DO SERVIÇO NO CONTROLLER
Para utilizar os métodos de um serviço em um controlador no NestJS, você precisa injetar o serviço no controlador e, em seguida, chamar os métodos desejados do serviço. Vou mostrar um exemplo de como fazer isso:

Suponha que você tenha um serviço `CatsService` que contém métodos para criar, listar e encontrar gatos, e um controlador `CatsController` que deseja utilizar esses métodos.

### 1. Criar o Serviço `CatsService`
Primeiro, vamos criar um serviço para lidar com operações relacionadas a gatos:

```typescript
// cats.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  private readonly cats: string[] = [];

  create(cat: string) {
    this.cats.push(cat);
  }

  findAll() {
    return this.cats;
  }

  findOne(id: number) {
    return this.cats[id];
  }
}
```

### 2. Criar o Controlador `CatsController`
Agora, vamos criar um controlador para manipular requisições relacionadas a gatos e utilizar os métodos do serviço `CatsService`:

```typescript
// cats.controller.ts
import { Controller, Get, Post, Param, Body } from '@nestjs/common';
import { CatsService } from './cats.service';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  create(@Body() cat: string) {
    this.catsService.create(cat);
  }

  @Get()
  findAll() {
    return this.catsService.findAll();
  }

  @Get(':id')
  findOne(@Param('id') id: number) {
    return this.catsService.findOne(id);
  }
}
```

### 3. Registrar o Serviço e o Controlador no Módulo
Agora, registre o serviço e o controlador no módulo da sua aplicação:

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```

Agora você pode utilizar os métodos do serviço `CatsService` no controlador `CatsController`. Por exemplo, ao fazer uma requisição POST para criar um novo gato, o método `create` do serviço é chamado. Ao fazer uma requisição GET para obter todos os gatos, o método `findAll` do serviço é chamado. E ao fazer uma requisição GET para obter um gato específico por ID, o método `findOne` do serviço é chamado.

Este é apenas um exemplo básico de como utilizar os métodos de um serviço em um controlador no NestJS. Você pode estender esse padrão para realizar operações mais complexas e fornecer funcionalidades mais avançadas na sua aplicação. 

## 10) CONHECENDO OS MÉTODOS PARA TRATAMENTO DE ERROS
No NestJS, existem várias maneiras de lidar com erros em sua aplicação, seja para erros internos, como exceções não tratadas, ou para erros específicos que você deseja controlar. Vou apresentar alguns métodos comuns para tratamento de erros no NestJS:

### 1. Exceções Integradas
O NestJS fornece um conjunto de exceções integradas que você pode lançar em seu código para representar diferentes tipos de erros. Alguns exemplos incluem `NotFoundException`, `BadRequestException`, `UnauthorizedException`, `ForbiddenException`, `InternalServerErrorException`, entre outros.

```typescript
import { Injectable, NotFoundException } from '@nestjs/common';

@Injectable()
export class CatsService {
  findOne(id: string) {
    const cat = this.cats.find(cat => cat.id === parseInt(id));
    if (!cat) {
      throw new NotFoundException(`Cat with ID ${id} not found`);
    }
    return cat;
  }
}
```

### 2. Filtros de Exceção
Você pode criar filtros de exceção para capturar exceções específicas e fornecer uma resposta personalizada ao cliente.

```typescript
import { ExceptionFilter, Catch, ArgumentsHost, HttpException, HttpStatus } from '@nestjs/common';
import { Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        message: exception.message,
      });
  }
}
```

### 3. Filtros de Exceção de Escopo Global
Você pode configurar um filtro de exceção de escopo global para lidar com todas as exceções lançadas por sua aplicação.

```typescript
import { Catch, ExceptionFilter, HttpException, ArgumentsHost } from '@nestjs/common';
import { Response } from 'express';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = exception instanceof HttpException ? exception.getStatus() : 500;

    response.status(status).json({
      statusCode: status,
      message: 'Internal server error',
    });
  }
}
```

### 4. Interceptores
Você pode usar interceptores para interceptar solicitações antes e depois de serem manipuladas pelos controladores. Isso pode ser útil para adicionar lógica de tratamento de erros em todas as solicitações.

### 5. Middlewares
Os middlewares podem ser usados para manipular solicitações antes que elas alcancem os controladores. Eles podem ser usados para validar entradas, tratar erros, etc.

### 6. Extensão de Filtros de Exceção
Você pode estender os filtros de exceção fornecidos pelo NestJS para criar filtros personalizados que atendam às suas necessidades específicas de tratamento de erros.

Estas são apenas algumas das maneiras comuns de lidar com erros no NestJS. A escolha do método depende das necessidades específicas da sua aplicação e do tipo de erro que você está lidando.

## 11) TRABALHANDO COM MÓDULOS NO NESTJS
Trabalhar com módulos no NestJS é uma prática fundamental para organizar e estruturar sua aplicação de forma modular. Os módulos ajudam a encapsular componentes relacionados, como controladores, serviços e provedores, e fornecem um contexto claro para a injeção de dependências. Vou explicar como trabalhar com módulos no NestJS:

### 1. Criar um Módulo
Para criar um módulo no NestJS, você pode usar o CLI ou criar manualmente os arquivos necessários.

#### Usando o CLI:
```bash
nest generate module nome-do-modulo
```

#### Criando Manualmente:
Crie um arquivo `.module.ts` e defina um decorador de módulo dentro dele.

```typescript
// nome-do-modulo.module.ts
import { Module } from '@nestjs/common';

@Module({})
export class NomeDoModuloModule {}
```

### 2. Definir Componentes Relacionados
Dentro do seu módulo, você pode definir componentes relacionados, como controladores, provedores e serviços.

#### Controladores:
```typescript
// cats.controller.ts
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string[] {
    return ['Cat1', 'Cat2', 'Cat3'];
  }
}
```

#### Serviços:
```typescript
// cats.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  findAll(): string[] {
    return ['Cat1', 'Cat2', 'Cat3'];
  }
}
```

### 3. Importar e Exportar Módulos
Você pode importar módulos em outros módulos para usar seus componentes e funcionalidades.

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { NomeDoModuloModule } from './nome-do-modulo.module';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  imports: [NomeDoModuloModule],
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```

Você também pode exportar componentes do seu módulo para torná-los disponíveis para outros módulos.

```typescript
// nome-do-modulo.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService], // Exportando o serviço para outros módulos
})
export class NomeDoModuloModule {}
```

### 4. Injeção de Dependência
Os componentes dentro de um módulo podem ser injetados em outros componentes usando a injeção de dependência.

```typescript
// cats.controller.ts
import { Controller, Get } from '@nestjs/common';
import { CatsService } from './cats.service'; // Importando o serviço

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {} // Injetando o serviço

  @Get()
  findAll(): string[] {
    return this.catsService.findAll(); // Usando o serviço
  }
}
```

### 5. Módulos de Funcionalidade
Você pode criar módulos específicos para diferentes funcionalidades da sua aplicação, tornando-a mais modular e fácil de manter.

### 6. Módulos Globais
Além dos módulos regulares, você também pode criar módulos globais que são carregados em toda a aplicação.

Estas são as noções básicas de como trabalhar com módulos no NestJS. Eles são uma parte fundamental da arquitetura de uma aplicação NestJS e ajudam a manter o código organizado, reutilizável e fácil de entender.

## 12) CRIANDO DTO - DATA TRANSFER OBJECT
Os DTOs (Data Transfer Objects) são objetos usados para transferir dados entre partes de um aplicativo. Eles são particularmente úteis em aplicativos RESTful para definir a estrutura dos dados que serão enviados ou recebidos por meio de solicitações HTTP. Vou mostrar como criar um DTO no NestJS:

### 1. Criar um DTO
Crie uma classe para o seu DTO. Por exemplo, vamos criar um DTO para representar os dados de um usuário.

```typescript
// user.dto.ts
export class UserDto {
  readonly name: string;
  readonly email: string;
  readonly age: number;
}
```

Neste exemplo, o DTO `UserDto` possui três propriedades: `name`, `email` e `age`. O `readonly` indica que essas propriedades só podem ser definidas uma vez e não podem ser modificadas posteriormente.

### 2. Utilizar o DTO em um Controlador
Você pode usar o DTO em um controlador para validar e manipular os dados recebidos em uma solicitação.

```typescript
// users.controller.ts
import { Controller, Post, Body } from '@nestjs/common';
import { UserDto } from './user.dto';

@Controller('users')
export class UsersController {
  @Post()
  createUser(@Body() userDto: UserDto) {
    // Aqui você pode usar o userDto para criar um novo usuário ou realizar outras operações
    console.log('Received user data:', userDto);
    return 'User created successfully';
  }
}
```

No exemplo acima, o controlador `UsersController` possui um método `createUser` que recebe um objeto `userDto` como corpo da solicitação. Este objeto será validado de acordo com a estrutura definida no DTO `UserDto`.

### 3. Enviar Dados Usando o DTO
Você pode enviar dados usando o DTO ao fazer uma solicitação HTTP para o endpoint correspondente.

Exemplo de solicitação HTTP POST usando cURL:

```bash
curl -X POST http://localhost:3000/users \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "age": 30
  }'
```

### 4. Validar Dados do DT
O NestJS fornece recursos integrados para validar automaticamente os dados do DTO com base em decoradores de classe.

Por exemplo, você pode adicionar decoradores de validação do `class-validator` ao DTO para definir regras de validação, como tornar um campo obrigatório, definir um comprimento mínimo ou máximo, validar um endereço de e-mail, etc.

```typescript
// user.dto.ts
import { IsNotEmpty, IsEmail, IsInt, Min } from 'class-validator';

export class UserDto {
  @IsNotEmpty()
  readonly name: string;

  @IsNotEmpty()
  @IsEmail()
  readonly email: string;

  @IsInt()
  @Min(18)
  readonly age: number;
}
```

### Conclusão
Os DTOs são uma parte essencial do desenvolvimento de aplicativos RESTful no NestJS. Eles ajudam a definir a estrutura dos dados que são transmitidos entre o cliente e o servidor, fornecendo uma maneira clara e estruturada de lidar com os dados da solicitação. 

## 13-14) VALIDAÇÃO DE DADOS - VALIDATIONPIPE  
No NestJS, o `ValidationPipe` é uma ferramenta poderosa para validar os dados de entrada de uma requisição HTTP com base nas regras definidas nos DTOs utilizando o pacote `class-validator`. Vou explicar como você pode utilizar o `ValidationPipe` para validar dados em suas aplicações.

### 1. Instalação dos Pacotes Necessários
Certifique-se de que os pacotes `@nestjs/common` e `class-validator` estejam instalados no seu projeto. Se não estiverem instalados, você pode instalá-los via npm ou yarn:

```bash
npm install --save @nestjs/common class-validator
```

ou

```bash
yarn add @nestjs/common class-validator
```

### 2. Importação e Configuração do `ValidationPipe`
Você precisa importar e configurar o `ValidationPipe` na aplicação para que ele possa ser utilizado. Isso geralmente é feito no módulo principal (`AppModule`).

```typescript
// app.module.ts
import { Module, ValidationPipe } from '@nestjs/common';
import { APP_PIPE } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
export class AppModule {}
```

Ao definir o `APP_PIPE`, você está dizendo ao NestJS para usar o `ValidationPipe` globalmente em toda a sua aplicação.

### 3. Uso do `ValidationPipe` em Controladores
Você pode aplicar o `ValidationPipe` em controladores ou métodos de controladores para validar os dados de entrada.

```typescript
// cats.controller.ts
import { Controller, Post, Body } from '@nestjs/common';
import { CatsService } from './cats.service';
import { CreateCatDto } from './dto/create-cat.dto';

@Controller('cats')
export class CatsController {
  constructor(private readonly catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    return this.catsService.create(createCatDto);
  }
}
```

### 4. Criando um DTO com Regras de Validação
No DTO utilizado no controlador, você pode adicionar decorações do `class-validator` para definir regras de validação.

```typescript
// create-cat.dto.ts
import { IsString, IsInt, IsNotEmpty } from 'class-validator';

export class CreateCatDto {
  @IsNotEmpty()
  @IsString()
  readonly name: string;

  @IsNotEmpty()
  @IsInt()
  readonly age: number;

  @IsNotEmpty()
  @IsString()
  readonly breed: string;
}
```

### 5. Testando a Validação
Agora, quando você enviar uma solicitação para o endpoint correspondente, o `ValidationPipe` será automaticamente aplicado e os dados serão validados de acordo com as regras definidas no DTO. Se os dados não estiverem em conformidade com as regras de validação, uma exceção `BadRequestException` será lançada automaticamente.

```bash
curl -X POST http://localhost:3000/cats \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Whiskers",
    "age": "3", // Deveria ser um número
    "breed": "Siamese"
  }'
```

### 6. Personalizando Mensagens de Erro
Você também pode personalizar as mensagens de erro que são retornadas ao cliente ao usar o `ValidationPipe`. Isso é feito através da configuração de opções no `ValidationPipe`:

```typescript
import { ValidationPipe } from '@nestjs/common';

@Module({
  providers: [
    {
      provide: APP_PIPE,
      useValue: new ValidationPipe({
        transform: true,
        whitelist: true,
        forbidNonWhitelisted: true,
        transformOptions: { enableImplicitConversion: true },
        validationError: { target: false, value: false },
        exceptionFactory: (errors) => new BadRequestException(errors),
      }),
    },
  ],
})
export class AppModule {}
```

### Conclusão
O `ValidationPipe` é uma ferramenta poderosa para validar os dados de entrada em aplicações NestJS, garantindo que os dados estejam corretos antes de serem processados. Ao combinar o `ValidationPipe` com os DTOs e as decorações do `class-validator`, você pode criar uma camada robusta de validação de dados em sua aplicação. 
