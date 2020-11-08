# Web Components 

## Basic Custom Component
**Definition**
```js
class Header extends HTMLElement{
    connectedCallback(){
        this.innerHTML =`<nav></nav>`
    }
}

customElements.defin('main-header', Header)
```
**Usage**
```html
<main-header></main-header>
```
