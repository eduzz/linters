# Default

Configuração padrão que utiliza o Houston como base do eslint.

## Dependências

```bash
yarn add --dev @eduzz/eslint-config-houston @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-prettier eslint-plugin-eslint-plugin eslint-plugin-import eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-unused-imports prettier
# ou
npm install --save-dev @eduzz/eslint-config-houston @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-prettier eslint-plugin-eslint-plugin eslint-plugin-import eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-unused-imports prettier
```

## .eslintrc

```json
{
  "extends": ["@eduzz/eslint-config-houston/native"],
  "rules": {
    // Caso queira sobescrever alguma regra
  }
}
```

## .prettierrc.js

Necessário ser .js pois o prettier não tem esquema de extends

```js
module.exports = {
  ...require('@eduzz/eslint-config-houston/.prettierrc')
  // Caso queira sobescrever alguma regra
};
```