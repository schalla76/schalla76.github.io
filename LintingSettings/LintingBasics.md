# Linting Settings

## React Typesciprt Setup

[Create React App, Using Typescript](https://codeburst.io/hello-create-react-app-cra-typescript-8e04f7012939)

## JS linting

### Pacagage needed for JS linting

```npm
npm install eslint prettier eslint-config-prettier eslint-plugin-prettier eslint-config-airbnb eslint-plugin-import babel-eslint eslint-plugin-jsx-a11y eslint-plugin-react eslint-plugin-jest -D
```

### .eslintrc file

```json
{
  "parser": "babel-eslint",
  "env": {
    "browser": true,
    "jest/globals": true
  },
  "extends": ["airbnb", "plugin:prettier/recommended"],
  "rules": {
    "strict": 0
  },
  "plugins": ["jest"]
}
```

### .env file

`SKIP_PREFLIGHT_CHECK=true`

### package.json

```json
"scripts": {
"lint-js": "eslint 'src/\*_/_.{js,jsx}'",
},
"lint-staged": {
"\*.{js,jsx}": ["eslint"]
}
```

## TS linting

### Pacagage needed for TS linting

```npm
npm install tslint prettier tslint-config-prettier tslint-plugin-prettier tslint-react -D
```

### .prettierrc file

```json
{
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "es5"
}
```

### tslint.json

```json
{
  "extends": ["tslint:recommended", "tslint-react", "tslint-config-prettier"],
  "rulesDirectory": ["tslint-plugin-prettier"],
  "rules": {
    "prettier": true,
    "interface-name": false
  }
}
```

### package.json file

```json
"scripts": {
"lint-ts": "tslint -c tslint.json 'src/\*_/_.{ts,tsx}'"
}
```
