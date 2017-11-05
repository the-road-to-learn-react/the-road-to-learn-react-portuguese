# React Básico

Este capítulo lhe apresentará os conceitos básicos de React. Componentes estáticos são um pouco maçantes, então trataremos de assuntos como estado e interações. Você também irá aprender os diferentes modos de declarar um componente e como fazer para mantê-los fáceis de reusar e de compor uns com os outros. Prepare-se para dar vida à eles.

## Estado Interno do Componente

O estado local, também chamado de estado interno do componente, lhe permite salvar, modificar e apagar propriedades que são armazenadas nele.

O componente de classe de ES6 usa um construtor para inicializar seu estado interno. Ele é chamado apenas uma vez quando o componente é inicializado. Abaixo, um construtor de classe:

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

# leanpub-start-insert
  constructor(props) {
    super(props);
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

Quando seu componente possui um construtor, torna-se obrigatória a chamado de `super();`, porque o componente App é uma subclasse de `Component` (`class App extends Component`). Mais tarde, você aprenderá mais sobre componentes de classe em ES6.

Você também pode invocar `super(props);` para definir `this.props` no contexto do seu construtor. Caso contrário, se tentar acessar `this.props`, receberá o valor `undefined`. Futuramente, estudaremos mais sobre as _props_ de um componente React.

A essa altura, o estado inicial do seu componente é composto por apenas uma lista de itens:

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  ...
];

class App extends Component {

  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      list: list,
    };
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

O estado local está amarrado à classe através do objeto `this`. Dessa forma, você pode acessá-lo em qualquer lugar do componente. Ele pode ser utilizado, por exemplo, no método `render()`. Anteriormente, você usou `map` com uma lista estática de itens, definida fora do componente,  em seu método `render()`. Agora, você irá usar a lista obtida do seu estado local.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {this.state.list.map(item =>
# leanpub-end-insert
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

A lista agora é parte do componente, residindo em seu estado interno. Você pode adicionar, mudar ou remover itens. Todas as vezes que mudar o estado do seu componente, o método `render()` será chamado novamente. Você simplesmente altera o estado interno, sabendo que o componente será de novo renderizado e que os dados corretos que são lidos serão exibidos.

Mas, tenha cuidado. Não altere o estado diretamente. Use um método chamado `setState()` para mudá-lo. Você irá saber mais a respeito no próximo capítulo.

### Exercícios:

* Experimente trabalhar com o estado local
  * Defina mais dados iniciais no estado em seu construtor
  * Use o estado no seu método `render()`
* Leia mais sobre [o construtor de classe ES6][1]

## Inicializando Objetos em ES6

Em JavaScript ES6, você pode usar uma sintaxe abreviada para inicializar seus objetos de forma mais concisa. Imagine o seguinte:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

Nesse caso, a propriedade do objeto e a variável são igualmente chamadas de `name`. Você poderia fazer desta forma:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

Aplicando o mesmo raciocínio na sua aplicação, com a variável `list` e a propriedade do estado local que compartilha o mesmo nome:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
this.state = {
  list: list,
};

// ES6
this.state = {
  list,
};
~~~~~~~~

Um outro atalho elegante é a declaração concisa de métodos em JavaScript ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  },
};

// ES6
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  },
};
~~~~~~~~

Por último, mas não menos importante, o uso de nomes computados de propriedades é permitido em JavaScript ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var user = {
  name: 'Robin',
};

// ES6
const key = 'name';
const user = {
  [key]: 'Robin',
};
~~~~~~~~

Talvez isso ainda não faça tanto sentido para você. Por que você utilizar nomes computados? Em um capítulo mais adiante, iremos nos deparar com a situação em que poderemos utilizadores-los para alocar valores por chave, de uma forma dinâmica em um objeto. É uma forma elegante em JavaScript de gerar _lookup tables_ (um tipo de estrutura de dados).

### Exercícios:

* Experimente trabalhar com inicialização de objetos com ES6
* Leia mais sobre [inicialização de objetos em ES6][2]

## Fluxo Unidirecional de Dados

Você tem agora em mãos um componente App com estado interno, que ainda não foi mexido. O estado é estático e, consequentemente, o seu componente também. Uma boa maneira de experimentar manipular o estado é criando alguma interação entre componentes.

Vamos adicionar um botão para cada item da lista que é exibida. O botão tem o rótulo "_Dismiss_" (dispensar) e irá remover o item, sendo útil quando você quer manter uma lista constando apenas itens ainda não lidos e dispensar os que não tem interesse, por exemplo.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
# leanpub-start-insert
            <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
# leanpub-end-insert
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

O método de classe `onDismiss()` ainda não foi definido, nós o faremos daqui a pouco.  Vamos primeiro focar no tratamento do `onClick` do elemento `button`. Como você pode ver, o método `onDismiss()` no `onClick` está encapsulado por uma _arrow function_. Nela, você tem acesso à propriedade `objectID` do objeto `item`, que identifica qual item será removido. Uma alternativa à isso seria definir a função fora e apenas passá-la para o `onClick`. Outro capítulo irá explicar em maiores detalhes o tópico de tratamento de eventos em elementos.

Você viu que o elemento button é declarado em múltiplas linhas? Note que, à medida que a quantidade de atributos cresce, deixar tudo em uma única linha pode acabar gerando uma bagunça. Por esse motivo, o elemento button foi escrito em múltiplas linhas e indentado. Não é obrigatório, mas é um estilo de código que eu recomendo fortemente.

Chegou a hora de implementar o comportamento de `onDismiss()`. A função recebe um id, para identificar qual item será removido e, por ser amarrada à classe, é um método de classe. Sendo assim, deve ser acessada com `this.onDismiss()` e não apenas `onDismiss()`. O objeto `this` é a instância da sua classe.

Para definir o `onDismiss()` como um método de classe, você precisa usar `bind` no construtor.  _Bindings_ serão explicados mais adiante, em outro capítulo.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-end-insert
  }

  render() {
    ...
  }
}
~~~~~~~~

O próximo passo é definir a funcionalidade em si, ou a lógica de negócio do método em sua classe, da seguinte maneira:

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onDismiss(id) {
    ...
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Feito isso, você pode escrever o que acontece dentro do método. Basicamente, você quer remover da lista o idem identificado pelo id e armazenar a lista atualizada no seu estado local. A nova lista será usada no método `render`, que será novamente chamado, para ser exibida. O item removido não irá mais aparecer.

É possível remover um item de uma lista usando a funcionalidade nativa de JavaScript _filter_, que é uma função que recebe outra função como entrada. Esta, por sua vez, tem acesso a cada valor na lista, pois _filter_ está iterando sobre ela, permitindo-lhe checar item a item na lista baseado na condição fornecida. Se o resultado da avaliação for _true_, o item permanece na lista. Caso contrário, será filtrado dela. Além disso, é bom saber que a função _filter_ retorna uma nova lista e não mexe no estado da antiga. Ela atende à convenção em React de manter estruturas de dados imutáveis.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

No próximo passo, você pode extrair a função e passá-la como argumento para _filter_.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  function isNotId(item) {
    return item.objectID !== id;
  }

  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Ainda pode fazê-lo de forma mais concisa, usando novamente uma _arrow function_ de JavaScript.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Pode até colocá-la _inline_ novamente, como fez no `onClick` do botão, mas pode ser que assim ela fique menos legível.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

A lista agora remove o item clicado. Entretanto, o estado local ainda não foi atualizado. Finalmente você pode usar o método `setState()` para atualizar a lista no estado interno do componente.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

Rode novamente sua aplicação e experimente clicar no botão "Dismiss". Provavelmente, irá funcionar e você estará testemunhando nesse momento o **fluxo unidirecional de dados** em React. Você dispara uma ação em sua _view_ com `onClick()`, uma função ou método de classe muda o estado interno do componente e `render()` é de novo executado para atualizar a _view_.

![Internal state update with unidirectional data flow][image-1]

### Exercícios:

* Leia mais sobre [o ciclo de vida do estado de componentes em React][3]

## Bindings

É importante aprender sobre _bindings_ em classes JavaScript quando se vai trabalhar com componentes de classe em React. No capítulo anterior, você os utilizou para ligar seu método `onDismiss()` à classe em seu construtor.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Em primeiro lugar: Por que você teve que fazer isso?

Esse passo é necessário porque a amarração do `this` com a instância de classe não é feita automaticamente pelos métodos. Vamos demostrar isso com a ajuda do componente a seguir:

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

O componente renderiza normalmente, mas quando você clica no botão, obtém `undefined` no console. Essa é uma das maiores fontes de _bugs_ quando se usa React. Você deseja usar `this.state` em um método de classe e ele não está acessível, porque `this` é `undefined`. Para corrigir isso, você precisa criar o _binding_ entre o método e `this`.

No exemplo a seguir, o método é propriamente vinculado a `this` dentro do construtor da classe.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
# leanpub-start-insert
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }
# leanpub-end-insert

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Executando novamente o teste, o objeto `this` (ou, sendo mais específico, a instância de classe) está definido nesse contexto e você tem acesso a `this.state`, ou `this.props`, que você conhecerá depois.  

O _binding_ de métodos também poderia ser feito em outros lugares, como no `render()`, por exemplo.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
# leanpub-start-insert
        onClick={this.onClickMe.bind(this)}
# leanpub-end-insert
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Apesar de possível, deve ser evitado, porque a vinculação do método seria feira todas as vezes que `render()` for chamado. Como basicamente ele roda todas as vezes que seu componente é atualizado, isso poderia trazer implicações de performance. Quando o _binding_ ocorre no construtor, o processo só ocorre uma vez: quando o componente é instanciado. Essa é a melhor abordagem a ser escolhida.

Outra coisa que, uma vez ou outra, alguém acaba fazendo, é definir a lógica de negócios dos métodos de classe dentro do construtor.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

# leanpub-start-insert
    this.onClickMe = () => {
      console.log(this);
    }
# leanpub-end-insert
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Essa prática também deveria ser evitada, porque ela irá transformar seu construtor em uma bagunça ao longo do tempo. A finalidade do construtor é instanciar sua classe com todas as propriedades. Por isso, a lógica de negócio dos métodos de classe deve ser definida fora dele.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.doSomething = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }

  doSomething() {
    // do something
  }

  doSomethingElse() {
    // do something else
  }

  ...
}
~~~~~~~~

Por fim, devo mencionar que métodos de classe podem ser automaticamente vinculados sem fazê-lo explicitamente, com o uso de _arrow functions_.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Se a repetição do processo de _binding_ no construtor não lhe agrada, você pode seguir nessa abordagem. Como a documentação oficial de React continua utilizando _bindings_ no construtor, este livro irá fazê-lo também.

### Exercícios:

* Experimente diferentes abordagens de _binding_ e veja o valor de `this` no console.

## Tratamento de Eventos

**Nota do tradutor**: De agora em diante, neste livro, usaremos "_event handler_" e "tratamento de evento" com o mesmo significado. Apesar de essa última não ser a tradução literal da primeira, é a forma mais conhecida para tal na língua portuguesa. Soaria estranho se usássemos "tratador de eventos", ou até "manipulador de eventos".

Este capítulo deve lhe dar um melhor entendimento sobre tratamento de eventos em elementos. Na sua aplicação, você usa o elemento `button` a seguir para remover um item da lista:

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Este exemplo é um tanto quanto complexo. Você tem que passar um valor para o método de classe e, para tal, precisa encapsulá-lo em outra função (uma _arrow function_). Basicamente, o que precisa ser passado como argumento para _event handler_ é uma função, não sua chamada. O código a seguir não funcionaria, porque o método de classe seria imediatamente executado quando você abrisse sua aplicação no navegador.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Quando usamos `onClick={executarAlgo()}`, a função `executarAlgo()` seria executada imediatamente após a aplicação abrir no _browser_. A expressão passada para o _handler_ é avaliada e, como o valor retornado não é uma função, nada irá acontecer quando você clicar no botão. Mas, quando fazemos `onClick={executarAlgo}`, onde `executarAlgo` é o nome de uma função, essa só será executada quando o botão for clicado. A mesma regra se aplica para o método `onDismiss` usado em sua aplicação.

Entretanto, não é suficiente declarar `onClick={this.onDismiss}`, porque precisamos passar a propriedade `item.objectID` para o método, para identificar qual item será removido. Por esse motivo, usamos uma _arrow function_ como _wrapper_, utilizando o conceito conhecido em JavaScript como _high-order function_ , que será brevemente explicado mais tarde.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Uma outra alternativa seria definir a função _wrapper_ em algum outro lugar e passá-la como argumento para to tratamento do evento. Uma vez que precisa ter acesso ao item, ela deve residir dentro do bloco da função _map_.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item => {
# leanpub-start-insert
          const onHandleDismiss = () =>
            this.onDismiss(item.objectID);
# leanpub-end-insert

          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
              <span>
                <button
# leanpub-start-insert
                  onClick={onHandleDismiss}
# leanpub-end-insert
                  type="button"
                >
                  Dismiss
                </button>
              </span>
            </div>
          );
        }
        )}
      </div>
    );
  }
}
~~~~~~~~

No fim das contas, o _event handler_ do elemento precisa receber uma função. Como exemplo, faça o teste com esse código, que faz o contrário:

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
            ...
            <span>
              <button
# leanpub-start-insert
                onClick={console.log(item.objectID)}
# leanpub-end-insert
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

A função é executada assim que você abre a aplicação no navegador, mas não quando você clica no botão. Enquanto que o código a seguir só roda no momento do clique. É uma função que é executada quando você dispara o evento.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={function () {
    console.log(item.objectID)
  }}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Visando manter o código conciso, você pode transformá-la de volta uma uma _arrow function_, fazendo o mesmo que fizemos com o método de classe `onDismiss()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={() => console.log(item.objectID)}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Frequentemente, novatos em React têm dificuldades com este tópico do uso de funções no tratamento de eventos. Por isso, me alonguei tentando explicar em maiores detalhes.

Feita isso, agora você deve ter o seguinte código no botão, com uma _arrow function_ _inline_  concisa que tem acesso à propriedade `objectID` do objeto item:

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            ...
            <span>
# leanpub-start-insert
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Outro tópico relevante que sempre é mencionado, relacionado à performance, trata sobre as implicações do uso de _arrow functions_ em _event handlers_. Por exemplo, tomemos o caso do `onClick` com uma _arrow function_ envolvendo o `onDismiss`. Todas as vezes que o método `render()` for executado, o _event handler_ irá instanciar a função. Isso _pode_ ter um certo impacto na performance da sua aplicação. Na maioria dos casos, porém, você não irar notar a diferença.

Imagine que você tem uma enorme tabela de dados com 1000 itens e cada linha ou coluna tem uma _arrow function_ sendo definida no _event handler_. Nesse caso, sim, é válida a preocupação a respeito da performance e você poderia implementar um componente dedicado `Button` com o _biding_ ocorrendo no construtor. Mas, preocupar-se com isso agora significa otimização prematura. É mais benéfico focar em aprender React em si.

### Exercícios:

* Experimente as diferentes abordagens de uso de funções no `onClick` do botão em sua aplicação.

## Interação com Forms e Eventos

Let's add another interaction for the application to experience forms and events in React. The interaction is a search functionality. The input of the search field should be used to filter your list temporary based on the title property of an item.

In the first step, you are going to define a form with an input field in your JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        <form>
          <input type="text" />
        </form>
# leanpub-end-insert
        {this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

In the following scenario you will type into the input field and filter the list temporarily by the search term that is used in the input field. To be able to filter the list based on the value of the input field, you need to store the value of the input field in your local state. But how do you access the value? You can use **synthetic events** in React to access the event payload.

Let's define a `onChange` handler for the input field.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
# leanpub-start-insert
          <input
            type="text"
            onChange={this.onSearchChange}
          />
# leanpub-end-insert
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

The function is bound to the component and thus a class method again. You have to bind and define the method.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onSearchChange() {
    ...
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

When using a handler in your element, you get access to the synthetic React event in your callback function's signature.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  onSearchChange(event) {
# leanpub-end-insert
    ...
  }

  ...
}
~~~~~~~~

The event has the value of the input field in its target object. Hence you are able to update the local state with the search term by using `this.setState()` again.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
# leanpub-start-insert
    this.setState({ searchTerm: event.target.value });
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

Additionally, you shouldn't forget to define the initial state for the `searchTerm` property in the constructor. The input field should be empty in the beginning and thus the value should be an empty string.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
# leanpub-start-insert
      searchTerm: '',
# leanpub-end-insert
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Now you store the input value to your internal component state every time the value in the input field changes.

A brief note about updating the local state in a React component. It would be fair to assume that when updating the `searchTerm` with `this.setState()` the list needs to be passed as well to preserve it. But that isn't the case. React's `this.setState()` is a shallow merge. It preserves the sibling properties in the state object when updating one sole property in it. Thus the list state, even though you have already dismissed an item from it, would stay the same when updating the `searchTerm` property.

Let's get back to your application. The list isn't filtered yet based on the input field value that is stored in the local state. Basically you have to filter the list temporarily based on the `searchTerm`. You have everything you need to filter it. So how to filter it temporarily now? In your `render()` method, before you map over the list, you can apply a filter on it. The filter would only evaluate if the `searchTerm` matches title property of the item. You have already used the built-in JavaScript filter functionality, so let's do it again. You can sneak in the filter function before the map function, because the filter function returns a new array and thus the map function can be used on it in such a convenient way.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(...).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Let's approach the filter function in a different way this time. We want to define the filter argument, the function that is passed to the filter function, outside of the ES6 class component. There we don't have access to the state of the component and thus we have no access to the `searchTerm` property to evaluate the filter condition. We have to pass the `searchTerm` to the filter function and have to return a new function to evaluate the condition. That's called a higher order function.

Normally I wouldn't mention higher order functions, but in a React book it makes total sense. It makes sense to know about higher order functions, because React deals with a concept called higher order components. You will get to know the concept later in the book. Now again, let's focus on the filter functionality.

First, you have to define the higher order function outside of your App component.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function(item) {
    // some condition which returns true or false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

The function takes the `searchTerm` and returns another function, because after all the filter function takes a function as its input. The returned function has access to the item object because it is the function that is passed to the filter function. In addition, the returned function will be used to filter the list based on the condition defined in the function. Let's define the condition.

{title="src/App.js",lang=javascript}
~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~

The condition says that you match the incoming `searchTerm` pattern with the title property of the item from your list. You can do that with the built-in `includes` JavaScript functionality. Only when the pattern matches, you return true and the item stays in the list. When the pattern doesn't match the item is removed from the list. But be careful with pattern matching: You shouldn't forget to lower case both strings. Otherwise there will be mismatches between a search term 'redux' and an item title 'Redux'. Since we are working on a immutable list and return a new list by using the filter function, the original list in the local state isn't modified at all.

One thing is left to mention: We cheated a bit by using the built-in includes JavaScript functionality. It is already an ES6 feature. How would that look like in JavaScript ES5? You would use the `indexOf()` function to get the index of the item in the list. When the item is in the list, `indexOf()` will return its index in the array.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Another neat refactoring can be done with an ES6 arrow function again. It makes the function more concise:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function(item) {
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
  }
}

// ES6
const isSearched = searchTerm => item =>
  item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~

One could argue which function is more readable. Personally I prefer the second one. The React ecosystem uses a lot of functional programming concepts. It happens often that you will use a function which returns a function (higher order functions). In JavaScript ES6, you can express these more concisely with arrow functions.

Last but not least, you have to use the defined `isSearched()` function to filter your list. You pass it the `searchTerm` property from your local state, it returns the filter input function, and filters your list based on the filter condition. Afterward it maps over the filtered list to display an element for each list item.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

The search functionality should work now. Try it yourself in the browser.

### Exercises:

* read more about [React events][4]
* read more about [higher order functions][5]

## ES6 Destructuring

There is a way in JavaScript ES6 for an easier access to properties in objects and arrays. It's called destructuring. Compare the following snippet in JavaScript ES5 and ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const user = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch

// ES6
const { firstname, lastname } = user;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch
~~~~~~~~

While you have to add an extra line each time you want to access an object property in JavaScript ES5, you can do it in one line in JavaScript ES6. A best practice for readability is to use multilines when you destructure an object into multiple properties.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

The same goes for arrays. You can destructure them too. Again, multilines will keep your code scannable and readable.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const users = ['Robin', 'Andrew', 'Dan'];
const [
  userOne,
  userTwo,
  userThree
] = users;

console.log(userOne, userTwo, userThree);
// output: Robin Andrew Dan
~~~~~~~~

Perhaps you have noticed that the local state object in the App component can get destructured the same way. You can shorten the filter and map line of code.

{title="src/App.js",lang=javascript}
~~~~~~~~
  render() {
# leanpub-start-insert
    const { searchTerm, list } = this.state;
# leanpub-end-insert
    return (
      <div className="App">
        ...
# leanpub-start-insert
        {list.filter(isSearched(searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
~~~~~~~~

You can do it the ES5 or ES6 way:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

But since the book uses JavaScript ES6 most of the time, you should stick to it.

### Exercises:

* read more about [ES6 destructuring][6]

## Controlled Components

You already learned about the unidirectional data flow in React. The same law applies for the input field, which updates the local state with the `searchTerm` in order to filter the list. When the state changes, the `render()` method runs again and uses the recent `searchTerm` from the local state to apply the filter condition.

But didn't we forget something in the input element? A HTML input tag comes with a `value` attribute. The value attribute usually has the value that is shown in the input field. In this case it would be the `searchTerm` property. However, it seems like we don't need that in React.

That's wrong. Form elements such as `<input>`, `<textarea>` and `<select>` hold their own state in plain HTML. They modify the value internally once someone changes it from the outside. In React that's called an **uncontrolled component**, because it handles its own state. In React, you should make sure to make those elements **controlled components**.

How should you do that? You only have to set the value attribute of the input field. The value is already saved in the `searchTerm` state property. So why not access it from there?

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <form>
          <input
            type="text"
# leanpub-start-insert
            value={searchTerm}
# leanpub-end-insert
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

That's it. The unidirectional data flow loop for the input field is self-contained now. The internal component state is the single source of truth for the input field.

The whole internal state management and unidirectional data flow might be new to you. But once you are used to it, it will be your natural flow to implement things in React. In general, React brought a novel pattern with the unidirectional data flow to the world of single page applications. It is adopted by several frameworks and libraries by now.

### Exercises:

* read more about [React forms][7]

## Split Up Components

You have one large App component now. It keeps growing and can become confusing eventually. You can start to split it up into chunks of smaller components.

Let's start to use a component for the search input and a component for the list of items.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search />
        <Table />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

You can pass those components properties which they can use themselves. In the case of the App component it needs to pass the properties managed in the local state and its class methods.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Now you can define the components next to your App component. Those components will be ES6 class components as well. They render the same elements like before.

The first one is the Search component.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...
}

# leanpub-start-insert
class Search extends Component {
  render() {
    const { value, onChange } = this.props;
    return (
      <form>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

The second one is the Table component.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <button
                onClick={() => onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Now you have three ES6 class components. Perhaps you have noticed the `props` object that is accessible via the class instance by using `this`. The props, short form for properties, have all the values you have passed to the components when you used them in your App component. That way, components can pass properties down the component tree.

By extracting those components from the App component, you would be able to reuse them somewhere else. Since components get there values by using the props object, you can pass every time different props to your components when you use them somewhere else. These components became reusable.

### Exercises:

* figure out further components that you could split up as you have done with the Search and Table components
  * but don't do it now, otherwise you will run into conflicts in the next chapters

## Composable Components

There is one more little property which is accessible in the props object: the `children` prop. You can use it to pass elements to your components from above, which are unknown to the component itself, but make it possible to compose components into each other. Let's see how this looks like when you only pass a text (string) as a child to the Search component.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        >
          Search
        </Search>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Now the Search component can destructure the children property from the props object. Then it can specify where the children should be displayed.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
# leanpub-start-insert
    const { value, onChange, children } = this.props;
# leanpub-end-insert
    return (
      <form>
# leanpub-start-insert
        {children} <input
# leanpub-end-insert
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

The "Search" text should be visible next to your input field now. When you use the Search component somewhere else, you can choose a different text if you like. After all, it is not only text that you can pass as children. You can pass an element and element trees (which can be encapsulated by components again) as children. The children property makes it possible to weave components into each other.

### Exercises:

* read more about [the composition model of React][8]

## Reusable Components

Reusable and composable components empower you to come up with capable component hierarchies. They are the foundation of React's view layer. The last chapters mentioned the term reusability. You can reuse the Table and Search components by now. Even the App component is reusable, because you could instantiate it somewhere else again.

Let's define one more reusable component, a Button component, which gets reused more often eventually.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className,
      children,
    } = this.props;

    return (
      <button
        onClick={onClick}
        className={className}
        type="button"
      >
        {children}
      </button>
    );
  }
}
~~~~~~~~

It might seem redundant to declare such a component. You will use a `Button` component instead of a `button` element. It only spares the `type="button"`. Except for the type attribute you have to define everything else when you want to use the Button component. But you have to think about the long term investment here. Imagine you have several buttons in your application, but want to change an attribute, style or behavior for the button. Without the component you would have to refactor every button. Instead the Button component ensures to have only one single source of truth. One Button to refactor all buttons at once. One Button to rule them all.

Since you already have a button element, you can use the Button component instead. It omits the type attribute, because the Button component specifies it.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
# leanpub-start-insert
              <Button onClick={() => onDismiss(item.objectID)}>
                Dismiss
              </Button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

The Button component expects a `className` property in the props. The `className` attribute is another React derivate for the HTML attribute class. But we didn't pass any `className` when the Button was used. In the code it should be more explicit in the Button component that the `className` is optional.

Therefore, you can use the default parameter which is a JavaScript ES6 feature.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
# leanpub-start-insert
      className = '',
# leanpub-end-insert
      children,
    } = this.props;

    ...
  }
}
~~~~~~~~

Now, whenever there is no `className` property specified when using the Button component, the value will be an empty string instead of `undefined`.

### Exercises:

* read more about [ES6 default parameters][9]

## Component Declarations

By now you have four ES6 class components. But you can do better. Let me introduce functional stateless components as alternative for ES6 class components. Before you will refactor your components, let's introduce the different types of components in React.

* **Functional Stateless Components:** These components are functions which get an input and return an output. The input are the props. The output is a component instance thus plain JSX. So far it is quite similar to an ES6 class component. However, functional stateless components are functions (functional) and they have no local state (stateless). You cannot access or update the state with `this.state` or `this.setState()` because there is no `this` object. Additionally, they have no lifecycle methods. You didn't learn about lifecycle methods yet, but you already used two: `constructor()` and `render()`. Whereas the constructor runs only once in the lifetime of a component, the `render()` class method runs once in the beginning and every time the component updates. Keep in mind that functional stateless component have no lifecycle methods, when you arrive at the lifecycle methods chapter later on.

* **ES6 Class Components:** You already used this type of component declaration in your four components. In the class definition, they extend from the React component. The `extend` hooks all the lifecycle methods, available in the React component API, to the component. That way you were able to use the `render()` class method. Additionally, you can store and manipulate state in ES6 class components by using `this.state` and `this.setState()`.

* **React.createClass:** The component declaration was used in older versions of React and still in JavaScript ES5 React applications. But [Facebook declared it as deprecated][10] in favor of JavaScript ES6. They even added a [deprecation warning in version 15.5][11]. You will not use it in the book.

So basically there are only two component declarations left. But when to use functional stateless components over ES6 class components? A rule of thumb is to use functional stateless components when you don't need local state or component lifecycle methods. Usually you start to implement your components as functional stateless components. Once you need access to the state or lifecycle methods, you have to refactor it to an ES6 class component. In our application, we started the other way around for the sake of learning React.

Let's get back to your application. The App component uses internal state. That's why it has to stay as an ES6 class component. But the other three of your ES6 class components are stateless. They don't need access to `this.state` or `this.setState()`. Even more, they have no lifecycle methods. Let's refactor together the Search component to a stateless functional component. The Table and Button component refactoring will remain as your exercise.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search(props) {
  const { value, onChange, children } = props;
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
# leanpub-end-insert
~~~~~~~~

That's basically it. The props are accessible in the function signature and the return value is JSX. But you can do more code wise in a functional stateless component. You already know the ES6 destructuring. The best practice is to use it in the function signature to destructure the props.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search({ value, onChange, children }) {
# leanpub-end-insert
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

But it can get better. You know already that ES6 arrow functions allow you to keep your functions concise. You can remove the block body of the function. In a concise body an implicit return is attached thus you can remove the return statement. Since your functional stateless component is a function, you can keep it concise as well.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({ value, onChange, children }) =>
  <form>
    {children} <input
      type="text"
      value={value}
      onChange={onChange}
    />
  </form>
# leanpub-end-insert
~~~~~~~~

The last step was especially useful to enforce only to have props as input and JSX as output. Nothing in between. Still, you could *do something* in between by using a block body in your ES6 arrow function.

{title="Code Playground",lang=javascript}
~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // do something

  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

But you don't need it for now. That's why you can keep the previous version without the block body. When using block bodies, people often tend to do too many things in the function. By leaving the block body out, you can focus on the input and output of your function.

Now you have one lightweight functional stateless component. Once you would need access to its internal component state or lifecycle methods, you would refactor it to an ES6 class component. In addition you saw how JavaScript ES6 can be used in React components to make them more concise and elegant.

### Exercises:

* refactor the Table and Button component to stateless functional components
* read more about [ES6 class components and functional stateless components][12]

## Styling Components

Let's add some basic styling to your application and components. You can reuse the *src/App.css* and *src/index.css* files. These files should already be in your project since you have bootstrapped it with *create-react-app*. They should be imported in your *src/App.js* and *src/index.js* files too. I prepared some CSS which you can simply copy and paste to these files, but feel free to use your own style at this point.

First, styling for your overall application.

{title="src/index.css",lang="css"}
~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial,sans-serif;
}

a {
  color: #222;
}

a:hover {
  text-decoration: underline;
}

ul, li {
  list-style: none;
  padding: 0;
  margin: 0;
}

input {
  padding: 10px;
  border-radius: 5px;
  outline: none;
  margin-right: 10px;
  border: 1px solid #dddddd;
}

button {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #dddddd;
  background: transparent;
  color: #808080;
  cursor: pointer;
}

button:hover {
  color: #222;
}

*:focus {
  outline: none;
}
~~~~~~~~

Second, styling for your components in the App file.

{title="src/App.css",lang="css"}
~~~~~~~~
.page {
  margin: 20px;
}

.interactions {
  text-align: center;
}

.table {
  margin: 20px 0;
}

.table-header {
  display: flex;
  line-height: 24px;
  font-size: 16px;
  padding: 0 10px;
  justify-content: space-between;
}

.table-empty {
  margin: 200px;
  text-align: center;
  font-size: 16px;
}

.table-row {
  display: flex;
  line-height: 24px;
  white-space: nowrap;
  margin: 10px 0;
  padding: 10px;
  background: #ffffff;
  border: 1px solid #e3e3e3;
}

.table-header > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.table-row > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.button-inline {
  border-width: 0;
  background: transparent;
  color: inherit;
  text-align: inherit;
  -webkit-font-smoothing: inherit;
  padding: 0;
  font-size: inherit;
  cursor: pointer;
}

.button-active {
  border-radius: 0;
  border-bottom: 1px solid #38BB6C;
}
~~~~~~~~

Now you can use the style in some of your components. Don't forget to use React `className` instead of `class` as HTML attribute.

First, apply it in your App ES6 class component.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
# leanpub-start-insert
      <div className="page">
        <div className="interactions">
# leanpub-end-insert
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
# leanpub-start-insert
        </div>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-start-insert
      </div>
# leanpub-end-insert
    );
  }
}
~~~~~~~~

Second, apply it in your Table functional stateless component.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
# leanpub-start-insert
  <div className="table">
# leanpub-end-insert
    {list.filter(isSearched(pattern)).map(item =>
# leanpub-start-insert
      <div key={item.objectID} className="table-row">
# leanpub-end-insert
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
        <span>
          <Button
            onClick={() => onDismiss(item.objectID)}
# leanpub-start-insert
            className="button-inline"
# leanpub-end-insert
          >
            Dismiss
          </Button>
        </span>
# leanpub-start-insert
      </div>
# leanpub-end-insert
    )}
# leanpub-start-insert
  </div>
# leanpub-end-insert
~~~~~~~~

Now you have styled your application and components with basic CSS. It should look quite decent. As you know, JSX mixes up HTML and JavaScript. Now one could argue to add CSS in the mix as well. That's called inline style. You can define JavaScript objects and pass them to the style attribute of an element.

Let's keep the Table column width flexible by using inline style.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    {list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
# leanpub-start-insert
        <span style={{ width: '40%' }}>
          <a href={item.url}>{item.title}</a>
        </span>
        <span style={{ width: '30%' }}>
          {item.author}
        </span>
        <span style={{ width: '10%' }}>
          {item.num_comments}
        </span>
        <span style={{ width: '10%' }}>
          {item.points}
        </span>
        <span style={{ width: '10%' }}>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
# leanpub-end-insert
      </div>
    )}
  </div>
~~~~~~~~

The style is inlined now. You could define the style objects outside of your elements to make it cleaner.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const largeColumn = {
  width: '40%',
};

const midColumn = {
  width: '30%',
};

const smallColumn = {
  width: '10%',
};
~~~~~~~~

After that you would use them in your columns: `<span style={smallColumn}>`.

In general, you will find different opinions and solutions for style in React. You used pure CSS and inline style now. That's sufficient to get started.

I don't want to be opinionated here, but I want to leave you some more options. You can read about them and apply them on your own. But if you are new to React, I would recommend to stick to pure CSS and inline style for now.

* [styled-components][13]
* [CSS Modules][14]

{pagebreak}

You have learned the basics to write your own React application! Let's recap the last chapters:

* React
  * use `this.state` and `setState()` to manage your internal component state
  * pass functions or class methods to your element handler
  * use forms and events in React to add interactions
  * unidirectional data flow is an important concept in React
  * embrace controlled components
  * compose components with children and reusable components
  * usage and implementation of ES6 class components and functional stateless components
  * approaches to style your components
* ES6
  * functions that are bound to a class are class methods
  * destructuring of objects and arrays
  * default parameters
* General
  * higher order functions

Again it makes sense to take a break. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far. Additionally you can read more in the official [documentation][15].

You can find the source code in the [official repository][16].

[1]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor
[2]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer
[3]:	https://facebook.github.io/react/docs/state-and-lifecycle.html
[4]:	https://facebook.github.io/react/docs/handling-events.html
[5]:	https://en.wikipedia.org/wiki/Higher-order_function
[6]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
[7]:	https://facebook.github.io/react/docs/forms.html
[8]:	https://facebook.github.io/react/docs/composition-vs-inheritance.html
[9]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters
[10]:	https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html
[11]:	https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html
[12]:	https://facebook.github.io/react/docs/components-and-props.html
[13]:	https://github.com/styled-components/styled-components
[14]:	https://github.com/css-modules/css-modules
[15]:	https://facebook.github.io/react/docs/installation.html
[16]:	https://github.com/rwieruch/hackernews-client/tree/4.2

[image-1]:	images/set-state-to-render-unidirectional.png