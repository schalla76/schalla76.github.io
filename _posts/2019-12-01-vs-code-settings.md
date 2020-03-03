#VS Code Settings

## Extensions

```
code --install-extension christian-kohler.npm-intellisense              # NPM Intellisence
code --install-extension dbaeumer.vscode-eslint                         # Es lint
code --install-extension donjayamanne.githistory                        # git History
code --install-extension DotJoshJohnson.xml                             # XML Tools
code --install-extension dsznajder.es7-react-js-snippets                # react/redux/graphql code snippets
code --install-extension eamodio.gitlens                                # git lens
code --install-extension eg2.vscode-npm-script                          # NPM package.json
code --install-extension esbenp.prettier-vscode                         # Prettier
code --install-extension formulahendry.auto-rename-tag                  # Auto Rename Tag
code --install-extension GrapeCity.gc-excelviewer                       # CSV Viewer
code --install-extension joelday.docthis                                # JS Documentation
code --install-extension mechatroner.rainbow-csv                        # CSV Editor
code --install-extension mhutchie.git-graph                             # git Graph
code --install-extension mitchdenny.ecdc                                # Encode Decode
code --install-extension ms-mssql.mssql                                 # MS SQL
code --install-extension ms-vscode.PowerShell                           # Power Shell
code --install-extension ms-vscode.vscode-typescript-tslint-plugin      # TS Lint
code --install-extension msjsdiag.debugger-for-chrome                   # Chrome debugger
code --install-extension ritwickdey.LiveServer                          # Live Server
code --install-extension steoates.autoimport                            # Auto Import
code --install-extension Tyriar.sort-lines                              # Sort Lines
code --install-extension uizhou.githd                                   # git Diff
code --install-extension wix.vscode-import-cost                         # Import Cost
code --install-extension yzhang.markdown-all-in-one                     # markdown util
code --install-extension davidanson.vscode-markdownlint                 # markdonw lint
code --install-extension yzane.markdown-pdf                             # markdonw pdf
code --install-extension streetsidesoftware.code-spell-checker          # spell checker
code --install-extension firefox-devtools.vscode-firefox-debug          # Firefox debugger
```

## Settings

```json
{
  "editor.minimap.enabled": false,
  "window.menuBarVisibility": "default",
  "editor.renderControlCharacters": true,
  "editor.renderWhitespace": "all",
  "git.ignoreMissingGitWarning": true,
  "window.zoomLevel": 0,
  "terminal.integrated.rendererType": "dom",
  "eslint.autoFixOnSave": true,
  "eslint.alwaysShowStatus": true,
  "prettier.disableLanguages": ["vue", "js"],
  "editor.formatOnSave": true,
  "prettier.printWidth": 120,
  "mssql.copyRemoveNewLine": false,
  "mssql.format.alignColumnDefinitionsInColumns": true,
  "mssql.format.datatypeCasing": "lowercase",
  "mssql.format.keywordCasing": "uppercase",
  "mssql.format.placeSelectStatementReferencesOnNewLine": true,
  "editor.fontLigatures": true,
  "editor.fontFamily": "'Fira Code iScript'",
  "editor.wordWrap": "on"
}
```

## Fonts

https://github.com/kencrocken/FiraCodeiScript

## Setting prettier for VSCode

https://www.lvguowei.me/post/vscode-eslint-prettier/

## Install ESLint and Prettier in VSCode.

npm i -g eslint prettier eslint-config-prettier eslint-plugin-prettier eslint-config-react-app eslint-plugin-import babel-eslint eslint-plugin-flowtype eslint-plugin-jsx-a11y eslint-plugin-react

```json
VSCode Setting:
{
"editor.formatOnSave": true,
"[javascript]": {
"editor.formatOnSave": false
},
"eslint.autoFixOnSave": true,
"eslint.alwaysShowStatus": true,
"prettier.disableLanguages": [
"js"
],
"files.autoSave": "onFocusChange"
}
```

## Install the extension on other machine

code --list-extensions
code --install-extension <Extension Name>
