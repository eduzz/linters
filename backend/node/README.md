# Linters para Node 

Recomendamos fortemente o uso do **@eduzz/eslint-config-houston** basta seguir o 
[tutorial aqui](https://eduzz.github.io/houston/eslint) ou a [documentação aqui](./default.md).

E caso precisar uma configuração standalone veja a [documentação aqui](./standalone.md).  
**LEMBRAMOS QUE É POSSÍVEL UTILIZAR O @eduzz/eslint-config-houston SOZINHO E NO BACK COM NODE** 

## Técnologias envolvidas

* Eslint
* Prettier
* Typescript

## Framework

Utilize o [NestJS](https://nestjs.com/) como framework padrão, tanto para API como para Microservices.

## VSCode

* Adicione a extensão do [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint).
  **SUGERIMOS DESISTALAR OU DESATIVAR A EXTENSÃO DO PRETTIER POIS O ESLINT QUE APLICARÁ O PRETTIER.**

* Crie/Adicione no .vscode/settings.json (não na suas configurações, pois assim ficará no projeto e o time já terá acesso):
```jsonc
{
  //... suas configurações
  "editor.codeActionsOnSave": {
    "source.organizeImports": false,
    "source.fixAll.eslint": true
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact"
  ],
  "editor.formatOnPaste": false,
  "editor.formatOnSave": false,
  "editor.formatOnType": false,
  "editor.tabSize": 2
}
```

Adicione as configurações abaixo para poder debuggar no VSCode:

```jsonc
//.vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [{
    "type": "node",
    "request": "attach",
    "name": "Debug",
    "address": "127.0.0.1",
    "port": 9229,
    "protocol": "inspector",
    "restart": true,
    "sourceMaps": true,
    "sourceMapPathOverrides": {
      "/server/*": "${workspaceRoot}/*"
    },
    "skipFiles": [
      "${workspaceRoot}/node_modules/**/*.js",
      "<node_internals>/**/*.js"
    ]
  }]
}
```

## Docker

(Veja aqui)[./docker.md] a documentações do docker.

## Typescript

Essa são essas configurações que utilizamos em projetos com node:

```bash
yarn add --dev ts-node-dev
```

```jsonc
//tsconfig.json
{
  "compilerOptions": {
    "target": "ES2019",
    "module": "commonjs",
    "moduleResolution": "node",
    "removeComments": true,
    "esModuleInterop": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "inlineSourceMap": true,
    "noImplicitAny": true,
    "noEmitOnError": true,
    "noUnusedLocals": true,
    "importHelpers": true,
    "inlineSources": true,
    "skipLibCheck": true,
    "alwaysStrict": true,
    "outDir": "./bin",
    "baseUrl": "./src"
  },
  "include": [
    "./src/**/*.ts"
  ]
}

//tsconfig.dev.json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "sourceMap": true,
    "inlineSourceMap": false,
    "inlineSources": false,
    "assumeChangesOnlyAffectDirectDependencies": true
  },
  "exclude": [
    "./src/**/*.spec.ts"
  ]
}

//tsconfig.build.json
{
  "extends": "./tsconfig.json",
  "exclude": ["node_modules", "dist", "test", "**/*spec.ts"]
}
```

```jsonc
// package.json
{
  "scripts": {
    "build": "tsc -p tsconfig.build.json",
    "dev": "yarn ts-node-dev  --inspect=0.0.0.0:9229 --respawn --no-notify --poll --no-deps --ignore-watch node_modules --transpile-only --project tsconfig.dev.json ./src/index.ts",
  }
}
```

## Mapeando os modulos

Para mapear os imports para **~/...**. Ex. from '../../../module/service' -> '~/module/service'

* é possível alterar o simbolo mas este já esta configurado no eslint para ordernar corretamente os imports.

```bash
yarn add module-alias source-map-support
```

```jsonc
//tsconfig.json
{
  "compilerOptions": {
    // suas configurações
    "baseUrl": ".", // Troque o baseUrl para impedir o import absoluto
    "paths": {
      "~/*": ["./src/*"] // Adicione o path apontando para o src 
    }
  }
}
```

Crie um arquivo chamado *global.ts* e import ele acima de tudo no seu *index.ts*.

```ts
// global.ts
import moduleAlias from 'module-alias';

moduleAlias.addAlias('~', __dirname);

// eslint-disable-next-line @typescript-eslint/no-require-imports
require('source-map-support').install({ hookRequire: true });

// index.ts
import './global';

// seu codigo
```

Pronto!

## Husky e Package.json

Adicione o husky no seu projeto para poder impedir commits que possam quebrar seu build, ele irá verificar o 
typescript e o eslint:

```bash
yarn add --dev husky concurrently

yarn husky add .husky/post-merge "yarn install"
yarn husky add .husky/pre-commit "yarn pre-commit"
```

```jsonc
// package.json
{
  "scripts": {
    "prepare": "yarn husky install",
    "lint": "yarn eslint \"./src/**/*.ts\"",
    "pre-commit": "yarn concurrently -r \"yarn lint\" \"yarn tsc --noEmit\""
  }
}
```