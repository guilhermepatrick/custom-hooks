# Custom Hooks
### O que vamos aprender ?
Hoje você irá aprender sobre a criação de hooks personalizados no React! Isso fará com que seu código fique mais **organizado e legível**, pois você poderá reutilizar o mesmo bloco de código em diferentes componentes.

### Você será capaz de :
- Criar Hooks customizados

### Por que isso é importante:
Os hooks do React são uma ferramenta valiosa, e às vezes as necessidades de uma aplicação web exigem soluções personalizadas. Os custom hooks permitem que você reutilize código comum entre componentes, eles também permitem que você compartilhe funcionalidades entre componentes sem ter que passar props complexas ou escrever código repetitivo.

## Utilizando um custom Hook na prática
>Custom Hooks são recursos poderosos que permitem reutilizar código para realizar requisições de API. Eles tornam mais fácil e eficiente realizar várias chamadas de API, sejam elas na mesma API ou em diferentes APIs.
#### Considere o seguinte código:
```js
1     import { useEffect, useState } from 'react';
2     
3     function App() {
4     const [useCatData, setCatData] = useState([])
5     const [useDogData, setDogData] = useState([])
6     
7     const fetchDog=()=>{
8       fetch("https://api.thedogapi.com/v1/images/search")
9       .then((res) => res.json())
10       .then((data) => setDogData(data));    
11     }
12     const fetchCat=()=>{
13       fetch("https://api.thecatapi.com/v1/images/search")
14       .then((res) => res.json())
15       .then((data) => setCatData(data));       
16     }
17     useEffect(()=>{
18       fetchDog()
19       fetchCat()
20     },[])
21       return (
22         <div className='content'>
23           {useDogData.map((dog)=> <img key={dog} src={dog.url} alt="Dog" /> )}
24           {useCatData.map((cat)=> <img key={cat} src={cat.url} alt="Cat" /> )}
25         </div>
26       );
27     }
28     export default App;
```
### Entendendo o código
Este é um código de um componente React que mostra imagens de gatos e cães a partir de duas APIs diferentes.

Na linha 4 e 5, o componente usa o hook `useState` para inicializar dois estados: `useCatData` e `useDogData`. Ambos serão preenchidos com dados da API posteriormente.

Em seguida, temos duas funções `fetchDog` e `fetchCat` na linhas 7 a 11 e 12 a 16 respectivamente, que são usadas para fazer requisições às APIs de gatos e cães. As respostas são armazenadas nos estados `useCatData` e `useDogData`.

Na linha 17, o hook `useEffect` é usado para fazer a requisição inicial de ambas as APIs, que são chamadas na linha 18 e 19. O hook é configurado com um array vazio como segundo argumento, o que significa que será executado apenas uma vez, quando o componente é montado.

Por fim, o componente renderiza uma div content na linha 22, e dentro dela duas listas de imagens, uma para gatos e outra para cães. As imagens são renderizadas usando o map para percorrer o estado `useCatData` e `useDogData`, e renderizar uma imagem para cada item da lista, com a URL da imagem como fonte.

### Criando um custom Hook
Ao analisar o código, vemos que podemos colocar as duas funções de `fetch` que buscam as informações das APIs em um `hook` personalizado. Isso significa que podemos separar a lógica de busca e usá-la em **outros componentes** conforme a nossa aplicação cresce, tornando o nosso código mais organizado e fácil de manter.

>**Atenção** 💡 : Temos algumas regrinhas na hora de criar nosso hook personalizado:

- Nome: Inicie com `use` seguido do nome da funcionalidade, ex: "**use**FetchData".
- Custom Hooks **devem manipular** outros hooks, caso contrário são meras funçoes JavaScript

Sabendo dessas regras, estamos prontos para criar o nosso primeiro custom hook.
Para manter nossos hooks organizados, precisamos criar uma pasta chamada **"hooks"** no diretório do nosso projeto. Desta forma, poderemos manter todos os `hooks` que criarmos durante o desenvolvimento do código em um único local, facilitando a **navegação e manutenção do código**.

![i](https://imageup.me/images/c13d6772-4aea-4e1f-b1e3-49c1d6424ce5.jpeg)

E seguindo a regra de nomenclatura, criamos o hook **useFetchData.js**
### Estrutura do custom Hook
```js
1 import { useState, useEffect } from 'react';
2
3 const useFetchData = () => {
4   const [dogData, setDogData] = useState([]);
5   const [catData, setCatData] = useState([]);
6
7   useEffect(() => {
8     const fetchDog = () => {
9       fetch("https://api.thedogapi.com/v1/images/search")
10         .then((res) => res.json())
11         .then((data) => setDogData(data));
12     };
13
14     const fetchCat = () => {
15       fetch("https://api.thecatapi.com/v1/images/search")
16         .then((res) => res.json())
17         .then((data) => setCatData(data));
18     };
19
20     fetchDog();
21     fetchCat();
22   }, []);
23
24   return [dogData, catData];
25 };
26
27 export default useFetchData;
```

> Obs  💡 : Lembrando da nossa segunda regrinha na criação do custom hook. Eles devem manipular outros hooks, tendo isso em mente vamos para a análise do código.

Na linha 4 e 5, dois estados são criados usando o hook `useState` para armazenar as informações dos cães e gatos, respectivamente. 

Na linha 7, o hook `useEffect` é usado para garantir que a busca dos dados ocorra apenas uma vez (com o array vazio na segunda argumento).

Na linha 8 a 12, é criada uma função `fetchDog` que realiza o fetch dos dados da API de cães e atualiza o estado de `dogData`.

Na linha 14 a 18, é criada uma função semelhante `fetchCat` para buscar os dados da API de gatos.

Na linha 20 e 21, as duas funções são chamadas.

Por fim nas linha 24 , o hook retorna o array `[dogData, catData]`, permitindo que as informações sejam acessadas em outros componentes. Este hook pode ser importado e reutilizado em outros componentes da aplicação, garantindo a organização e a centralização da lógica de busca de informações.

Na linha 27, o `hook` é exportado para ser usado em outros componentes.

### Refatorando o código para usar nosso custom hook
Com nosso `hook` pronto, estamos prontos para a refatoração, isolamos toda a lógica que faz o `fetch` no hook `useFetchData`, que por sua vez nos retorna um array com a `dogData` e `catData`. Começamos importanto o hook para o componente:
``` js
 import useFetchData from './hooks/useFetchData';
```
Agora vamos substituir toda a lógica de `fetch` e os estados que criamos usando o `useState`.
##### Então todo esse codigo:

```js
const fetchDog=()=>{
  fetch("https://api.thedogapi.com/v1/images/search")
  .then((res) => res.json())
  .then((data) => setDogData(data));    
}
const fetchCat=()=>{
  fetch("https://api.thecatapi.com/v1/images/search")
  .then((res) => res.json())
  .then((data) => setCatData(data));       
}
useEffect(()=>{
  fetchDog()
  fetchCat()
},[])
```
Será sobrescrito por apenas essa linha, que trás para o componente toda a informação obtida pelo hook `useFetchData`
```js
const [dogData, catData] = useFetchData();
```
Por fim, ajustamos os nomes das variáveis que estão dentro do retorno para serem renderizadas corretamente usando utilizando o método `map`
```js
return (
    <div>
      {dogData.map((dog)=> <img key={dog} src={dog.url} alt="Dog" /> )}
      {catData.map((cat)=> <img key={cat} src={cat.url} alt="Cat" /> )}
    </div>
  );
}
```
Agora com o nosso código organizado, sabemos podemos executar o `fetch` dessas informacoes a partir de ***qualquer*** componente.
### Antes da refatoração o código estava assim :
```js
1     import { useEffect, useState } from 'react';
2     
3     function App() {
4     const [useCatData, setCatData] = useState([])
5     const [useDogData, setDogData] = useState([])
6     
7     const fetchDog=()=>{
8       fetch("https://api.thedogapi.com/v1/images/search")
9       .then((res) => res.json())
10       .then((data) => setDogData(data));    
11     }
12     const fetchCat=()=>{
13       fetch("https://api.thecatapi.com/v1/images/search")
14       .then((res) => res.json())
15       .then((data) => setCatData(data));       
16     }
17     useEffect(()=>{
18       fetchDog()
19       fetchCat()
20     },[])
21       return (
22         <div className='content'>
23           {useDogData.map((dog)=> <img key={dog} src={dog.url} alt="Dog" /> )}
24           {useCatData.map((cat)=> <img key={cat} src={cat.url} alt="Cat" /> )}
25         </div>
26       );
27     }
28     export default App;
```
### Após a refatoração:
```js
1 import './App.css';
2 import useFetchData from './hooks/useFetchData';
3
4 function App() {
5 const [dogData, catData] = useFetchData();
6 
7 return (
8   <div>
9     {dogData.map((dog)=> <img key={dog} src={dog.url} alt="Dog" /> )}
10    {catData.map((cat)=> <img key={cat} src={cat.url} alt="Cat" /> )}
11  </div>
12 );
13 }
14 export default App;
```
