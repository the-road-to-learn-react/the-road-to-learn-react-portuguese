# React Básico

Este capítulo irá lhe apresentar os conceitos básicos de React. Trataremos de assuntos como estado e interações, pois componentes estáticos são um pouco maçantes, não acha? Você também irá aprender os diferentes modos de declarar um componente e como fazer para mantê-los fáceis de reusar e de compor uns com os outros. Prepare-se para dar vida à eles.

## Estado Interno do Componente

O estado local, também chamado de estado interno do componente, lhe permite salvar, modificar e apagar propriedades que nele são armazenadas. Componentes de classe usam inicializam seu estado interno utilizando um construtor. Ele é chamado apenas uma vez (quando o componente é inicializado).

Abaixo, introduzimos um construtor de classe:

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

Quando seu componente possui um construtor, torna-se obrigatória a chamada de `super();`, porque o componente App é uma subclasse de `Component` (`class App extends Component`). Mais tarde, você aprenderá mais sobre componentes de classe em ES6. Você também pode invocar `super(props);` para definir `this.props` no contexto do seu construtor. Caso contrário, se tentar acessar `this.props`, receberá o valor `undefined`. Futuramente, estudaremos mais sobre as _props_ de um componente React.

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

O estado local está amarrado à classe através do objeto `this`. Dessa forma, você pode acessá-lo em qualquer lugar do componente. Por exemplo, no método `render()`. Anteriormente, você usou `map` com uma lista estática de itens (definida fora do componente)  em seu método `render()`. Agora, você irá usar a lista obtida do seu estado local.

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

A lista agora é parte do componente, residindo em seu estado interno. Você pode adicionar, alterar ou remover itens. Todas as vezes que o estado do seu componente mudar, o método `render()` será chamado novamente. Você simplesmente altera o estado interno, sabendo que o componente será de novo renderizado exibindo os dados corretos.

Mas, tenha cuidado. Não altere o estado diretamente, use um método chamado `setState()` para mudá-lo. Você verá mais a respeito no próximo capítulo.

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

Nesse caso, em que a propriedade do objeto e a variável são igualmente chamadas de `name`, você poderia fazer desta forma:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

Aplicando o mesmo raciocínio na sua aplicação, com a variável `list` e a propriedade do estado local que compartilha do mesmo nome:

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

Talvez isso ainda não faça tanto sentido para você. Por que você utilizar nomes computados? Em um capítulo mais adiante, iremos nos deparar com a situação em que poderemos utilizá-los para alocar valores por chave, de uma forma dinâmica em um objeto. É uma forma elegante em JavaScript de gerar _lookup tables_ (um tipo de estrutura de dados).

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

Em primeiro lugar: Por que você teve que fazer isso? Esse passo é necessário porque a amarração do `this` com a instância de classe não é feita automaticamente pelos métodos. Vamos demostrar isso com a ajuda do componente a seguir:

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

Apesar de possível, esta prática deve ser evitada, porque a vinculação do método seria feira todas as vezes que `render()` for chamado. Como basicamente ele roda todas as vezes que seu componente é atualizado, isso poderia trazer implicações de performance. Quando o _binding_ ocorre no construtor, o processo só ocorre uma vez: quando o componente é instanciado. Essa é a melhor abordagem a ser escolhida.

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

Esta outra prática também deveria ser evitada, porque ela irá transformar seu construtor em uma bagunça ao longo do tempo. A finalidade do construtor é instanciar sua classe com todas as propriedades. Por isso, a lógica de negócio dos métodos de classe deve ser definida fora dele.

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

Se não lhe agrada repetidamente fazer o _binding_ no construtor, você pode seguir nesta abordagem. Como a documentação oficial de React continua utilizando _bindings_ no construtor, o livro irá seguir fazendo-o também.

### Exercícios:

* Experimente diferentes abordagens de _binding_ e veja o valor de `this` no console.

## Tratamento de Eventos

**Nota do tradutor**: De agora em diante, neste livro, usaremos "_event handler_" e "tratamento de evento" com o mesmo significado. Apesar de essa última não ser a tradução literal da primeira, é a forma mais conhecida para tal na língua portuguesa. Soaria estranho se usássemos "tratador de eventos", ou até "manipulador de eventos".

Este capítulo deve lhe dar um melhor entendimento sobre tratamento de eventos em elementos. Na sua aplicação, você usa o elemento `button` (a seguir) para remover um item da lista:

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

Este exemplo é um tanto quanto complexo. Você tem que passar um valor para o método de classe e, para tal, precisa encapsulá-lo em outra função (uma _arrow function_). Basicamente, o que precisa ser passado como argumento para o _event handler_ é uma função, não sua chamada. O código a seguir não funcionaria, porque o método de classe seria imediatamente executado quando você abrisse sua aplicação no navegador.

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

Frequentemente, novatos em React têm dificuldades com este tópico do uso de funções no tratamento de eventos. Por este motivo, eu me alonguei tentando explicá-lo em maiores detalhes. Depois de tudo, você deve ter o seguinte código no botão, com uma _arrow function_ _inline_ , concisa, que tem acesso à propriedade `objectID` do objeto item:

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

Outro tópico relevante que sempre é mencionado, com relação à performance, é o das implicações do uso de _arrow functions_ em _event handlers_. Por exemplo, tomemos o caso do `onClick` com uma _arrow function_ envolvendo o `onDismiss`. Todas as vezes que o método `render()` for executado, o _event handler_ irá instanciar a função. Isso _pode_ ter um certo impacto na performance da sua aplicação. Na maioria dos casos, porém, você não irar notar a diferença.

Imagine que você tem uma enorme tabela de dados com 1000 itens e cada linha ou coluna tem uma _arrow function_ sendo definida no _event handler_. Nesse caso, sim, é válida a preocupação a respeito da performance e você poderia implementar um componente dedicado `Button` com o _biding_ ocorrendo no construtor. Mas, preocupar-se com isso agora significa otimização prematura. É mais benéfico focar em aprender apenas React.

### Exercícios:

* Experimente as diferentes abordagens de uso de funções no `onClick` do botão em sua aplicação.

## Interação com Forms e Eventos

Adicionemos outra interação à aplicação, tendo uma experiência com _forms_ e eventos em React. A interação em questão é uma funcionalidade de busca. O valor de entrada no campo de busca será usado para filtrar temporariamente sua lista, baseado na propriedade _title_ de cada item.

O primeiro passo é definir um _form_ com um campo de _input_ em seu JSX.

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

Você irá digitar no _input_ e filtrar a lista por esse termo de busca. Para tanto, você precisa armazenar o valor digitado em seu estado local. Mas, como acessar o valor? É possível utilizar **synthetic events** em React para acessar os detalhes do evento.

Vamos definir um _event handler_  `onChange` para o campo _input_.

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

A função está vinculada ao componente e, portanto, novamente temos um método de classe. Você ainda precisa do _binding_  e definir o método em si.

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

Utilizando um _event handler_ em seu elemento, você ganha acesso ao _synthetic event_ de React na assinatura da função que utilizou como _callback_.

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

O evento tem o _value_ do campo _input_ no seu objeto _target_. Consequentemente, você consegue atualizar o estado local com o termo da busca utilizando `this.setState()` novamente.

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

Ademais, você não deve se esquecer de definir o estado inicial para a propriedade `searchTerm` em seu construtor. O campo _input_ estará vazio de início e, portanto, o valor deveria ser uma _string_ vazia.

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

Agora, todas as vezes que o calor no campo de _input _ muda, você está armazenando o valor digitado no estado interno do seu componente.

Um breve comentário a respeito da atualização do estado local em um componente React: Seria normal se achássemos que, quando atualizamos `searchTerm` com `this.setState`, também deveríamos informar o valor de `list`. Mas não é o caso. O método `this.setState()` de React faz o que chamamos de _shallow merge_. Ele preserva o valor das outras propriedades do objeto estado quando apenas uma delas é atualizada. O estado da lista permanecerá o mesmo, inclusive sem o item que você removeu, quando apenas a propriedade `searchTerm` for alterada.

Voltemos à sua aplicação. A lista ainda não é temporariamente filtrada com base no valor do campo _input_ que está armazenado no seu estado local, mas você já tem em mão tudo o que precisa para fazê-lo. Como? No seu método `render()`, antes de iterar sobre a lista usando _map_, você pode aplicar um filtro à ela, que apenas avaliaria se `searchTerm` coincide com o a propriedade _title_ do item. Vamos usar a funcionalidade _filter_, nativa de JavaScript e já demonstrada anteriormente. Como _filter_ retorna um novo _array_, você pode convenientemente chamá-la antes de _map_.

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

Desta vez, iremos adotar uma abordagem diferente sobre a função _filter_. Queremos definir o seu argumento (outra função) fora do componente. Lá, não temos acesso ao estado do componente e, por consequência, à propriedade `searchTerm` para avaliar a condição de filtragem. Teremos que passar o `searchTerm` como argumento e retornar uma nova função, que avalia a condição. Esse tipo de função retornada por outra função é chamada de _high-order function_.

Normalmente eu não mencionaria _higher-order functions_, mas faz todo o sentido em um livro sobre React. Faz sentido porque React trabalha com um conceito chamado de _high-order components_. Mais tarde, você aprenderá mais sobre isso. Vamos focar agora na funcionalidade _filter_.

Primeiro, você terá que definir a _high-order function_ fora do componente App.

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

A função recebe o `searchTerm` e retorna outra função, porque é o que _filter_ espera como entrada. A função retornada terá acesso ao objeto item, pois será argumento da função _filter_. A filtragem será feita baseada na condição definida nela, que é o que faremos agora.

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

A condição diz que devemos comparar o padrão recebido em `searchTerm` com a propriedade `title` do item da lista. Você pode fazê-lo utilizando `includes`, funcionalidade nativa de JavaScript. Quando o padrão coincide, você retorna _true_ e o item permanece na lista. Senão, o item é removido. Mas, tenha cuidado com comparações de padrões: Você não pode esquecer de formatar ambas as _strings_, transformando seus caracteres em minúsculas. Caso contrário, o título "Redux" e o termo de busca "redux" serão considerados diferentes. Uma vez que estamos trabalhando com listas imutáveis e uma nova lista é retornada pela função _filter_, a lista original permanecerá sem ser modificada.

Uma última coisa a mencionar: "Apelamos" um pouco, utilizando o recuso _includes_ já de ES6. Como faríamos o mesmo, em JavaScript ES5? Você usaria a função `inderOf()` para pegar o índice do item na lista.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Outra refatoração elegante pode ser feita utilizando-se novamente uma _arrow function_, tornando a função mais concisa:

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

Qual das funções é mais legível, vai depender de cada um. Pessoalmente, eu prefiro a segunda opção. O ecossistema de React usa muitos conceitos de programação funcional. Corriqueiramente, você irá utilizar uma função que retorna outra função (_high order function_). Em JavaScript ES6, você pode expressá-las de forma mais concisa com _arrow functions_.

Por fim, você tem que usar a função definida `isSearched()` para filtrar sua lista. Você passa a propriedade `searchTerm` do seu estado local para a função, ela retorna outra função como _input_ de _filter_ e filtra sua lista baseada na condição descrita. Depois disso tudo, iteramos sobre a lista filtrada usando _map_ para exibir um elemento para cada item da lista.

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

A funcionalidade de buscar deve funcionar agora. Tente você mesmo, no seu navegador.

### Exercícios:

* Leia mais sobre [React events][4]
* Leia mais sobre [higher order functions][5]

## ES6 _Destructuring_

Existe um jeito, em JavaScript ES6, de acessar propriedades de objetos mais facilmente: É chamado de _destructuring_. Compare os seguintes trechos de código a seguir, em JavaScript ES5 e ES6:

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

Enquanto que, em JavaScript ES5, você precisa de uma instrução de código a mais todas as vezes que quer acessar uma propriedade de um objeto, em ES6 você pode fazê-lo de uma só vez, em uma única linha.

Uma boa prática, visando legibilidade, é fazer _destructuring_ de múltiplas propriedades quebrando-o em várias linhas:

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

O mesmo vale para _arrays_, que também podem sofrer _destructuring_.

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

Talvez você tenha notado que o objeto do estado local do componente App pode ser “desestruturado” da mesma forma (iremos nos alternar entre “desestruturação” e o original “_destructuring_” no texto, uma vez que é importante saber o termo amplamente conhecido na comunidade). A linha de código com _filter_ e _map_ ficará menor.

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

Novamente, o jeito ES5 e o ES6:

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

Uma vez que o livro usa JavaScript ES6 a maior parte do tempo, é aconselhável que você também faça esta opção.

### Exercícios:

* leia mais a respeito de [ES6 destructuring][6]

## Componentes Controlados

Você já tomou conhecimento do fluxo unidirecional de dados em React. A mesma lógica se aplica para o campo de _input_, que atualiza o estado local com o `searchTerm` para filtrar a lista. Quando o estado é alterado, o método  `render()` é executado novamente e utiliza o `searchTerm` mais recente do estado local para aplicar a condição de filtragem.

Mas, não teríamos esquecido de alguma coisa no elemento _input_? A _tag_ HTML “input” possui um atributo `value`. Este, por sua vez, geralmente contém o valor que é mostrado no campo. Neste caso, a propriedade `searchTerm`. Acho que ficou a impressão de que não precisamos disso em React.

Errado. Elementos de _forms_ como `<input>`, `<textarea>` e `<select>` possuem seu próprio estado em HTML puro. Eles modificam o valor internamente quando alguém de fora do componente o muda. Em React, isso é chamado de um **componente não controlado**, porque ele gerencia seu próprio estado. Você deve garantir-se de que os transformou em **componentes controlados**.

Mas, como fazê-lo? Você só precisa definir o atributo “_value_” de um campo de _input_. O valor aqui, neste exemplo, já está salvo na propriedade `searchTerm` do estado do componente. Por que não acessá-lo, então?

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

É só isso. O _loop_ do fluxo de dados unidirecional do campo _input_ torna-se auto-contido. O estado interno do componente é a única fonte confiável de dados (_single source of truth_) para o _input_.

Toda essa história de gerenciamento de estado interno e fluxo unidirecional de dados deve ser nova para você. Mas, uma vez acostumado, será o seu jeito natural de implementar coisas em React. Em geral, React trouxe um interessante novo padrão (com o fluxo de dados unidirecional) para o mundo das SPA (_single page applications_). Agora, este padrão é adotado por diversos _frameworks_ e bibliotecas no momento.

### Exercícios:

* Leia mais sobre [React forms][7]

## Dividindo componentes

Você tem em mãos um componente _App_ de tamanho considerável. Ele continua crescendo e, eventualmente, pode tornar-se confuso. É possível dividi-lo partes, ou seja, componentes menores.

Comecemos usando um componente para o _input_ de busca e um componente para a lista de itens.

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

Você pode passar, para estes componentes, propriedades que eles podem utilizar. No caso, o componente _App_ precisa passar as propriedades gerenciadas no seu estado local e os seus métodos de classe.

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

Agora, você pode definir os componentes ao lado de App. Estes também serão classes ES6. Eles irão renderizar os mesmos elementos de antes.

O primeiro é o componente _Search_:

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

O segundo é o componente _Table_:

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

Agora que você tem esses três componentes, talvez tenha notado o objeto `props`, que é acessível na instância de classe com o uso de `this`. As `props` (diminutivo de propriedades) têm todos os valores que você passou para os componentes quando os utilizou dentro de _App_. Desta forma, componentes podem passar propriedades para os níveis abaixo, na árvore de componentes.

Tendo extraído esses componentes de _App_, você estaria apto a reutilizá-los em qualquer outro lugar. Uma vez que componentes recebem valores através do objeto _props_, você pode passar valores diferentes para seu componente a cada vez que utilizá-lo.

### Exercícios:

* imagine outros componentes que você poderia separar, como fez com _Search_ e _Table_.
  * entretanto, não o faça agora, na prática, para não ter conflitos com o que faremos nos capítulos seguintes.

## Componentes Integráveis

Existe ainda uma pequena propriedade que pode ser acessada no objeto de  _props_: a _prop_ `children`. Você pode usá-la para passar elementos dos componentes acima na hierarquia para os que estão abaixo, tornando possível integrar componentes com outros. Vejamos como isso é feito, quando você manda apenas um texto (_string_) como _child_ para o componente _Search_.

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

Agora, o componente _Search_ pode desestruturar a propriedade _children_ do objeto _props_. Aí, poderá especificar onde _children_ deverá ser mostrado.

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

O texto “Search” deverá estar visível próximo do campo de _input_ agora. Quando você utilizar o componente _Search_ em algum outro lugar, você poderá escolher um texto diferente, se assim quiser. No fim das contas, não é só texto que pode ser passado assim. Você pode enviar um elemento (e árvores de elementos, encapsuladas por outros componentes) como _children_. Esta propriedade permite entrelaçar componentes.

### Exercícios:

* Leia mais sobre [o modelo de composição de React][8]

## Componentes Reutilizáveis

Ter componentes reutilizáveis e combináveis lhe dá o poder de produzir hierarquias de componentes com competência. Eles são a base da camada de visão do React. Os últimos capítulos mencionaram o termo reusabilidade e, agora, você pode reutilizar os componentes _Table_ e _Search_. Até mesmo o componente _App_ é reutilizável, porque você poderia instanciá-lo em algum outro lugar.

Vamos definir mais um componente reutilizável, _Button_, que irá, eventualmente, ser utilizado com mais frequência. 

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

Pode parecer redundante declarar um componente como este. Você irá utilizar um componente `Button` ao invés do elemento `button`, poupando apenas o `type="button"`. Exceto por este atributo de tipo, quando você decide usar o componente `Button`, você precisará definir todo o resto. Mas, você tem que pensar em termos de um investimento de longo prazo. Imagine que possui vários botões em sua aplicação, quer mudar um atributo, estilo ou comportamento do botão. Sem o componente recém criado, você teria que manualmente refatorar cada botão. O componente _Button_ garante que existirá uma referência única, um _Button_ para refatorar todos os botões de uma só vez. “_One Button to rule them all_.”

Uma vez que você já tem um elemento de botão, substitua-o pelo componente _Button_. Note que ele omite o atributo _type_, já especificado dentro do próprio componente.

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

O componente espera receber uma propriedade `className` via props. O atributo `className` é mais uma especificidade React, derivando do atributo HTML `class`. Mas, nós não passamos este atributo quando utilizamos _Button_ e, sendo assim, deveria ser mais explícito no código do componente que `className` é opcional. 

Portanto, você deveria definir um valor padrão para o parâmetro (mais uma funcionalidade de JavaScript ES6).

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

Agora, sempre que não houver propriedade `className` especificada, quando o componente _Button_ for utilizado, o valor do atributo será uma _string_ vazia, ao invés de `undefined`.

### Exercícios:

* Leia mais a respeito de [parâmetros _default_ em ES6][9]

## Declarações de Componentes

No momento, você tem quatro componentes implementados com classes ES6. É possível melhorar ainda mais este cenário. Deixe-me introduzir para você os **componentes funcionais sem estado** (_stateless funcional componentes_), como uma alternativa a componentes de classe. Vamos apresentar os diferentes tipos de componentes em React, antes de partir para a refatoração.

* **_Functional Stateless Components_:** Esses componentes são funções que recebem uma entrada e retornam uma saída. As _props_ do componente são a entrada. A saída é uma instância de componente (ou seja, puro e simples JSX). Em termos, é bem semelhante a componentes de classe. Contudo, eles são funções (_functional_) e não possuem estado local (_stateless_). Você não consegue acessar ou atualizar o estado com `this.state` ou `this.setState()`, porque não existe o objeto `this` aqui. Além disso, eles não possuem métodos de ciclo de vida (_lifecycle methods_). Apesar de não ter explicitamente aprendido a respeito ainda, você já utilizou dois: `constructor()` e `render()`. Ao passo que o construtor roda apenas uma vez durante todo o tempo de vida de um componente, o método `render()` é executado uma vez no início e também todas as vezes que o componente é atualizado. Tenha em mente este detalhe dos _stateless funcional components_ (ausência de métodos de ciclo de vida), para quando chegarmos a este assunto posteriormente.

* **Componentes de Classe ES6:** Você já utilizou este tipo de declaração de componente nos quatro que construiu até aqui, extendendo o componente React. O `extends` atrela ao componente todos os métodos de ciclo de vida, disponíveis na API de componentes React. Assim, você pôde utilizar o método `render()`. Além disso, é possível armazenar e manipular o estado através de `this.state` e `this.setState()`.

* **React.createClass:** Esta forma era utilizada nas versões mais antigas de React e ainda é, em aplicações React que utilizam JavaScript ES5. Mas, o [Facebook a declarou como _deprecated_][10], preferindo o uso de JavaScript ES6. Um [deprecation warning foi adicionado na versão 15.5][11]. Ela não será utilizada no livro.

Assim, basicamente, sobram-nos apenas duas maneiras de declarar componentes. Mas, quando usar cada uma? Uma regra simples é: use _functional stateless components_ quando você não precisa de estado ou de métodos de ciclo de vida. Geralmente, comece implementando seu componente desta forma e, uma vez que você precisa acessar o estado ou métodos de ciclo de vida, você pode refatorá-lo para um componente de classe ES6. Fizemos o inverso aqui no livro, apenas para fins de aprendizado.

Voltemos para sua aplicação. O componente App usa diretamente o seu estado interno. Por este motivo, ele tem que permanecer escrito como um componente de classe. Mas, os outros três componentes não precisam acessar `this.state` ou `this.setState()`, muito menos possuem algum método de ciclo de vida. Vamos, juntos, refatorar o componente _Search_ para um _stateless functional component_. _Table_ e _Button_ ficarão como exercício para você.

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

Basicamente, é isso: as props estão acessíveis na assinatura da função e o seu retorno é código JSX. Mas, você pode melhorar ainda mais o código. Da mesma forma que você utilizou _destructuring_ antes, pode fazê-lo novamente no parâmetro _props_ da assinatura da função.

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

De novo, pode melhorar. Você ja sabe que _arrow functions_ permitem que você escreva funções mais concisas. Você pode remover os caracteres de declaração de bloco no corpo da função. Ocorre um retorno implícito e, desta forma, você pode remover também a instrução _return_.

Uma vez que seu _stateless functional component_ é uma função, você pode escrevê-lo da forma concisa também.

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

Este último passo foi especialmente útil: ele forçou que a função tenha apenas _props_ como entrada e JSX como saída. Contudo, se você realmente precisar fazer alguma coisa (representada aqui pelo comentário _do something_) além, sempre é possível devolver as declarações de bloco de corpo e o retorno da função.

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

Não precisa fazê-lo agora. A motivação para manter a versão concisa é: Quando utilizando a versão mais explícita, as pessoas geralmente tendem a fazer muitas coisas em uma função. Deixando a função com retorno implícito, você pode focar nos seus _inputs_ e _outputs_.

Agora, você tem um _stateless functional component_ super enxuto. Se, em algum momento, você precisar acessar seu estado local ou métodos de ciclo de vida, bastará refatorar seu código para um componente de classe ES6. Ademais, vimos aqui como ES6 pode ser utilizado em componentes React para torná-los mais concisos e elegantes.

### Exercícios:

* Refatore _Table_ e _Button_ para _stateless functional compotes_
* Leia mais a respeito de [componentes de classe ES6 e functional stateless components][12]

## Estilizando Componentes

Adicionemos alguns estilos básicos à nossa aplicação e aos nossos componentes. Você pode reutilizar os arquivos _src/App.css_ e _src/index.css_. Estes arquivos já devem estar presentes em seu projeto, uma vez que você o criou utilizado o _create-react-app_. Devem ter sido importados em seus arquivos _src/App.js_ e _src/Index.js_, respectivamente. Eu preparei alguns estilos que você pode simplesmente copiar e colar nesses arquivos. Mas, sinta-se livre para user seus próprios CSS, se quiser.

Primeiramente, estilizando sua aplicação de um modo geral:

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

Em seguida, estilos para seus componentes no arquivo _App_:

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

Agora, você pode utilizá-los em alguns dos seus componentes. Não se esqueça de usar, como atributo HTML, `className` (de React) no lugar de `class`.

Primeiramente, aplicaremos os estilos no componente _App_:

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

Depois, faça o mesmo para o componente _Table_.

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

Você estilizou sua aplicação e componentes com CSS básico. Agora, ela deve ter um visual minimamente decente. Como você bem sabe, JSX mistura HTML e JavaScript. Sendo assim, alguém pode argumentar que é perfeitamente aceitável misturar CSS também (_inline styles_). Você pode definir objetos JavaScript e passá-los para o atributo _style_ de um elemento.  

Vamos definir uma coluna de largura dinâmica em Table, utilizando um _inline style_:

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

Se quiser, você pode definir seus objetos de estilos fora dos elementos, mantendo-os mais limpos.

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

Finalmente, você pode utilizá-los em suas colunas: `<span style={smallColumn}>`.

Em geral, você irá encontrar diferentes opções e soluções para aplicar estilos em React. Utilizamos CSS puro e _inline styles_, até agora. É o suficiente para iniciar.

Não quero ser dogmático aqui e irei lhe deixar mais algumas opções. Você pode ler a respeito delas e aplicá-las por sua própria conta. Mas, se você é novo em React, eu recomendaria que permaneça com CSS puro e estilos _inline_, por enquanto.

* [styled-components][13]
* [CSS Modules][14]

{pagebreak}

Você aprendeu o básico para escrever sua própria aplicação React! Recapitulemos os últimos capítulos:

* React
  * use `this.state` e `setState()` para gerenciar o estado interno do seu componente
   * como passar funções ou métodos de classe para _element handlers_
   * utilizando _forms_ e eventos em React para adicionar interações
   * o fluxo de dados unidirecional unidirectional é um importante conceito em React
   * adote a prática de componentes controlados
   * integre componentes com componentes filhos reutilizáveis
   * implementação e uso de componentes de classe ES6 e _stateless functional components_
   * abordagens para estilizar seus componentes
* ES6
   * funções que são atreladas à uma classe são métodos de classe
   * _destructuring_ de objetos e arrays
   * parâmetros _default_
* Geral
   * _higher order functions_

Mais uma vez, é recomendável fazer um intervalo na leitura. Internalize o que aprendeu de novo e pratique. Faça experiências com o código-fonte que você escreveu até agora. Ademais, você pode também ler mais a respeito dos assuntos aqui discorridos na [documentação oficial][15].

Você pode encontrar o código-fonte deste capítulo no [repositório oficial][16].

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