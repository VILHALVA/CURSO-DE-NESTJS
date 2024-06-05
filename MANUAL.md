# MANUAL
## 1. INSTALAR O NESTJS CLI:
Primeiro, você precisa ter o Node.js instalado no seu sistema. Você pode baixar o Node.js [aqui](https://nodejs.org/).

Depois de instalar o Node.js, instale o NestJS CLI globalmente usando npm:

```bash
npm install -g @nestjs/cli
```

## 2. CRIAR UM NOVO PROJETO:
Use o NestJS CLI para criar um novo projeto:

```bash
nest new project-name
```

Substitua `project-name` pelo nome do seu projeto. O CLI perguntará qual gerenciador de pacotes você deseja usar (npm ou yarn). Escolha o que preferir.

## 3. ESTRUTURA INICIAL DE DIRETÓRIOS:
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

- **dist/**: Diretório onde os arquivos compilados são armazenados.
- **node_modules/**: Diretório onde as dependências do projeto são instaladas.
- **src/**: Diretório principal do código-fonte da aplicação.
  - **app.controller.spec.ts**: Arquivo de teste unitário para o controlador.
  - **app.controller.ts**: Arquivo do controlador principal.
  - **app.module.ts**: Módulo principal da aplicação.
  - **app.service.ts**: Serviço principal da aplicação.
  - **main.ts**: Arquivo de entrada da aplicação.
- **test/**: Diretório para testes end-to-end.
- **.eslintrc.js**: Configuração do ESLint.
- **.gitignore**: Arquivo para ignorar certos arquivos/diretórios no controle de versão.
- **.prettierrc**: Configuração do Prettier.
- **nest-cli.json**: Configuração do Nest CLI.
- **package.json**: Arquivo de configuração do projeto npm.
- **README.md**: Arquivo de descrição do projeto.
- **tsconfig.build.json**: Configuração de compilação do TypeScript.
- **tsconfig.json**: Configuração principal do TypeScript.

## 4. NAVEGAR ATÉ O DIRETÓRIO DO PROJETO:
Após a criação do projeto, navegue até o diretório do projeto:

```bash
cd project-name
```

## 5. INICIAR A APLICAÇÃO:
Para iniciar a aplicação, use o comando:

```bash
npm run start
```

Para iniciar a aplicação em modo de desenvolvimento (com hot-reload), use:

```bash
npm run start:dev
```

## 6. INSTALAR DEPENDÊNCIAS:
Para adicionar uma nova dependência ao seu projeto, use npm ou yarn. Por exemplo, para adicionar o pacote `axios`:

```bash
npm install axios
```

Ou, se estiver usando yarn:

```bash
yarn add axios
```

## 7. EXEMPLO DE ADIÇÃO DE NOVO MÓDULO, CONTROLADOR E SERVIÇO:
### CRIAR UM NOVO MÓDULO:
Para criar um novo módulo, você pode usar o NestJS CLI:

```bash
nest generate module cats
```

### CRIAR UM NOVO CONTROLADOR:
Para criar um novo controlador no módulo `cats`:

```bash
nest generate controller cats
```

### CRIAR UM NOVO SERVIÇO:
Para criar um novo serviço no módulo `cats`:

```bash
nest generate service cats
```

## 8. ESTRUTURA APÓS ADIÇÃO DE NOVO MÓDULO:
A estrutura de diretórios ficará assim após a adição do módulo, controlador e serviço:

```
project-name/
├── dist/
├── node_modules/
├── src/
│   ├── cats/
│   │   ├── cats.controller.spec.ts
│   │   ├── cats.controller.ts
│   │   ├── cats.module.ts
│   │   ├── cats.service.spec.ts
│   │   ├── cats.service.ts
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

Agora, você tem um módulo, controlador e serviço configurados para `cats`. Você pode começar a adicionar lógica de negócio no serviço e rotas no controlador.

