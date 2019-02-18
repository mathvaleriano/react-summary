# React Resume

### Props
Props são atributos que podem ser passados para componentes (não mude eles dentro do component que vc está usando)

Pros are attributes that can be passed to components (don't change them into the component which is using them)

### States
A melhor forma de manipular as props que foram passadas e outros atributos do seu component 

The best way to manipulate the props that has been passed and another attributes of your own component

### Function Component

```
const foo = (props) => <div>{props.bar}</div> // uma linha
const foo = (props) => <div>{props.bar}</div> // one line

const foo2 = ({ bar }) => <div>{bar}</div> // com destructuring
const foo2 = ({ bar }) => <div>{bar}</div> // with destructuring

//varias linhas
//multiple lines
const foo3 = () => ( 
  <div>
    ...
  </div>
)

// com a palavra reservada "return"
// with return word
const foo4 = () => {
  return (
    <div>
    ...
    </div>
  )
}

```

### Class Component/PureComponent 
#### [(LIFECYCLES HERE)](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

```
import React, { Component, PureComponent } from 'react'

class Foo extends Component {
  state = {...}

  // ... outros lifecycles
  // ... other lifecycles

  shouldComponentUpdate() {...}

  render () { ... }
}


class FooPure extends PureComponent {
  state = {...}

  // ... outros lifecycles
  // ... other lifecycles

  // shouldComponentUpdate será implementado para vc automaticamente
  // shouldComponentUpdate will be implemented for you automatically

  render () { ... }
}

```

### Como colocar JS no seu component (How to introduce JS into your component)

Vc precisa usar {} pra inserir o javascript

You need to use {} syntax to insert javascript

```
import React, { Component } from 'react';

const Foo = (props) => <div>{ props.bar }</div>

// ou
// or

class Foo2 extends Component {
  render () {
    // considere usar destructuring
    // consider use destructuring 
    const { bla } = this.props

    return (
      <div>
        { this.props.bar }
        { bla }
        { /* isso é um comentário */ }
        { /* this is a comment */ }
      </div>
    )
  }
}

```

### Como mudar o state (How to change state)

```
class Foo extends Component {
  state = {
    foo: ''
  }

  handleClick = () => {
    // isso vai mudar o valor de state.foo para 'bar'
    // it will change state.foo value to 'bar'
    
    this.setState({ foo: 'bar' }) 
  }

  handleClick2 = () => {
    // isso vai mudar o valor de state.foo para o seu inverso
    // it will change state.foo value to reverse value
    this.setState(previousState => ({
      foo: [...previousState.foo].reverse().join('')
    }))
  }

  handleClick3 = () => {
    // mesmo coisa que handleClick2 só que com destructuring
    // same of handleClick2 but with destructuring
    this.setState(({ foo }) => ({
      foo: [...foo].reverse().join('')
    }))
  }
}

```

### Como lidar com um evento (How to handle an event)
```
// onClick quando for executado receberá como argumento o evento de click
// When onClick is executed it will receive  click event as argument
const Foo = ({ onClick }) => <Button onClick={onClick}>B</Button>

// onClick quando for executado NÃO receberá como argumento o evento de click
// When onClick is executed it will NOT receive  click event as argument
const Off = ({ onClick }) => (
  <Button onClick={() => onClick()}>B</Button>
)

// Usando a palavra reservada this (para classes) 
// Using this word (for classes)
class FooClass extends Component {
  handleClick = () => {}

  render() {
    return (
      <div>
        <Button onClick={this.handleClick}>B</Button>
        <Button onClick={() => this.handleClick()}>B</Button>
      </div>
    )
  }
}
```

### Renderização condicional (Conditional render)
Renderiza somente o que passar na condição

Render something only when conditions passed

```
const Foo = ({ conditionProp }) => (
  <div>
    { coditionProp && <span>Passou na condição | Condition is passed</span> }

    { coditionProp ? 
      <span>Passou na condição | Condition is passed</span> :
      <span>Não passou na condição | Condition is not passed</span>
    }
  </div>
)
```

### Renderização de múltiplos elementos (Render multiple elements)
Quando voce tem uma lista de itens

When you have an array of items

```
// voce pode usar um valor padrão para evitar possíveis erros (tipo: undefined has no map)
// you can use a default value to avoid possible errors (like: undefined has no map)

const Foo = ({ items = [] }) => (
  <div>
  {
    // isso vai renderizar vários spans | it will renders multiple spans
    // o atributo key é importante | key attribute is important
    // porque ele vai ajudar o react a detectar qual elemento mudou 
    // because it will help react to indicate what elements have changed
    // USE ESSA PARADA AÍ | USE IT
    items.map(item => <span key={item.id}>{item}</span>)
  }
  </div>
)
```

### Como referenciar um elemento ou component (How to refer an element or component)
Quando você não quer usar **document.get...**

When you don't wanna use **document.get...**

```
import React, { Component } from 'react'

class Foo extends Component {
  myRef = React.createRef()
  
  render() {
    <div>
      <button ref={this.myRef}>Meu botão referenciado | My Referenced Button</button>
    </div>
  }
}

const Bar = () => {
  const myRef = React.createRef()

  return (
    <div>
      <button ref={myRef}>Meu botão referenciado | My Referenced Button</button>
    </div>
  )
}

```

### Styled Components
`Obs.: instale styled-components no seu projeto`[>>**DOCUMENTATION**<<](https://www.styled-components.com/docs/basics#installation)

`Obs.: install styled-components on your project` [>>**DOCUMENTATION**<<](https://www.styled-components.com/docs/basics#installation)

Vc pode passar as props e o styledcomponent vai gerar classes para vc com os estilos gerados

You can pass props and the styledcomponent will generate classes for you with the generated styles

```
import styled, { css } from 'styled-components'

const Button = styled.button`
  box-sizing: border-box;
  border: 0;
  cursor: pointer;
  
  color: ${({color = '#fff'}) => color};

  font-weigth: ${({ fontWeight = 'normal' }) => fontWeight};
  
  ${({ width }) => width && css`width: ${width}`}
  
  ${({ height }) => height && css`height: ${height}`}
  
  ${({ primary }) => getThemedColorsStyle({
    theme: primary,
    color: colorPrimary
  })}
  
  ${({ secondary }) => getThemedColorsStyle({
    theme: secondary,
    color: colorSecondary
  })}
  
  ${({ radius = 5 }) => css`border-radius: ${radius}%;`}
  
  ${({ size }) => {
    switch(size) {
      case 'small':
        return css`
          font-size: .5rem;
          padding: .39rem;
        `
      case 'large':
        return css`
          font-size: 1.1rem;
          padding: .7rem;
        `
      case 'largest':
        return css`
          font-size: 1.8rem;
          padding: 1.1rem;
        `
      default:
        return css`
          font-size: .7rem;
          padding: .5rem;
        `
    }
  }}
`;

```

### Hooks
`Obs.: Use a última versão`

`Obs.: Use the latest version`

#### useState
Uma função que retorna um array com dois valores **state e dispatch**

A function that returns an array with two values **state and dispatch**

`obs.: dispatch = setState do estado que foi retornado`

`obs.: dispatch = setState of the state that has been returned`

```
import React, { useState } from 'react'

const Foo = () => {
  const [bar, setBar] = useState()

  const [bear, setB] = useState('brown') 
  // o parametro representa o valor inicial | the parameter represents the initial value
  // vc não precisa usar a mesma palavra no dispatch... | you don't need use the same word in dispatch... 
  // mas é mais fácil quando vc usa | but it's easier when you use it
}
```

#### useEffect

Roda toda vez que acontece uma renderização. Passando um 2º argumento, vai fazer rodar somente quando algum dos itens do array for alterado (via shallow equality). Se quiser que rode apenas 1 vez na inicialização do componente, basta passar um array vazio.

Runs on every render. Passing a 2º it will runs only when any item of the array has been changed (by shallow equality). If you want to run once on component initialization, just pass an empty array.


```
import React, { useEffect } from 'react'

const Foo = props => {
  useEffect(() => {
    // Especifica como limpar as coisas depois de executar o efeito
    // Specify how to clean up after this effect
    return () => {}
  })

  // voce pode ter mais de um useEffect no seu component :3
  // you can have more than one useEffect on your component :3
  useEffect(() => {
    document.title = 'new title'

    return () => {
      document.title = 'old title'
    }
  })

  // em casos reais vc pode limpar os subscribers/timers/listeners/etc
  // in real cases you can use the clean up to clean subscribers/timers/listeners/etc
  useEffect(() => {
    const timer = setTimeout(() => {}, 1000)

    return () => {
      clearTimeout(timer)
    }
  })

  // executa uma vez só
  // execute once
  useEffect(() => {
    return () => {}
  }, [])
  
  // executa baseado nas props que mudaram
  // execute based on props which changes
  useEffect(() => {

  }, [props.bar]) 
}
```

#### useContext
Faz a mesma coisa que o Context.Consumer mas com o Context.Provider mais próximo

Do the same thing like Context.Consumer perhaps with the closest Context.Provider

```
import React from 'react'

const MyContext = React.createContext()
const { Provider, Consumer } = MyContext

function Wrapper () {
  return (
    <Provider value={10}>
      <MyComponentWithoutHook />
      <MyComponentWithHook />
    </Provider>
  )
}

function MyComponentWithoutHook () {
  return (
    <Consumer>
      {value => <div>{value}</div>}
    </Consumer>
  )
}

function MyComponentWithHook () {
  const value = useContext(MyContext)
  
  return (
    <div>{value}</div>
  )
}
```
