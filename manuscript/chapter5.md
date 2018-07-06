# Conceitos Avançados de Componentes React

Este capítulo irá focar na implementação de conceitos avançados de componentes React. Você irá aprender à respeito de _high-order components_ e como implementá-los. Também irá mergulhar em outros tópicos mais avançados e implementar interações mais complexas em React.

## Usando _ref_ com um elemento do DOM

Às vezes, você precisa interagir com os nós da árvore do DOM (_DOM nodes_) em React. O atributo `ref` lhe dá acesso à um nó em seus elementos. Geralmente isso significa um anti-padrão em React, porque você deveria utilizar a sua forma mais declarativa de fazer as coisas e também seu fluxo unidirecional de dados, como você bem aprendeu quando adicionou seu primeiro campo de _input_ para consultas. Mas, existem certos casos onde você realmente precisa acessar um nó no DOM.  A documentação oficial menciona três deles:

* para usar a API do DOM (focus, media playback, etc.)
* para invocar animações em nós do DOM
* para integrar-se com uma biblioteca de terceiros que precisa ter acesso à arvore de nós do DOM (exemplo: [D3.js][1])

Façamos um exemplo com um componente _Search_. Quando a aplicação é renderizada pela primeira vez, o campo de _input_ deveria receber o foco. Este é um caso onde você precisaria acessar a API do DOM. Este capítulo lhe mostrará como isto funciona, mas, uma vez que não é muito útil para a aplicação em si, omitiremos estas mudanças em capítulos posteriores. Você pode mantê-las no seu próprio código, se quiser.

Em geral, você pode usar o atributo `ref` em ambos os tipos de componentes React (_functional stateless components_ e componentes de classe ES6). No caso do foco, você precisará usar um método de ciclo de vida e, por isso, um componente de classe ES6 foi utilizado na demonstração.

O passo inicial é refatorar o componente funcional para passar a ser um componente de classe.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	class Search extends Component {
	  render() {
	    const {
	      value,
	      onChange,
	      onSubmit,
	      children
	    } = this.props;
	
	    return (
	# leanpub-end-insert
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
	# leanpub-start-insert
	    );
	  }
	}
	# leanpub-end-insert

O objeto `this` de um componente de classe ES6 nos ajuda a referenciar o _DOM node_ com o atributo `ref`.

{title="src/App.js",lang=javascript}
	class Search extends Component {
	  render() {
	    const {
	      value,
	      onChange,
	      onSubmit,
	      children
	    } = this.props;
	
	    return (
	      <form onSubmit={onSubmit}>
	        <input
	          type="text"
	          value={value}
	          onChange={onChange}
	# leanpub-start-insert
	          ref={(node) => { this.input = node; }}
	# leanpub-end-insert
	        />
	        <button type="submit">
	          {children}
	        </button>
	      </form>
	    );
	  }
	}

Agora, você pode definir o foco para o campo _input_ quando o componente é montado, usando o objeto `this`, o método de ciclo de vida apropriado e a API do DOM.

{title="src/App.js",lang=javascript}
	class Search extends Component {
	# leanpub-start-insert
	  componentDidMount() {
	    if(this.input) {
	      this.input.focus();
	    }
	  }
	# leanpub-end-insert
	
	  render() {
	    const {
	      value,
	      onChange,
	      onSubmit,
	      children
	    } = this.props;
	
	    return (
	      <form onSubmit={onSubmit}>
	        <input
	          type="text"
	          value={value}
	          onChange={onChange}
	          ref={(node) => { this.input = node; }}
	        />
	        <button type="submit">
	          {children}
	        </button>
	      </form>
	    );
	  }
	}

O campo _input_ deverá receber o foco quando a aplicação é renderizada. Basicamente, assim é o uso do atributo `ref`.

Mas, como você obteria acesso ao atributo `ref` em um _stateless functional component_, sem o objeto `this`? O código a seguir demonstra como fazê-lo:

{title="src/App.js",lang=javascript}
	const Search = ({
	  value,
	  onChange,
	  onSubmit,
	  children
	}) => {
	# leanpub-start-insert
	  let input;
	# leanpub-end-insert
	  return (
	    <form onSubmit={onSubmit}>
	      <input
	        type="text"
	        value={value}
	        onChange={onChange}
	# leanpub-start-insert
	        ref={(node) => input = node}
	# leanpub-end-insert
	      />
	      <button type="submit">
	        {children}
	      </button>
	    </form>
	  );
	}

Você agora estaria apto a acessar o elemento _input_ do DOM. Não funcionaria para o exemplo com o foco, pois você não tem acesso a métodos de ciclo de vida em um _stateless functional component_ para disparar a ação de foco. Mas, no futuro, você pode se deparar com casos de uso onde faz sentido ter acesso ao atributo `ref` em um componente deste tipo.

### Exercícios

* Leia mais sobre [o uso do atributo ref em React][2]
* Leia mais sobre [o atributo ref, em termos gerais, em React][3]

## _Loading_ ...

Voltemos à aplicação. Você pode querer exibir um indicador de carregamento (_loading_) quando faz uma requisição de busca na API Hacker News.  A requisição é assíncrona e seria bom se você exibisse algum _feedback_ ao usuário, demonstrando que algo ainda está para acontecer. Vamos definir um componente _Loading_, reutilizável, no seu arquivo _src/App.js_.

{title="src/App.js",lang=javascript}
	const Loading = () =>
	  <div>Loading ...</div>

Você precisa agora de uma propriedade para armazenar o estado de _loading_. Baseado neste estado, você pode decidir se exibe ou não o componente _Loading_.

{title="src/App.js",lang=javascript}
	class App extends Component {
	  _isMounted = false;
	
	  constructor(props) {
	    super(props);
	
	    this.state = {
	      results: null,
	      searchKey: '',
	      searchTerm: DEFAULT_QUERY,
	      error: null,
	# leanpub-start-insert
	      isLoading: false,
	# leanpub-end-insert
	    };
	
	    ...
	  }
	
	  ...
	
	}

O valor inicial da propriedade `isLoading` é _false_. Você não carrega nada antes do componente _App_ ser montado.

Quando você faz a requisição, você altera o estado de _loading_ par _true_. Eventualmente, a requisição irá ter sucesso e você poderá alterá-lo novamente para _false_.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  setSearchTopStories(result) {
	    ...
	
	    this.setState({
	      results: {
	        ...results,
	        [searchKey]: { hits: updatedHits, page }
	      },
	# leanpub-start-insert
	      isLoading: false
	# leanpub-end-insert
	    });
	  }
	
	  fetchSearchTopStories(searchTerm, page = 0) {
	# leanpub-start-insert
	    this.setState({ isLoading: true });
	# leanpub-end-insert
	
	    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
	      .then(result => this._isMounted && this.setSearchTopStories(result.data))
	      .catch(error => this._isMounted && this.setState({ error }));
	  }
	
	  ...
	
	}

Como último passo, você irá usar o component _Loading_ em _App_. Uma renderização condicional, baseada no estado de _loading_, irá determinar quando mostrar o componente _Loading_ ou o componente _Button_. Este último é o botão que carrega mais dados.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const {
	      searchTerm,
	      results,
	      searchKey,
	      error,
	# leanpub-start-insert
	      isLoading
	# leanpub-end-insert
	    } = this.state;
	
	    ...
	
	    return (
	      <div className="page">
	        ...
	        <div className="interactions">
	# leanpub-start-insert
	          { isLoading
	            ? <Loading />
	            : <Button
	              onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
	              More
	            </Button>
	          }
	# leanpub-end-insert
	        </div>
	      </div>
	    );
	  }
	}

Inicialmente, o componente _Loading_ irá aparecer quando você inicia a sua aplicação, porque você faz uma requisição em `componentDidMount()`. Não existe componente _Table_, porque a lista está vazia. Quando uma resposta é retornada da chamada à API Hacker News, o resultado é mostrado, o estado de _loading_ é alterado para _false_ e o componente _Loading_ desaparece. No lugar dele, o botão “More” (que obtém mais dados) aparece. Uma vez que você obtém mais dados, o botão irá desaparecer novamente e _Loading_ irá ser exibido.

### Exercícios:

* Use uma biblioteca como [Font Awesome][4] para exibir um ícone de _loading_ no lugar do texto "Loading ..."

## Higher-Order Components

Higher-order components (HOC) are an advanced concept in React. HOCs are an equivalent to higher-order functions. They take any input - most of the time a component, but also optional arguments - and return a component as output. The returned component is an enhanced version of the input component and can be used in your JSX.

HOCs are used for different use cases. They can prepare properties, manage state or alter the representation of a component. One use case could be to use a HOC as a helper for a conditional rendering. Imagine you have a List component that renders a list of items or nothing, because the list is empty or null. The HOC could shield away that the list would render nothing when there is no list. On the other hand, the plain List component doesn't need to bother anymore about an non existent list. It only cares about rendering the list.

Let's do a simple HOC which takes a component as input and returns a component. You can place it in your *src/App.js* file.

{title="src/App.js",lang=javascript}
	function withFoo(Component) {
	  return function(props) {
	    return <Component { ...props } />;
	  }
	}

One neat convention is to prefix the naming of a HOC with `with`. Since you are using JavaScript ES6, you can express the HOC more concisely with an ES6 arrow function.

{title="src/App.js",lang=javascript}
	const withFoo = (Component) => (props) =>
	  <Component { ...props } />

In the example, the input component would stay the same as the output component. Nothing happens. It renders the same component instance and passes all of the props to the output component. But that's useless. Let's enhance the output component. The output component should show the Loading component, when the loading state is true, otherwise it should show the input component. A conditional rendering is a great use case for a HOC.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const withLoading = (Component) => (props) =>
	  props.isLoading
	    ? <Loading />
	    : <Component { ...props } />
	# leanpub-end-insert

Based on the loading property you can apply a conditional rendering. The function will return the Loading component or the input component.

In general it can be very efficient to spread an object, like the props object in the previous example, as input for a component. See the difference in the following code snippet.

{title="Code Playground",lang="javascript"}
	// before you would have to destructure the props before passing them
	const { foo, bar } = props;
	<SomeComponent foo={foo} bar={bar} />
	
	// but you can use the object spread operator to pass all object properties
	<SomeComponent { ...props } />

There is one little thing that you should avoid. You pass all the props including the `isLoading` property, by spreading the object, into the input component. However, the input component may not care about the `isLoading` property. You can use the ES6 rest destructuring to avoid it.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const withLoading = (Component) => ({ isLoading, ...rest }) =>
	  isLoading
	    ? <Loading />
	    : <Component { ...rest } />
	# leanpub-end-insert

It takes one property out of the object, but keeps the remaining object. It works with multiple properties as well. You might have already read about it in the [destructuring assignment][5].

Now you can use the HOC in your JSX. An use case in the application could be to show either the "More" button or the Loading component. The Loading component is already encapsulated in the HOC, but an input component is missing. In the use case of showing a Button component or a Loading component, the Button is the input component of the HOC. The enhanced output component is a ButtonWithLoading component.

{title="src/App.js",lang=javascript}
	const Button = ({
	  onClick,
	  className = '',
	  children,
	}) =>
	  <button
	    onClick={onClick}
	    className={className}
	    type="button"
	  >
	    {children}
	  </button>
	
	const Loading = () =>
	  <div>Loading ...</div>
	
	const withLoading = (Component) => ({ isLoading, ...rest }) =>
	  isLoading
	    ? <Loading />
	    : <Component { ...rest } />
	
	# leanpub-start-insert
	const ButtonWithLoading = withLoading(Button);
	# leanpub-end-insert

Everything is defined now. As a last step, you have to use the ButtonWithLoading component, which receives the loading state as an additional property. While the HOC consumes the loading property, all other props get passed to the Button component.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    ...
	    return (
	      <div className="page">
	        ...
	        <div className="interactions">
	# leanpub-start-insert
	          <ButtonWithLoading
	            isLoading={isLoading}
	            onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
	            More
	          </ButtonWithLoading>
	# leanpub-end-insert
	        </div>
	      </div>
	    );
	  }
	}

When you run your tests again, you will notice that your snapshot test for the App component fails. The diff might look like the following on the command line:

{title="Command Line",lang="text"}
	-    <button
	-      className=""
	-      onClick={[Function]}
	-      type="button"
	-    >
	-      More
	-    </button>
	+    <div>
	+      Loading ...
	+    </div>

You can either fix the component now, when you think there is something wrong about it, or can accept the new snapshot of it. Because you introduced the Loading component in this chapter, you can accept the altered snapshot test on the command line in the interactive test.

Higher-order components are an advanced technique in React. They have multiple purposes like improved reusability of components, greater abstraction, composability of components and manipulations of props, state and view. Don't worry if you don't understand them immediately. It takes time to get used to them.

I encourage you to read the [gentle introduction to higher-order components][6]. It gives you another approach to learn them, shows you an elegant way to use them in a functional programming way and solves specifically the problem of conditional rendering with higher-order components.

### Exercises:

* read [a gentle introduction to higher-order components][7]
* experiment with the HOC you have created
* think about a use case where another HOC would make sense
  * implement the HOC, if there is a use case

## Advanced Sorting

You have already implemented a client- and server-side search interaction. Since you have a Table component, it would make sense to enhance the Table with advanced interactions. What about introducing a sort functionality for each column by using the column headers of the Table?

It would be possible to write your own sort function, but personally I prefer to use a utility library for such cases. [Lodash][8] is one of these utility libraries, but you can use whatever library suits you. Let's install Lodash and use it for the sort functionality.

{title="Command Line",lang="text"}
	npm install lodash

Now you can import the sort functionality of Lodash in your *src/App.js* file.

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	import axios from 'axios';
	# leanpub-start-insert
	import { sortBy } from 'lodash';
	# leanpub-end-insert
	import './App.css';

You have several columns in your Table. There are title, author, comments and points columns. You can define sort functions whereas each function takes a list and returns a list of items sorted by a specific property. Additionally, you will need one default sort function which doesn't sort but only returns the unsorted list. That will be your initial state.

{title="src/App.js",lang=javascript}
	...
	
	# leanpub-start-insert
	const SORTS = {
	  NONE: list => list,
	  TITLE: list => sortBy(list, 'title'),
	  AUTHOR: list => sortBy(list, 'author'),
	  COMMENTS: list => sortBy(list, 'num_comments').reverse(),
	  POINTS: list => sortBy(list, 'points').reverse(),
	};
	# leanpub-end-insert
	
	class App extends Component {
	  ...
	}
	...

You can see that two of the sort functions return a reversed list. That's because you want to see the items with the highest comments and points rather than to see the items with the lowest counts when you sort the list for the first time.

The `SORTS` object allows you to reference any sort function now.

Again your App component is responsible for storing the state of the sort. The initial state will be the initial default sort function, which doesn't sort at all and returns the input list as output.

{title="src/App.js",lang=javascript}
	this.state = {
	  results: null,
	  searchKey: '',
	  searchTerm: DEFAULT_QUERY,
	  error: null,
	  isLoading: false,
	# leanpub-start-insert
	  sortKey: 'NONE',
	# leanpub-end-insert
	};

Once you choose a different `sortKey`, let's say the `AUTHOR` key, you will sort the list with the appropriate sort function from the `SORTS` object.

Now you can define a new class method in your App component that simply sets a `sortKey` to your local component state. Afterward, the `sortKey` can be used to retrieve the sorting function to apply it on your list.

{title="src/App.js",lang=javascript}
	class App extends Component {
	  _isMounted = false;
	
	  constructor(props) {
	
	    ...
	
	    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
	    this.setSearchTopStories = this.setSearchTopStories.bind(this);
	    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
	    this.onSearchSubmit = this.onSearchSubmit.bind(this);
	    this.onSearchChange = this.onSearchChange.bind(this);
	    this.onDismiss = this.onDismiss.bind(this);
	# leanpub-start-insert
	    this.onSort = this.onSort.bind(this);
	# leanpub-end-insert
	  }
	
	  ...
	
	# leanpub-start-insert
	  onSort(sortKey) {
	    this.setState({ sortKey });
	  }
	# leanpub-end-insert
	
	  ...
	
	}

The next step is to pass the method and `sortKey` to your Table component.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const {
	      searchTerm,
	      results,
	      searchKey,
	      error,
	      isLoading,
	# leanpub-start-insert
	      sortKey
	# leanpub-end-insert
	    } = this.state;
	
	    ...
	
	    return (
	      <div className="page">
	        ...
	        <Table
	          list={list}
	# leanpub-start-insert
	          sortKey={sortKey}
	          onSort={this.onSort}
	# leanpub-end-insert
	          onDismiss={this.onDismiss}
	        />
	        ...
	      </div>
	    );
	  }
	}

The Table component is responsible for sorting the list. It takes one of the `SORT` functions by `sortKey` and passes the list as input. Afterward it keeps mapping over the sorted list.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const Table = ({
	  list,
	  sortKey,
	  onSort,
	  onDismiss
	}) =>
	# leanpub-end-insert
	  <div className="table">
	# leanpub-start-insert
	    {SORTS[sortKey](list).map(item =>
	# leanpub-end-insert
	      <div key={item.objectID} className="table-row">
	        ...
	      </div>
	    )}
	  </div>

In theory the list would get sorted by one of the functions. But the default sort is set to `NONE`, so nothing is sorted yet. So far, no one executes the `onSort()` method to change the `sortKey`. Let's extend the Table with a row of column headers that use Sort components in columns to sort each column.

{title="src/App.js",lang=javascript}
	const Table = ({
	  list,
	  sortKey,
	  onSort,
	  onDismiss
	}) =>
	  <div className="table">
	# leanpub-start-insert
	    <div className="table-header">
	      <span style={{ width: '40%' }}>
	        <Sort
	          sortKey={'TITLE'}
	          onSort={onSort}
	        >
	          Title
	        </Sort>
	      </span>
	      <span style={{ width: '30%' }}>
	        <Sort
	          sortKey={'AUTHOR'}
	          onSort={onSort}
	        >
	          Author
	        </Sort>
	      </span>
	      <span style={{ width: '10%' }}>
	        <Sort
	          sortKey={'COMMENTS'}
	          onSort={onSort}
	        >
	          Comments
	        </Sort>
	      </span>
	      <span style={{ width: '10%' }}>
	        <Sort
	          sortKey={'POINTS'}
	          onSort={onSort}
	        >
	          Points
	        </Sort>
	      </span>
	      <span style={{ width: '10%' }}>
	        Archive
	      </span>
	    </div>
	# leanpub-end-insert
	    {SORTS[sortKey](list).map(item =>
	      ...
	    )}
	  </div>

Each Sort component gets a specific `sortKey` and the general `onSort()` function. Internally it calls the method with the `sortKey` to set the specific key.

{title="src/App.js",lang=javascript}
	const Sort = ({ sortKey, onSort, children }) =>
	  <Button onClick={() => onSort(sortKey)}>
	    {children}
	  </Button>

As you can see, the Sort component reuses your common Button component. On a button click each individual passed `sortKey` will get set by the `onSort()` method. Now you should be able to sort the list when you click on the column headers.

There is one minor improvement for an improved look. So far, the button in a column header looks a bit silly. Let's give the button in the Sort component a proper `className`.

{title="src/App.js",lang=javascript}
	const Sort = ({ sortKey, onSort, children }) =>
	# leanpub-start-insert
	  <Button
	    onClick={() => onSort(sortKey)}
	    className="button-inline"
	  >
	# leanpub-end-insert
	    {children}
	  </Button>

It should look nice now. The next goal would be to implement a reverse sort as well. The list should get reverse sorted once you click a Sort component twice. First, you need to define the reverse state with a boolean. The sort can be either reversed or non reversed.

{title="src/App.js",lang=javascript}
	this.state = {
	  results: null,
	  searchKey: '',
	  searchTerm: DEFAULT_QUERY,
	  error: null,
	  isLoading: false,
	  sortKey: 'NONE',
	# leanpub-start-insert
	  isSortReverse: false,
	# leanpub-end-insert
	};

Now in your sort method, you can evaluate if the list is reverse sorted. It is reverse if the `sortKey` in the state is the same as the incoming `sortKey` and the reverse state is not already set to true.

{title="src/App.js",lang=javascript}
	onSort(sortKey) {
	# leanpub-start-insert
	  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
	  this.setState({ sortKey, isSortReverse });
	# leanpub-end-insert
	}

Again you can pass the reverse prop to your Table component.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	    const {
	      searchTerm,
	      results,
	      searchKey,
	      error,
	      isLoading,
	      sortKey,
	# leanpub-start-insert
	      isSortReverse
	# leanpub-end-insert
	    } = this.state;
	
	    ...
	
	    return (
	      <div className="page">
	        ...
	        <Table
	          list={list}
	          sortKey={sortKey}
	# leanpub-start-insert
	          isSortReverse={isSortReverse}
	# leanpub-end-insert
	          onSort={this.onSort}
	          onDismiss={this.onDismiss}
	        />
	        ...
	      </div>
	    );
	  }
	}

The Table has to have an arrow function block body to compute the data now.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const Table = ({
	  list,
	  sortKey,
	  isSortReverse,
	  onSort,
	  onDismiss
	}) => {
	  const sortedList = SORTS[sortKey](list);
	  const reverseSortedList = isSortReverse
	    ? sortedList.reverse()
	    : sortedList;
	
	  return(
	# leanpub-end-insert
	    <div className="table">
	      <div className="table-header">
	        ...
	      </div>
	# leanpub-start-insert
	      {reverseSortedList.map(item =>
	# leanpub-end-insert
	        ...
	      )}
	    </div>
	# leanpub-start-insert
	  );
	}
	# leanpub-end-insert

The reverse sort should work now.

Last but not least, you have to deal with one open question for the sake of an improved user experience. Can a user distinguish which column is actively sorted? So far, it is not possible. Let's give the user a visual feedback.

Each Sort component gets its specific `sortKey` already. It could be used to identify the activated sort. You can pass the `sortKey` from the internal component state as active sort key to your Sort component.

{title="src/App.js",lang=javascript}
	const Table = ({
	  list,
	  sortKey,
	  isSortReverse,
	  onSort,
	  onDismiss
	}) => {
	  const sortedList = SORTS[sortKey](list);
	  const reverseSortedList = isSortReverse
	    ? sortedList.reverse()
	    : sortedList;
	
	  return(
	    <div className="table">
	      <div className="table-header">
	        <span style={{ width: '40%' }}>
	          <Sort
	            sortKey={'TITLE'}
	            onSort={onSort}
	# leanpub-start-insert
	            activeSortKey={sortKey}
	# leanpub-end-insert
	          >
	            Title
	          </Sort>
	        </span>
	        <span style={{ width: '30%' }}>
	          <Sort
	            sortKey={'AUTHOR'}
	            onSort={onSort}
	# leanpub-start-insert
	            activeSortKey={sortKey}
	# leanpub-end-insert
	          >
	            Author
	          </Sort>
	        </span>
	        <span style={{ width: '10%' }}>
	          <Sort
	            sortKey={'COMMENTS'}
	            onSort={onSort}
	# leanpub-start-insert
	            activeSortKey={sortKey}
	# leanpub-end-insert
	          >
	            Comments
	          </Sort>
	        </span>
	        <span style={{ width: '10%' }}>
	          <Sort
	            sortKey={'POINTS'}
	            onSort={onSort}
	# leanpub-start-insert
	            activeSortKey={sortKey}
	# leanpub-end-insert
	          >
	            Points
	          </Sort>
	        </span>
	        <span style={{ width: '10%' }}>
	          Archive
	        </span>
	      </div>
	      {reverseSortedList.map(item =>
	          ...
	      )}
	    </div>
	  );
	}

Now in your Sort component, you know based on the `sortKey` and `activeSortKey` whether the sort is active. Give your Sort component an extra `className` attribute, in case it is sorted, to give the user a visual feedback.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const Sort = ({
	  sortKey,
	  activeSortKey,
	  onSort,
	  children
	}) => {
	  const sortClass = ['button-inline'];
	
	  if (sortKey === activeSortKey) {
	    sortClass.push('button-active');
	  }
	
	  return (
	    <Button
	      onClick={() => onSort(sortKey)}
	      className={sortClass.join(' ')}
	    >
	      {children}
	    </Button>
	  );
	}
	# leanpub-end-insert

The way to define the `sortClass` is a bit clumsy, isn't it? There is a neat little library to get rid of this. First you have to install it.

{title="Command Line",lang="text"}
	npm install classnames

And second you have to import it on top of your *src/App.js* file.

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	import axios from 'axios';
	import { sortBy } from 'lodash';
	# leanpub-start-insert
	import classNames from 'classnames';
	# leanpub-end-insert
	import './App.css';

Now you can use it to define your component `className` with conditional classes.

{title="src/App.js",lang=javascript}
	const Sort = ({
	  sortKey,
	  activeSortKey,
	  onSort,
	  children
	}) => {
	# leanpub-start-insert
	  const sortClass = classNames(
	    'button-inline',
	    { 'button-active': sortKey === activeSortKey }
	  );
	# leanpub-end-insert
	
	  return (
	# leanpub-start-insert
	    <Button
	      onClick={() => onSort(sortKey)}
	      className={sortClass}
	    >
	# leanpub-end-insert
	      {children}
	    </Button>
	  );
	}

Again, when you run your tests, you should see failing snapshot tests but also failing unit tests for the Table component. Since you changed again your component representations, you can accept the snapshot tests. But you have to fix the unit test. In your *src/App.test.js* file, you need to provide a `sortKey` and the `isSortReverse` boolean for the Table component.

{title="src/App.test.js",lang=javascript}
	...
	
	describe('Table', () => {
	
	  const props = {
	    list: [
	      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
	      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
	    ],
	# leanpub-start-insert
	    sortKey: 'TITLE',
	    isSortReverse: false,
	# leanpub-end-insert
	  };
	
	  ...
	
	});

Once again you might need to accept the failing snapshot tests for your Table component, because you provided extended props for the Table component.

Finally your advanced sort interaction is complete now.

### Exercises:

* use a library like [Font Awesome][9] to indicate the (reverse) sort
  * it could be an arrow up or arrow down icon next to each Sort header
* read more about the [classnames library][10]

{pagebreak}

You have learned advanced component techniques in React! Let's recap the last chapters:

* React
  * the ref attribute to reference DOM nodes
  * higher-order components are a common way to build advanced components
  * implementation of advanced interactions in React
  * conditional classNames with a neat helper library
* ES6
  * rest destructuring to split up objects and arrays

You can find the source code in the [official repository][11].

[1]:	https://d3js.org/
[2]:	https://www.robinwieruch.de/react-ref-attribute-dom-node/
[3]:	https://reactjs.org/docs/refs-and-the-dom.html
[4]:	http://fontawesome.io/
[5]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
[6]:	https://www.robinwieruch.de/gentle-introduction-higher-order-components/
[7]:	https://www.robinwieruch.de/gentle-introduction-higher-order-components/
[8]:	https://lodash.com/
[9]:	http://fontawesome.io/
[10]:	https://github.com/JedWatson/classnames
[11]:	https://github.com/the-road-to-learn-react/hackernews-client/tree/5.5