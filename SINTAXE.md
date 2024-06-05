# SINTAXE
## 1. CRIANDO UM MÓDULO:
```typescript
// app.module.ts
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

  - O `AppModule` importa os controladores e serviços que são necessários para a aplicação. `imports` são outros módulos que este módulo depende.

## 2. CRIANDO UM CONTROLADOR:
```typescript
// app.controller.ts
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

  - O `AppController` define um ponto de entrada HTTP. No exemplo, ele lida com requisições GET na raiz (`@Get()`).

## 3. CRIANDO UM SERVIÇO:
```typescript
// app.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class AppService {
  getHello(): string {
    return 'Hello World!';
  }
}
```

  - O `AppService` encapsula a lógica de negócio. Ele pode ser injetado em controladores ou outros serviços usando a injeção de dependências do NestJS.

## 4. ROTAS COM PARÂMETROS:
```typescript
// app.controller.ts
import { Controller, Get, Param } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get(':id')
  findOne(@Param('id') id: string): string {
    return `This action returns a cat with id ${id}`;
  }
}
```

## 5. USANDO DTOS (DATA TRANSFER OBJECTS):
```typescript
// create-cat.dto.ts
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}

// app.controller.ts
import { Controller, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './create-cat.dto';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Body() createCatDto: CreateCatDto): string {
    return `This action adds a new cat: ${createCatDto.name}`;
  }
}
```

## CONCLUSÃO:
O NestJS oferece uma estrutura robusta para construir aplicações escaláveis com Node.js e TypeScript. Entender os conceitos de módulos, controladores e serviços é fundamental para desenvolver com NestJS. 