---
layout: post
title: "Learning ReactJS"
date: 2020-03-16
categories: js, react, udemy
---

## Course links

[Udemy Course: Modern React with Redux](https://www.udemy.com/course/react-redux/)

[Udemy Author: Stephen Grider](https://github.com/StephenGrider)

[Github Sources](https://github.com/StephenGrider/ReduxCasts)

## Setting up the react js typescript environment

[Source](https://levelup.gitconnected.com/typescript-and-react-using-create-react-app-a-step-by-step-guide-to-setting-up-your-first-app-6deda70843a4)

## Installing Create React App pacakge

```
npm install -g create-react-app
```

## Creating react js typescript app

```
npx create-react-app app-name --template typescript
```

## Converting JSX to JS

[User Babel Tester](https://babeljs.io/repl)

### Converting inline css to jsx

In JSX we reference the Javascript object by inclosing the style with two curly brace. First curly brace is to specify Javascript and next one it for denote the Javascript object. The key and values are separating using colon, similar to JSON. To make a key word the hyphen is removed and the preceding letter is capitalized.

Other changes are class becomes className, for becomes htmlFor

We can reference the Javascript object with curly braces.

```html
<div style="background-color: red;"></div>
```

```jsx
<div style={{ backgroundColor: "red" }}></div>
```

### Install React router

```cmd
npm install react-router-dom
```

### Create Fake Data

[Faker JS](https://github.com/marak/Faker.js/)

[Semantic UI](https://semantic-ui.com/)

## Bootstrap Sass

[Inspired by](https://github.com/loljoho/react-bootstrap-sass)

### Install node-sass and bootstrap

```cmd
npm install node-sass bootstrap
```

### Rename all _.css to _.scss

example:

```cmd
mv src/index.css to src/index.scss
```

### Change the reference to scss

exmaple: index.scss

```css
@import "~bootstrap/scss/bootstrap";
```

### Copy the variables file to the src

```cmd
cp node_modules/bootstrap/scss/_variables.scss ./src/
```

### Add imports to index.scss

```css
@import "~bootstrap/scss/functions";
@import "variables";
@import "~bootstrap/scss/bootstrap";
```
