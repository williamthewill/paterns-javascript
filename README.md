# DOCUMENTAÇÃO DE ARQUITETURA E PATTERNS PARA PROJETOS VUEJS

_Este documento propõe regras de arquitetura e patterns para projetos front-end desenvolvidos em vue, dentro da Dígitro._

## Patterns de desenvolvimento:

1. [Javascript styleGuides](#JSstyleGuides)
1. [CSS no Javascript](#CSSinJavascript)
1. [Vue styleGuides](#VUEstyleGuides)
1. [Resources](#Resources)

## Javascript styleGuides - JSstyleGuides

-   Use padrões do ES6+
    > Muita coisa do ES5 foi melhorada para que a liguagem ficasse mais versátil e segura, portanto padrões antigos não se aplicam mais as novas versões, logo evite seguir padrões de versões antigas que conflitem com as novas.

```js
//ruim
var obj = {}

//bom
let obj = {}

//bom
const obj = {}
```
-   Nunca mais use var

    > A partir das ultimas versões os browser já suportam let e const,a ecma implementou o let para sanar problemas de escopo que aconteciam quando se declarava com var, então não tem mais porque usar var.

```js
//ruim
var obj = {}

//bom
let obj = {}

//bom
const obj = {}
 ```
-   Objeto sem new
    
    > Use a sintaxe literal  pra criação de objetos. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

```javascript
// ruim
const item = new Object()

// bom
const item = {}
```
- Use nome de propriedade computadas ao invés de criar propriedades nomes de              dinamicamente (Computed property names (ES2015))
    > Criar propriedades com nome dinamico dentro do objeto centraliza sua propriedades conhecidas, faça diferente a menos que seja estritamente necessário

```js

function getKey(k) {
    return `a key named ${k}`
}

// ruim
const obj = {
    id: 5,
    name: 'San Francisco',
}

obj[getKey('enabled')] = true;

// bom
const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
}
 ```
- Shorthand property names (ES2015)
    > Notação ES6 para nomes de propriedades. (eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html))

```js
//ruim
let a = 'foo', b = 42, c = {}
let o = {
    a: a, 
    b: b, 
    c: c
}

//bom
let a = 'foo', b = 42, c = {}
let o = {a, b, c}
```
- Shorthand method names (ES2015)
> Notação ES6 para nomes de metodods
```js
//ruim
let o = {
    property: function (parameters) {}
}
//bom
let o = {
    property(parameters) {}
}
```
- Propriedades Shorthand agrupada
    > Agrupando as propiedades shorthand a leitura se torna mais intuitiva

```js
const anakinSkywalker = 'Anakin Skywalker'
const lukeSkywalker = 'Luke Skywalker'

// ruim
const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker
}

// bom
const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4
}
```
- Objetos quoted-props
    > Torna a leitura mais fácil. Ele melhora o realce de sintaxe e também é mais facilmente otimizado por muitos mecanismos JS. (eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html))

```js
// ruim
const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5
}

// bom
const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5
}
```
- rest-spread
    > Prefira utilizar o operador spread ao invés de object.assign. ((https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign))

```js
// bem ruim
const original = { a: 1, b: 2 }
const copy = Object.assign(original, { c: 3 })
delete copy.a

// ruim
const original = { a: 1, b: 2 }
const copy = Object.assign({}, original, { c: 3 })

// bom
const original = { a: 1, b: 2 }
const copy = { ...original, c: 3 }

const { a, ...noA } = copy
```
- arrays--literals
    > Use a sintaxe literal para ciração de arrays, os motores dos browser renderizam mais rápido. (eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html))

 ```js
// ruim
const items = new Array()

// bom
const items = []
 ```
- utilize array spreads para copiar arrays
    > O operador rest-spread do ES6 trouxe muitas facilidades, porque não usar

 ```js
// ruim
const len = items.length
const itemsCopy = []
let i

for (i = 0; i < len; i += 1) {
    itemsCopy[i] = items[i]
}

// bom
const itemsCopy = [...items]
 ```
- destructuring--object
    > Utilize o desestruturador de objeto porque ele cria uma referencia temporária das propriedades. (eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring))

 ```js
// ruim
function getFullName(user) {
    const firstName = user.firstName
    const lastName = user.lastName

    return `${firstName} ${lastName}`
}

// bom
function getFullName(user) {
    const { firstName, lastName } = user
    return `${firstName} ${lastName}`
}

// recomendado
function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`
}
```
- Utilize sempre aspas simples para strings
    > Como conveção ja à muito tempo é recomendável utilizar sempre aspas simples. (strings. eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html))

```js
// ruim
const name = "Capt. Janeway"

// ruim - template literals devem conter interpolação ou novas linhas
const name = `Capt. Janeway`

// bom
const name = 'Capt. Janeway'
```
- Não quebre string com mais de 100 caracteres em novas linhas
    > Quebrar concatenar strings é muito custoso, por isso evite sempre que puder

```js
// ruim
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// ruim
const errorMessage = 'This is a super long error that was thrown because ' +
'of Batman. When you stop to think about how Batman had anything to do ' +
'with this, you would get nowhere fast.';

// bom
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```
- Templates literais
    > Use e abuse de templates literais, eles  fornecem uma sintaxe legível e concisa com recursos de novas linhas e interpolação de strings adequados. (eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing))

```js
// ruim
function sayHi(name) {
    return 'How are you, ' + name + '?'
}

// ruim
function sayHi(name) {
    return ['How are you, ', name, '?'].join()
}

// ruim
function sayHi(name) {
    return `How are you, ${ name }?`
}

// bom
function sayHi(name) {
    return `How are you, ${name}?`
}
```
- Utilize eval somente em casos extremos
    > eval é extremamente custoso e pode gerar sérios problemas de segurança no seu sistema. (eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval))
- Parâmetros default
    > Atribua valores default sempre na assinatura da função
```js
// muito ruim
function handleThings(opts) {
    // Não! Não devemos alterar argumentos de função.
    // Double bad: se opts for falso, será definido como um objeto que pode
    // seja o que você quer, mas pode introduzir bugs sutis.
    opts = opts || {}
}

// ruim
function handleThings(opts) {
    if (opts === void 0) {
        opts = {};
    }
}

// bom
function handleThings(opts = {}) {}
```
- Espaços na assinatura da função
    > Mantenha a concistencia das assinaturas de suas funções. (eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks))

```js
// ruim
const f = function(){}
const g = function (){}
const h = function() {}

// bom
const x = function () {}
const y = function a() {}
```
- Prefira sempre utilizar parametros nomeados
    > Não é raro desenvolvedores procurarem entender um código lendo-o diretamente em vez de consultar sua documentação. O código nunca mente, ou ele funciona ou não funciona, já uma documentação pode ser incompleta ou até mesmo desatualizada. Nesse sentido, facilitar a leitura é importante não só para terceiros, mas também para quem desenvolve o código.
    
```js
// ruim
const moveFrame = (from, to) => {
    /*
        Acessa o elemento do DOM
        removendo e adicionando classes
    */
};
moveFrame ('sprite1', 'sprite2');

//bom
const moveFrame = ({ from, to }) => {
    /*
        Mudamos o parâmetro, mas o restante do código 
        continou como estava
    */
};

move_frame (from="sprite1", to="sprite2")
```
- Identação de assinatura de função
    > Funções com multiplos parâmetros em suas assinaturas devem ser recuadas exatamente como todas as outras listas de múltiplas linhas neste guia: com cada item em uma linha sozinho, com uma vírgula final no último item. (eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline))

```js
// ruim
function foo(bar,
            baz,
            quux) {
// ...
}

// bom
function foo(
    bar,
    baz,
    quux,
    ) {
    // ...
}

// ruim
console.log(foo,
    bar,
    baz)

// bom
console.log(
    foo,
    bar,
    baz,
)
```
- Classes
    > Utilize classes sempre que precisar manipular o prototype do objeto. Classes são mais concisas e fáceis de entender.

```js
// ruim
function Queue(contents = []) {
    this.queue = [...contents];
}
Queue.prototype.pop = function () {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
};

// bom
class Queue {
    constructor(contents = []) {
        this.queue = [...contents];
    }
    pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
    }
}
```
- Encadeamento de contrutor
    > Metodos podem retornar this, isso pode ser super prático em alguns casos

```js
// ruim
Jedi.prototype.jump = function () {
    this.jumping = true
    return true
};

Jedi.prototype.setHeight = function (height) {
    this.height = height
};

const luke = new Jedi()
luke.jump() // => true
luke.setHeight(20) // => undefined

// bom
class Jedi {
    jump() {
        this.jumping = true
        return this
    }

    setHeight(height) {
        this.height = height
        return this
    }
}

const luke = new Jedi()
luke.jump().setHeight(20)
```
- Não duplicar imports
    > Um único import para o mesmo caminho. (eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports))

```js
// ruim
import foo from 'foo';
// … some other imports … //
import { named1, named2 } from 'foo'

// bom
import foo, { named1, named2 } from 'foo'
```
- Exports sempre imutaveis
    > eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

```js
// ruim
let foo = 3
export { foo }

// bom
const foo = 3
export { foo }
```
- Prefira export default
    > Para incentivar que exista apenas um export por arquivo, isso melhora a legibilidade.(eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md))

```js
// ruim
export function foo() {}

// bom
export default function foo() {}
```
- Imports sempre no topo do arquivo
    > Manter os imports sempre no topo impede que aja compoertamentos estranhos. (eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md))

```js
// ruim
import foo from 'foo'
foo.init()

import bar from 'bar'

// bom
import foo from 'foo'
import bar from 'bar'

foo.init()
```
- Iteradores não
    > Prefira sempre javascript de alta ordem, conceitos de programação funcional se aplicam muito bem com javascript, não é a toa que a comunidade e o próprio tc39 trabalham tão arduamente para padroniza-los e inclui-los na linguagem. Utilize `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... para iterar arrays, e `Object.keys()` / `Object.values()` / `Object.entries()` para iterar objetos. (eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax))

```js
const numbers = [1, 2, 3, 4, 5]

// ruim
let sum = 0
for (let num of numbers) {
    sum += num
}
sum === 15

// bom
let sum = 0
numbers.forEach(num => sum += num)
sum === 15

// recomendavel (utilize o poder da programação funcional)
const sum = numbers.reduce((total, num) => total + num, 0)
sum === 15

// ruim
const increasedByOne = []
for (let i = 0; i < numbers.length; i++) {
    increasedByOne.push(numbers[i] + 1)
}

// bom
const increasedByOne = []
numbers.forEach((num) => {
    increasedByOne.push(num + 1)
});

// recomendavel (prefira sempre um caminho funcional)
const increasedByOne = numbers.map(num => num + 1)
```
- Um único const por declaração de variável
    > É mais fácil adicionar novas declarações de variáveis dessa maneira, e você nunca precisa se preocupar em trocar um `;` por um `,` ou introduzir os diffs somente de pontuação. Você também pode percorrer cada declaração com o depurador, em vez de passar por todos eles de uma só vez.
    > eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)
    
```js
// ruim
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z'

// bom
const items = getItems()
const goSportsTeam = true
const dragonball = 'z'
```
- Um único let por declaração de variável
    >Isso é útil quando, mais tarde, você pode precisar atribuir uma variável dependendo de uma das variáveis atribuídas anteriormente.

```javascript
// bom
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true

// ruim
let i
const items = getItems()
let dragonball
const goSportsTeam = true
let len

// bom
const goSportsTeam = true
const items = getItems()
let dragonball
let i
let length
```
- Chamadas de função onde realmente se usa
    >Evite processamento desnecessário, chame suas funções onde elas realmente serão usadas

```javascript
// ruim - chamamda de função desnecessária
function checkName(hasName) {
    const name = getName()

    if (hasName === 'test') {
        return false
    }

    if (name === 'test') {
        this.setName('')
        return false
    }

    return name
}

// bom
function checkName(hasName) {
    if (hasName === 'test') {
        return false
    }

    const name = getName();

    if (name === 'test') {
        this.setName('')
        return false
    }

    return name
}
```
- Não use atribuição de variavel encadeada
    >Assign encadeado de variáveis cria implicitamente variáveis globais. (eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign))

```javascript
// ruim
(function example() {
    // JavaScript interprets this as
    // let a = ( b = ( c = 1 ) );
    // The let keyword only applies to variable a; variables b and c become
    // global variables.
    let a = b = c = 1;
}());

console.log(a); // throws ReferenceError
console.log(b); // 1
console.log(c); // 1

// bom
(function example() {
    let a = 1;
    let b = a;
    let c = a;
}());

console.log(a); // throws ReferenceError
console.log(b); // throws ReferenceError
console.log(c); // throws ReferenceError
```
- Não quebre linha na atribuição de variavel
    >Quebra de linha na atribuição de variável pode ofuscar o valor assinado. (https://eslint.org/docs/rules/operator-linebreak.html)

```js
    // ruim
const foo =
superLongLongLongLongLongLongLongLongFunctionName()

// ruim
const foo
= 'superLongLongLongLongLongLongLongLongString'

// bom
const foo = (
superLongLongLongLongLongLongLongLongFunctionName()
)

// bom
const foo = 'superLongLongLongLongLongLongLongLongString'
```
- Remova variáveis não utilizadas
    >Variáveis ninca utilizadas podem causar problemas de leitura e entendimento do código. (https://eslint.org/docs/rules/no-unused-vars)
    
```js
// ruim
var some_unused_var = 42

// Write-only variables are not considered as used.
var y = 10
y = 5

// A read for a modification of itself is not considered as used.
var z = 0
z = z + 1

// Unused function arguments.
function getX(x, y) {
    return x
}

// bom
function getXPlusY(x, y) {
    return x + y
}

var x = 1
var y = a + 2

alert(getXPlusY(x, y))

// 'type' is ignored even if unused because it has a rest property sibling.
// This is a form of extracting an object that omits the specified keys.
var { type, ...coords } = data
// 'coords' is now the 'data' object without its 'type' property.
```
- Compare sempre com eqeqeq
    >Use sempre `===`, `!==` ao invés de `==`, `!=`. Não usar o comparador estrito pode gerar erros de operações artiméticas por exemplo.(https://eslint.org/docs/rules/eqeqeq.html)
- Shortcut de comparação
    >Use shortcuts para comparações booleanas, porém para string e números comparações explícitas. (comparison--shortcuts)
    
```js
// ruim
if (isValid === true) {
    // ...
}

// bom
if (isValid) {
    // ...
}

// ruim
if (name) {
    // ...
}

// bom
if (name !== '') {
    // ...
}

// ruim
if (collection.length) {
    // ...
}

// bom
if (collection.length > 0) {
    // ...
}
```
- Comparação em blocos switch
    >Declarações léxicas são visíveis em todo o bloco `switch`, mas somente são inicializadas quando atribuídas, o que só acontece quando seu` case` é atingido. Isso causa problemas quando várias cláusulas `case` tentam definir a mesma coisa.
    
```js
// ruim
switch (foo) {
    case 1:
        let x = 1
        break
    case 2:
        const y = 2
        break
    case 3:
        function f() {
        // ...
        }
        break
    default:
        class C {}
}

// bom
switch (foo) {
    case 1: {
        let x = 1
        break
    }
    case 2: {
        const y = 2
        break
    }
    case 3: {
        function f() {
        // ...
        }
        break
    }
    case 4:
        bar()
        break
    default: {
        class C {}
    }
}
```
- Comparações sem ternario
    >Comparações booleanas em alguns casos não tem necessidade de se empregar ternário.(eslint:[`no-unneeded-ternary`])

```js
// ruim
const foo = a ? a : b
const bar = c ? true : false
const baz = c ? false : true

// bom
const foo = a || b
const bar = !!c
const baz = !c
```
- Block-braces. 
    >(https://eslint.org/docs/rules/nonblock-statement-body-position)
    
```js
// ruim
if (test)
return false;

// bom
if (test) return false;

// bom
if (test) {
    return false;
}

// ruim
function foo() { return false; }

// bom
function bar() {
    return false;
}
```
- Blocks--cuddled-elses.
    >Mantenha o else na mesma linha que a chave de fechamento do if.(eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html))
    
```js
// ruim
if (test) {
    thing1()
    thing2()
}
else {
    thing3()
}

// bom
if (test) {
    thing1()
    thing2()
} else {
    thing3()
}
```
- Blocos de else sem retorno.
    > Se um bloco if sempre executa um return o bloco else é desnecessário. (eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return))

```js
// ruim
function foo() {
    if (x) {
        return x
    } else {
        return y
    }
}

// ruim
function cats() {
    if (x) {
        return x
    } else if (y) {
        return y
    }
}

// ruim
function dogs() {
    if (x) {
        return x
    } else {
        if (y) {
            return y
        }
    }
}

// bom
function foo() {
    if (x) {
        return x
    }

    return y
}

// bom
function cats() {
    if (x) {
        return x
    }

    if (y) {
        return y
    }
}

// bom
function dogs(x) {
    if (x) {
        if (z) {
            return y
        }
    } else {
        return z
    }
}
```
- Controle de declarações
    > Caso sua instrução de controle (`if`,` while` etc.) seja muito longa ou exceda o comprimento máximo da linha, cada condição (agrupada) pode ser colocada em uma nova linha. O operador lógico deve iniciar a linha.
    
```js
// ruim
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
    thing1()
}

// ruim
if (foo === 123 &&
    bar === 'abc') {
    thing1()
}

// ruim
if (foo === 123
    && bar === 'abc') {
    thing1()
}

// ruim
if (
    foo === 123 &&
    bar === 'abc'
    ) {
    thing1()
}

// bom
if (
    foo === 123
    && bar === 'abc'
) {
    thing1()
}

// bom
if (
    (foo === 123 || bar === 'abc')
    && doesItLookGoodWhenItBecomesThatLong()
    && isThisReallyHappening()
    ) {
    thing1()
}

// bom
if (foo === 123 && bar === 'abc') {
    thing1()
}
```
- Commentário de muitas linhas
    > Use sempre /* ... */ para comentários de muitas linhas
    
```js
// ruim
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {
    // ...
    return element
}

// bom
/**
* make() returns a new element
* based on the passed-in tag name
*/
function make(tag) {
    // ...
    return element
}
```
- Idente sempre com quatro espaços
    >(eslint: [`indent`](https://eslint.org/docs/rules/indent.html))

```js
// ruim
function bar() {
∙let name;
}

// ruim
function baz() {
∙∙let name;
}

// bom
function foo() {
∙∙∙∙let name;
}
```
- Use espaços antes de blocos
    >(eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html))
    
```js
    // ruim
function test(){
    console.log('test')
}

// bom
function test() {
    console.log('test')
}

// ruim
dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
})

// bom
dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
})
```
- Não use espaços antes dos parenteses.
    >(eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html))
    
```javascript
// ruim
if(isJedi) {
    fight ()
}

// bom
if (isJedi) {
    fight()
}

// ruim
function fight () {
    console.log ('Swooosh!')
}

// bom
function fight() {
    console.log('Swooosh!')
}
```
- Uses espaços entre os operadores.
    >eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)
    
```js
    // ruim
    const x=y+5;

    // bom
    const x = y + 5;
```
- Tenha sempre a ultima linha do arquivo em branco
>eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

```js
    // ruim
    import { es6 } from './AirbnbStyleGuide'
    // ...
    export default es6
```

```js
    // ruim
    import { es6 } from './AirbnbStyleGuide'
    // ...
    export default es6;↵
↵
```

```js
// bom
import { es6 } from './AirbnbStyleGuide'
// ...
export default es6;↵
```
- Whitespace-chains
    > eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

```javascript
// bom
$('#items').find('.selected').highlight().end().find('.open').updateCount()

// bom
$('#items').
find('.selected').
    highlight().
    end().
find('.open').
    updateCount()

// bom
$('#items')
.find('.selected')
    .highlight()
    .end()
.find('.open')
    .updateCount()

// bom
const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', `translate(${radius + margin},${radius + margin})`)
    .call(tron.led)

// bom
const leds = stage.selectAll('.led')
    .data(data)
.enter().append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
.append('svg:g')
    .attr('transform', `translate(${radius + margin},${radius + margin})`)
    .call(tron.led)

// bom
const leds = stage.selectAll('.led').data(data)
```
- Use sempre uma linha em branco após o fim de um bloco

```js
// ruim
if (foo) {
    return bar
}
return baz

// bom
if (foo) {
    return bar
}

return baz;

// ruim
const obj = {
    foo() {
    },
    bar() {
    },
}
return obj

// bom
const obj = {
    foo() {
    },

    bar() {
    },
}

return obj

// ruim
const arr = [
    function foo() {
    },
    function bar() {
    },
]
return arr;

// bom
const arr = [
    function foo() {
    },

    function bar() {
    },
]

return arr
```
- Não utilize espaços entre os parenteses e os parâmetros
    >(eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html))
    
```javascript
// ruim
function bar( foo ) {
    return foo
}

// bom
function bar(foo) {
    return foo
}

// ruim
if ( foo ) {
    console.log(foo)
}

// bom
if (foo) {
    console.log(foo)
}
```
- Não utilize espaços entrs colchetes e os parâmetros
    >(eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html))
    
```js
// ruim
const foo = [ 1, 2, 3 ]
console.log(foo[ 0 ])

// bom
const foo = [1, 2, 3]
console.log(foo[0])
```
- Utilize no máximo 100 caracteres por linha
    > eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

```js
// ruim
$.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

// bom
const foo = jsonData
&& jsonData.foo
&& jsonData.foo.bar
&& jsonData.foo.bar.baz
&& jsonData.foo.bar.baz.quux
&& jsonData.foo.bar.baz.quux.xyzzy

// bom
$.ajax({
    method: 'POST',
    url: 'https://airbnb.com/',
    data: { name: 'John' },
})
.done(() => console.log('Congratulations!'))
.fail(() => console.log('You have failed this city.'))
```
- Utilize espaços entre chave e valor
    > eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)
    
```js
// ruim
var obj = { "foo" : 42 }
var obj2 = { "foo":42 }

// bom
var obj = { "foo": 42 }
```
- Não deixe espaços extras no fim de um linha ou em linhas em branco
    >eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

```js
//ruim
var foo = 0//•••••
var baz = 5//••
//•••••

//bom
var foo = 0
var baz = 5
```
- Não utilize multiplas linhas em branco
    >eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)
    
```js
// ruim
var x = 1



var y = 2

// bom
var x = 1

var y = 2
```
- Utilize ponto e virgula ao fim de cada instrução
    >O javascript aceita instruções sem ponto e vŕgula no fim, porém ele usa um conjunto de regras chamado [Automatic Semicolon Insertion] (https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) para determinar se ele é ou não deve considerar essa quebra de linha como o final de uma instrução e (como o nome indica) colocar um ponto-e-vírgula em seu código antes da quebra de linha, se achar que sim. No entanto, o ASI contém alguns comportamentos excêntricos e seu código será interrompido se o JavaScript interpretar incorretamente sua quebra de linha. Essas regras se tornarão mais complicadas à medida que novos recursos se tornarem parte do JavaScript. Explicitamente encerrar suas declarações e configurar seu linter para detectar ponto-e-vírgulas ausentes ajudará a impedir que você encontre problemas.

```js
// ruim - raises exception
const luke = {}
const leia = {}
[luke, leia].forEach(jedi => jedi.father = 'vader')

// ruim - raises exception
const reaction = "No! That’s impossible!"
(async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
    // ...
}())

// ruim - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
function foo() {
    return 
        'search your feelings, you know it to be foo'
}

// bom
const luke = {};
const leia = {};
[luke, leia].forEach((jedi) => {
    jedi.father = 'vader';
});

// bom
const reaction = "No! That’s impossible!";
(async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
    // ...
}());

// bom
function foo() {
    return 'search your feelings, you know it to be foo';
}
```
- Use camelCase para nome
    >eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)
    
```js
// ruim
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// bom
const thisIsMyObject = {};
function thisIsMyFunction() {}
```
- Use PascalCase para nomes de classes 
    > eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

```js
// ruim
function user(options) {
    this.name = options.name
}

const bad = new user({
    name: 'nope',
});

// bom
class User {
    constructor(options) {
        this.name = options.name
    }
}

const good = new User({
    name: 'yup',
})
```
- Não utilize underscore para denotar variáveis privadas, nem para metaprogramação
    > JavaScript não possui o conceito de privacidade em termos de propriedades ou métodos. Embora um sublinhado principal seja uma convenção comum para significar "privado", na verdade, essas propriedades são totalmente públicas e, como tal, fazem parte do seu contrato de API público. Essa convenção pode levar os desenvolvedores a pensar erroneamente que uma mudança não será considerada uma quebra ou que os testes não são necessários. (eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html))

```js
// ruim
this.__firstName__ = 'Panda'
this.firstName_ = 'Panda'
this._firstName = 'Panda'

// bom
this.firstName = 'Panda'

// bom, in environments where WeakMaps are available
// see https://kangax.github.io/compat-table/es6/#test-WeakMap
const firstNames = new WeakMap()
firstNames.set(this, 'Panda')
```
- Não salve a referencia de this
    > Prefira utilizar arrow function ou bind da função para manter o scopo
    
```js
// ruim
function foo() {
    const self = this
    return function () {
        console.log(self)
    }
}

// ruim
function foo() {
    const that = this
    return function () {
        console.log(that)
    }
}

// bom
function foo() {
    return () => {
        console.log(this)
    }
}
```

## Vue styleGuides - VUEstyleGuides

-   Mantenha uma ordem das funções padrão do VUE

    > Primeiramente, tudo que é externo, e será recebido por este componente (imports, props, components, mixins, directives). Em seguida, os dados que serão manipulados por toda a vida do componente (data, computed). Logo após, os filters que realizam geralmente a formatação de como os dados serão apresentados (obs: que não é aconselhável criar um filter por componente, e sim um que seja criado externamente e utilizado por outros componentes, como, por exemplo, criar um filter para formatar um CNPJ, CPF, etc). Posteriormente, um abaixo do outro (em ordem), todo ciclo de vida de um componente (beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, beforeDestroy, destroyed). Logo após, todos os métodos (methods), e finalmente os observadores (watch).

```js
//ruim
<script>
    export default {
        watch: {}
        mounted() {},
        beforeDestroy() {},
        directives: {},
        data() { return {} },
        filters: {},
        beforeCreate() {},
        methods: {},
        props: {},
        beforeUpdate() {},
        computed: {},
        destroy() {},
        components: {},
        update() {},
        mixins: [],
    }
</script>

//bom
<script>
    export default {
        props: {},
        components: {},
        mixins: [],
        directives: {},
        data() { return {} },
        computed: {},
        filters: {},
        beforeCreate() {},
        mounted() {},
        beforeUpdate() {},
        update() {},
        beforeDestroy() {},
        destroy() {},
        methods: {},
        watch: {}
    }
</script>
```

-   Sempre use o tipo correto(tipagem esplícita) dos atributos em props

    > Props de um componente podem ser escritas de algumas maneiras diferentes. Algumas delas há “tipagem” explícita. Porém, muitas vezes pela facilidade e agilidade, há pessoas que optam por criar seus componentes sem a “tipagem” explícita, o problema é que pode ocorrer confusões entre o tipo recebido.

```js
//O atributo pessoa espera um objeto

//ruim
props: {
    pessoa: ['Pessoa'];
}

//bom
props: {
    pessoa: {}
}
```

-   Não passe como parâmetro atributos globais

    > Atributos que estão acessíveis globalmente não tem a necessidade de serem passados como parâmetro, visto que podem ser vistos por qualquer função do componente.

```js
//ruim
data() {
    return {
        selectedTab: ''
    };
}
methods: {
    getSelected(tabName, selectedTab) {
        if (this.data.tabs[tabName] && this.data.tabs[tabName].selected) {
            this.selectedTab = tabName;
            return true;
        }
        return false;
    }
}

//bom
data() {
    return {
        selectedTab: ''
    };
}
methods: {
    getSelected(tabName) {
        if (this.data.tabs[tabName] && this.data.tabs[tabName].selected) {
            this.selectedTab = tabName;
            return true;
        }
        return false;
    }
}
```

-   Use apenas inglês para escrever seus códigos

    > Para manter a linearidade da leitura do código sempre escreva em inglês, isto é uma convenção para qualquer projeto profissional.

```js
//ruim
getPessoa()

//ruim
createEmpresa()

//bom
getAge()
```

-   Use sempre custom css variables para permitir que os estilos dos componentes sejam dinamizados externamente
    >Com variables css as regras css ficam desacopladas do javascript mantendo a organização e facilidade de manutenção do código.

```css
#div2 {
    background-color: var(--main-bg-color, #0f0f0f);
    color: var(--main-txt-color, red);
    padding: var(--main-padding, 100px);
}
```
## Resources

**Learning ES6+**

  - [Latest ECMA spec](https://tc39.github.io/ecma262/)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)
  - [Book](http://cangaceirojavascript.com.br)

**Tools && Other Style Guides**

  - Code Style Linters
    - [ESlint](https://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    - [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
  - Neutrino Preset - [@neutrinojs/airbnb](https://neutrinojs.org/packages/airbnb/)
  - [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
  - [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)
  - [StandardJS](https://standardjs.com)
