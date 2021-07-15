# Standalone

Essas são as configurações de base que o Houston implementa.

## Dependências

```bash
yarn add --dev @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-prettier eslint-plugin-eslint-plugin eslint-plugin-import eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-unused-imports prettier
# ou
npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint-config-prettier eslint-plugin-eslint-plugin eslint-plugin-import eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-unused-imports prettier
```

## .eslintrc

```json
{
  "settings": {
    "react": {
      "version": "detect"
    },
    "import/internal-regex": "(^@eduzz|react)"
  },
  "plugins": ["react", "react-native", "react-hooks", "prettier", "eslint-plugin-unused-imports"],
  "extends": [
    "plugin:react/recommended", 
    "plugin:prettier/recommended",
    "plugin:import/recommended",
    "plugin:import/typescript"
  ],
  "parserOptions": {
    "ecmaVersion": 10,
    "sourceType": "module",
    "ecmaFeatures": { "modules": true, "jsx": true }
  },
  "env": {
    "react-native/react-native": true
  },
  "rules": {
    "no-restricted-globals": ["error"],
    "react/display-name": ["off"],
    "react/prop-types": ["off"],
    "no-restricted-imports": ["error", "date-fns", "mdi-react", "lodash", "@material-ui/core", "@material-ui/styles"],
    "linebreak-style": ["error", "unix"],
    "max-lines": ["error", 300],
    "max-len": ["warn", 125, 2, { 
      "ignorePattern": "^(import|export)", 
      "ignoreUrls": true, 
      "ignoreStrings": true, 
      "ignoreTemplateLiterals": true 
    }],
    "no-multiple-empty-lines": ["error", { "max": 1 }],
    "no-trailing-spaces": ["error"],
    "no-extra-semi": ["error"],
    "no-var": ["error"],
    "quotes": ["error", "single", { "avoidEscape": true }],
    "quote-props": "off",
    "eqeqeq": 0,
    "react-native/no-inline-styles": ["warn"],
    "react-native/no-unused-styles": ["error"],
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": ["warn", { 
      "additionalHooks": "^(useCallbackGenerator|useObservable|useObservableCallback|useObservableRefresh|usePromise|usePromiseRefresh)$"
    }],
    "react/style-prop-object": "off",
    "no-useless-escape": "error",
    "unused-imports/no-unused-imports-ts": "error",
    "import/no-unresolved": "off",
    "import/named": "off",
    "import/namespace": "off",
    "import/default": "off",
    "import/no-named-as-default-member": "off",
    "import/no-named-as-default": "off",
    "import/no-cycle": "off",
    "import/no-deprecated": "off",
    "import/no-unused-modules": "off",
    "import/newline-after-import": "error",
    "import/first": "error",
    "import/order": ["error", {
      "alphabetize": { "order": "asc", "caseInsensitive": true },
      "groups": [ "builtin", ["external", "internal"], ["parent", "sibling", "index"], "type", "object"],
      "newlines-between": "always",
      "pathGroups": [
        { "pattern": "react", "group": "external", "position": "before" },
        { "pattern": "@eduzz/**", "group": "internal", "position": "after" },
        { "pattern": "~/**", "group": "internal", "position": "after" },
      ]
    }],
    "@typescript-eslint/no-unused-vars": ["warn"],
    "@typescript-eslint/adjacent-overload-signatures": ["error"],
    "@typescript-eslint/naming-convention": ["error", {
        "selector": "interface",
        "format": ["PascalCase"],
        "custom": { "regex": "^I[A-Za-z]", "match": true }
    }],
    "@typescript-eslint/no-namespace": ["error"],
    "@typescript-eslint/no-require-imports": ["error"],
    "@typescript-eslint/no-object-literal-type-assertion": "off",
    "@typescript-eslint/no-useless-constructor": "error",
    "@typescript-eslint/no-empty-interface": "off",
    "@typescript-eslint/explicit-function-return-type": "off",
    "@typescript-eslint/no-var-requires": "off",
    "@typescript-eslint/no-explicit-any": "off",
    "@typescript-eslint/explicit-module-boundary-types": "off",
    "@typescript-eslint/explicit-member-accessibility": ["error", { "accessibility": "off" }]
  }
}
```

## .prettierrc
```json
{
  "singleQuote": true,
  "jsxSingleQuote": true,
  "quoteProps": "as-needed",
  "trailingComma": "none",
  "useTabs": false,
  "tabWidth": 2,
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid",
  "endOfLine": "lf",
  "printWidth": 120,
  "semi": true
}
```