#VS Code Settings

## Extensions

| Extension Name                    | Install Command Line                                               |
| --------------------------------- | ------------------------------------------------------------------ |
| AI Intellisense                   | code --install-extension visualstudioexptteam.vscodeintellicode    |
| Angular Extension                 | code --install-extension alexiv.vscode-angular2-files              |
| Angular Extension                 | code --install-extension angular.ng-template                       |
| Angular Extension                 | code --install-extension infinity1207.angular2-switcher            |
| Angular Extension                 | code --install-extension johnpapa.angular2                         |
| Auto Import                       | code --install-extension steoates.autoimport                       |
| Auto Rename Tag                   | code --install-extension formulahendry.auto-rename-tag             |
| Brackets                          | code --install-extension coenraads.bracket-pair-colorizer-2        |
| C# Decompile                      | code --install-extension icsharpcode.ilspy-vscode                  |
| C# Extension                      | code --install-extension jchannon.csharpextensions                 |
| C# Extension                      | code --install-extension jmrog.vscode-nuget-package-manager        |
| C# Extension                      | code --install-extension ms-dotnettools.csharp                     |
| CSS Bootstrap Extension           | code --install-extension thekalinga.bootstrap4-vscode              |
| CSS Extension                     | code --install-extension pranaygp.vscode-css-peek                  |
| CSS Extension                     | code --install-extension zignd.html-css-class-completion           |
| CSV Editor                        | code --install-extension mechatroner.rainbow-csv                   |
| CSV Viewer                        | code --install-extension GrapeCity.gc-excelviewer                  |
| Chrome Debugger                   | code --install-extension msjsdiag.debugger-for-chrome              |
| Clojure Extension                 | code --install-extension betterthantomorrow.calva                  |
| ES Lint                           | code --install-extension dbaeumer.vscode-eslint                    |
| Encode Decode                     | code --install-extension mitchdenny.ecdc                           |
| Firefox Debugger                  | code --install-extension firefox-devtools.vscode-firefox-debug     |
| Git Diff                          | code --install-extension uizhou.githd                              |
| Git Graph                         | code --install-extension mhutchie.git-graph                        |
| Git History                       | code --install-extension donjayamanne.githistory                   |
| Git Lens                          | code --install-extension eamodio.gitlens                           |
| Icons                             | code --install-extension vscode-icons-team.vscode-icons            |
| Import Cost                       | code --install-extension wix.vscode-import-cost                    |
| JS Documentation                  | code --install-extension joelday.docthis                           |
| Live Server                       | code --install-extension ritwickdey.LiveServer                     |
| Markdown Lint                     | code --install-extension davidanson.vscode-markdownlint            |
| Markdown Pdf                      | code --install-extension yzane.markdown-pdf                        |
| Markdown Util                     | code --install-extension yzhang.markdown-all-in-one                |
| MS Sql                            | code --install-extension ms-mssql.mssql                            |
| Npm Intellisense                  | code --install-extension christian-kohler.npm-intellisense         |
| Npm Package.Json                  | code --install-extension eg2.vscode-npm-script                     |
| Power Shell                       | code --install-extension ms-vscode.PowerShell                      |
| Prettier                          | code --install-extension esbenp.prettier-vscode                    |
| React/Redux/Graphql code snippets | code --install-extension dsznajder.es7-react-js-snippets           |
| Sort Lines                        | code --install-extension Tyriar.sort-lines                         |
| Spell Checker                     | code --install-extension streetsidesoftware.code-spell-checker     |
| Ts Lint                           | code --install-extension ms-vscode.vscode-typescript-tslint-plugin |
| Xml Tools                         | code --install-extension DotJoshJohnson.xml                        |
| Xunit Test                        | code --install-extension formulahendry.dotnet-test-explorer        |

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
  "editor.wordWrap": "on",
  "window.autoDetectHighContrast": false
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
