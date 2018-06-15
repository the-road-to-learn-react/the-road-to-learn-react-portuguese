# React Básico

Este capítulo irá lhe apresentar os conceitos básicos de React. Trataremos de assuntos como estado e interações, pois componentes estáticos são um pouco maçantes, não acha? Você também irá aprender os diferentes modos de declarar um componente e como fazer para mantê-los fáceis de reusar e de compor uns com os outros. Prepare-se para dar vida à eles.

## Estado Interno do Componente

O estado local, também chamado de estado interno do componente, lhe permite salvar, modificar e apagar propriedades que nele são armazenadas.

Componentes de classe usam inicializam seu estado interno utilizando um construtor. Ele é chamado apenas uma vez (quando o componente é inicializado). Abaixo, um construtor de classe:

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	# leanpub-start-insert
	  constructor(props) {
	    super(props);
	  }
	# leanpub-end-insert
	
	  ...
	
	}

Quando seu componente possui um construtor, torna-se obrigatória a chamada de `super();`, porque o componente App é uma subclasse de `Component` (`class App extends Component`). Mais tarde, você aprenderá mais sobre componentes de classe em ES6.

Você também pode invocar `super(props);` para definir `this.props` no contexto do seu construtor. Caso contrário, se tentar acessar `this.props`, receberá o valor `undefined`. Futuramente, estudaremos mais sobre as _props_ de um componente React.

A essa altura, o estado inicial do seu componente é composto por apenas uma lista de itens:

{title="src/App.js",lang=javascript}
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

O estado local está amarrado à classe através do objeto `this`. Dessa forma, você pode acessá-lo em qualquer lugar do componente. Por exemplo, no método `render()`.

Anteriormente, você usou `map` com uma lista estática de itens (definida fora do componente)  em seu método `render()`. Agora, você irá usar a lista obtida do seu estado local.

{title="src/App.js",lang=javascript}
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
	const name = 'Robin';
	
	const user = {
	  name: name,
	};

Nesse caso, em que a propriedade do objeto e a variável são igualmente chamadas de `name`, você poderia fazer desta forma:

{title="Code Playground",lang="javascript"}
	const name = 'Robin';
	
	const user = {
	  name,
	};

Aplicando o mesmo raciocínio na sua aplicação, com a variável `list` e a propriedade do estado local que compartilha do mesmo nome:

{title="Code Playground",lang="javascript"}
	// ES5
	this.state = {
	  list: list,
	};
	
	// ES6
	this.state = {
	  list,
	};

Um outro atalho elegante é a declaração concisa de métodos em JavaScript ES6.

{title="Code Playground",lang="javascript"}
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

Por último, mas não menos importante, o uso de nomes computados de propriedades é permitido em JavaScript ES6.

{title="Code Playground",lang="javascript"}
	// ES5
	var user = {
	  name: 'Robin',
	};
	
	// ES6
	const key = 'name';
	const user = {
	  [key]: 'Robin',
	};

Talvez isso ainda não faça tanto sentido para você. Por que você utilizar nomes computados? Em um capítulo mais adiante, iremos nos deparar com a situação em que poderemos utilizá-los para alocar valores por chave, de uma forma dinâmica em um objeto. É uma forma elegante em JavaScript de gerar _lookup tables_ (um tipo de estrutura de dados).

### Exercícios:

* Experimente trabalhar com inicialização de objetos com ES6
* Leia mais sobre [inicialização de objetos em ES6][2]

## Fluxo Unidirecional de Dados

Você tem agora em mãos um componente App com estado interno, que ainda não foi mexido. O estado é estático e, consequentemente, o seu componente também. Uma boa maneira de experimentar manipular o estado é criando alguma interação entre componentes.

Vamos adicionar um botão para cada item da lista que é exibida. O botão tem o rótulo "_Dismiss_" (dispensar) e irá remover o item, sendo útil quando você quer manter uma lista constando apenas itens ainda não lidos e dispensar os que não tem interesse, por exemplo.

{title="src/App.js",lang=javascript}
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

O método de classe `onDismiss()` ainda não foi definido, nós o faremos daqui a pouco.  Vamos primeiro focar no tratamento do `onClick` do elemento `button`. Como você pode ver, o método `onDismiss()` no `onClick` está encapsulado por uma _arrow function_. Nela, você tem acesso à propriedade `objectID` do objeto `item`, que identifica qual item será removido. Uma alternativa à isso seria definir a função fora e apenas passá-la para o `onClick`. Outro capítulo irá explicar em maiores detalhes o tópico de tratamento de eventos em elementos.

Você viu que o elemento button é declarado em múltiplas linhas? Note que, à medida que a quantidade de atributos cresce, deixar tudo em uma única linha pode acabar gerando uma bagunça. Por esse motivo, o elemento button foi escrito em múltiplas linhas e indentado. Não é obrigatório, mas é um estilo de código que eu recomendo fortemente.

Chegou a hora de implementar o comportamento de `onDismiss()`. A função recebe um id, para identificar qual item será removido e, por ser amarrada à classe, é um método de classe. Sendo assim, deve ser acessada com `this.onDismiss()` e não apenas `onDismiss()`. O objeto `this` é a instância da sua classe.

Para definir o `onDismiss()` como um método de classe, você precisa usar `bind` no construtor.  _Bindings_ serão explicados mais adiante, em outro capítulo.

{title="src/App.js",lang=javascript}
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

O próximo passo é definir a funcionalidade em si, ou a lógica de negócio do método em sua classe, da seguinte maneira:

{title="src/App.js",lang=javascript}
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

Feito isso, você pode escrever o que acontece dentro do método. Basicamente, você quer remover da lista o idem identificado pelo id e armazenar a lista atualizada no seu estado local. A nova lista será usada no método `render`, que será novamente chamado, para ser exibida. O item removido não irá mais aparecer.

É possível remover um item de uma lista usando a funcionalidade nativa de JavaScript _filter_, que é uma função que recebe outra função como entrada. Esta, por sua vez, tem acesso a cada valor na lista, pois _filter_ está iterando sobre ela, permitindo-lhe checar item a item na lista baseado na condição fornecida. Se o resultado da avaliação for _true_, o item permanece na lista. Caso contrário, será filtrado dela. Além disso, é bom saber que a função _filter_ retorna uma nova lista e não mexe no estado da antiga. Ela atende à convenção em React de manter estruturas de dados imutáveis.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	# leanpub-start-insert
	  const updatedList = this.state.list.filter(function isNotId(item) {
	    return item.objectID !== id;
	  });
	# leanpub-end-insert
	}

No próximo passo, você pode extrair a função e passá-la como argumento para _filter_.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	# leanpub-start-insert
	  function isNotId(item) {
	    return item.objectID !== id;
	  }
	
	  const updatedList = this.state.list.filter(isNotId);
	# leanpub-end-insert
	}

Ainda pode fazê-lo de forma mais concisa, usando novamente uma _arrow function_ de JavaScript.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	# leanpub-start-insert
	  const isNotId = item => item.objectID !== id;
	  const updatedList = this.state.list.filter(isNotId);
	# leanpub-end-insert
	}

Pode até colocá-la _inline_ novamente, como fez no `onClick` do botão, mas pode ser que assim ela fique menos legível.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	# leanpub-start-insert
	  const updatedList = this.state.list.filter(item => item.objectID !== id);
	# leanpub-end-insert
	}

A lista agora remove o item clicado. Entretanto, o estado local ainda não foi atualizado. Finalmente você pode usar o método `setState()` para atualizar a lista no estado interno do componente.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	  const isNotId = item => item.objectID !== id;
	  const updatedList = this.state.list.filter(isNotId);
	# leanpub-start-insert
	  this.setState({ list: updatedList });
	# leanpub-end-insert
	}

Rode novamente sua aplicação e experimente clicar no botão "Dismiss". Provavelmente, irá funcionar e você estará testemunhando nesse momento o **fluxo unidirecional de dados** em React. Você dispara uma ação em sua _view_ com `onClick()`, uma função ou método de classe muda o estado interno do componente e `render()` é de novo executado para atualizar a _view_.

![Internal state update with unidirectional data flow][image-1]

### Exercícios:

* Leia mais sobre [o ciclo de vida do estado de componentes em React][3]

## Bindings

É importante aprender sobre _bindings_ em classes JavaScript quando se vai trabalhar com componentes de classe em React. No capítulo anterior, você os utilizou para ligar seu método `onDismiss()` à classe em seu construtor.

{title="src/App.js",lang=javascript}
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

Em primeiro lugar: Por que você teve que fazer isso?

Esse passo é necessário porque a amarração do `this` com a instância de classe não é feita automaticamente pelos métodos. Vamos demostrar isso com a ajuda do componente a seguir:

{title="Code Playground",lang=javascript}
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

O componente renderiza normalmente, mas quando você clica no botão, obtém `undefined` no console. Essa é uma das maiores fontes de _bugs_ quando se usa React. Você deseja usar `this.state` em um método de classe e ele não está acessível, porque `this` é `undefined`. Para corrigir isso, você precisa criar o _binding_ entre o método e `this`.

No exemplo a seguir, o método é propriamente vinculado a `this` dentro do construtor da classe.

{title="Code Playground",lang=javascript}
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

Executando novamente o teste, o objeto `this` (ou, sendo mais específico, a instância de classe) está definido nesse contexto e você tem acesso a `this.state`, ou `this.props`, que você conhecerá depois.  

O _binding_ de métodos também poderia ser feito em outros lugares, como no `render()`, por exemplo.

{title="Code Playground",lang=javascript}
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

Apesar de possível, deve ser evitado, porque a vinculação do método seria feira todas as vezes que `render()` for chamado. Como basicamente ele roda todas as vezes que seu componente é atualizado, isso poderia trazer implicações de performance. Quando o _binding_ ocorre no construtor, o processo só ocorre uma vez: quando o componente é instanciado. Essa é a melhor abordagem a ser escolhida.

Outra coisa que, uma vez ou outra, alguém acaba fazendo, é definir a lógica de negócios dos métodos de classe dentro do construtor.

{title="Code Playground",lang=javascript}
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

Essa prática também deveria ser evitada, porque ela irá transformar seu construtor em uma bagunça ao longo do tempo. A finalidade do construtor é instanciar sua classe com todas as propriedades. Por isso, a lógica de negócio dos métodos de classe deve ser definida fora dele.

{title="Code Playground",lang=javascript}
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

Por fim, devo mencionar que métodos de classe podem ser automaticamente vinculados sem fazê-lo explicitamente, com o uso de _arrow functions_.

{title="Code Playground",lang=javascript}
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

Se a repetição do processo de _binding_ no construtor não lhe agrada, você pode seguir nessa abordagem. Como a documentação oficial de React continua utilizando _bindings_ no construtor, este livro irá fazê-lo também.

### Exercícios:

* Experimente diferentes abordagens de _binding_ e veja o valor de `this` no console.

## Tratamento de Eventos

**Nota do tradutor**: De agora em diante, neste livro, usaremos "_event handler_" e "tratamento de evento" com o mesmo significado. Apesar de essa última não ser a tradução literal da primeira, é a forma mais conhecida para tal na língua portuguesa. Soaria estranho se usássemos "tratador de eventos", ou até "manipulador de eventos".

Este capítulo deve lhe dar um melhor entendimento sobre tratamento de eventos em elementos. Na sua aplicação, você usa o elemento `button` a seguir para remover um item da lista:

{title="src/App.js",lang=javascript}
	...
	
	<button
	  onClick={() => this.onDismiss(item.objectID)}
	  type="button"
	>
	  Dismiss
	</button>
	
	...

Este exemplo é um tanto quanto complexo. Você tem que passar um valor para o método de classe e, para tal, precisa encapsulá-lo em outra função (uma _arrow function_). Basicamente, o que precisa ser passado como argumento para _event handler_ é uma função, não sua chamada. O código a seguir não funcionaria, porque o método de classe seria imediatamente executado quando você abrisse sua aplicação no navegador.

{title="src/App.js",lang=javascript}
	...
	
	<button
	  onClick={this.onDismiss(item.objectID)}
	  type="button"
	>
	  Dismiss
	</button>
	
	...

Quando usamos `onClick={executarAlgo()}`, a função `executarAlgo()` seria executada imediatamente após a aplicação abrir no _browser_. A expressão passada para o _handler_ é avaliada e, como o valor retornado não é uma função, nada irá acontecer quando você clicar no botão. Mas, quando fazemos `onClick={executarAlgo}`, onde `executarAlgo` é o nome de uma função, essa só será executada quando o botão for clicado. A mesma regra se aplica para o método `onDismiss` usado em sua aplicação.

Entretanto, não é suficiente declarar `onClick={this.onDismiss}`, porque precisamos passar a propriedade `item.objectID` para o método, para identificar qual item será removido. Por esse motivo, usamos uma _arrow function_ como _wrapper_, utilizando o conceito conhecido em JavaScript como _high-order function_ , que será brevemente explicado mais tarde.

{title="src/App.js",lang=javascript}
	...
	
	<button
	  onClick={() => this.onDismiss(item.objectID)}
	  type="button"
	>
	  Dismiss
	</button>
	
	...

Uma outra alternativa seria definir a função _wrapper_ em algum outro lugar e passá-la como argumento para to tratamento do evento. Uma vez que precisa ter acesso ao item, ela deve residir dentro do bloco da função _map_.

{title="src/App.js",lang=javascript}
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

No fim das contas, o _event handler_ do elemento precisa receber uma função. Como exemplo, faça o teste com esse código, que faz o contrário:

{title="src/App.js",lang=javascript}
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

A função é executada assim que você abre a aplicação no navegador, mas não quando você clica no botão. Enquanto que o código a seguir só roda no momento do clique. É uma função que é executada quando você dispara o evento.

{title="src/App.js",lang=javascript}
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

Visando manter o código conciso, você pode transformá-la de volta uma uma _arrow function_, fazendo o mesmo que fizemos com o método de classe `onDismiss()`.

{title="src/App.js",lang=javascript}
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

Frequentemente, novatos em React têm dificuldades com este tópico do uso de funções no tratamento de eventos. Por isso, me alonguei tentando explicar em maiores detalhes.

Feita isso, agora você deve ter o seguinte código no botão, com uma _arrow function_ _inline_  concisa que tem acesso à propriedade `objectID` do objeto item:

{title="src/App.js",lang=javascript}
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

Outro tópico relevante que sempre é mencionado, relacionado à performance, trata sobre as implicações do uso de _arrow functions_ em _event handlers_. Por exemplo, tomemos o caso do `onClick` com uma _arrow function_ envolvendo o `onDismiss`. Todas as vezes que o método `render()` for executado, o _event handler_ irá instanciar a função. Isso _pode_ ter um certo impacto na performance da sua aplicação. Na maioria dos casos, porém, você não irar notar a diferença.

Imagine que você tem uma enorme tabela de dados com 1000 itens e cada linha ou coluna tem uma _arrow function_ sendo definida no _event handler_. Nesse caso, sim, é válida a preocupação a respeito da performance e você poderia implementar um componente dedicado `Button` com o _biding_ ocorrendo no construtor. Mas, preocupar-se com isso agora significa otimização prematura. É mais benéfico focar em aprender React em si.

### Exercícios:

* Experimente as diferentes abordagens de uso de funções no `onClick` do botão em sua aplicação.

## Interação com Forms e Eventos

Adicionemos outra interação à aplicação, tendo uma experiência com _forms_ e eventos em React. A interação em questão é uma funcionalidade de busca. O valor de entrada no campo de busca será usado para filtrar temporariamente sua lista, baseado na propriedade _title_ de cada item.

O primeiro passo é definir um _form_ com um campo de _input_ em seu JSX.

{title="src/App.js",lang=javascript}
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

Você irá digitar no _input_ e filtrar a lista por esse termo de busca. Para tanto, você precisa armazenar o valor digitado em seu estado local. Mas, como acessar o valor? É possível utilizar **synthetic events** em React para acessar os detalhes do evento.

Vamos definir um _event handler_  `onChange` para o campo _input_.

{title="src/App.js",lang=javascript}
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

A função está vinculada ao componente e, portanto, novamente temos um método de classe. Você ainda precisa do _binding_  e definir o método em si.

{title="src/App.js",lang=javascript}
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

Utilizando um _event handler_ em seu elemento, você ganha acesso ao _synthetic event_ de React na assinatura da função que utilizou como _callback_.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	# leanpub-start-insert
	  onSearchChange(event) {
	# leanpub-end-insert
	    ...
	  }
	
	  ...
	}

O evento tem o _value_ do campo _input_ no seu objeto _target_. Consequentemente, você consegue atualizar o estado local com o termo da busca utilizando `this.setState()` novamente.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  onSearchChange(event) {
	# leanpub-start-insert
	    this.setState({ searchTerm: event.target.value });
	# leanpub-end-insert
	  }
	
	  ...
	}

Ademais, você não deveria esquecer de definir o estado inicial para a propriedade `searchTerm` em seu construtor. O campo _input_ estará vazio de início e, portanto, o valor deveria ser uma _string_ vazia.

{title="src/App.js",lang=javascript}
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

Agora, todas as vezes que o calor no campo de _input _ muda, você está armazenando o valor digitado no estado interno do seu componente.

Um breve comentário a respeito da atualização do estado local em um componente React: Seria normal se achássemos que, quando atualizamos `searchTerm` com `this.setState`, também deveríamos informar o valor de `list`. Mas não é o caso. O método `this.setState()` de React faz o que chamamos de _shallow merge_. Ele preserva o valor das outras propriedades do objeto estado quando apenas uma delas é atualizada. O estado da lista permanecerá o mesmo, inclusive sem o item que você removeu, quando apenas a propriedade `searchTerm` for alterada.

Voltemos à sua aplicação. A lista ainda não é temporariamente filtrada com base no valor do campo _input_ que está armazenado no seu estado local, mas você já tem em mão tudo o que precisa para fazê-lo. Como? No seu método `render()`, antes de iterar sobre a lista usando _map_, você pode aplicar um filtro à ela, que apenas avaliaria se `searchTerm` coincide com o a propriedade _title_ do item. Vamos usar a funcionalidade _filter_, nativa de JavaScript e já demonstrada anteriormente. Como _filter_ retorna um novo _array_, você pode convenientemente chamá-la antes de _map_.

{title="src/App.js",lang=javascript}
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

Desta vez, iremos adotar uma abordagem diferente sobre a função _filter_. Queremos definir o seu argumento (outra função) fora do componente. Lá, não temos acesso ao estado do componente e, por consequência, à propriedade `searchTerm` para avaliar a condição de filtragem. Teremos que passar o `searchTerm` como argumento e retornar uma nova função, que avalia a condição. Esse tipo de função retornada por outra função é chamada de _high-order function_.

Normalmente eu não mencionaria _higher-order functions_, mas faz todo o sentido em um livro sobre React. Faz sentido porque React trabalha com um conceito chamado de _high-order components_. Mais tarde, você aprenderá mais sobre isso. Vamos focar agora na funcionalidade _filter_.

Primeiro, você terá que definir a _high-order function_ fora do componente App.

{title="src/App.js",lang=javascript}
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

A função recebe o `searchTerm` e retorna outra função, porque é o que _filter_ espera como entrada. A função retornada terá acesso ao objeto item, pois será argumento da função _filter_. A filtragem será feita baseada na condição definida nela, que é o que faremos agora.

{title="src/App.js",lang=javascript}
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

A condição diz que devemos comparar o padrão recebido em `searchTerm` com a propriedade `title` do item da lista. Você pode fazê-lo utilizando `includes`, funcionalidade nativa de JavaScript. Quando o padrão coincide, você retorna _true_ e o item permanece na lista. Senão, o item é removido. Mas, tenha cuidado com comparações de padrões: Você não pode esquecer de formatar ambas as _strings_, transformando seus caracteres em minúsculas. Caso contrário, o título "Redux" e o termo de busca "redux" serão considerados diferentes.

Uma vez que estamos trabalhando com listas imutáveis e uma nova lista é retornada pela função _filter_, a lista original permanecerá sem ser modificada.

Uma última coisa a mencionar: "Apelamos" um pouco, utilizando o recuso _includes_ já de ES6. Como faríamos o mesmo, em JavaScript ES5? Você usaria a função `inderOf()` para pegar o índice do item na lista.

{title="Code Playground",lang="javascript"}
	// ES5
	string.indexOf(pattern) !== -1
	
	// ES6
	string.includes(pattern)

Outra refatoração elegante pode ser feita utilizando-se novamente uma _arrow function_, tornando a função mais concisa:

{title="Code Playground",lang="javascript"}
	// ES5
	function isSearched(searchTerm) {
	  return function(item) {
	    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
	  }
	}
	
	// ES6
	const isSearched = searchTerm => item =>
	  item.title.toLowerCase().includes(searchTerm.toLowerCase());

Qual das funções é mais legível, vai depender de cada um. Pessoalmente, eu prefiro a segunda opção. O ecossistema de React usa muitos conceitos de programação funcional. Corriqueiramente, você irá utilizar uma função que retorna outra função (_high order function_). Em JavaScript ES6, você pode expressá-las de forma mais concisa com _arrow functions_.

Por fim, você tem que usar a função definida `isSearched()` para filtrar sua lista. Você passa a propriedade `searchTerm` do seu estado local para a função, ela retorna outra função como _input_ de _filter_ e filtra sua lista baseada na condição descrita. Depois disso tudo, iteramos sobre a lista filtrada usando _map_ para exibir um elemento para cada item da lista.

{title="src/App.js",lang=javascript}
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

A funcionalidade de buscar deve funcionar agora. Tente você mesmo, no seu navegador.

### Exercícios:

* Leia mais sobre [React events][4]
* Leia mais sobre [higher order functions][5]

## ES6 _Destructuring_

Existe um jeito em JavaScript ES6 para acessar propriedades de objetos mais facilmente: É chamado de _destructuring_. Compare os seguintes trechos de código, em JavaScript ES5 e ES6:

{title="Code Playground",lang="javascript"}
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

Enquanto que, em JavaScript ES5, você precisa de uma instrução de código a mais todas as vezes que quer acessar uma propriedade de um objeto, em ES6 você pode fazê-lo de uma só vez, em uma única linha.

Uma boa prática, visando legibilidade, é fazer _destructuring_ de múltiplas propriedades quebrando-o em várias linhas:

{title="Code Playground",lang="javascript"}
	const {
	  firstname,
	  lastname
	} = user;

O mesmo vale para _arrays_, que também podem sofrer _destructuring_.

{title="Code Playground",lang="javascript"}
	const users = ['Robin', 'Andrew', 'Dan'];
	const [
	  userOne,
	  userTwo,
	  userThree
	] = users;
	
	console.log(userOne, userTwo, userThree);
	// output: Robin Andrew Dan

Talvez você tenha notado que o objeto do estado local do componente App pode ser “desestruturado” (iremos nos alternar entre “desestruturação” e o original “\_destructuring\_” no texto, uma vez que é importante saber o termo amplamente conhecido na comunidade) da mesma forma. A linha de código com _filter_ e _map_ ficará menor.

{title="src/App.js",lang=javascript}
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

Novamente, o jeito ES5 e o ES6:

{title="Code Playground",lang="javascript"}
	// ES5
	var searchTerm = this.state.searchTerm;
	var list = this.state.list;
	
	// ES6
	const { searchTerm, list } = this.state;

Contudo, uma vez que o livro usa JavaScript ES6 a maior parte do tempo, é aconselhável que você também faça esta opção.

### Exercícios:

* leia mais a respeito de [ES6 destructuring][6]

## Componentes Controlados

Você já tomou conhecimento do fluxo unidirecional de dados em React. A mesma lógica se aplica para o campo de _input_, que atualiza o estado local com o `searchTerm` para filtrar a lista. Quando o estado é alterado, o método  `render()` é executado novamente e utiliza o `searchTerm` mais recente do estado local para aplicar a condição de filtragem.

Mas, não teríamos esquecido de alguma coisa no elemento _input_? A _tag_ HTML “input” possui um atributo `value`. Este, por sua vez, geralmente contém o valor que é mostrado no campo. Neste caso, a propriedade `searchTerm`. Contudo, ficou a impressão de que não precisamos disso em React.

Errado. Elementos de _forms_ como `<input>`, `<textarea>` e `<select>` possuem seu próprio estado em HTML puro. Eles modificam o valor internamente quando alguém de fora do componente o muda. Em React, isso é chamado de um **componente não controlado**, porque ele gerencia seu próprio estado. Você deve garantir-se de fazê-los **componentes controlados**.

Mas, como fazê-lo? Você só precisa definir o atributo “\_value\_” de um campo de _input_. O valor aqui, neste exemplo, já está salvo na propriedade `searchTerm` do estado do componente. Por que não acessá-lo, então?

{title="src/App.js",lang=javascript}
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

É só isso. O _loop_ do fluxo de dados unidirecional do campo _input_ torna-se auto-contido. O estado interno do componente é a única fonte confiável de dados (_single source of truth_) para o campo de _input_.

Toda essa história de gerenciamento de estado interno e fluxo unidirecional de dados deve ser nova para você. Mas, uma vez acostumado, esta será o seu jeito natural de implementar coisas em React. Em geral, React trouxe um interessante novo padrão, com o fluxo de dados unidirecional, para o mundo das SPA (_single page applications_). Agora, este padrão é adotado por diversos _frameworks_ e bibliotecas no momento.

### Exercícios:

* Leia mais sobre [React forms][7]

## Dividindo componentes

Você tem em mãos um componente _App_ de tamanho considerável. Ele continua crescendo e, eventualmente, pode tornar-se confuso. É possível dividi-lo partes, componentes menores.

Comecemos usando um componente para o _input_ de busca e um componente para a lista de itens.

{title="src/App.js",lang=javascript}
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

Você pode passar, para estes componentes, propriedades que eles podem utilizar. No caso, o componente _App_ precisa passar as propriedades gerenciadas no seu estado local e os seus métodos de classe.

{title="src/App.js",lang=javascript}
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

Agora, você pode definir os componentes ao lado de App. Estes também serão classes ES6. Eles irão renderizar os mesmos elementos de antes.

O primeiro é o componente _Search_:

{title="src/App.js",lang=javascript}
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

O segundo é o componente _Table_:

{title="src/App.js",lang=javascript}
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

Agora que você tem esses três componentes, talvez tenha notado o objeto `props`que é acessível na instância de classe com o uso de `this`. As `props` (diminutivo de propriedades) têm todos os valores que você passou para os componentes quando os utilizou dentro de _App_. Desta forma, componentes podem passar propriedades níveis abaixo na árvore de componentes.

Tendo extraído esses componentes de _App_, você estaria apto a reutilizá-los em qualquer outro lugar. Uma vez que componentes recebem valores através do objeto _props_, você pode passar valores diferentes para seu componente a cada vez que utilizá-lo. Em outras palavras, eles se tornaram bastante reutilizáveis.

### Exercícios:

* imagine outros componentes que você poderia separar, como fez com _Search_ e _Table_.
  * entregando, não o faça agora na prática, para não ter conflitos com o que faremos nos capítulos seguintes.

## Composição de Componentes

Existe ainda uma pequena propriedade que pode ser acessada no objeto de  _props_: a _prop_ `children`. Você pode usá-la para passar elementos dos componentes acima na hierarquia para os que estão abaixo, tornando possível combinar componentes dentro uns dos outros. Vejamos como isso é feito quando você manda apenas um texto (_string_) como _child_ para o componente _Search_.

{title="src/App.js",lang=javascript}
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

Agora, o componente _Search_ pode desestruturar a propriedade _children_ do objeto _props_. Aí, poderá especificar onde _children_ deverá ser mostrado.

{title="src/App.js",lang=javascript}
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

O texto “Search” deverá estar visível próximo do campo de _input_ agora. Quando você utilizar o componente _Search_ em algum outro lugar, você poderá escolher um texto diferente, se assim quiser. No fim das contas, não é só texto que pode ser passado assim Você pode enviar um elemento (e árvores de elementos, encapsuladas por outros componentes) como _children_. Esta propriedade permite entrelaçar componentes.

### Exercícios:

* Leia mais sobre [o modelo de composição de React][8]

## Reusable Components

Reusable and composable components empower you to come up with capable component hierarchies. They are the foundation of React's view layer. The last chapters mentioned the term reusability. You can reuse the Table and Search components by now. Even the App component is reusable, because you could instantiate it somewhere else again.

Let's define one more reusable component, a Button component, which gets reused more often eventually.

{title="src/App.js",lang=javascript}
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

It might seem redundant to declare such a component. You will use a `Button` component instead of a `button` element. It only spares the `type="button"`. Except for the type attribute you have to define everything else when you want to use the Button component. But you have to think about the long term investment here. Imagine you have several buttons in your application, but want to change an attribute, style or behavior for the button. Without the component you would have to refactor every button. Instead the Button component ensures to have only one single source of truth. One Button to refactor all buttons at once. One Button to rule them all.

Since you already have a button element, you can use the Button component instead. It omits the type attribute, because the Button component specifies it.

{title="src/App.js",lang=javascript}
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

The Button component expects a `className` property in the props. The `className` attribute is another React derivate for the HTML attribute class. But we didn't pass any `className` when the Button was used. In the code it should be more explicit in the Button component that the `className` is optional.

Therefore, you can use the default parameter which is a JavaScript ES6 feature.

{title="src/App.js",lang=javascript}
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

That's basically it. The props are accessible in the function signature and the return value is JSX. But you can do more code wise in a functional stateless component. You already know the ES6 destructuring. The best practice is to use it in the function signature to destructure the props.

{title="src/App.js",lang=javascript}
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

But it can get better. You know already that ES6 arrow functions allow you to keep your functions concise. You can remove the block body of the function. In a concise body an implicit return is attached thus you can remove the return statement. Since your functional stateless component is a function, you can keep it concise as well.

{title="src/App.js",lang=javascript}
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

The last step was especially useful to enforce only to have props as input and JSX as output. Nothing in between. Still, you could *do something* in between by using a block body in your ES6 arrow function.

{title="Code Playground",lang=javascript}
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

But you don't need it for now. That's why you can keep the previous version without the block body. When using block bodies, people often tend to do too many things in the function. By leaving the block body out, you can focus on the input and output of your function.

Now you have one lightweight functional stateless component. Once you would need access to its internal component state or lifecycle methods, you would refactor it to an ES6 class component. In addition you saw how JavaScript ES6 can be used in React components to make them more concise and elegant.

### Exercises:

* refactor the Table and Button component to stateless functional components
* read more about [ES6 class components and functional stateless components][12]

## Styling Components

Let's add some basic styling to your application and components. You can reuse the *src/App.css* and *src/index.css* files. These files should already be in your project since you have bootstrapped it with *create-react-app*. They should be imported in your *src/App.js* and *src/index.js* files too. I prepared some CSS which you can simply copy and paste to these files, but feel free to use your own style at this point.

First, styling for your overall application.

{title="src/index.css",lang="css"}
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

Second, styling for your components in the App file.

{title="src/App.css",lang="css"}
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

Now you can use the style in some of your components. Don't forget to use React `className` instead of `class` as HTML attribute.

First, apply it in your App ES6 class component.

{title="src/App.js",lang=javascript}
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

Second, apply it in your Table functional stateless component.

{title="src/App.js",lang=javascript}
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

Now you have styled your application and components with basic CSS. It should look quite decent. As you know, JSX mixes up HTML and JavaScript. Now one could argue to add CSS in the mix as well. That's called inline style. You can define JavaScript objects and pass them to the style attribute of an element.

Let's keep the Table column width flexible by using inline style.

{title="src/App.js",lang=javascript}
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

The style is inlined now. You could define the style objects outside of your elements to make it cleaner.

{title="Code Playground",lang="javascript"}
	const largeColumn = {
	  width: '40%',
	};
	
	const midColumn = {
	  width: '30%',
	};
	
	const smallColumn = {
	  width: '10%',
	};

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