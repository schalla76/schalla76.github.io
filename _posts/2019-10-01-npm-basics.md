# NPM Basic Commands

## Find node version

`node -v`

## Intialition commands

### Initialization the npm with questions

`npm init`

### Initialization the npm with no questions

`npm init -y`

### Install npm package globally

`npm install <package-name > -g, --save`

### Install npm package as dependency

`npm install <package-name > -S, --save`

### Install npm package as development dependency

`npm install <package-name > -D, --save-dev`

### Install npm package as optional dependency

`npm install <package-name > -O, --save-optional`

## Query/Listing commands

### List the global packages

`npm list -g --depth 0`

### List the outdated global packages

`npm outdated -g --depth=0`

### Update the packages in package.json

#### Safe version and package-lock.json

- Packages with ^ minor version can be upgraded safely. Example 17.0.0 can be upgrade to 17.5.1
- Packages with ~ patch version can be upgraded safely. Example 17.0.0 can be upgrade to 17.0.2
- If node_modules folder exists and packages are already install then npm install won't upgrade any thing
- If node_modules folder doesn't exist i.e. packages are not installed and there exists a package-lock.json file then npm install will install the exact version specified in the package.json file
- If node_modules folder doesn't exist i.e. packages are not installed and these is no package-lock.json file then npm install will install the latest safe version. It won't upgrade the package.json with the latest safe version number but the node_modules folder would be upgrade to latest safe version.

#### Upgrade using npm

- Command to check the out of date dependencies

`npm outdated`

- Command to update all packages to safe version

`npm update`

- Command to update individual packages to safe version

`npm update "react" "react-dom"`

- Command to update packages to major version (not safe version)

`npm install <packagename1>@latest <packagename2>@latest `

#### Upgrade using the tool(npm-check-updates)

- Command to display the updates

`npx npm-check-updates`

- Command to update dependencies in package.json. Take a backup of package.json and package-lock.json before running this command

`npx npm-check-updates -u`

- Command to install

`npm install`
