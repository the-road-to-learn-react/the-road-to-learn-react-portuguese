# Familiarizando-se com uma API

É o momento de “melar as mãos” com uma API, uma vez que é entediaste lidar sempre com dados estáticos.

Se você não está familiarizado, lhe encorajo a [ler a minha jornada onde tomei conhecimento de APIs][1].

Conhece a plataforma [Hacker News][2]? É uma grande agregadora de tópicos de tecnologia. Neste livro, você irá usar a API do Hacker News para consultar assuntos populares da plataforma. Existe uma API [básica][3] e uma de [consulta][4], ambas para recuperar dados da plataforma. Esta última faz mais sentido em ser utilizada em nossa aplicação, parar consultar notícias no Hacker News. Visite a especificação da API para obter um melhor entendimento da estrutura de dados.

## Métodos de Ciclo de Vida

Você precisará entender sobre métodos de ciclo de vida de React antes de começar a obter dados em seus componentes utilizando uma API. Estes métodos são um gancho para o ciclo de vida de um componente. Eles podem ser utilizados em um componente de classe ES6, mas não em _statelles functional components_.

Lembra-se de quando, em um capítulo anterior, você aprendeu sobre classes de JavaScript ES6 e como elas são utilizadas em React? Além do método `render()`, existem vários outros que podem ser sobrescritos em um componente de classe. Todos eles são chamados de métodos de ciclo de vida e mergulharemos neles agora:

Você já conhece dois destes métodos: `constructor()` e `render()`.

O método `constructor` só é chamado quando uma instância de componente é criada e inserida no DOM. Este processo é chamado de **montagem de um componente**.

O método `render()` também é chamado durante o processo de montagem e quando o componente é atualizado. Cada vez que o estado interno ou _props_ de um componente mudam, o método `render()` é disparado.

Agora você já sabe um pouco mais sobre dois métodos de ciclo de vida e quando eles são chamados, além de já os ter utilizado. Mas, existem outros mais além deles.

A montagem de um componente tem mais dois métodos de ciclo de vida: `componentWillMount()` e `componentDidMount()`. O construtor é chamado primeiro, `componentWillMount()` chamado antes de `render()` e `componentDidMount`é invocado depois do método `render()`.

Em resumo, o processo de montagem tem 4 métodos de cicloOverall the mounting process has 4 lifecycle methods. Eles são invocados na seguinte ordem:

* constructor()
* componentWillMount()
* render()
* componentDidMount()

Mas, e quanto ao ciclo de vida de **atualização** de um componente, o que acontece quando o _state_ ou as _props_ mudam?  No total, existem 5 métodos, chamados na seguinte ordem:

* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* render()
* componentDidUpdate()

Por último, mas não menos importante, existe o ciclo de vida de desmontagem de um componente. Ele possui apenas um método, `componentWillUnmount()`.

No fim das contas, você não precisa conhecer todos esses métodos desde o começo. Pode ser um pouco intimidador ter que decorar e você provavelmente nem usará todos eles. Mesmo em uma aplicação React de maior escala, você usará apenas um subconjunto de métodos além de `render()` e `constructor`. Mas, é legal saber que existem outros, que podem ser usados em casos específicos:

* **constructor(props)** -É chamado quando o componente é inicializado. Você pode definir um estado inicial do componente e realizar o _binding_ de métodos de classe aqui.

* **componentWillMount()** - Invocado antes do método `render()`. Pode ser utilizado para definir o estado interno do componente, porque ele não irá disparar uma segunda renderização do componente. No entanto, geralmente, é recomendado utilizar `constructor()` para definir este estado inicial.

* **render()** - Este método de ciclo de vida é mandatório e retorna os elementos como saída do componente. Deve ser puro e, logo, não deve modificar o estado do componente. Ele recebe como entrada o _state_ e as _props_ e retorna um elemento.

* **componentDidMount()** - É chamado apenas uma vez, quando o component é montado. É o momento perfeito para fazer qualquer requisição assíncrona para obter dados de uma API. Os dados obtidos serão armazenados no estado interno do componente, para serem mostrados via `render()`.

* **componentWillReceiveProps(nextProps)** - Este é chamado durante um ciclo de atualização do componente. Como entrada, ele recebe as novas _props_ recebidas. Você pode comparar `nextProps` com as anteriores, com`this.props`, para aplicar um comportamento específico baseado nas diferenças encontradas. Adicionalmente, você pode definir o `state` baseado nessas novas props.

* **shouldComponentUpdate(nextProps, nextState)** - É sempre chamado quando o componente atualiza devido a mudanças de `state` ou `props`.  Você utilizará este método em uma aplicação React mais madura, visando otimizações de performance. Dependendo do valor booleano retornado aqui, o componente e todos os seus filhos irão ser novamente renderizados no ciclo de atualização. Você pode evitar que o método `render()` seja invocado.

* **componentWillUpdate(nextProps, nextState)** - Imediatamente invocado antes do `render()`. Aqui, você já tem as novas _props_ e o novo estado à sua disposição. O método é a última oportunidade para fazer preparações antes que `render()` seja executado. Saiba que você não pode disparar `setState()` novamente aqui. Se você desejar computar o estado baseado nas próximas _props_, você deve utilizar `componentWillReceiveProps`.

* **componentDidUpdate(prevProps, prevState)** - Este método é chamado imediatamente depois de `render()`. Você pode utilizá-lo como uma oportunidade de realizar operações no DOM ou de efetuar requisições assíncronas depois do componente já estar montado.

* **componentWillUnmount()** - Chamado antes de você destruir seu componente. Você pode usá-lo para realizar qualquer tarefa de limpeza, por exemplo.

Os métodos de ciclo de vida `constructor()` e `render()` já foram utilizados por você, uma vez que são comumente utilizados em componentes de classe ES6. O método `render()` é, na verdade, obrigatório, caso contrário você não conseguiria retornar uma instância do componente.

Existe mais um método de ciclo de vida: `componentDidCatch(error, info)`. Ele foi introduzido no [React 16][5] e é utilizado para capturar erros em componentes. No nosso contexto, a lista de dados da sua aplicação está funcionando muito bem. Mas, pode existir o caso da lista no estado local ser definida, por acidente, com o valor nulo (por exemplo, quando os dados são obtidos de uma API externa, mas a requisição falha e você define o valor do estado local para `null`).  Ocorrido esta situação, não será mais possível filtrar ou mapear a lista, porque ela tem valor `null` ao invés de ser uma lista vazia. O componente quebraria e a aplicação inteira poderia falhar. Agora, com o uso do `componentDidCatch()`, você capturar o erro, armazená-lo no estado local e, opcionalmente, exibir uma mensagem para o usuário informando que algo de errado ocorreu.

### Exercícios:

* Leia mais sobre [métodos de ciclo de vida em React][6]
* Leia mais sobre [o estado e sua relação com métodos de ciclo de vida][7]
* Leia mais sobre [tratamento de erros em componentes][8]

## Obtendo Dados

Você está agora preparado para obter dados da API Hacker News. Um método de ciclo de vida foi mencionado como apropriado para este uso: `componentDidMount()`. Você usará a API **fetch**, nativa de JavaScript, para efetuar a requisição.

Antes que possamos fazê-lo, vamos configurar as constantes de URL e parâmetros _default_, para decompor a requisição em pedaços menores.

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	import './App.css';
	
	# leanpub-start-insert
	const DEFAULT_QUERY = 'redux';
	
	const PATH_BASE = 'https://hn.algolia.com/api/v1';
	const PATH_SEARCH = '/search';
	const PARAM_SEARCH = 'query=';
	# leanpub-end-insert
	
	...

Em JavaScript ES6, você pode utilizar [template strings][9] para concatenar strings. Você fará isso aqui, para concatenar sua URL para o _endpoint_ da API.

{title="Code Playground",lang="javascript"}
	// ES6
	const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;
	
	// ES5
	var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;
	
	console.log(url);
	// output: https://hn.algolia.com/api/v1/search?query=redux

Este formato irá manter a composição da sua URL flexível no futuro.

Agora, vejamos a requisição à API, onde você usará esta URL. O processo inteiro de obtenção dos dados será apresentado de uma só vez, mas cada passo será explicado posteriormente.

{title="src/App.js",lang=javascript}
	...
	
	class App extends Component {
	
	  constructor(props) {
	    super(props);
	
	    this.state = {
	# leanpub-start-insert
	      result: null,
	      searchTerm: DEFAULT_QUERY,
	# leanpub-end-insert
	    };
	
	# leanpub-start-insert
	    this.setSearchTopStories = this.setSearchTopStories.bind(this);
	# leanpub-end-insert
	    this.onSearchChange = this.onSearchChange.bind(this);
	    this.onDismiss = this.onDismiss.bind(this);
	  }
	
	# leanpub-start-insert
	  setSearchTopStories(result) {
	    this.setState({ result });
	  }
	
	  componentDidMount() {
	    const { searchTerm } = this.state;
	
	    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
	      .then(response => response.json())
	      .then(result => this.setSearchTopStories(result))
	      .catch(error => error);
	  }
	# leanpub-end-insert
	
	  ...
	}

Um monte de coisas aconteceu neste código. Eu até pensei em quebrá-lo em pedaços menores. Mas, de novo, ficaria difícil de compreender as relações entre as partes. Deixe-me explicar cada passo em detalhes.

Primeiro, você pode remover a lista de itens criada como amostra de dados, porque a API do Hacker News retorna uma lista real, a amostra não será mais utilizada. O estado inicial do seu componente tem um `result`vazio e um termo de busca (`searchTerm`) padrão agora. O mesmo termo de busca é usado no campo _input_ do componente _Search_ e na sua primeira requisição.

Segundo, você utiliza o método `componentDidMount` para obter os dados depois que o componente é montado. Na primeira chamada de _fetch_, o termo de busca _default_ do estado local é utilizado. Ela irá buscar discussões relacionadas com “redux”.

Terceiro, a API nativa _fetch_ é utilizada. O uso de _template strings_ de JavaScript ES6 possibilita a composição da URL com o `searchTerm`. Ela (a URL) é o argumento para a função nativa _fetch_.  A resposta então precisa ser transformada em uma estrutura de dados JSON, um passo mandatório neste caso (_fetch_ nativo lidando com estruturas de dados JSON). E, finalmente, a resposta pode ser atribuída a _result_no estado interno do component. Ademais, o block `catch` é utilizado em caso de um erro ocorrer. Se isto acontecer, o fluxo será desviado para o bloco _catch\_  ao invés do \_then\_. Em um capítulo futuro, você irá incluir o tratamento para os erros.

Por último, não menos importante, não se esqueça de fazer o _binding_ do novo método no seu construtor.

Você agora consegue utilizar os dados obtidos ao invés da amostra inicial. Contudo, tenha cuidado. O resultado não é apenas uma lista de dados. [É um objeto complexo com _metadados_ e uma lista de itens que são, no nosso caso, nossas discussões][10]. Você pode imprimir o estado interno com `console.log(this.state);` no seu método `render()`, a fim de visualizá-lo melhor.

(**Nota do Tradutor:** No texto original, o autor refere-se às discussões como _stories_. Iremos usar as formas “discussões” e “posts” aqui). 

No passo seguinte, você irá renderizar o valor em _result_. Iremos evitar que qualquer coisa seja renderizada, retornando _null_ quando não existir um resultado.  Uma vez que a requisição à API foi bem sucedida o resultado é salvo em _state_ e o componente _App_ será re-renderizado, desta vez com o estado atualizado.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	# leanpub-start-insert
	    const { searchTerm, result } = this.state;
	
	    if (!result) { return null; }
	
	# leanpub-end-insert
	    return (
	      <div className="page">
	        ...
	        <Table
	# leanpub-start-insert
	          list={result.hits}
	# leanpub-end-insert
	          pattern={searchTerm}
	          onDismiss={this.onDismiss}
	        />
	      </div>
	    );
	  }
	}

Vamos recapitular o que acontece durante o ciclo de vida do componente. Ele é inicializado pelo seu construtor. Depois disso, ele é renderizado pela primeira vez. Mas, você evitou que ele tentasse exibir qualquer coisa, porque `result`, no estado local, é _null_. É permitido a um componente retornar o valor _null_ para que nada seja exibido. Então, o método `componentDidMount` é executado. Neste método, você obtém os dados da API do Hacker News de forma assíncrona. Uma vez que os dados chegam, o estado interno do componente é alterado por `setSearchTopStories()`. Depois disso, o ciclo de vida de atualização entra em cena, uma vez que o estado foi atualizado. O componente executa o método `render()` novamente, mas desta vez com o resultado poupado no estado interno. O componente atual e, consequentemente, o componente _Table_ e seu conteúdo são renderizados novamente.

Você utilizou a função _fetch_ que é suportada pela maioria dos _browsers_ para realizar uma requisição assíncrona para uma API. A configuração do _create-react-app_ assegura que ela é suportada por todos eles. Existem, todavia, pacotes node de terceiros que podem ser utilizados como substitutos à API nativa _fetch_: [superagent][11] e [axios][12].

Este livro se baseia fortemente na notação JavaScript para checagem de dados. No exemplo anterior, `if (!result)` foi utilizado no lugar de `if (result === null)`. O mesmo se aplica para outros casos ao longo do livro, da mesma forma. Por exemplo, `if (!list.length)` é usado ao invés de `if (list.length === 0)` ou `if (someString)` no lugar de `if (someString !== '')`. Recomendo ler mais sobre este tópico, se não estiver bem familiarizado com ele.

De volta a sua aplicação: A lista de resultados deve ser visível agora, Entretanto, existem dois _bugs_ que foram deixados para trás. Primeiro, o botão “Dismiss” está quebrado. Ele não conhece o objeto complexo de resultado e ainda funciona baseado na lista simples da amostra de dados. Segundo, quando a lista com dados do servidor é exibida e você tenta buscar por outra coisa, a filtragem ocorre apenas do lado do cliente. O comportamento esperado é que outra consulta à API ocorra. Ambos os _bugs_ serão corrigidos nos capítulos seguintes.

### Exercises:

* Leia mais a respeito de [ES6 template strings][13]
* Leia mais a respeito de [a API nativa _fetch_][14]
* Leia mais a respeito de [obtendo dados com React][15]

## ES6 e o Operador _Spread_

O botão “Dismiss” não funciona porque o método `onDismiss()` não conhece o objeto do resultado exibido. Ele ainda trata como se fosse a lista simples que era armazenada no estado local. Vamos mudar a função, para que opere sobre o objeto ao invés da lista.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	  const isNotId = item => item.objectID !== id;
	# leanpub-start-insert
	  const updatedHits = this.state.result.hits.filter(isNotId);
	  this.setState({
	    ...
	  });
	# leanpub-end-insert
	}

Mas  o que acontece no `setState()` agora? Infelizmente, o resultado é um objeto complexo. A lista de resultados (aqui chamados de _hits_) é apenas uma das múltiplas propriedades do objeto. Contudo, somente a lista é atualizada quando um item é removido e as outras propriedades devem permanecer as mesmas.

Uma possível abordagem seria a de mudar os _hits_ no objeto resultado, como  demonstro abaixo. Mas, não será a que adotaremos.

{title="Code Playground",lang="javascript"}
	// don`t do this
	this.state.result.hits = updatedHits;

React abraça a imutabilidade de estruturas de dados. Dessa forma, você não deveria modificar diretamente um objeto. Uma abordagem melhor é a de gerar um novo objeto, baseado na informação que você tem. Nenhum dos objetos é realmente manipulado, você irá manter as estruturas de dados imutáveis, sempre retornando um novo objeto.

Você pode usar `Object.assign()`, de JavaScript ES6, que recebe como primeiro argumento o objeto alvo. Todos os outros argumentos são objetos fontes de dados, que são combinados no objeto alvo que, por sua vez, pode ser um objeto vazio. A imutabilidade é preservada aqui, uma vez que nenhum dos objetos originais é modificado. Seria algo mais ou menos assim:

{title="Code Playground",lang="javascript"}
	const updatedHits = { hits: updatedHits };
	const updatedResult = Object.assign({}, this.state.result, updatedHits);

Quando objetos de origem possuírem propriedades de mesmo nome, as do objeto que aparecer primeiro como argumento serão sobrescritas por aquelas do que aparecer depois, no objeto de destino. Apliquemos o mesmo raciocínio para o método `onDismiss()`:

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	  const isNotId = item => item.objectID !== id;
	  const updatedHits = this.state.result.hits.filter(isNotId);
	  this.setState({
	# leanpub-start-insert
	    result: Object.assign({}, this.state.result, { hits: updatedHits })
	# leanpub-end-insert
	  });
	}

Esta poderia ser, muito bem, a solução. Mas, existe um jeito ainda mais simples, em JavaScript ES6 e _releases_ futuras. Gostaria de lhe introduzir o operador _spread_, que consiste apenas em três pontos: `...`. Quando utilizado, todos os valores de um array ou objeto são copiados para outro array ou objeto.

Apesar de não precisar dele ainda, vamos examinar como funciona o operador _spread_ de um **array**.

{title="Code Playground",lang="javascript"}
	const userList = ['Robin', 'Andrew', 'Dan'];
	const additionalUser = 'Jordan';
	const allUsers = [ ...userList, additionalUser ];
	
	console.log(allUsers);
	// output: ['Robin', 'Andrew', 'Dan', 'Jordan']

A variável `allUsers` é um _array_ completamente novo. As outras variáveis `userList` e `additionalUser` permanecem as mesmas. Você poderia combinar até dois _arrays_ em um novo, da mesma forma.

{title="Code Playground",lang="javascript"}
	const oldUsers = ['Robin', 'Andrew'];
	const newUsers = ['Dan', 'Jordan'];
	const allUsers = [ ...oldUsers, ...newUsers ];
	
	console.log(allUsers);
	// output: ['Robin', 'Andrew', 'Dan', 'Jordan']

Olhemos, agora, o mesmo operador _spread_ com objetos. Neste caso, não é JavaScript ES6. É apenas uma [proposta para a próxima versão de JavaScript][16],  mas, mesmo assim já utilizada pela comunidade React. Por causa disso, *create-react-app* incorporou essa funcionalidade em sua configuração.

Basicamente, é o mesmo que o operador _spread_ de array, só que com objetos. Ele copia cada par chave-valor em um novo objeto.

{title="Code Playground",lang="javascript"}
	const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
	const age = 28;
	const user = { ...userNames, age };
	
	console.log(user);
	// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }

Também a exemplo de _array_, múltiplos objetos podem ser combinados.

{title="Code Playground",lang="javascript"}
	const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
	const userAge = { age: 28 };
	const user = { ...userNames, ...userAge };
	
	console.log(user);
	// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }

No fim das contas, ele pode ser utilizado no lugar de `Object.assign()`.

{title="src/App.js",lang=javascript}
	onDismiss(id) {
	  const isNotId = item => item.objectID !== id;
	  const updatedHits = this.state.result.hits.filter(isNotId);
	  this.setState({
	# leanpub-start-insert
	    result: { ...this.state.result, hits: updatedHits }
	# leanpub-end-insert
	  });
	}

Agora, o botão “Dismiss” voltará a funcionar, uma vez que `onDismiss()` saberá como atualizar o objeto complexo em _result_, quando um item da lista for removido.

### Exercícios:

* Leia mais a respeito de [ES6 Object.assign()][17]
* Leia mais sobre [o operador _spread_ de _array_ em ES6][18]
	 * O operador _spread_ de objetos é brevemente mencionado.

## Conditional Rendering

Conditional rendering is introduced pretty early in React applications. But not in the case of the book, because there wasn't such a use case yet. The conditional rendering happens when you want to make a decision to render either one or another element. Sometimes it means to render an element or nothing. After all, a conditional rendering simplest usage can be expressed by an if-else statement in JSX.

The `result` object in the internal component state is `null` in the beginning. So far, the App component returned no elements when the `result` hasn't arrived from the API. That's already a conditional rendering, because you return earlier from the `render()` lifecycle method for a certain condition. The App component either renders nothing or its elements.

But let's go one step further. It makes more sense to wrap the Table component, which is the only component that depends on the `result`, in an independent conditional rendering. Everything else should be displayed, even though there is no `result` yet. You can simply use a ternary operator in your JSX.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	# leanpub-start-insert
	    const { searchTerm, result } = this.state;
	# leanpub-end-insert
	    return (
	      <div className="page">
	        <div className="interactions">
	          <Search
	            value={searchTerm}
	            onChange={this.onSearchChange}
	          >
	            Search
	          </Search>
	        </div>
	# leanpub-start-insert
	        { result
	          ? <Table
	            list={result.hits}
	            pattern={searchTerm}
	            onDismiss={this.onDismiss}
	          />
	          : null
	        }
	# leanpub-end-insert
	      </div>
	    );
	  }
	}

That's your second option to express a conditional rendering. A third option is the logical `&&` operator. In JavaScript a `true && 'Hello World'` always evaluates to 'Hello World'. A `false && 'Hello World'` always evaluates to false.

{title="Code Playground",lang="javascript"}
	const result = true && 'Hello World';
	console.log(result);
	// output: Hello World
	
	const result = false && 'Hello World';
	console.log(result);
	// output: false

In React you can make use of that behavior. If the condition is true, the expression after the logical `&&` operator will be the output. If the condition is false, React ignores and skips the expression. It is applicable in the Table conditional rendering case, because it should return a Table or nothing.

{title="src/App.js",lang=javascript}
	{ result &&
	  <Table
	    list={result.hits}
	    pattern={searchTerm}
	    onDismiss={this.onDismiss}
	  />
	}

These were a few approaches to use conditional rendering in React. You can read about [more alternatives in an exhaustive list of examples for conditional rendering approaches][19]. Moreover you will get to know their different use cases and when to apply them.

After all, you should be able to see the fetched data in your application. Everything except the Table is displayed when the data fetching is pending. Once the request resolves the result and stores it into the local state, the Table is displayed because the `render()` method runs again and the condition in the conditional rendering resolves in favor of displaying the Table component.

### Exercises:

* read more about [different ways for conditional renderings][20]
* read more about [React conditional rendering][21]

## Client- or Server-side Search

When you use the Search component with its input field now, you will filter the list. That's happening on the client-side though. Now you are going to use the Hacker News API to search on the server-side. Otherwise you would deal only with the first API response which you got on `componentDidMount()` with the default search term parameter.

You can define an `onSearchSubmit()` method in your App component which fetches results from the Hacker News API when executing a search in the Search component.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  constructor(props) {
	    super(props);
	
	    this.state = {
	      result: null,
	      searchTerm: DEFAULT_QUERY,
	    };
	
	    this.setSearchTopStories = this.setSearchTopStories.bind(this);
	    this.onSearchChange = this.onSearchChange.bind(this);
	# leanpub-start-insert
	    this.onSearchSubmit = this.onSearchSubmit.bind(this);
	# leanpub-end-insert
	    this.onDismiss = this.onDismiss.bind(this);
	  }
	
	  ...
	
	# leanpub-start-insert
	  onSearchSubmit() {
	    const { searchTerm } = this.state;
	  }
	# leanpub-end-insert
	
	  ...
	}

The `onSearchSubmit()` method should use the same functionality as the `componentDidMount()` lifecycle method, but this time with a modified search term from the local state and not with the initial default search term. Thus you can extract the functionality as a reusable class method.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  constructor(props) {
	    super(props);
	
	    this.state = {
	      result: null,
	      searchTerm: DEFAULT_QUERY,
	    };
	
	    this.setSearchTopStories = this.setSearchTopStories.bind(this);
	# leanpub-start-insert
	    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
	# leanpub-end-insert
	    this.onSearchChange = this.onSearchChange.bind(this);
	    this.onSearchSubmit = this.onSearchSubmit.bind(this);
	    this.onDismiss = this.onDismiss.bind(this);
	  }
	
	  ...
	
	# leanpub-start-insert
	  fetchSearchTopStories(searchTerm) {
	    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
	      .then(response => response.json())
	      .then(result => this.setSearchTopStories(result))
	      .catch(error => error);
	  }
	# leanpub-end-insert
	
	  componentDidMount() {
	    const { searchTerm } = this.state;
	# leanpub-start-insert
	    this.fetchSearchTopStories(searchTerm);
	# leanpub-end-insert
	  }
	
	  ...
	
	  onSearchSubmit() {
	    const { searchTerm } = this.state;
	# leanpub-start-insert
	    this.fetchSearchTopStories(searchTerm);
	# leanpub-end-insert
	  }
	
	  ...
	}

Now the Search component has to add an additional button. The button has to explicitly trigger the search request. Otherwise you would fetch data from the Hacker News API every time when your input field changes. But you want to do it explicitly in a `onClick()` handler.

As alternative you could debounce (delay) the `onChange()` function and spare the button, but it would add more complexity at this time and maybe wouldn't be the desired effect. Let's keep it simple without a debounce for now.

First, pass the `onSearchSubmit()` method to your Search component.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const { searchTerm, result } = this.state;
	    return (
	      <div className="page">
	        <div className="interactions">
	          <Search
	            value={searchTerm}
	            onChange={this.onSearchChange}
	# leanpub-start-insert
	            onSubmit={this.onSearchSubmit}
	# leanpub-end-insert
	          >
	            Search
	          </Search>
	        </div>
	        { result &&
	          <Table
	            list={result.hits}
	            pattern={searchTerm}
	            onDismiss={this.onDismiss}
	          />
	        }
	      </div>
	    );
	  }
	}

Second, introduce a button in your Search component. The button has the `type="submit"` and the form uses its `onSubmit()` attribute to pass the `onSubmit()` method. You can reuse the children property, but this time it will be used as the content of the button.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const Search = ({
	  value,
	  onChange,
	  onSubmit,
	  children
	}) =>
	  <form onSubmit={onSubmit}>
	    <input
	      type="text"
	      value={value}
	      onChange={onChange}
	    />
	    <button type="submit">
	      {children}
	    </button>
	  </form>
	# leanpub-end-insert

In the Table, you can remove the filter functionality, because there will be no client-side filter (search) anymore. Don't forget to remove the `isSearched()` function as well. It will not be used anymore. The result comes directly from the Hacker News API now after you have clicked the "Search" button.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const { searchTerm, result } = this.state;
	    return (
	      <div className="page">
	        ...
	        { result &&
	          <Table
	# leanpub-start-insert
	            list={result.hits}
	            onDismiss={this.onDismiss}
	# leanpub-end-insert
	          />
	        }
	      </div>
	    );
	  }
	}
	
	...
	
	# leanpub-start-insert
	const Table = ({ list, onDismiss }) =>
	# leanpub-end-insert
	  <div className="table">
	# leanpub-start-insert
	    {list.map(item =>
	# leanpub-end-insert
	      ...
	    )}
	  </div>

When you try to search now, you will notice that the browser reloads. That's a native browser behavior for a submit callback in a HTML form. In React you will often come across the `preventDefault()` event method to suppress the native browser behavior.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	onSearchSubmit(event) {
	# leanpub-end-insert
	  const { searchTerm } = this.state;
	  this.fetchSearchTopStories(searchTerm);
	# leanpub-start-insert
	  event.preventDefault();
	# leanpub-end-insert
	}

Now you should be able to search different Hacker News stories. Perfect, you interact with a real world API. There should be no client-side search anymore.

### Exercises:

* read more about [synthetic events in React][22]
* experiment with the [Hacker News API][23]

## Paginated Fetch

Did you have a closer look at the returned data structure yet? The [Hacker News API][24] returns more than a list of hits. Precisely it returns a paginated list. The page property, which is `0` in the first response, can be used to fetch more paginated sublists as result. You only need to pass the next page with the same search term to the API.

Let's extend the composable API constants so that it can deal with paginated data.

{title="src/App.js",lang=javascript}
	const DEFAULT_QUERY = 'redux';
	
	const PATH_BASE = 'https://hn.algolia.com/api/v1';
	const PATH_SEARCH = '/search';
	const PARAM_SEARCH = 'query=';
	# leanpub-start-insert
	const PARAM_PAGE = 'page=';
	# leanpub-end-insert

Now you can use the new constant to add the page parameter to your API request.

{title="Code Playground",lang="javascript"}
	const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;
	
	console.log(url);
	// output: https://hn.algolia.com/api/v1/search?query=redux&page=

The `fetchSearchTopStories()` method will take the page as second argument. If you don't provide the second argument, it will fallback to the `0` page for the initial request. Thus the `componentDidMount()` and `onSearchSubmit()` methods fetch the first page on the first request. Every additional fetch should fetch the next page by providing the second argument.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	# leanpub-start-insert
	  fetchSearchTopStories(searchTerm, page = 0) {
	    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`)
	# leanpub-end-insert
	      .then(response => response.json())
	      .then(result => this.setSearchTopStories(result))
	      .catch(error => error);
	  }
	
	  ...
	
	}

The page argument uses the JavaScript ES6 default parameter to introduce the fallback to page `0` in case no defined page argument is provided for the function.

Now you can use the current page from the API response in `fetchSearchTopStories()`. You can use this method in a button to fetch more stories on a `onClick` button handler. Let's use the Button to fetch more paginated data from the Hacker News API. You only need to define the `onClick()` handler which takes the current search term and the next page (current page + 1).

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const { searchTerm, result } = this.state;
	# leanpub-start-insert
	    const page = (result && result.page) || 0;
	# leanpub-end-insert
	    return (
	      <div className="page">
	        <div className="interactions">
	        ...
	        { result &&
	          <Table
	            list={result.hits}
	            onDismiss={this.onDismiss}
	          />
	        }
	# leanpub-start-insert
	        <div className="interactions">
	          <Button onClick={() => this.fetchSearchTopStories(searchTerm, page + 1)}>
	            More
	          </Button>
	        </div>
	# leanpub-end-insert
	      </div>
	    );
	  }
	}

In addition, in your `render()` method you should make sure to default to page 0 when there is no result yet. Remember that the `render()` method is called before the data is fetched asynchronously in the `componentDidMount()` lifecycle method.

There is one step missing. You fetch the next page of data, but it will override your previous page of data. It would be ideal to concatenate the old and new list of hits from the local state and new result object. Let's adjust the functionality to add the new data rather than to override it.

{title="src/App.js",lang=javascript}
	setSearchTopStories(result) {
	# leanpub-start-insert
	  const { hits, page } = result;
	
	  const oldHits = page !== 0
	    ? this.state.result.hits
	    : [];
	
	  const updatedHits = [
	    ...oldHits,
	    ...hits
	  ];
	
	  this.setState({
	    result: { hits: updatedHits, page }
	  });
	# leanpub-end-insert
	}

A couple of things happen in the `setSearchTopStories()` method now. First, you get the hits and page from the result.

Second, you have to check if there are already old hits. When the page is 0, it is a new search request from `componentDidMount()` or `onSearchSubmit()`. The hits are empty. But when you click the "More" button to fetch paginated data the page isn't 0. It is the next page. The old hits are already stored in your state and thus can be used.

Third, you don't want to override the old hits. You can merge old and new hits from the recent API request. The merge of both lists can be done with the JavaScript ES6 array spread operator.

Fourth, you set the merged hits and page in the local component state.

You can make one last adjustment. When you try the "More" button it only fetches a few list items. The API URL can be extended to fetch more list items with each request. Again, you can add more composable path constants.

{title="src/App.js",lang=javascript}
	const DEFAULT_QUERY = 'redux';
	# leanpub-start-insert
	const DEFAULT_HPP = '100';
	# leanpub-end-insert
	
	const PATH_BASE = 'https://hn.algolia.com/api/v1';
	const PATH_SEARCH = '/search';
	const PARAM_SEARCH = 'query=';
	const PARAM_PAGE = 'page=';
	# leanpub-start-insert
	const PARAM_HPP = 'hitsPerPage=';
	# leanpub-end-insert

Now you can use the constants to extend the API URL.

{title="src/App.js",lang=javascript}
	fetchSearchTopStories(searchTerm, page = 0) {
	# leanpub-start-insert
	  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
	# leanpub-end-insert
	    .then(response => response.json())
	    .then(result => this.setSearchTopStories(result))
	    .catch(error => error);
	}

Afterward, the request to the Hacker News API fetches more list items in one request than before. As you can see, a powerful API such as the Hacker News API gives you plenty of ways to experiment with real world data. You should make use of it to make your endeavours when learning something new more exciting. That's [how I learned about the empowerment that APIs provide][25] when learning a new programming language or library.

### Exercises:

* read more about [ES6 default parameters][26]
* experiment with the [Hacker News API parameters][27]

## Client Cache

Each search submit makes a request to the Hacker News API. You might search for "redux", followed by "react" and eventually "redux" again. In total it makes 3 requests. But you searched for "redux" twice and both times it took a whole asynchronous roundtrip to fetch the data. In a client-sided cache you would store each result. When a request to the API is made, it checks if a result is already there. If it is there, the cache is used. Otherwise an API request is made to fetch the data.

In order to have a client cache for each result, you have to store multiple `results` rather than one `result` in your internal component state. The results object will be a map with the search term as key and the result as value. Each result from the API will be saved by search term (key).

At the moment, your result in the local state looks similar to the following:

{title="Code Playground",lang="javascript"}
	result: {
	  hits: [ ... ],
	  page: 2,
	}

Imagine you have made two API requests. One for the search term "redux" and another one for "react". The results object should look like the following:

{title="Code Playground",lang="javascript"}
	results: {
	  redux: {
	    hits: [ ... ],
	    page: 2,
	  },
	  react: {
	    hits: [ ... ],
	    page: 1,
	  },
	  ...
	}

Let's implement a client-side cache with React `setState()`. First, rename the `result` object to `results` in the initial component state. Second, define a temporary `searchKey` which is used to store each `result`.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  constructor(props) {
	    super(props);
	
	    this.state = {
	# leanpub-start-insert
	      results: null,
	      searchKey: '',
	# leanpub-end-insert
	      searchTerm: DEFAULT_QUERY,
	    };
	
	    ...
	
	  }
	
	  ...
	
	}

The `searchKey` has to be set before each request is made. It reflects the `searchTerm`. You might wonder: Why don't we use the `searchTerm` in the first place? That's a crucial part to understand before continuing with the implementation. The `searchTerm` is a fluctuant variable, because it gets changed every time you type into the Search input field. However, in the end you will need a non fluctuant variable. It determines the recent submitted search term to the API and can be used to retrieve the correct result from the map of results. It is a pointer to your current result in the cache and thus can be used to display the current result in your `render()` method.

{title="src/App.js",lang=javascript}
	componentDidMount() {
	  const { searchTerm } = this.state;
	# leanpub-start-insert
	  this.setState({ searchKey: searchTerm });
	# leanpub-end-insert
	  this.fetchSearchTopStories(searchTerm);
	}
	
	onSearchSubmit(event) {
	  const { searchTerm } = this.state;
	# leanpub-start-insert
	  this.setState({ searchKey: searchTerm });
	# leanpub-end-insert
	  this.fetchSearchTopStories(searchTerm);
	  event.preventDefault();
	}

Now you have to adjust the functionality where the result is stored to the internal component state. It should store each result by `searchKey`.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  setSearchTopStories(result) {
	    const { hits, page } = result;
	# leanpub-start-insert
	    const { searchKey, results } = this.state;
	
	    const oldHits = results && results[searchKey]
	      ? results[searchKey].hits
	      : [];
	# leanpub-end-insert
	
	    const updatedHits = [
	      ...oldHits,
	      ...hits
	    ];
	
	    this.setState({
	# leanpub-start-insert
	      results: {
	        ...results,
	        [searchKey]: { hits: updatedHits, page }
	      }
	# leanpub-end-insert
	    });
	  }
	
	  ...
	
	}

The `searchKey` will be used as the key to save the updated hits and page in a `results` map.

First, you have to retrieve the `searchKey` from the component state. Remember that the `searchKey` gets set on `componentDidMount()` and `onSearchSubmit()`.

Second, the old hits have to get merged with the new hits as before. But this time the old hits get retrieved from the `results` map with the `searchKey` as key.

Third, a new result can be set in the `results` map in the state. Let's examine the `results` object in `setState()`.

{title="src/App.js",lang=javascript}
	results: {
	  ...results,
	  [searchKey]: { hits: updatedHits, page }
	}

The bottom part makes sure to store the updated result by `searchKey` in the results map. The value is an object with a hits and page property. The `searchKey` is the search term. You already learned the `[searchKey]: ...` syntax. It is an ES6 computed property name. It helps you to allocate values dynamically in an object.

The upper part needs to spread all other results by `searchKey` in the state by using the object spread operator. Otherwise you would lose all results that you have stored before.

Now you store all results by search term. That's the first step to enable your cache. In the next step, you can retrieve the result depending on the non fluctuant `searchKey` from your map of results. That's why you had to introduce the `searchKey` in the first place as non fluctuant variable. Otherwise the retrieval would be broken when you would use the fluctuant `searchTerm` to retrieve the current result, because this value might change when you would use the Search component.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	# leanpub-start-insert
	    const {
	      searchTerm,
	      results,
	      searchKey
	    } = this.state;
	
	    const page = (
	      results &&
	      results[searchKey] &&
	      results[searchKey].page
	    ) || 0;
	
	    const list = (
	      results &&
	      results[searchKey] &&
	      results[searchKey].hits
	    ) || [];
	
	# leanpub-end-insert
	    return (
	      <div className="page">
	        <div className="interactions">
	          ...
	        </div>
	# leanpub-start-insert
	        <Table
	          list={list}
	          onDismiss={this.onDismiss}
	        />
	# leanpub-end-insert
	        <div className="interactions">
	# leanpub-start-insert
	          <Button onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
	# leanpub-end-insert
	            More
	          </Button>
	        </div>
	      </div>
	    );
	  }
	}

Since you default to an empty list when there is no result by `searchKey`, you can spare the conditional rendering for the Table component now. Additionally you will need to pass the `searchKey` rather than the `searchTerm` to the "More" button. Otherwise your paginated fetch depends on the `searchTerm` value which is fluctuant. Moreover make sure to keep the fluctuant `searchTerm` property for the input field in the "Search" component.

The search functionality should work again. It stores all results from the Hacker News API.

Additionally the `onDismiss()` method needs to get improved. It still deals with the `result` object. Now it has to deal with multiple `results`.

{title="src/App.js",lang=javascript}
	  onDismiss(id) {
	# leanpub-start-insert
	    const { searchKey, results } = this.state;
	    const { hits, page } = results[searchKey];
	# leanpub-end-insert
	
	    const isNotId = item => item.objectID !== id;
	# leanpub-start-insert
	    const updatedHits = hits.filter(isNotId);
	
	    this.setState({
	      results: {
	        ...results,
	        [searchKey]: { hits: updatedHits, page }
	      }
	    });
	# leanpub-end-insert
	  }

The "Dismiss" button should work again.

However, nothing stops the application from sending an API request on each search submit. Even though there might be already a result, there is no check that prevents the request. Thus the cache functionality is not complete yet. It caches the results, but it doesn't make use of them. The last step would be to prevent the API request when a result is available in the cache.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  constructor(props) {
	
	    ...
	
	# leanpub-start-insert
	    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
	# leanpub-end-insert
	    this.setSearchTopStories = this.setSearchTopStories.bind(this);
	    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
	    this.onSearchChange = this.onSearchChange.bind(this);
	    this.onSearchSubmit = this.onSearchSubmit.bind(this);
	    this.onDismiss = this.onDismiss.bind(this);
	  }
	
	# leanpub-start-insert
	  needsToSearchTopStories(searchTerm) {
	    return !this.state.results[searchTerm];
	  }
	# leanpub-end-insert
	
	  ...
	
	  onSearchSubmit(event) {
	    const { searchTerm } = this.state;
	    this.setState({ searchKey: searchTerm });
	# leanpub-start-insert
	
	    if (this.needsToSearchTopStories(searchTerm)) {
	      this.fetchSearchTopStories(searchTerm);
	    }
	
	# leanpub-end-insert
	    event.preventDefault();
	  }
	
	  ...
	
	}

Now your client makes a request to the API only once although you search for a search term twice. Even paginated data with several pages gets cached that way, because you always save the last page for each result in the `results` map. Isn't that a powerful approach to introduce caching to your application? The Hacker News API provides you with everything you need to even cache paginated data effectively.

## Error Handling

Everything is in place for your interactions with the Hacker News API. You even have introduced an elegant way to cache your results from the API and make use of its paginated list functionality to fetch an endless list of sublists of stories from the API. But there is one piece missing. Unfortunately it is often missed when developing applications nowadays: error handling. It is too easy to implement the happy path without worrying about the errors that can happen along the way.

In this chapter, you will introduce an efficient solution to add error handling for your application in case of an erroneous API request. You have already learned about the necessary building blocks in React to introduce error handling: local state and conditional rendering. Basically, the error is only another state in React. When an error occurs, you will store it in the local state and display it with a conditional rendering in your component. That's it. Let's implement it in the App component, because it's the component that is used to fetch the data from the Hacker News API in the first place. First, you have to introduce the error in the local state. It is initialized as null, but will be set to the error object in case of an error.

{title="src/App.js",lang=javascript}
	class App extends Component {
	  constructor(props) {
	    super(props);
	
	    this.state = {
	      results: null,
	      searchKey: '',
	      searchTerm: DEFAULT_QUERY,
	# leanpub-start-insert
	      error: null,
	# leanpub-end-insert
	    };
	
	    ...
	  }
	
	...
	
	}

Second, you can use the catch block in your native fetch to store the error object in the local state by using `setState()`. Every time the API request isn't successful, the catch block would be executed.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  fetchSearchTopStories(searchTerm, page = 0) {
	    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
	      .then(response => response.json())
	      .then(result => this.setSearchTopStories(result))
	# leanpub-start-insert
	      .catch(error => this.setState({ error }));
	# leanpub-end-insert
	  }
	
	  ...
	
	}

Third, you can retrieve the error object from your local state in the `render()` method and display a message in case of an error by using React's conditional rendering.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const {
	      searchTerm,
	      results,
	      searchKey,
	# leanpub-start-insert
	      error
	# leanpub-end-insert
	    } = this.state;
	
	    ...
	
	# leanpub-start-insert
	    if (error) {
	      return <p>Something went wrong.</p>;
	    }
	# leanpub-end-insert
	
	    return (
	      <div className="page">
	        ...
	      </div>
	    );
	  }
	}

That's it. If you want to test that your error handling is working, you can change the API URL to something else that is non existent.

{title="src/App.js",lang=javascript}
	const PATH_BASE = 'https://hn.foo.bar.com/api/v1';

Afterward, you should get the error message instead of your application. It is up to you where you want to place the conditional rendering for the error message. In this case, the whole app isn't displayed anymore. That wouldn't be the best user experience. So what about displaying either the Table component or the error message? The remaining application would still be visible in case of an error.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const {
	      searchTerm,
	      results,
	      searchKey,
	      error
	    } = this.state;
	
	    const page = (
	      results &&
	      results[searchKey] &&
	      results[searchKey].page
	    ) || 0;
	
	    const list = (
	      results &&
	      results[searchKey] &&
	      results[searchKey].hits
	    ) || [];
	
	    return (
	      <div className="page">
	        <div className="interactions">
	          ...
	        </div>
	# leanpub-start-insert
	        { error
	          ? <div className="interactions">
	            <p>Something went wrong.</p>
	          </div>
	          : <Table
	            list={list}
	            onDismiss={this.onDismiss}
	          />
	        }
	# leanpub-end-insert
	        ...
	      </div>
	    );
	  }
	}

In the end, don't forget to revert the URL for the API to the existent one.

{title="src/App.js",lang=javascript}
	const PATH_BASE = 'https://hn.algolia.com/api/v1';

Your application should still work, but this time with error handling in case the API request fails.

### Exercises:

* read more about [React's Error Handling for Components][28]

## Axios instead of Fetch

In one of the previous chapters, you have introduced the native fetch API to perform a request to the Hacker News platform. The browser enables you to use this native fetch API. However, not all browsers, especially older browsers, support it. In addition, once you start to test your application in a headless browser environment (there is no browser, instead it is only mocked), there can be issues regarding the fetch API. Such a headless browser environment can happen when writing and executing tests for your application which don't run in a real browser. There are a couple of ways to make fetch work in older browsers (polyfills) and in tests ([isomorphic-fetch][29]), but we won't go down this rabbit hole in this book.

An alternative way to solve it would be to substitute the native fetch API with a stable library such as [axios][30]. Axios is a library that solves only one problem, but it solves it with a high quality: performing asynchronous requests to remote APIs. That's why you will use it in this book. On a concrete level, the chapter should show you how you can substitute a library (which is a native API of the browser in this case) with another library. On an abstract level, it should show you how you can always find a solution for the quirks (e.g. old browsers, headless browser tests) in web development. So never stop to look for solutions if anything gets in your way.

Let's see how the native fetch API can be substituted with axios. Actually everything said before sounds more difficult than it is. First, you have to install axios on the command line:

{title="Command Line",lang="text"}
	npm install --save axios

Second, you can import axios in your App component's file:

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	# leanpub-start-insert
	import axios from 'axios';
	# leanpub-end-insert
	import './App.css';
	
	...

And last but not least, you can use it instead of `fetch()`. Its usage looks almost identical to the native fetch API. It takes the URL as argument and returns a promise. You don't have to transform the returned response to JSON anymore. Axios is doing it for you and wraps the result into a `data` object in JavaScript. Thus make sure to adapt your code to the returned data structure.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  fetchSearchTopStories(searchTerm, page = 0) {
	# leanpub-start-insert
	    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
	      .then(result => this.setSearchTopStories(result.data))
	# leanpub-end-insert
	      .catch(error => this.setState({ error }));
	  }
	
	  ...
	
	}

That's it for replacing fetch with axios in this chapter. In your code, you are calling `axios()` which uses by default a HTTP GET request. You can make the GET request explicit by calling `axios.get()`. Also you can use another HTTP method such as HTTP POST with `axios.post()` instead. There you can already see how axios is a powerful library to perform requests to remote APIs. I often recommend to use it over the native fetch API when your API requests become complex or you have to deal with web development quirks with promises. In addition, in a later chapter, you will introduce testing in your application. Then you don't need to worry anymore about a browser or headless browser environment.

I want to introduce another improvement for the Hacker News request in the App component. Imagine your component mounts when the page is rendered for the first time in the browser. In `componentDidMount()` the component starts to make the request, but then, because your application introduced some kind of navigation, you navigate away from this page to another page. Your App component unmounts, but there is still a pending request from your `componentDidMount()` lifecycle method. It will attempt to use `this.setState()` eventually in the `then()` or `catch()` block of the promise. Perhaps then it's the first time you will see the following warning on your command line or in your browser's developer output:

{title="Command Line",lang="text"}
	Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.

You can deal with this issue by aborting the request when your component unmounts or preventing to call `this.setState()` on an unmounted component. It's a best practice in React, even though it's not followed by many developers, to preserve an clean application without any annoying warnings. However, the current promise API doesn't implement aborting a request. Thus you need to help yourself on this issue. This might also be the case why not many developers are following this best practice. The following implementation seems more like a workaround than a sustainable implementation. Because of that, you can decide on your own if you want to implement it to work around the warning because of an unmounted component. Nevertheless, keep the warning in mind in case it comes up in a later chapter of this book or in your own application one day. Then you know how to deal with it.

Let's start to work around it. You can introduce a class field which holds the lifecycle state of your component. It can be initialized as `false` when the component initializes, changed to `true` when the component mounted, but then again set to `false` when the component unmounted. This way, you can keep track of your component's lifecycle state. It has nothing to do with the local state stored and modified with `this.state` and `this.setState()`, because you should be able to access it directly on the component instance without relying on React's local state management. Moreover, it doesn't lead to any re-rendering of the component when the class field is changed this way.

{title="src/App.js",lang=javascript}
	class App extends Component {
	# leanpub-start-insert
	  _isMounted = false;
	# leanpub-end-insert
	
	  constructor(props) {
	    ...
	  }
	
	  ...
	
	  componentDidMount() {
	# leanpub-start-insert
	    this._isMounted = true;
	# leanpub-end-insert
	
	    const { searchTerm } = this.state;
	    this.setState({ searchKey: searchTerm });
	    this.fetchSearchTopStories(searchTerm);
	  }
	
	# leanpub-start-insert
	  componentWillUnmount() {
	    this._isMounted = false;
	  }
	# leanpub-end-insert
	
	  ...
	
	}

Finally, you can use this knowledge not to abort the request itself but to avoid calling `this.setState()` on your component instance even though the component already unmounted. It will prevent the mentioned warning.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  fetchSearchTopStories(searchTerm, page = 0) {
	    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
	# leanpub-start-insert
	      .then(result => this._isMounted && this.setSearchTopStories(result.data))
	      .catch(error => this._isMounted && this.setState({ error }));
	# leanpub-end-insert
	  }
	
	  ...
	
	}

Overall the chapter has shown you how you can replace one library with another library in React. If you run into any issues, you can use the vast library ecosystem in JavaScript to help yourself. In addition, you have seen a way how you can avoid calling `this.setState()` in React on an unmounted component. If you dig deeper into the axios library, you will find a way to cancel the request in the first place too. It's up to you to read up more about this topic.

### Exercises:

* read more about [why frameworks matter][31]
* learn more about [an alternative React component syntax][32]

{pagebreak}

You have learned to interact with an API in React! Let's recap the last chapters:

* React
  * ES6 class component lifecycle methods for different use cases
  * componentDidMount() for API interactions
  * conditional renderings
  * synthetic events on forms
  * error handling
  * aborting a remote API request
* ES6 and beyond
  * template strings to compose strings
  * spread operator for immutable data structures
  * computed property names
  * class fields
* General
  * Hacker News API interaction
  * native fetch browser API
  * client- and server-side search
  * pagination of data
  * client-side caching
  * axios as an alternative for the native fetch API

Again it makes sense to take a break. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far. You can find the source code in the [official repository][33].

[1]:	https://www.robinwieruch.de/what-is-an-api-javascript/
[2]:	https://news.ycombinator.com/
[3]:	https://github.com/HackerNews/API
[4]:	https://hn.algolia.com/api
[5]:	https://www.robinwieruch.de/what-is-new-in-react-16/
[6]:	https://reactjs.org/docs/react-component.html
[7]:	https://reactjs.org/docs/state-and-lifecycle.html
[8]:	https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html
[9]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals
[10]:	https://hn.algolia.com/api
[11]:	https://github.com/visionmedia/superagent
[12]:	https://github.com/mzabriskie/axios
[13]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals
[14]:	https://developer.mozilla.org/en/docs/Web/API/Fetch_API
[15]:	https://www.robinwieruch.de/react-fetching-data/
[16]:	https://github.com/sebmarkbage/ecmascript-rest-spread
[17]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign
[18]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator
[19]:	https://www.robinwieruch.de/conditional-rendering-react/
[20]:	https://www.robinwieruch.de/conditional-rendering-react/
[21]:	https://reactjs.org/docs/conditional-rendering.html
[22]:	https://reactjs.org/docs/events.html
[23]:	https://hn.algolia.com/api
[24]:	https://hn.algolia.com/api
[25]:	https://www.robinwieruch.de/what-is-an-api-javascript/
[26]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters
[27]:	https://hn.algolia.com/api
[28]:	https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html
[29]:	https://github.com/matthew-andrews/isomorphic-fetch
[30]:	https://github.com/axios/axios
[31]:	https://www.robinwieruch.de/why-frameworks-matter/
[32]:	https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax
[33]:	https://github.com/the-road-to-learn-react/hackernews-client/tree/5.3.1