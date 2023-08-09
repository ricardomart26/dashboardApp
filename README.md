# dashboardApp

Como funciona a arvore de componentes do react?

### REACT HOOKS ###

#### useState ####

#### useContext ####

##### EN ######

`useContext` is a React hook that allows you to access the context values that have been defined using the `React.createContext` API. Context provides a way to pass data through the component tree without having to pass props down manually at every level. It's particularly useful for sharing data that is needed by multiple components at different levels of the component tree.

Here's how `useContext` works:

1. **Creating a Context**: First, you need to create a context using the `React.createContext` function. This function returns an object with a `Provider` component and a `Consumer` component (though `useContext` simplifies the consumer usage).

2. **Providing Context Value**: Wrap the part of your component tree that should have access to the context value with the `Provider` component. This is usually done higher up in the tree.

3. **Consuming Context Value**: Use the `useContext` hook within components that are descendants of the `Provider` to access the context value directly.

Here's a basic example of how to use `useContext`:

```jsx
import React, { createContext, useContext } from 'react';

// Step 1: Create a context
const ThemeContext = createContext();

// Step 2: Provide the context value
const App = () => {
    const theme = 'dark'; // or 'light'
    return (
        <ThemeContext.Provider value={theme}>
            <Header />
            <MainContent />
        </ThemeContext.Provider>
    );
};

// Step 3: Consume the context value
const Header = () => {
    const theme = useContext(ThemeContext);
    return <header style={{ backgroundColor: theme === 'dark' ? 'black' : 'white' }}>Header</header>;
};

const MainContent = () => {
    const theme = useContext(ThemeContext);
    return <div style={{ color: theme === 'dark' ? 'white' : 'black' }}>Main Content</div>;
};
```

In this example, the `ThemeContext` is created using `createContext`. The `App` component provides the theme value to the context using the `Provider` component. The `Header` and `MainContent` components then use the `useContext` hook to directly access the theme value from the context without needing to pass it as a prop.

Important points to note about `useContext`:

- You can use `useContext` in functional components only.
- `useContext` accepts a context object as its argument and returns the current context value.
- If the context value changes, the component that uses `useContext` will re-render.
- If the component using `useContext` is not wrapped in a `Provider` component higher up in the tree, it will get the default value provided when creating the context.

`useContext` is particularly handy when dealing with global themes, user authentication, localization, and other scenarios where data needs to be shared across multiple components without the hassle of passing it through props.

##### PT #####

`useContext` é um hook do react que permite aceder a valores de context que foram definidos ao usar a API `React.createContext`. Context dá uma forma de passar data atraves da arvores de components sem ter que passar os props de forma manual a cada nivel. É usada para partilhar dados que são precisos em multiplos componentes em niveis diferentes da arvore de componentes.

Como o `useContext` funciona:

1. **Criar um contexto**: Primeiro precisamos de criar um context ao usar a função `React.createContext`. Esta função retorna um componente `Provider` e o componente `Consumer` (Apesar de o `useContext` simplifica o uso do consumer).

2. **Fornecer o valor do contexto**: Envolve a parte da arvore de componentes que podem aceder ao valor do contexto com o componente `Provider`. Isto é normalmente feito mais acima na arvore.

3. **Consumir o valor do contexto**: Usar o hook `useContext` dentro dos componentes que são descendentes do `Provider` para aceder ao valor do contexto diretamente.

Um exemplo basico de como usar o `useContext`:

```jsx

import React, { createContext, useContext } from "react";

// Passo 1: Criar o contexto
const ThemeContext = createContext();

// Passo 2: Fornecer o valor do contexto
const App = () => {
    const theme = 'dark'; // ou 'light'
    return (
        <ThemeContext.Provider value={theme}>
            <Header/>
            <MainContent/>
        </ThemeContext.Provider>
    );
}

const Header = () => {
    const theme = useContext(ThemeContext);
    return <header style={{backgroundColor: theme === 'dark' ? 'black' : 'white' }}> Header</header>;
}

const MainContent = () => {
    const theme = useContext(ThemeContext);
    return <div style={{ color: theme === 'dark' ? 'white' : 'black' }}>Main Content</div>;
};
```

Neste exemplo, o `ThemeContext` é criado atraves do `createContext`. O componente `App` fornece o valor do tema ao contexto atravez do componente `Provider`. O `Header` e o componente `MainContent` usam o hook `useContext` para aceder diretamente ao valor do tema sem ser necessario passar o valor atraves de props.

Pontos importantes sobre o `useContext`:

- Podes usar o `useContext` em apenas em componente funcionais
<!-- TODO: Perceber o que esta frase significa -->
- `useContext` aceita um objeto de contexto como argumento e retorna o valor atual do valor do contexto
- Se o valor do contexto for alterado, o componente que usa o `useContext` vai ser re-renderizado.
- Se o componente que usa o `useContext` não estiver envolvido no componente do `Provider` mais acima na arvore, vai receber o valor padrão do contexto.

`useContext` é particularmente util quando lidamos com temas globais, autenticação de utilizador, localização e outros cenarios que os dados precisem de ser partilhados por multiplos componentes sem o trabalho de passar atraves de props.


#### useMemo ####

##### EN #####

**Teste**
`useMemo` is a hook in React that is used to memoize the result of a function so that it is only recomputed when its dependencies change. It is primarily used to optimize performance by avoiding unnecessary re-computations of expensive operations.

Here's how `useMemo` works and why it's beneficial:

**Avoiding Unnecessary Recomputations**: In React, when a component re-renders, all the code within its function body is executed again, including any computations or functions. Sometimes, certain computations are expensive in terms of CPU usage or memory, and re-computing them on every render can lead to performance issues.

**Memoization**: Memoization is a technique where the results of a function are cached based on its input arguments. If the same input arguments are provided again, the cached result is returned instead of recomputing the result. `useMemo` provides a way to apply memoization to a function's result within a React component.

**Dependencies**: The `useMemo` hook takes two arguments, the first argument is the function whose result you want to memoize, and the second argument is an array of dependencies. The result of the function will only be recalculated if any of the dependencies in the array have changed since the last render.

Here's a basic example of how to use `useMemo`:

```jsx

import React, { useMemo } from 'react';

const ExpensiveComponent = ({ value }: { value: number }) => {

    // Esta função só vai ser recomputada se o `value` for alterado
    const expensiveResult = useMemo(() => {
        console.log('Ẽxpensive computation');
        // Performa alguma computação exigente que usa a `value`
        return (value * 2);
    }, [value]);

    return (
        <div>
            <p>Expensive result: {expensiveResult}</p>
        </div>
    )
}
```
 
In this example, the expensiveResult value is calculated only when the value prop changes. If the ExpensiveComponent re-renders for some other reason (like when other props or state change), the cached result will be used, and the expensive calculations won't be repeated.

In summary, useMemo is a React hook that enables memoization of a function's result based on its dependencies, helping to optimize performance by avoiding unnecessary re-computations.

##### PT #####

useMemo é um hook no react que é usado para memorizar o resultado de uma função que só é recomputada quando a dependencia muda.
É primariamente usado para otimizar o desempenho ao evitar recomputações desnecessarias de operações caras.

Como o useMemo funciona e como é benefico:

**Evitar recomputações desnecessarias**: Em react, quando um componente é renderizado, todo o codigo é executado novamente, incluindo todas as computações ou funções. As vezes, certas computações são dispendiosas em termos de uso de CPU ou memoria, e recomputador em cada renderização pode levar a problemas de desempenho.

**Memorização**: Memorização é uma tecnica onde o resultado de uma função é guardada em cache baseado no input. Se o mesmo input são fornecidos de novo, o resultado em cache é returnado em vez de recomputar o resultado. O useMemo fornece uma forma de aplicar a memorização para o resultado de uma função dentro de um componente do react.

**Dependencias**: O useMemo recebe dois argumentos, o primeiro argumento é uma função do qual o resultado queres memorizar, e o segundo argumento é um array de dependencias. O resultado da função só vai ser recalculado se alguma dependencia do array ser alterada desde a ultima renderização.

Um exemplo basico de como usar o useMemo:

```jsx

import React, { useMemo } from 'react';

const ExpensiveComponent = ({ value }: { value: number }) => {
    // This function will only recompute if `value` changes
    const expensiveResult = useMemo(() => {
        console.log('Expensive computation');
        // Perform some expensive calculations using `value`
        return value * 2;
    }, [value]);

    return (
        <div>
            <p>Expensive Result: {expensiveResult}</p>
        </div>
    );
};
```

Neste exemplo, o valor do expensiveResult é recalculado apenas quando o prop for alterado. Se o ExpensiveComponent for re-renderizado por outra razão (Como se outro prop ou state ser alterado), o resultado em cache vai ser usado, e a calculação não vai ser repetida.
    
Num resumo, useMemo é um hook do react que permite memorizar o resultado de uma função baseado nas suas dependencias, o que ajuda a otimizar o desempenho ao evitar recomputações desnecessarias.
    
