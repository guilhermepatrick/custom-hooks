# Custom Hooks
### O que vamos aprender ?
Hoje você irá aprender sobre a criação de hooks personalizados no React! Isso fará com que seu código fique mais **organizado e legível**, pois você poderá reutilizar o mesmo bloco de código em diferentes componentes.

### Você será capaz de :
- Criar Hooks customizados

### Por que isso é importante:
Os hooks do React são uma ferramenta valiosa, e às vezes as necessidades de uma aplicação web exigem soluções personalizadas. Os custom hooks permitem que você reutilize código comum entre componentes, eles também permitem que você compartilhe funcionalidades entre componentes sem ter que passar props complexas ou escrever código repetitivo.

### Vamos para nossa aplicação
```js
1 import React, { useState } from 'react';
2 import './Score.css';
3 
4 const Score = () => {
5   const [score1, setScore1] = useState(0);
6   const [score2, setScore2] = useState(0);
7 
8   const incrementScore1 = () => setScore1(score1 + 1);
9   const decrementScore1 = () => setScore1(score1 - 1);
10  const incrementScore2 = () => setScore2(score2 + 1);
11  const decrementScore2 = () => setScore2(score2 - 1);
12 
13  return (
14    <div className="score-container">
15      <div className="player">
16        <p>Jogador 1: {score1} pts</p>
17        <button onClick={incrementScore1}>+1 Ponto</button>
18        <button onClick={decrementScore1}>-1 Ponto</button>
19      </div>
20      <div className="player">
21        <p>Jogador 2: {score2} pts</p>
22        <button onClick={incrementScore2}>+1 Ponto</button>
23        <button onClick={decrementScore2}>-1 Ponto</button>
24      </div>
25    </div>
26  );
27 };
28 
29 export default Score;
```
Esse código representa um componente React para um contador de placares. Ele usa o hook `useState` para manter o estado dos placares de dois jogadores. O componente retorna uma estrutura de divisões que exibe os placares dos jogadores e botões para aumentar ou diminuir o placar.

Analisando o código rapidamente, percebemos que as funções que gerenciam os placares são iguais. Portanto, podemos criar um hook personalizado para separar essa lógica do componente.
```js
  const [score1, setScore1] = useState(0);
  const [score2, setScore2] = useState(0);

  const incrementScore1 = () => setScore1(score1 + 1);
  const decrementScore1 = () => setScore1(score1 - 1);
  const incrementScore2 = () => setScore2(score2 + 1);
  const decrementScore2 = () => setScore2(score2 - 1);
```
## Criando o custom Hook
>**Atenção** 💡 : Temos algumas regrinhas na hora de criar nosso hook :

- Nome: Inicie com `use` seguido do nome da funcionalidade, ex: "**use**Score".
- Custom Hooks **devem manipular** outros hooks, caso contrário são meras funçoes JavaScript

Sabendo dessas regras e qual lógica queremos isolar em nosso código, estamos prontos para criar o nosso primeiro custom hook.
Para manter nossos hooks organizados, podemos criar uma pasta chamada **"hooks"** no diretório do nosso projeto. Desta forma, poderemos manter todos os `hooks` que criarmos durante o desenvolvimento do código em um único local, facilitando a **navegação e manutenção do código**.

Tudo certo, vamos criar então o nosso hook `useScore`, respeitando assim a regra de nomenclatura.

Após criação do arquivo js, vamos à estrutura do código. Lembrando da nossa segunda regra, o nosso hook deve manipular outros hooks, nesse caso será o hook `useState`.
```js
1 import { useState } from 'react';
2
3 const useScore = () => {
4 const [score, setScore] = useState(0);
```
Então implementamos a lógica de `incrementScore` e `decrementScore` usando a função de manipulção de estado `setScore` que criamos na **linha 4**
```js
const incrementScore = () => setScore(score + 1);
const decrementScore = () => setScore(score - 1);
```
Por fim fazemos o `return`, que irá retornar para o componente onde for chamado um array com as seguintes informações:
- O estado atual de `score`
- A função que incrementa
- A função que decrementa
```js
return [score, incrementScore, decrementScore];
```
o Código completo do nosso hook `useScore` no final das implementações então ficará assim:
```js
1 import { useState } from 'react';
2 
3 const useScore = () => {
4 const [score, setScore] = useState(0);
5 
6 const incrementScore = () => setScore(score + 1);
7 const decrementScore = () => setScore(score - 1);
8 
9 return [score, incrementScore, decrementScore];
10 };
11 
12 export default useScore;
```
# Refatorando o código para usar nosso hook
O primeiro passo para refatorar o nosso código é importar o hook que criamos para o componente.
```js
import useScore from './hooks/useScore';
```
Agora podemos deletar sem medo toda a lógica de manipulação de estado que está presente nesse componente. Vamos substituir esses estados por novos estados manipulados pelo hook `useScore`.
```JS
const [score1, incrementScore1, decrementScore1] = useScore();
const [score2, incrementScore2, decrementScore2] = useScore();
```
O restante do componente mántem sua mesma estrutura, seu código final terá o seguinte formato:
```js
1 import React from 'react';
2 import useScore from './hooks/useScore';
3 import './Score.css';
4 
5 const Score = () => {
6 const [score1, incrementScore1, decrementScore1] = useScore();
7 const [score2, incrementScore2, decrementScore2] = useScore();
8 
9 return (
10 <div className="score-container">
11   <div className="player">
12     <p>Jogador 1: {score1} pts</p>
13     <button onClick={incrementScore1}>+1 Ponto</button>
14     <button onClick={decrementScore1}>-1 Ponto</button>
15   </div>
16   <div className="player">
17     <p>Jogador 2: {score2} pts</p>
18     <button onClick={incrementScore2}>+1 Ponto</button>
19     <button onClick={decrementScore2}>-1 Ponto</button>
20   </div>
21 </div>
22 );
23 };
24 
25 export default Score;
```
Com isso criamos nosso primeiro hook, deixamos nosso código organizado e limpo, e agora qualquer componente  na nossa aplicação consegue acessar de maneira rápida e fácil a lógica de score que isolamos no hook `useScore`.

## Utilizando hooks para fazer chamadas à APIs
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

### Criando o Hook
Ao analisar o código, vemos que podemos colocar as duas funções de `fetch` que buscam as informações das APIs em um `hook` personalizado. Isso significa que podemos separar a lógica de busca e usá-la em **outros componentes** conforme a nossa aplicação cresce, tornando o nosso código mais organizado e fácil de manter.

E seguindo a regra de nomenclatura, criamos o hook **useFetchData.js**
### Estrutura do useFetchData.js
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
import './App.css';
import useFetchData from './hooks/useFetchData';

function App() {
const [dogData, catData] = useFetchData();

  return (
    <div>
      {dogData.map((dog)=> <img key={dog} src={dog.url} alt="Dog" /> )}
      {catData.map((cat)=> <img key={cat} src={cat.url} alt="Cat" /> )}
    </div>
  );
}
export default App;
```
