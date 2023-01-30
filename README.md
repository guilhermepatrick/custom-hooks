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

Sabendo dessas regras, estamos prontos para criar o nosso primeiro custom hook
