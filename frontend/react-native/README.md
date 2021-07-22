# Linters para React 

Recomendamos fortemente o uso do **@eduzz/eslint-config-houston** 
basta seguir o [tutorial aqui](https://eduzz.github.io/houston/eslint)
ou a [documentação aqui](./default.md).

E caso precisar uma configuração standalone veja a [documentação aqui](./standalone.md).  
**LEMBRAMOS QUE É POSSÍVEL UTILIZAR O @eduzz/eslint-config-houston SOZINHO** 

## Técnologias envolvidas

* Eslint
* Prettier
* Typescript
* React
* React Native

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

## Typescript

Essa são essas configurações que utilizamos em projetos de react:

```jsonc
{
  "compilerOptions": {
    "outDir": "build/dist",
    "module": "esnext",
    "target": "es5",
    "experimentalDecorators": true,
    "noFallthroughCasesInSwitch": true,
    "lib": [
      "es2017",
      "dom"
    ],
    "sourceMap": true,
    "allowJs": true,
    "jsx": "preverse",
    "moduleResolution": "node",
    "rootDir": "src",
    "forceConsistentCasingInFileNames": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitAny": true,
    "suppressImplicitAnyIndexErrors": true,
    "allowSyntheticDefaultImports": true,
    "noUnusedLocals": true,
    "importHelpers": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "strict": false,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "baseUrl": "./src"
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx"
  ],
  "exclude": [
    "node_modules",
    "build",
    "scripts",
    "acceptance-tests",
    "webpack",
    "jest",
    "src/setupTests.ts",
    "analyze.js"
  ]
}
```

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
    "lint": "yarn eslint \"./src/**/*.ts\" \"./src/**/*.tsx\"",
    "pre-commit": "yarn concurrently -r \"yarn lint\" \"yarn tsc --noEmit\""
  }
}
```

## React 17 e JSX

Para utilizar a nova versão do React com jsx-runtime basta seguir o [tutorial do blog](https://pt-br.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html),
mas resumidamente é: 

```bash
# Removendo Imports React não Utilizadas
npx react-codemod update-react-imports
```

tsconfig.json
```jsonc
{
  //... suas configurações
  "compilerOptions": {
    "jsx": "react-jsx" //Troque esse configuração
  }
}
```

.eslintrc
```jsonc
{
  "extends": ["@eduzz/eslint-config-houston"],
  "rules": {
    //Adicione essas rules
    "react/jsx-uses-react": "off",
    "react/react-in-jsx-scope": "off"
  }
}
```