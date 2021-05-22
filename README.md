# Tutorial Vue 3

Ao utilizar o visual studio code, instalar a extensão "es6-string-html"

Dentro de index.html importar o vue.js

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

Com o app criado, importado e montado, podemos começar a mostrar os dados

index.html:

```javascript
<div id="app">
  <h1>{{ product }}</h1>
</div>
```




