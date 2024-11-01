# JavaScript

## Useful

```javascript title="位數逗號分隔"
console.log(n.toLocaleString());
```

```text
parseInt('') => NaN
'' === '' => true
NaN === NaN => false
```

```javascript
// regex: Capturing Group
/\[(.*)\]__(.*)__([0-9]*)/.exec('[10]__foo__234')
// => [ "[10]__fofo__234", "10", "fofo", "234" ]
```
[Capturing group: (...) - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Regular_expressions/Capturing_group)

## Vanilla
- [My top JavaScript utilities, in just One Line of Code! | Phuoc Nguyen](https://phuoc.ng/collection/1-loc/)
- [The JavaScript fetch() method | Go Make Things](https://gomakethings.com/the-javascript-fetch-method/)
- [FormData API | 12 Days of Web](https://12daysofweb.dev/2022/formdata-api/)

```javascript title="ready (ignore old IE8, IE9)"
document.addEventListener('DOMContentLoaded', function () {
  // do something here ...
}, false);
```

```javascript title="如果要等所有 external resource (css, images) loaded"
window.addEventListener('load', function () {
  // do something here ...
}, false);
```

### Form

- [Form Validation Part 1: Constraint Validation In HTML | CSS-Tricks](https://css-tricks.com/form-validation-part-1-constraint-validation-html/)

### Patterns
- [Boilerplates | The Vanilla JS Toolkit](https://vanillajstoolkit.com/boilerplates/#Revealing-Module-Pattern)
- [How to add private variables and helper methods to your vanilla JS constructor patterns | Go Make Things](https://gomakethings.com/how-to-add-private-variables-and-helper-methods-to-your-vanilla-js-constructor-patterns/)

#### IIFE

[The many ways to write an Immediately Invoked Function Expression (IIFE) in JavaScript | Go Make Things](https://gomakethings.com/the-many-ways-to-write-an-immediately-invoked-function-expression-iife-in-javascript/)

```javascript
(function () {
	// Code goes here...
})();
```

#### Revealing Module Pattern

[The vanilla JS revealing module pattern | Go Make Things](https://gomakethings.com/the-vanilla-js-revealing-module-pattern/)

a collection of helper functions

```javascript
let calculator = (function () {

	// ...

	/**
	 * Divide two or more numbers
	 * @param {...Number} nums The numbers to divide
	 */
	function divide (...nums) {
		// ...
	}

	return {add, subtract, multiply, divide};

})();
```



#### Constructor Pattern

[The vanilla JS constructor pattern | Go Make Things](https://gomakethings.com/the-vanilla-js-constructor-pattern/) 

like jQuery

#### Class Pattern

[The vanilla JS Class pattern | Go Make Things](https://gomakethings.com/the-vanilla-js-class-pattern/)

s- static

- private


## console

[Let's be fancy with a console signature - DEV Community](https://dev.to/basilebong/let-s-be-fancy-with-a-console-signature-dad)

```javascript
var consoleSignatureStyle = "font-size: 16px;" +
  "background: linear-gradient(to right, #e66465, #9198e5);" +
  "color: white;" +
  "text-align: center;" +
  "padding: 10px 15px;" +
  "width: 100%;" +
  "border-radius: 20px;";

var consoleSignatureText = "%cDon't steal my cookies! 🍪";

console.log(consoleSignatureText, consoleSignatureStyle);
```

## async

- [Practical Async JavaScript - DEV Community](https://dev.to/bgdnvarlamov/practical-async-javascript-ep5?context=digest)

### Tools

[NProgress: slim progress bars in JavaScript](https://rstacruz.github.io/nprogress/)

## Package manager

### Yarn

不建議用`npm install -g yarn`，Node.js 14.9/16.9後就有`corepack enable`的指令。enable後，就有yarn的指令可以用了

```bash
corepack enable
```



## Svelte

原本的 `npm create svelte@latest my-app` 會裝一堆東西，包括sveltekit，用vite安裝純Svelte就好

只要client-side

```bash
npm init vite myapp
```
選單 (JavaScript, TypeScript)

```bash
cd myapp
npm install
npm run dev
```
