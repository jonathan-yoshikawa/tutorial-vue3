# Tutorial Vue 3

Ao utilizar o visual studio code, instalar a extensão "es6-string-html"

Dentro de index.html importar o vue.js
```javascript
<html>
  <head>
      <script src="./js/vue.global.js"></script>
  </head> 
</html>
```

## Criando um app Vue

Em um arquivo main.js criar o app com:
```javascript
const app = Vue.createApp({
  data() {
    return {
      product: 'Socks'
    }
  }
})
```
Importar o app Vue dentro do index.html
```javascript
<!-- Import App -->
<script src="./main.js"></script>
```

## Montando o app

Com o app criado, precisamos montar o app dentro do DOM em index.html

```javascript
<!-- Mount App -->
<script>
  const mountedApp = app.mount('#app')
</script>
```
## Mostrando dados

Com o app criado, importado e montado, podemos começar a manipular os dados

index.html:

```javascript
<div id="app">
  <h1>{{ product }}</h1>
</div>
```

## Attribute Binding

```javascript
<img v-bind:src="image"> <! -- src attribute bound to the image data -->
```
Forma abreviada para o v-bind
```javascript
<img :src="image"> 
```
## Renderização condicional
```javascript
<p v-if="inventory > 10">In Stock</p>
<p v-else-if="inventory <= 10 && inventory > 0">Almost sold out!</p>
<p v-else>Out of Stock</p>
```
Mostrar e esconder
```javascript
<p v-show="inStock">In Stock</p>
```

A diretiva v-show altera o style **visibility** ao invés de adicionar ou remover o elemento no DOM

Caso seja necessário mostrar ou esconder um elemento com muita frequência a diretiva v-show é mais indicada do que que v-if

## Renderizando Listas

main.js
```javascript
const app = Vue.createApp({
    data() {
        return {
            ...
            details: ['50% cotton', '30% wool', '20% polyester']
        }
    }
})
```

index.html
```javascript
<ul>
  <li v-for="detail in details">{{ detail }}</li>
</ul>
```
Atributo key
```javascript
<div v-for="variant in variants" :key="variant.id">{{ variant.color }}</div>
```

Nós estamos usando uma abreviação de v-bind para ligar o id com o atributo key. Isso dá a cada elemento do DOM um chave única.

Quando os dados mudam, o Vue usa essas keys para saber quais elementos HTML remover, atualizar ou se precisa criar algum outro elemento.

##Eventos

```javascript
<button class="button" v-on:click="logic to run">Add to Cart</button>
``` 
Abreviação para v-on
```javascript
<button class="button" @click="addToCart">Add to Cart</button>
```

## Class & Style

Style Biding
```javascript
<div :style="{ backgroundColor: variant.color }">
</div>
```
Aqui estamos setando o background-color com o valor da propriedade variant.color

Como estamos utilizando objetos javascript o nome da propriedade CSS deve ser usado no estilo camelCased. Se tentar usar background-color o javascript interpretra como uma operação matemática. Se quiser usar a notação 'kebab-cased' deve-se usar asas simples, igual a:

```javascript
<div :style="{ 'background-color': variant.color }></div>
```

Ambas opçoes funcionam

## Class Binding
```javascript
<button 
  class="button" 
  :class="{ disabledButton: !inStock }" 
  :disabled="!inStock" 
  @click="addToCart">
  Add to Cart
</button>
```

Se *inStock* for false a class disableButton será aplicada

Operação ternária

```javascript
<div :class="[isActive ? activeClass : '']">
```

## Computed Properties

main.js
```javascript
...
computed: {
  title() {
    return this.brand + ' ' + this.product
  }
}
```

Similiar as functions, porém computed properties utiliza cache

## Components & Props

Aplicações Vue são blocos de componentes, parecidos com o Lego em que você pode plugar uns aos outros. 

Sintaxe para criar um component

components/ProductDisplay.js
```javascript 
app.component('product-display', {})
```

O primeiro argumento é o nome do componente e o segundo argumento é um objeto que configura o component (similar ao objeto usado para configurar nosso app Vue raiz)

### Template

Todo o código html deve ser colocado aqui

```javascript
app.component('product-display', {
  template: 
    /*html*/ 
    `<div class="product-display">
      <div class="product-container">
        <div class="product-image">
          <img v-bind:src="image">
        </div>
        <div class="product-info">
          <h1>{{ title }}</h1>
  
          <p v-if="inStock">In Stock</p>
          <p v-else>Out of Stock</p>
  
          <div 
            v-for="(variant, index) in variants" 
            :key="variant.id" 
            @mouseover="updateVariant(index)" 
            class="color-circle" 
            :style="{ backgroundColor: variant.color }">
          </div>
          
          <button 
            class="button" 
            :class="{ disabledButton: !inStock }" 
            :disabled="!inStock" 
            v-on:click="addToCart">
            Add to Cart
          </button>
        </div>
      </div>
    </div>`
})
```

### Dados e métodos

```javascript
app.component('product-display', {
  template: 
    /*html*/ 
    `<div class="product-display">
      ...
    </div>`,
  data() {
    return {
        product: 'Socks',
        brand: 'Vue Mastery',
        selectedVariant: 0,
        details: ['50% cotton', '30% wool', '20% polyester'],
        variants: [
          { id: 2234, color: 'green', image: './assets/images/socks_green.jpg', quantity: 50 },
          { id: 2235, color: 'blue', image: './assets/images/socks_blue.jpg', quantity: 0 },
        ]
    }
  },
  methods: {
      addToCart() {
          this.cart += 1
      },
      updateVariant(index) {
          this.selectedVariant = index
      }
  },
  computed: {
      title() {
          return this.brand + ' ' + this.product
      },
      image() {
          return this.variants[this.selectedVariant].image
      },
      inStock() {
          return this.variants[this.selectedVariant].quantity
      }
  }
})
```

### Importando o componente

index.html
```javascript
<!-- Import Components -->
<script src="./components/ProductDisplay.js"></script>
```

Depois de importado podemos usar no nosso template
```javascript
<div id="app">
  <div class="nav-bar"></div>

  <div class="cart">Cart({{ cart }})</div>
  <product-display></product-display>
</div>
```

### Props

Usados para passar dados do componente pai para o componente filho

components/ProductDisplay.js
```javascript
app.component('product-display', {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  ...
}
```

index.html
```javascript
<div id="app">
  <div class="nav-bar"></div>

  <div class="cart">Cart({{ cart }})</div>
  <product-display :premium="premium"></product-display>
</div>
```
