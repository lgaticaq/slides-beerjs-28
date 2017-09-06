title: Beerjs #28
author:
  name: Leonardo Gatica
  twitter: lgaticaq
  url: https://about.me/lgatica
  email: lgatica@protonmail.com
output: index.html
theme: juanbrujo/cleaver-beerjs
style: style.css
controls: true

--

<h1>Tips para Node y Caracteristicas de ES6/7/8</h1>

--

# nvm

Utilidad para manejar múltiples versiones de Node

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.4/install.sh | bash
echo "v8" > .nvmrc
nvm install v8
nvm use
```

--

# npm init

```shell
# Override default values
npm config set init.author.name "Leonardo Gatica"
npm config set init.author.email lgatica@protonmail.com
npm config set init.author.url https://about.me/lgatica
npm config set init.license MIT
npm config set init.version 0.0.0
mkdir my-app && cd my-app
npm init -y
```

--

# eslint

Linter para JavaScript y JSX

```shell
npm i -D eslint
npx eslint --init

? How would you like to configure ESLint? Use a popular style guide
? Which style guide do you want to follow? Standard
? What format do you want your config file to be in? JSON
...
Successfully created .eslintrc.json file in /home/leonardo/projects/my-app

npx eslint --fix
```

--

# nodemon + inspec

Utilidad que reinicia la app ante cualquier cambio en el código
Para usar `--inspect` es necesario tener Node >= v6.3.0

```shell
npm i -D nodemon
node --inspect index.js
node --inspect-brk index.js # inicia la app con un breakpoint al inicio
npx nodemon -- --inspect index.js
```

Pro TIP: Extensión de chrome para abrir automáticamente el V8 inspector
NIM (Nodo Inspector Administrador) https://tupo.to/38Co

--

![inspector](img/node-inspector.png "inspector")

--

# Prettier

Formateador de código con soporte para: JS, JSX, Flow, TypeScript, CSS, LESS, SCSS, JSON y Graph:ql:

```shell
npm i -D prettier-standard
npx prettier-standard 'app/**/*.js'
```

--

# Var, Let y Const

Usar `const` para toda variable que sera una sola vez definida

```javascript
const express = require('express')
const app = express()
```

Para el resto usar `let`

```javascript
let message = ''
for (let i=0, i<10, i++) {
  if (i < 9) {
    message += 'index ' + i + '\n'
  } else {
    message = 'finish'
  }
}
```

Usar `var` si te gusta lo _vintage_ o para jugar en el REPL y autocompletar los nombres

--

# ES6
## Array methods

```javascript
const myArray = [1, 2, 3, 4, 5]
myArray.find(number => number > 4) // 5
myArray.findIndex(number => number > 3) // 2
myArray.map(number => ({old: number, new: number + 1})) // [{old: 1, new: 2}, {old: 2, new: 3}, ...]
myArray.filter(number => number > 3) // [4, 5]
myArray.reduce((accumulator, current) => {
  if (current < 3) {
    accumulator.first.push(current)
  } else {
    accumulator.second.push(current)
  }
  return accumulator
}, {first: [], second: []}) // {first: [1, 2], second: [3, 4, 5]}
const options = Array.from(document.querySelector.all('option'))
```

--

# Extended Parameter Handling
## Default Parameter Values
```javascript
const f1 = (x, y = 7, z = 42) => x + y + z
f1(1) === 50
```

## Rest Parameter
```javascript
const f2 = (x, y, ...a) => (x + y) * a.length
f2(1, 2, 'hello', true, 7) === 9
```

## Spread Operator
```javascript
const params = [ 'hello', true, 7 ]
const other = [ 1, 2, ...params ] // [ 1, 2, 'hello', true, 7 ]
f2(1, 2, ...params) === 9
const str = 'foo'
const chars = [ ...str ] // [ "f", "o", "o" ]
```

--

# Template Literals

```javascript
const customer = { name: 'Foo' }
const card = { amount: 7, product: 'Bar', unitprice: 42 }
const message = `Hello ${customer.name},
want to buy ${card.amount} ${card.product} for
a total of ${card.amount * card.unitprice} bucks?`
```

# Property Shorthand

```javascript
const x = 1
const y = 2
const obj1 = { x, y, z: 3 }
const obj2 = {
  foo: 'bar',
  [ 'baz' + quux() ]: 42
}
```

--

# Destructuring Assignment

```javascript
const list = [ 1, 2, 3 ]
const [ a, , b ] = list
[ b, a ] = [ a, b ]
const { x, y, z } = { x: 1, y: 2, z: 3 }
const { x: a, y: { a: b }, z: c } = { x: 1, y: {a: 2}, z: 3 }
const { a, b = 2 } = { a: 1 }
const f = ([ name, val ]) => console.log(name, val)
const g = ({ name: n, val: v }) => console.log(n, v)
const h = ({ name, val }) => console.log(name, val)
f([ "bar", 42 ])
g({ name: "foo", val:  7 })
h({ name: "bar", val: 42 })
const [ a = 1, b = 2, c = 3, d ] = [ 7, 42 ]
a === 7
b === 42
c === 3
d === undefined
```

--

# Modules

```javascript
//  lib/math.js
export const sum = (x, y) => x + y
export const pi = 3.141593

//  someApp.js
import * as math from 'lib/math'
console.log(`2π = ${math.sum(math.pi, math.pi)}`)

//  otherApp.js
import { sum, pi } from 'lib/math'
console.log(`2π = ${sum(pi, pi)}`)

//  lib/mathplusplus.js
export * from 'lib/math'
export const e = 2.71828182846
export default (x) => Math.exp(x)

//  someApp.js
import exp, { pi, e } from 'lib/mathplusplus'
console.log(`e^{π} = ${exp(pi)}`)
```

--

# Clases

```javascript
const killMixin = Base => class extends Base {
  kill() { console.log(`~KILL ALL~`) }
};
class Cat {
  constructor(name) { this._name = name }
  get name () { return this._name }
  set name (name) { this._name = name }
  speak() { console.log(`${this._name} makes a noise.`) }
  static familyName () { console.log('Felines') }
}
class Lion extends killMixin(Cat) {
  speak() {
    super.speak()
    console.log(`${this._name} roars.`)
  }
}
const l = new Lion('Fuzzy')
l.speak()
// Fuzzy makes a noise.
// Fuzzy roars.
l.kill()
// ~KILL ALL~
Lion.familyName()
```

--

# New Built-In Methods

```javascript
const dest = { quux: 0 }
const src1 = { foo: 1, bar: 2 }
const src2 = { foo: 3, baz: 4 }
Object.assign(dest, src1, src2)
dest.quux === 0
dest.foo  === 3
dest.bar  === 2
dest.baz  === 4
'foo'.repeat(3) === 'foofoofoo'
'hello'.startsWith('he') === true
'hello'.startsWith('el', 1) === true
'hello'.endsWith('lo') === true
'hello'.includes('ell') === true
'hello'.includes('ell', 2) === false
Number.isNaN(42) === false
Number.isNaN('42') === false
Number.isNaN(NaN) === true
Math.trunc(42.7) === 42
```

--

# Internationalization & Localization

```javascript
const number = 123456.789
console.log(new Intl.NumberFormat('es-CL').format(number))
// → 123.456,789
console.log(new Intl.NumberFormat('ar-EG').format(number))
// → ١٢٣٤٥٦٫٧٨٩
console.log(new Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' }).format(number))
// → $123.457
```

```shell
// Necesario para node
npm i full-icu
node --icu-data-dir=node_modules/full-icu

```

--

# Promises

```javascript
const {promisify} = require('util')
const fs = require('fs')
const filePath = '/etc/environment'
const options = {encoding: 'utf8'}
const readFileAsync1 = file => {
  return new Promise((resolve, reject) => {
    fs.readFile(file, options, (err, content) => {
      if (err) return reject(err)
      resolve(content)
    })
  })
}
const readFileAsync2 = promisify(fs.readFile)
Promise.all([
  readFileAsync1(filePath),
  readFileAsync2(filePath, options)
]).then(data => {
    const [ content1, content2 ] = data
    console.log(content1 === content2)
}, (err) => console.log(`error: ${err}`))
```

--

# ES7/8

## String padding

```javascript
'x'.padStart(5, 'ab') === 'ababx'
'x'.padEnd(5, 'ab') === 'xabab'
```

## Array.prototype.includes

```javascript
const arr = ['react', 'angular', 'vue']
arr.includes('react') === true
```

## Exponentiation Operator

```javascript
Math.pow(7, 12) === 7 ** 12
```

## Object.values/Object.entries

```javascript
const obj = {a: 1, b: 2, c: 3}
Object.keys(obj) // ['a', 'b', 'c']
Object.values(obj) // ['1', '2', '3']
Object.entries(obj) // [['a', 1], ['b', 2], ['c', 3]]
```

--

# Async Functions

```javascript
async fetchData(query) => {
  try {
    const response = await axios.get(`/q?query=${query}`)
    return response.data
  } catch (err) {
    console.log(err)
  }
}
fetchData(query).then(data => console.log(data))
```
