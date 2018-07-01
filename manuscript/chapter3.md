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

A montagem de um componente tem mais dois métodos de ciclo de vida: `getDerivedStateFromProps()` e `componentDidMount()`. O construtor é chamado primeiro, `getDerivedStateFromProps()` chamado antes de `render()` e `componentDidMount`é invocado depois do método `render()`.

Em resumo, o processo de montagem tem 4 métodos de ciclo de vida. Eles são invocados na seguinte ordem:

* constructor()
* getDerivedStateFromProps()
* render()
* componentDidMount()

Mas, e quanto ao ciclo de vida de **atualização** de um componente, o que acontece quando o _state_ ou as _props_ mudam?  No total, existem 5 métodos, chamados na seguinte ordem:

* getDerivedStateFromProps()
* shouldComponentUpdate()
* render()
* getSnapshotBeforeUpdate()
* componentDidUpdate()

Por último, mas não menos importante, existe o ciclo de vida de desmontagem de um componente. Ele possui apenas um método, `componentWillUnmount()`.

No fim das contas, você não precisa conhecer todos esses métodos desde o começo. Pode ser um pouco intimidador ter que decorar e você provavelmente nem usará todos eles. Mesmo em uma aplicação React de maior escala, você usará apenas um subconjunto de métodos além de `render()` e `constructor`. Mas, é legal saber que existem outros, que podem ser usados em casos específicos:

* **constructor(props)** -É chamado quando o componente é inicializado. Você pode definir um estado inicial do componente e realizar o _binding_ de métodos de classe aqui.

* ** static getDerivedStateFromProps(props, state)** - Invocado antes do método `render()`, tanto na montagem inicial do componente, quanto em atualizações subsequentes. Ele deve retornar um objeto para atualização do estado (ou _null_ para não atualizar nada). Este método existe para ser usado em **raras** situações onde o estado depende das mudanças de _props_ ao longo do tempo. Importante notar que este é um método estático e não tem acesso à instância do componente.

* **render()** - Este método de ciclo de vida é mandatório e retorna os elementos como saída do componente. Deve ser puro e, logo, não deve modificar o estado do componente. Ele recebe como entrada o _state_ e as _props_ e retorna um elemento.

* **componentDidMount()** - É chamado apenas uma vez, quando o component é montado. É o momento perfeito para fazer qualquer requisição assíncrona para obter dados de uma API. Os dados obtidos serão armazenados no estado interno do componente, para serem mostrados via `render()`.

* **shouldComponentUpdate(nextProps, nextState)** - É sempre chamado quando o componente atualiza devido a mudanças de `state` ou `props`.  Você utilizará este método em uma aplicação React mais madura, visando otimizações de performance. Dependendo do valor booleano retornado aqui, o componente e todos os seus filhos irão ser novamente renderizados no ciclo de atualização. Você pode evitar que o método `render()` seja invocado.

* ** getSnapshotBeforeUpdate(prevProps, prevState)** - Este método de ciclo de vida é invocado imediatamente antes da renderização mais recente ser aplicada no DOM. Em raros casos, quando o componente precisa capturar a informação do DOM antes que ela seja alterada, este método lhe permite fazê-lo. Outro método de ciclo de vida (`componentDidMount`) irá receber como um parâmetro qualquer valor retornado por `getSnapshotBeforeUpdate()`.

* **componentDidUpdate(prevProps, prevState, snapshot)** - Este método é chamado imediatamente depois de uma atualização do componente, com exceção da renderização inicial. Você pode utilizá-lo como uma oportunidade de realizar operações no DOM ou de efetuar requisições assíncronas depois do componente já estar montado. Se `getSnapshotBeforeUpdate()` tiver sido implementado no seu componente, o valor por ele retornado será recebido aqui, através do parâmetro `snapshot`.

* **componentWillUnmount()** - Chamado antes de você destruir seu componente. Você pode usá-lo para realizar qualquer tarefa de limpeza, por exemplo.

Os métodos de ciclo de vida `constructor()` e `render()` já foram utilizados por você, uma vez que são comumente utilizados em componentes de classe ES6. O método `render()` é, na verdade, obrigatório, caso contrário você não conseguiria retornar uma instância do componente.

Existe ainda um outro método de ciclo de vida em componentes React: `componentDidCatch(error, info)`. Ele foi introduzido no [React 16][5] e é utilizado para capturar erros em componentes. No nosso contexto, a lista de dados da sua aplicação está funcionando muito bem. Mas, pode existir o caso da lista no estado local ser definida, por acidente, com o valor nulo (por exemplo, quando os dados são obtidos de uma API externa, mas a requisição falha e você define o valor do estado local para `null`).  Ocorrido esta situação, não será mais possível filtrar ou mapear a lista, porque ela tem valor `null` ao invés de ser uma lista vazia. O componente quebraria e a aplicação inteira poderia falhar. Agora, com o uso do `componentDidCatch()`, você capturar o erro, armazená-lo no estado local e, opcionalmente, exibir uma mensagem para o usuário informando que algo de errado ocorreu.

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

## Renderização Condicional

Renderização condicional é algo que, logo cedo, aparece em aplicações React. No livro, ainda não apareceu um caso em que fosse precisa. A renderização condicional acontece quando você quer decidir, em tempo de execução, entre renderizar um elemento ou outro. Algumas vezes, renderizar um elemento ou nada. No fim das contas, pode ser expressada como uma declaração if-else em JSX.

O objeto `result` no estado interno do componente é, inicialmente, `null`. Até então, o component App não retornou nenhum elemento quando `result` não contém nada avindo da API. Não deixa de ser uma renderização condicional, uma vez que você opta por retornar mais cedo no `render()`, dada uma condição. O component App ou renderiza nada ou seus elementos.

Mas, vamos dar um passo adiante. Faz mais sentido envolver o componente _Table_, que é um componente que depende de `result`, em uma condição independente de renderização. Todo o resto deve ser exibido, mesmo que ainda não haja nada em `result`. Você pode simplesmente usar um operador ternário em seu código JSX.

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

Esta é sua segunda opção para expressar uma renderização condicional. Uma terceira opção é o operador lógico `&&`. Em JavaScript, um `true && 'Hello World'` sempre resulta em ‘Hello World’. Um `false && 'Hello World'` sempre resulta como _false_.

{title="Code Playground",lang="javascript"}
	const result = true && 'Hello World';
	console.log(result);
	// output: Hello World
	
	const result = false && 'Hello World';
	console.log(result);
	// output: false

Em React, você pode tomar proveito deste comportamento. Se a condição é verdadeira, a expressão após o operador lógico `&&` será a saída. Se a condição é falsa, React ignora e descarta a expressão. Isso pode ser aplicado no caso da renderização condicional de _Table_, porque o retorno será _Table_ ou nada.

{title="src/App.js",lang=javascript}
	{ result &&
	  <Table
	    list={result.hits}
	    pattern={searchTerm}
	    onDismiss={this.onDismiss}
	  />
	}

Estas foram algumas abordagens de como implementar a renderização condicional em React. Você poderá ler [mais alternativas nesta exaustiva lista de exemplos de renderização condicional][19]. Você irá conhecer seus diferentes casos de uso e quando aplicá-los.

Finalmente, você deverá estar vendo os dados obtidos em sua aplicação. Tudo, menos _Table_, é exibido quando a consulta dos dados ainda está pendente. Uma vez que a requisição é completada, o resultado é armazenado no estado local e _Table_ é exibido, porque o método `render()` é novamente invocado e a condição avaliada resulta em seu favor.

### Exercícios:

* Leia mais sobre [diferentes formas de renderizações condicionais][20]
* Leia mais sobre [renderização condicional em React][21]

## Consultando no lado do cliente ou do servidor

Atualmente, quando você usa o componente _Search_ com o seu campo _input_, você irá filtrar a lista, embora isso esteja ocorrendo apenas no lado do cliente. Agora, você irá usar a API do Hacker News para realizar a busca do lado do servidor. Caso contrário, você só teria o primeiro resultado de chamada à API, feita no `componentDidMount()` com o termo de busca _default_ como parâmetro.

Você pode definir um método `onSearchSubmit()` no seu componente _App_, que obtém resultados da API do Hacker News quando executando uma busca no componente _Search_.

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

O método `onSearchSubmit()` poderia utilizar a mesma funcionalidade que `componentDidMount()`, porém com o termo de busca modificado do estado local e não com o termo _default_ inicial. Sendo assim, você pode extrair a funcionalidade como um método de classe reutilizável.

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

Agora, é necessário que um novo botão seja aficionado no componente _Search_. Este botão tem que, explicitamente, disparar a requisição de consulta. Caso contrário, os dados da API Hacker News seriam obtidos todas as vezes que o campo _input_ mudasse. O que você espera é que este comportamento aconteça explicitamente em um tratamento do evento `onClick()`.

Uma alternativa seria o uso de um _debounce_ (um técnica de “atraso” na ação) na função `onChange()`, evitando a necessidade do botão, mas isto adicionaria mais complexidade no momento e, talvez, não teria o efeito desejado. Vamos manter as coisas simples, sem o _debounce_ por enquanto.

Primeiramente, passe o método `onSearchSubmit()` ao seu componente `Search`.

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

Segundo, crie um botão no componente _Search_. O botão possui um `type="submit"` e o _form_ irá utilizar o atributo `onSubmit` para com o método `onSubmit()`. Você pode reutilizar a propriedade _children_, mas desta vez será usada como conteúdo de _button_.

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

Em _Table_, você pode remover a funcionalidade _filter_, porque não mais existirá um filtro (busca) do lado do cliente. Não esqueça de remover a função `isSearched()` também, uma vez que não será mais utilizada. O resultado virá diretamente da API Hacker News, depois que o botão _”Search”_ for clicado. 

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

Quando tentar realizar uma consulta agora, você irá notar que o _browser_ irá recarregar o conteúdo. Este é um comportamento natural do navegador para uma função de _callback_ de _submit_ em um _form_ HTML. Em React, frequentemente você suprimir este comportamento nativo através do método `preventDefault()`.

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

Enfim, você deverá conseguir consultar diferentes discussões na API Hacker News, sem mais haver filtragem no lado do cliente.

### Exercícios:

* Leia mais sobre [_SyntheticEvent_ em React][22]
* Faça experiências com [Hacker News API][23]

## Paginação de dados

Você já deu uma olhada na estrutura de dados retornada? A [API Hacker News][24] retorna mais de uma lista de resultados. Mais precisamente, ela retorna uma lista paginada. A propriedade _page_, que é `0` na primeira resposta à requisição, pode ser utilizada para consultar mais sulistas paginadas como resultado. Você precisa apenas passar a próxima página com o mesmo termo de busca (_search term_) para a API.

Vamos estender a lista de constantes de API, para que seja possível lidar com dados paginados.

{title="src/App.js",lang=javascript}
	const DEFAULT_QUERY = 'redux';
	
	const PATH_BASE = 'https://hn.algolia.com/api/v1';
	const PATH_SEARCH = '/search';
	const PARAM_SEARCH = 'query=';
	# leanpub-start-insert
	const PARAM_PAGE = 'page=';
	# leanpub-end-insert

Agora, você pode utilizar a nova constante para adicionar o parâmetro _page_ à sua requisição de API.

{title="Code Playground",lang="javascript"}
	const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;
	
	console.log(url);
	// output: https://hn.algolia.com/api/v1/search?query=redux&page=

O método `fetchSearchTopStories()`irá receber a página como o segundo argumento. Se você não o fornecer ao método, ele irá retornar a página `0` como resultado da requisição. Sendo assim, os métodos `componentDidMount()` e `onSearchSubmit()` consultam a primeira página na primeira requisição. Cada consulta adicional deve buscar a próxima página, fornecendo o segundo argumento.

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

O argumento que representa a página usa um parâmetro _default_ de JavaScript ES6, para definir o valor de página `0` no caso de nenhum outro ser fornecido para a função.

Você pode agora usar a página atual do resultado obtido no `fetchSearchTopStories()`. Use este método em um botão, para obter mais discussões em um tratamento de evento `onClick`. Utilizaremos _Button_ para obter mais dados paginados da API Hacker News, sendo necessário apenas definir o tratamento do `onClick  
()`, que recebe o termo de busca atual e a próxima página (página atual + 1).

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

Além disso, no seu método `render()`, você deverá assegurar, por padrão, a página `0` quando ainda não existir nenhum resultado. Lembre-se que o método `render()`é chamado antes que os dados sejam, de forma assíncrona, obtidos em `componentDidMount()`.

Falta ainda um passo. Você obtém sua próxima página de dados, mas ela irá sobrescrever os dados da página anterior. Seria ideal concatenar as duas listas (a antiga e a nova) do estado local e do novo objeto resultante. Ajustemos a funcionalidade para que os novos dados sejam adicionados, ao invés de sobrescritos.

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

Mais coisas acontecem em `setSearchTopStories()`. Primeiro, você recebe, no resultado, os itens (ou\_hits\_) e a página.

Segundo, você checa se já existiam _hits_ de consultas anteriores. Quando a página é 0, significa que é uma nova consulta feita em `componentDidMount()` ou `onSearchSubmit()`. O valor em _hits_ é vazio. Mas, quando você clica no botão “More”, visando obter mais dados paginados, o valor de _page_ não é 0, mas o da próxima página. Os itens do resultado anterior já se encontram armazenados em seu estado e, desta forma, podem ser utilizados.

Terceiro, você não deseja sobrescrever o valor antigo em _hits_. Você pode fundi-lo com os novos, da requisição mais recente à API. A junção das duas listas pode ser feita através do operador _spread_ de _arrays_ em JavaScript ES6.

Quarto, você define o novo estado local do componente, com os _hits_ combinados e o valor da página.

Um último ajuste pode ser feito. Quando você testa o botão “More”, ele obtém apenas alguns itens da lista. A URL da API pode ser editada para obter mais itens a cada requisição. Novamente, você pode adicionar mais constantes aqui.

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

Pronto, você pode utilizar as constantes para estender a URL da API.

{title="src/App.js",lang=javascript}
	fetchSearchTopStories(searchTerm, page = 0) {
	# leanpub-start-insert
	  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
	# leanpub-end-insert
	    .then(response => response.json())
	    .then(result => this.setSearchTopStories(result))
	    .catch(error => error);
	}

No final, a requisição à API Hacker News irá obter mais itens de uma vez, em relação ao que fazia antes. Como você pode ver, uma API tão poderosa como esta lhe dá uma rica variedade de alternativas para realizar experimentos com dados do mundo real. Faça uso dela quando estiver aprendendo algo novo, de um jeito mais excitante. Veja [como eu aprendi sobre os “poderes” que APIs provêem][25] quando se está aprendendo uma nova linguagem de programação ou biblioteca.

### Exercícios:

* Leia mais sobre [parâmetros _default_ ES6][26]
* Experimente as possibilidades dos [parâmetros da Hacker News API][27]

## _Cache_ do Cliente

Cada _submit_ de consulta faz uma requisição à API Hacker News. Você poderia buscar por “redux”, depois por “react” e, eventualmente, “redux” novamente. No total, 3 requisições. Mas, você buscou por “redux” duas vezes e, em ambas, todo o caminho de requisição assíncrona para obter dados foi percorrido. Em uma implementação de _cache_ do lado do cliente, você armazena cada resultado. Quando uma requisição à API é feita, ela antes checa se o resultado já existe. Se sim, o _cache_ é utilizado. Caso contrário, o processo completo é seguido.

A fim de ter um _cache_ no cliente para cada resultado, você terá que armazenar múltiplos `results` ao invés de um só `result` no estado interno do seu componente. O objeto _results_ consistirá em um mapa com o termo de busca como chave e o _result_ como valor. Cada resultado da API será salvo com este conjunto chave-valor.

No momento, o resultado no estado local se encontra da seguinte forma:

{title="Code Playground",lang="javascript"}
	result: {
	  hits: [ ... ],
	  page: 2,
	}

Imaginando que você tenha feito duas requisições, uma para o termo “redux” e outra para “react”, o objeto _results_ deverá se parecer com o seguinte:

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

Vamos implementar a lógica do _cache_ do cliente com `setState()`. Primeiro, renomeie o objeto `result` para `results` no estado inicial do componente. Segundo, defina uma `searchKey` temporária, que é utilizada para armazenar cada `result`.

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

A `searchKey` deve ser definida antes de cada requisição ser feita. Ela reflete o `searchTerm`. Você deve estar se perguntando: Por que não utilizar `searchTerm` diretamente? Esta é uma parte crucial a ser entendida, antes de continuar com a implementação. O `searchTerm` é uma variável **instável**, porque ele é alterado todas as vezes que você digita no campo _input_ do _Search_. Entretanto, você precisará de uma variável mais **estável** para determinar o termo de busca recentemente submetido à API para recuperar o resultado correto do mapa de resultados. Ela é um ponteiro para seu resultado atual no _cache_ e, desta forma, pode ser utilizado para exibi-lo no método `render()`.

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

Com isso, você também precisa ajustar a funcionalidade onde o resultado é armazenado no estado interno do componente. Ela agora deve gravar cada resultado por `searchKey`.

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

A `searchKey` será usada para salvar os _hits_ e página atualizados em um mapa de `results`.

Primeiro, você deve recuperá-la do estado do componente. Lembre-se de que `searchKey` tem o valor definido em `componentDidMount()` e `onSearchSubmit()`.

Segundo, os resultados anteriores precisam ser combinados com os novos, como já ocorria antes. Mas, desta vez, os valores antigos são recuperados do mapa com a chave `searchKey`.

Terceiro, um novo resultado pode ser colocado em `results` no estado do componente. Third, a new result can be set in the `results` map in the state. Observemos o objeto `results` dentro do `setState()`.

{title="src/App.js",lang=javascript}
	results: {
	  ...results,
	  [searchKey]: { hits: updatedHits, page }
	}

A parte inferior garante que o resultado atualizado é armazenado por `searchKey` no mapa. O valor é um objeto com propriedades _hits_ e _page_. A `searchKey` é o termo de busca. Você já aprendeu o que significa a sintaxe `[searchKey]: ...` em ES6: é uma propriedade com nome computado. Ela lhe ajuda a alocar valores dinamicamente em um objeto.

A parte superior precisa copiar todos os outros resultados no state, por `searchKey`, utilizando o operador _spread_ de objetos. Caso contrário, você perderia tudo o que foi armazenado anteriormente.

Todos os resultados são agora armazenados por termo de busca. Este foi o primeiro passo para habilitar o comportamento de _cache_. No passo seguinte, você pode recuperar o resultado através da variável `searchKey` no mapa de resultados. Este é o principal motivo pelo qual `searchKey` teve que ser definido como uma variável mais estável. Se não fosse assim, o processo de recuperação em _cache_ nem sempre funcionaria, uma vez que o valor em `searchTerm` muda frequentemente enquanto você utiliza o componente _Search_.

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

Uma vez que você definiu uma lista vazia como padrão, quando não existe resultado para a `searchKey`, você pode poupar a renderização condicional no componente _Table_. Você também irá precisar passar a `searchKey`, ao invés do `searchTerm`, para o botão “More”. Caso contrário, sua consulta paginada dependerá deste último que, como já falamos, varia de uma forma instável. Garanta que `searchTerm` será utilizado com o campo _input_ do componente “Search”.

A funcionalidade de consulta deve voltar a funcionar. Ela armazena localmente todos os resultados da API Hacker News.

Ademais, o método `onDismiss()` precisa ser melhorado. Isto porque ele ainda trabalha com o objeto `result`. Agora ele deverá saber lidar com múltiplos resultados no objeto `results`.

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

Pronto. O botão “Dismiss” deverá voltar a funcionar.

Entretanto, não existe nada que impeça a aplicação de uma requisição à API a cada _submit_ de busca. Mesmo que já exista um resultado, a requisição será feita assim mesmo. Precisamos completar o comportamento da funcionalidade de _cache_, que já mantém os resultados, mas ainda não toma proveito deles. Resumindo, o último passo é: evitar uma nova requisição à API, caso já exista um resultado disponível em _cache_.

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

Agora, seu cliente fará apenas uma requisição à API, quando você buscar pelo mesmo termo duas ou mais vezes. Até mesmo dados paginados, com diversas páginas, são armazenados em _cache_ desta forma, porque você sempre salva a última página exibida, para cada resultado, no mapa `results`. Esta é uma abordagem muito arrojada de incluir _caching_ na sua aplicação, não acha? A API Hacker News provê tudo o que você precisa para fazer _cache_ até mesmo de dados paginados de forma efetiva.

## Tratamento de Erros

Tudo está pronto para que você interaja com a API do Hacker News. Você até mesmo introduziu um jeito muito elegante de manter _cache_ dos seus resultados e implementou paginação para obter listas de infinitos resultados de discussões da API. Mas, ainda falta uma peça no quebra-cabeças. Infelizmente, uma peça que geralmente tem faltado em aplicações desenvolvidas nos dias de hoje: tratamento de erros. É muito fácil implementar o “caminho feliz” sem a preocupação sobre erros que poderiam ocorrer no processo.

Neste capítulo, você irá incluir uma solução eficiente de tratamento de erros em sua aplicação, para o caso de problemas em uma requisição à API. Você já aprendeu tudo sobre as partes necessárias para adicionar tratamento de erros em React: estado local e renderização condicional. Basicamente, um erro é apenas mais um estado em React. Quando ele ocorre, você irá guardá-lo em um estado local e exibi-lo através de uma renderização condicional em seu componente. E só. Vamos implementar este tratamento no componente _App_, porque ele é o componente utilizado na consulta de dados da API. Primeiro, você tem que incluir o estado local `error`, inicializado com _null_, para receber um objeto em caso de erro.

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

Segundo, você pode utilizar o bloco `catch`, da função nativa _fetch_, para chamar `setState()` e guardar o objeto de erro no estado local. Todas as vezes que uma requisição à API não é bem sucedida, o bloco _catch_ será executado.

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

Terceiro, você pode recuperar este objeto do estado local no método `render()` e exibir uma mensagem em caso de erro, utilizando a renderização condicional de React.

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

É isto. Se quiser testar que o seu tratamento de erros está funcionando, você pode mudar a URL da API para algum valor inexistente.

{title="src/App.js",lang=javascript}
	const PATH_BASE = 'https://hn.foo.bar.com/api/v1';

Se o fizer, você deverá receber uma mensagem de erro, ao invés do conteúdo da aplicação. Fica a seu critério o local onde deseja colocar a renderização condicional para a mensagem de erro. No caso aqui, a aplicação inteira não é mais exibida, o que não seria exatamente a melhor experiência de usuário. Que tal, então, exibir a mensagem apenas no lugar do componente _Table_? O restante da aplicação ainda será visível em caso de erro.

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

Depois de feitos os testes, não se esqueça de restaurar a URL original da API.

{title="src/App.js",lang=javascript}
	const PATH_BASE = 'https://hn.algolia.com/api/v1';

Sua aplicação estará funcionando, com a adição de um tratamento de erros em caso de problemas com a API.

### Exercícios:

* Leia mais sobre [tratamento de erros em componentes React][28]

## _Axios_ no lugar de _Fetch_

Em um capítulo anterior, você utilizou as funções nativas de _fetch_ para realizar requisições à plataforma do Hacker News. A maior parte dos navegadores lhe permitem fazê-lo. Mas, alguns, especialmente os mais antigos, não oferecem suporte à essa API nativa de JavaScript. Além disso, uma vez que você começar a testar sua aplicação com ambientes que simulam um _browser_, também pode se deparar com problemas em relação ao uso de _fetch_. Existem algumas maneiras de contornar estes problemas, fazendo com que _fetch_ funcione tanto em navegadores antigos (polyfills), quando em testes ([isomorphic-fetch][29]), mas nós não entraremos em maiores detalhes sobre isso aqui.

Uma solução alternativa é substituir o _fetch_ por uma biblioteca mais estável como a [axios][30]. Axios é uma biblioteca dedicada a resolver apenas um problema, mas o faz com alta qualidade: efetuar requisições assíncronas à APIs remotas. Por este motivo, você irá utilizá-la neste livro. Em termos práticos, este capítulo deve lhe mostrar como substituir uma biblioteca pela outra. Em um nível mais conceitual, ele lhe mostra como sempre é possível achar uma solução para esses pequenos caprichos, existentes no desenvolvimento _web_. Nunca pare de buscar soluções para obstáculos que venham a aparecer no seu caminho.

Vejamos como _fetch_ pode ser substituído por _axios_. Tudo que foi falado até então parece ser mais difícil do que realmente é. Primeiramente, você tem que instalar a biblioteca _axios_ via linha de comando:

{title=“Linha de Comando“,lang="text"}
	npm install --save axios

Segundo, você irá importar _axios_ no seu componente _App_:

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	# leanpub-start-insert
	import axios from 'axios';
	# leanpub-end-insert
	import './App.css';
	
	...

E por último, mas não menos importante, você usará a biblioteca, de forma quase idêntica à API nativa _fetch_. Ela recebe a URL como argumento e retorna uma _promise_. Você não precisa transformar a resposta para JSON, no então. _Axios_ faz isto para você e transforma o resultado em um objeto JavaScript chamado `data`. Certifique-se de que adaptou seu código à estrutura de dados retornada.

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

É somente isto, no que se refere a substituir _fetch_ por _axios_ neste capítulo. No seu código, você está chamando `axios()`, que usa por padrão uma requisição HTTP GET. Você pode fazer uma requisição GET explícita chamando `axios.get()`. Outros métodos HTTP como POST podem ser usados com `axios.post()`. Neste ponto, você já deve conseguir enxergar como a biblioteca _axios_ é poderosa. Eu sempre recomendo que seja utilizada no lugar de _fetch_ quando suas requisições se tornarem muito complexas ou quando você tem que lidar com caprichos do desenvolvimento web. Em adição a isto, em um capítulo mais na frente, você incluirá testes na sua aplicação. Não precisará, então, se preocupar mais com navegadores ou ambientes que simulam eles.

Eu gostaria de introduzir outra melhoria para a consulta ao Hacker News no componente _App_. Imagine que seu componente seja montado quando a página é renderizado pela primeira vez no _browser_. Em `componentDidMount()` o componente começa a fazer a requisição mas, logo depois, porque sua aplicação disponibilizou algum tipo de navegação, você sai da página atual e navega para outra. Seu componente _App_ é desmontado, mas ainda existirá uma requisição pendente disparada no método e ciclo de vida `componentDidMount()`. Ela tentará, eventualmente, utilizar `this.setState()` no `then()` ou no `catch()` da _promise_. Provavelmente, pela primeira vez, você verá o seguinte _warning_ na linha de comando ou no _console_ do seu navegador:

{title="Command Line",lang="text"}
	Warning: Can only update a mounted or mounting component. This usually means you called setState, replaceState, or forceUpdate on an unmounted component. This is a no-op.

Você pode tratar isto abortando a requisição quando seu componente é desmontado, prevenindo a chamada de `this.setState()` em um componente nesta situação. É uma boa prática em React (mesmo que não seja seguida por muitos desenvolvedores) para preservar a aplicação limpa e sem _warnings_. Contudo, a atual API de _promises_ não permite abortar requisições. Você precisa se virar para fazê-lo, o que pode ser o motivo pelo qual poucos desenvolvedores têm seguido esta boa prática. A implementação a seguir se parece mais com uma solução de contorno do que uma implementação sustentável. Sabendo disso, fica em suas mãos a escolha de fazê-lo, ou não. Esteja apenas ciente do _warning_, para o caso dele aparecer novamente em algum capítulo do livro ou uma aplicação sua, no futuro. Se acontecer, você saberá tratá-lo.

Para começar, você pode adicionar uma variável de classe que mantém o estado do ciclo de vida do seu componente. Ele pode ser inicializado com o valor `false` quando o componente inicializa, mudado para `true` quando o componente é montado e, novamente, `false` quando ele é desmontado. Assim, você será capaz de rastrear o estado do ciclo de vida. Este campo nada tem a ver com o estado local, nem é acessado ou modificado com `this.state` e `this.setState()`, uma vez que você deveria poder acessá-lo diretamente na instância do componente sem depender do gerenciamento de estado de React. Ele também não leva a nenhuma nova renderização do componente quando seu valor é modificado.

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

Finalmente, você pode utilizar este conhecimento, não para abortar a requisição por si mesma, mas para evitar que `setState()` seja chamado depois do componente ter sido desmontado, evitando o _warning_ antes mencionado.

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

No geral, este capítulo lhe mostrou como você pode substituir uma biblioteca por outra em React. Se você se deparar com problemas, coloque a seu favor o vasto ecossistema de bibliotecas JavaScript. Você também viu uma forma de evitar que `this.setState()` seja chamado em um component React não montado. Se você investigar a biblioteca _axios_ mais a fundo, encontrará uma outra forma de cancelar a requisição. Fica a seu critério se aprofundar no tópico, ou não.

### Exercícios:

* Leia mais sobre [porque _frameworks_ importam][31]
* Leia mais sobre [uma sintaxe alternativa para componentes React][32]

{pagebreak}

Você aprendeu a interagir com uma API em React! Vamos recapitular os últimos tópicos:

* React
	* Métodos de ciclo de vida de componentes de classe e seus diferentes casos de uso
	* componentDidMount() para interações com APIs
	* renderização condicional
	* _synthetic events_ em _forms_
	* tratamento de erros
	* abortando uma requisição à uma API remota
* ES6 e além
	* _template strings_
	* operador _spread_ para estruturas de dados imutáveis
	* nomes de propriedades computados
	* campos (variáveis) de classe 
* Geral
	* Integração com a Hacker News API
	* API nativa _fetch_
	* buscas do lado do cliente e do servidor
	* dados paginados
	* _caching_ no cliente
	* _axios_  como uma alternativa à API nativa de _fetch_

Novamente, faz sentido fazer uma pausa. Internalize o aprendizado e aplique-o você mesmo. Você pode fazer experiências com o código que escreveu até agora. Você também pode achar o código-fonte no [repositório oficial][33].

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