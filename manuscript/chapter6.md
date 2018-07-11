# Gerenciamento de Estado em React (e além)

Você já aprendeu o básico sobre gerenciamento de estado em React nos capítulos anteriores. Este capítulo “cava” um pouco mais fundo neste tópico. Você aprenderá as melhores práticas, como aplicá-las e por que você deveria considerar utilizar uma biblioteca de terceiros para gerenciar o estado de uma aplicação.

## Realocando o Estado

Apenas _App_ é um componente _stateful_ (com estado) em sua aplicação. Ele lida com um bastante estado e lógica em seus métodos de classe. Talvez você tenha notado que você passa muitas propriedades para o componente _Table_ e muitas delas só são utilizadas nele mesmo. Alguém pode afirmar, então, que não faz sentido que _App_ tenha conhecimento sobre elas.

A funcionalidade de ordenação, como um todo, só é utilizada no componente _Table_. Você pode movê-la para ele, _App_ não precisa dela para nada. O processo de refatoração onde um pedaço do estado é movido de um componente para o outro é conhecido como realocação de estado (_lifting state_). No seu caso, você quer mover o estado que não é utilizado no componente _App_ para o componente _Table_. O estado move-se para baixo, do componente pai para o filho.

Com o intuito de lidar com estado local e métodos de classe no componente _Table_, ele deverá se tornar um componente de classe ES6. A refatoração de um _stateless functional component_ para um componente de classe é um processo simples.

_Table_ como um _stateless functional component_:

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
	    ...
	  );
	}

_Table_ como um componente de classe ES6:

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	class Table extends Component {
	  render() {
	    const {
	      list,
	      sortKey,
	      isSortReverse,
	      onSort,
	      onDismiss
	    } = this.props;
	
	    const sortedList = SORTS[sortKey](list);
	    const reverseSortedList = isSortReverse
	      ? sortedList.reverse()
	      : sortedList;
	
	    return (
	      ...
	    );
	  }
	}
	# leanpub-end-insert

Uma vez que você deseja trabalhar com estado e métodos no seu componentes, precisa adicionar também um construtor e o estado inicial.

{title="src/App.js",lang=javascript}
	class Table extends Component {
	# leanpub-start-insert
	  constructor(props) {
	    super(props);
	
	    this.state = {};
	  }
	# leanpub-end-insert
	
	  render() {
	    ...
	  }
	}

Você pode agora mover o estado e os métodos de classe relacionados com a funcionalidade de ordenação do componente _App_ para o componente _Table_.

{title="src/App.js",lang=javascript}
	class Table extends Component {
	  constructor(props) {
	    super(props);
	
	# leanpub-start-insert
	    this.state = {
	      sortKey: 'NONE',
	      isSortReverse: false,
	    };
	
	    this.onSort = this.onSort.bind(this);
	# leanpub-end-insert
	  }
	
	# leanpub-start-insert
	  onSort(sortKey) {
	    const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
	    this.setState({ sortKey, isSortReverse });
	  }
	# leanpub-end-insert
	
	  render() {
	    ...
	  }
	}

Não se esqueça de remover o estado que foi movido e o método `onSort()` do componente _App_.

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
	      isLoading: false,
	    };
	
	    this.setSearchTopStories = this.setSearchTopStories.bind(this);
	    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
	    this.onDismiss = this.onDismiss.bind(this);
	    this.onSearchSubmit = this.onSearchSubmit.bind(this);
	    this.onSearchChange = this.onSearchChange.bind(this);
	    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
	  }
	
	  ...
	
	}

Além disso, você pode fazer com que a API do componente _Table_ fique mais enxuta. Remova as _props_ que não são mais utilizadas, pois são valores tratados internamente no componente agora.

{title="src/App.js",lang=javascript}
	class App extends Component {
	
	  ...
	
	  render() {
	# leanpub-start-insert
	    const {
	      searchTerm,
	      results,
	      searchKey,
	      error,
	      isLoading
	    } = this.state;
	# leanpub-end-insert
	
	    ...
	
	    return (
	      <div className="page">
	        ...
	        { error
	          ? <div className="interactions">
	            <p>Something went wrong.</p>
	          </div>
	          : <Table
	# leanpub-start-insert
	            list={list}
	            onDismiss={this.onDismiss}
	# leanpub-end-insert
	          />
	        }
	        ...
	      </div>
	    );
	  }
	}

Agora, você pode utilizar método `onSort()` e o estado interno no seu componente _Table_.

{title="src/App.js",lang=javascript}
	class Table extends Component {
	
	  ...
	
	  render() {
	# leanpub-start-insert
	    const {
	      list,
	      onDismiss
	    } = this.props;
	
	    const {
	      sortKey,
	      isSortReverse,
	    } = this.state;
	# leanpub-end-insert
	
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
	# leanpub-start-insert
	              onSort={this.onSort}
	# leanpub-end-insert
	              activeSortKey={sortKey}
	            >
	              Title
	            </Sort>
	          </span>
	          <span style={{ width: '30%' }}>
	            <Sort
	              sortKey={'AUTHOR'}
	# leanpub-start-insert
	              onSort={this.onSort}
	# leanpub-end-insert
	              activeSortKey={sortKey}
	            >
	              Author
	            </Sort>
	          </span>
	          <span style={{ width: '10%' }}>
	            <Sort
	              sortKey={'COMMENTS'}
	# leanpub-start-insert
	              onSort={this.onSort}
	# leanpub-end-insert
	              activeSortKey={sortKey}
	            >
	              Comments
	            </Sort>
	          </span>
	          <span style={{ width: '10%' }}>
	            <Sort
	              sortKey={'POINTS'}
	# leanpub-start-insert
	              onSort={this.onSort}
	# leanpub-end-insert
	              activeSortKey={sortKey}
	            >
	              Points
	            </Sort>
	          </span>
	          <span style={{ width: '10%' }}>
	            Archive
	          </span>
	        </div>
	        { reverseSortedList.map((item) =>
	          ...
	        )}
	      </div>
	    );
	  }
	}

Sua aplicação deve continuar funcionando. Mas, você fez uma refatoração crucial, movendo funcionalidade e estado para outro componente. Outros componentes ficaram menos pesados, assim como a assinatura do componente _Table_, pois ele lida internamente com a funcionalidade de ordenação.

O processo de realocação de estado pode acontecer no sentido oposto, da mesma forma: de um componente filho, para componente pai. Podemos chamar este processo de “promoção de estado” (_lifting state up_). Imagine que você estava lidando com o estado interno em um componente filho. Agora, você precisa atender à um requisito exibindo o estado também no componente pai. Você teria que promover este estado para o componente pai. Mas, iremos mais além. Imagine que você deseja exibir o mesmo estado em componentes “irmãos” (dois componentes com o mesmo componente pai). Mais uma vez, você terá que promover o estado para o componente pai, que irá tratar dele e expô-lo para ambos os componentes filhos.

### Exercícios:

* Leia mais sobre [realocação de estado em React][1]
* Leia mais sobre realocação do estado em [aprenda React antes de utilizar Redux][2]

## Revisitando: setState()

So far, you have used React `setState()` to manage your internal component state. You can pass an object to the function where you can update partially the internal state.

{title="Code Playground",lang="javascript"}
	this.setState({ foo: bar });

But `setState()` doesn't take only an object. In its second version, you can pass a function to update the state.

{title="Code Playground",lang="javascript"}
	this.setState((prevState, props) => {
	  ...
	});

Why should you want to do that? There is one crucial use case where it makes sense to use a function over an object. It is when you update the state depending on the previous state or props. If you don't use a function, the internal state management can cause bugs.

But why does it cause bugs to use an object over a function when the update depends on the previous state or props? The React `setState()` method is asynchronous. React batches `setState()` calls and executes them eventually. It can happen that the previous state or props changed in between when you would rely on it in your `setState()` call.

{title="Code Playground",lang="javascript"}
	const { fooCount } = this.state;
	const { barCount } = this.props;
	this.setState({ count: fooCount + barCount });

Imagine that `fooCount` and `barCount`, thus the state or the props, change somewhere else asynchronously when you call `setState()`. In a growing application, you have more than one `setState()` call across your application. Since `setState()` executes asynchronously, you could rely in the example on stale values.

With the function approach, the function in `setState()` is a callback that operates on the state and props at the time of executing the callback function. Even though `setState()` is asynchronous, with a function it takes the state and props at the time when it is executed.

{title="Code Playground",lang="javascript"}
	this.setState((prevState, props) => {
	  const { fooCount } = prevState;
	  const { barCount } = props;
	  return { count: fooCount + barCount };
	});

Now, lets get back to your code to fix this behavior. Together we will fix it for one place where `setState()` is used and relies on the state or props. Afterward, you are able to fix it at other places too.

The `setSearchTopStories()` method relies on the previous state and thus is a perfect example to use a function over an object in `setState()`. Right now, it looks like the following code snippet.

{title="src/App.js",lang=javascript}
	setSearchTopStories(result) {
	  const { hits, page } = result;
	  const { searchKey, results } = this.state;
	
	  const oldHits = results && results[searchKey]
	    ? results[searchKey].hits
	    : [];
	
	  const updatedHits = [
	    ...oldHits,
	    ...hits
	  ];
	
	  this.setState({
	    results: {
	      ...results,
	      [searchKey]: { hits: updatedHits, page }
	    },
	    isLoading: false
	  });
	}

You extract values from the state, but update the state depending on the previous state asynchronously. Now you can use the functional approach to prevent bugs because of a stale state.

{title="src/App.js",lang=javascript}
	setSearchTopStories(result) {
	  const { hits, page } = result;
	
	# leanpub-start-insert
	  this.setState(prevState => {
	    ...
	  });
	# leanpub-end-insert
	}

You can move the whole block that you have already implemented into the function. You only have to exchange that you operate on the `prevState` rather than `this.state`.

{title="src/App.js",lang=javascript}
	setSearchTopStories(result) {
	  const { hits, page } = result;
	
	  this.setState(prevState => {
	# leanpub-start-insert
	    const { searchKey, results } = prevState;
	
	    const oldHits = results && results[searchKey]
	      ? results[searchKey].hits
	      : [];
	
	    const updatedHits = [
	      ...oldHits,
	      ...hits
	    ];
	
	    return {
	      results: {
	        ...results,
	        [searchKey]: { hits: updatedHits, page }
	      },
	      isLoading: false
	    };
	# leanpub-end-insert
	  });
	}

That will fix the issue with a stale state. There is one more improvement. Since it is a function, you can extract the function for an improved readability. That's one more advantage to use a function over an object. The function can live outside of the component. But you have to use a higher-order function to pass the result to it. After all, you want to update the state based on the fetched result from the API.

{title="src/App.js",lang=javascript}
	setSearchTopStories(result) {
	  const { hits, page } = result;
	  this.setState(updateSearchTopStoriesState(hits, page));
	}

The `updateSearchTopStoriesState()` function has to return a function. It is a higher-order function. You can define this higher-order function outside of your App component. Note how the function signature changes slightly now.

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const updateSearchTopStoriesState = (hits, page) => (prevState) => {
	  const { searchKey, results } = prevState;
	
	  const oldHits = results && results[searchKey]
	    ? results[searchKey].hits
	    : [];
	
	  const updatedHits = [
	    ...oldHits,
	    ...hits
	  ];
	
	  return {
	    results: {
	      ...results,
	      [searchKey]: { hits: updatedHits, page }
	    },
	    isLoading: false
	  };
	};
	# leanpub-end-insert
	
	class App extends Component {
	  ...
	}

That's it. The function over an object approach in `setState()` fixes potential bugs yet increases readability and maintainability of your code. Furthermore, it becomes testable outside of the App component. You could export it and write a test for it as exercise.

### Exercise:

* read more about [React using state correctly][3]
* export updateSearchTopStoriesState from the file
 * write a test for it which passes the a payload (hits, page) and a made up previous state and finally expect a new state
* refactor your `setState()` methods to use a function
  * but only when it makes sense, because it relies on props or state
* run your tests again and verify that everything is up to date

## Taming the State

The previous chapters have shown you that state management can be a crucial topic in larger applications. In general, not only React but a lot of SPA frameworks struggle with it. Applications got more complex in the recent years. One big challenge in web applications nowadays is to tame and control the state.

Compared to other solutions, React already made a big step forward. The unidirectional data flow and a simple API to manage state in a component are indispensable. These concepts make it easier to reason about your state and your state changes. It makes it easier to reason about it on a component level and to a certain degree on an application level.

In a growing application, it gets harder to reason about state changes. You can introduce bugs by operating on stale state when using an object over a function in `setState()`. You have to lift state around to share necessary or hide unnecessary state across components. It can happen that a component needs to lift up state, because its sibling component depends on it. Perhaps the component is far away in the component tree and thus you have to share the state across the whole component tree. In conclusion components get involved to a greater extent in state management. But after all, the main responsibility of components should be representing the UI, shouldn't it?

Because of all these reasons, there exist standalone solutions to take care of the state management. These solutions are not only used in React. However, that's what makes the React ecosystem such a powerful place. You can use different solutions to solve your problems. To address the problem of scaling state management, you might have heard of the libraries [Redux][4] or [MobX][5]. You can use either of these solutions in a React application. They come with extensions, [react-redux][6] and [mobx-react][7], to integrate them into the React view layer.

Redux and MobX are outside of the scope of this book. When you have finished the book, you will get guidance on how you can continue to learn React and its ecosystem. One learning path could be to learn Redux. Before you dive into the topic of external state management, I can recommend to read this [article][8]. It aims to give you a better understanding of how to learn external state management.

### Exercises:

* read more about [external state management and how to learn it][9]
* check out my second ebook about [state management in React][10]

{pagebreak}

You have learned advanced state management in React! Let's recap the last chapters:

* React
  * lift state management up and down to suitable components
  * setState can use a function to prevent stale state bugs
  * existing external solutions that help you to tame the state

You can find the source code in the [official repository][11].

[1]:	https://reactjs.org/docs/lifting-state-up.html
[2]:	https://www.robinwieruch.de/learn-react-before-using-redux/
[3]:	https://reactjs.org/docs/state-and-lifecycle.html#using-state-correctly
[4]:	http://redux.js.org/docs/introduction/
[5]:	https://mobx.js.org/
[6]:	https://github.com/reactjs/react-redux
[7]:	https://github.com/mobxjs/mobx-react
[8]:	https://www.robinwieruch.de/redux-mobx-confusion/
[9]:	https://www.robinwieruch.de/redux-mobx-confusion/
[10]:	https://roadtoreact.com/
[11]:	https://github.com/the-road-to-learn-react/hackernews-client/tree/5.6