# Resolução Desafio 01.

Começamos analisando o código para saber qual lógica iremos isolar em um hook.
```js
import React, { useState } from 'react';

const Message = () => {
  const [isVisible, setIsVisible] = useState(true);

  return (
    <div>
      {isVisible && <p>Esta é uma mensagem qualquer.</p>}
      <button onClick={() => setIsVisible(!isVisible)}>
        {isVisible ? 'Esconder' : 'Mostrar'} mensagem
      </button>
    </div>
  );
};
export default Message;
```

Neste caso, iremos isolar a função que está no `OnClick` do botão.
```js
<button onClick={() => setIsVisible(!isVisible)}>
```

Então, criamos o nosso hook `useToogle`, respeitando a regra de nomenclatura para hooks, e criamos um estado usando o hook `useState` que recebe o valor incial de `true`

```js
import { useState } from 'react'

function useToogle() {
  const [isVisible, setIsVisible] = useState(true)
}

export default useToogle
```
O proximo passo é implementar a função que será chamada no `onClick` no botão, neste exemplo irei chamar essa função de `handleClick` .
```js
function handleClick(){
    setIsVisible(!isVisible)
  }
```
E por fim retornaremos um `array` [isVisible, handleClick], que retorna para o componente o estado atual do `isVisible` e a função que manipula este estado.

### Código final do hook useToogle
```js
import { useState } from 'react'

function useToogle() {
  const [isVisible, setIsVisible] = useState(true)
  
  function handleClick(){
    setIsVisible(!isVisible)
  }

  return [isVisible, handleClick]
}

export default useToogle;
```

# Resolução Desafio 02
Com o hook `isToogle` criado, agora temos que implementá-lo em nossa aplicação. O primeiro passo é importar o hook para o componente.
```js
import useToogle from './hooks/useToogle';
```
Próximo passo é deletar a lógica que cria e controla estados nesse componente e em seguida recuperar as informações trazidas pelo hook `isToogle`
```js
const [isVisible, handleClick] = useToogle();
```
Agora é so ajustar a função do `onClick` do botão para chamar a função `handleClick` resgatada no passo anterior
```js
<button onClick={() => handleClick()}>
```
### Código final do componente
```js
import useToogle from './hooks/useToogle';


const Message = () => {
  const [isVisible, handleClick] = useToogle();

  return (
    <div>
      {isVisible && <p>Esta é uma mensagem qualquer.</p>}
      <button onClick={() => handleClick()}>
        {isVisible ? 'Esconder' : 'Mostrar'} mensagem
      </button>
    </div>
  );
};

export default Message;
```

# Resolução Desafio 03
Começamos analisando o código para saber qual lógica iremos isolar em um hook.
```js
import React, { useState } from 'react';

const TodoList = () => {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');

  const handleSubmit = (event) => {
    event.preventDefault();
    setTodos([...todos, inputValue]);
    setInputValue('');
  };

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={inputValue}
          onChange={(event) => setInputValue(event.target.value)}
        />
        <button type="submit">Adicionar tarefa</button>
      </form>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
    </div>
  );
};

export default TodoList;
```
Neste caso, iremos isolar o controlador do input text e a função de submit do form. Tendo isso em mente iremos criar o hook `useTodoList`. Começamos criando os estados que vamos manipular usando o hook `useState`, um array de *todos* e o estado do *input*
```js
import { useState } from 'react'

function useTodoList() {
const [todos, setTodos] = useState([])
const [inputValue, setInputValue] = useState('')
}

export default useTodoList
```
O proximo passo é criar a função de `submit` do form, neste caso daremos o nome de `handleSubmit`
```js
function handleSubmit(event){
  event.preventDefault();
  setTodos([...todos, inputValue]);
  setInputValue('');
}
```
> De olho na dica 👀 :  A primeira linha, "event.preventDefault()", impede o comportamento padrão de recarregar a página quando o formulário é enviado.

Por fim retonarmos um array com as informações que queremos levar para o outro componente.
```js
return [todos, inputValue, setInputValue, handleSubmit]
```
> Note que a função `setTodos` não está sendo retornada no array, neste caso o retorno dela não é necessário, pois ela está sendo chamada dentro do `handleSubmit`

### Código final do hook `useTodoList`
```js
import { useState } from 'react'

function useTodoList() {
const [todos, setTodos] = useState([])
const [inputValue, setInputValue] = useState('')

function handleSubmit(event){
  event.preventDefault();
  setTodos([...todos, inputValue]);
  setInputValue('');
}

return [todos, inputValue, setInputValue, handleSubmit]
}

export default useTodoList
```

Com o hook criado, agora vamos implementá-lo em nossa aplicação. Começamos sempre importando o nosso hook para o componente.
```js
import useTodoList from './hooks/useTodoList';
```
Próximo passo é deletar a lógica que cria e controla estados nesse componente e em seguida recuperar as informações trazidas pelo hook `useTodoList`
```js
const [todos, inputValue, setInputValue, handleSubmit] = useTodoList();
```
Agora ajustamos a função que é chamada no `onSubmit` do formulário

Código final do componente
```js
import useTodoList from './hooks/useTodoList';

const TodoList = () => {
  const [todos, inputValue, setInputValue, handleSubmit] = useTodoList();

  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={inputValue}
          onChange={(event) => setInputValue(event.target.value)}
        />
        <button type="submit">Adicionar tarefa</button>
      </form>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
    </div>
  );
};

export default TodoList;
```

# Gabarito Fixando o conteúdo 
### 1 - Oque é um custom hook?

Alternativa correta:
c)  Um custom hook é uma ferramenta de gerenciamento de estado do React que permite compartilhar lógica entre vários componentes.
### 2- Qual é a diferença entre um custom hook e um hook padrão do React?
Alternativa correta:
d) Hooks padrão não podem ser personalizados, enquanto custom hooks podem ser.

### 3- Considere as seguintes afirmativas, depois assinale qual opção que demonstra as afirmativas corretas.
Alternativa correta: 
b) Apenas IV