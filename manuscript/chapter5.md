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

Como último passo, você irá usar o componente _Loading_ em _App_. Uma renderização condicional, baseada no estado de _loading_, irá determinar quando mostrar o componente _Loading_ ou o componente _Button_. Este último é o botão que carrega mais dados.

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

_Higher-order components_ (HOC) são um conceito avançado em React. HOCs são equivalentes a _higher-order functions_, pois recebem qualquer coisa como parâmetro de entrada - na maior parte do tempo um componente, mas também argumentos opcionais - e retornam um componente. O componente retornado é uma versão aprimorada do componente de entrada e pode ser usado em seu código JSX.

HOCs são utilizados em diferentes casos. Eles podem podem preparar propriedades, gerenciar estado ou alterar a representação de um componente. Um dos casos poderia ser o de utilizar o HOC como um utilitário para renderização condicional. Imagine que você tem um componente _List_, que renderiza ou uma lista de itens, ou nada, quando a lista é vazia ou tem valor _null_. O HOC poderia tratar e evitar o caso em que a lista não existe, fazendo com que o componente _List_ não precise mais se preocupar com isso, focando apenas em renderizar a lista.

Vamos criar um HOC simples, que recebe um componente como entrada e retorna um outro componente. Você pode colocá-lo em seu arquivo *src/App.js*.

{title="src/App.js",lang=javascript}
  function withFoo(Component) {
    return function(props) {
      return <Component { ...props } />;
    }
  }

Existe a convenção de prefixar o nome de HOCs com `with`. Uma vez que você usa JavaScript ES6, pode expressar o HOC de forma mais concisa com uma _arrow function_.

{title="src/App.js",lang=javascript}
  const withFoo = (Component) => (props) =>
    <Component { ...props } />

Neste exemplo, o componente de entrada é igual ao de saída. Nada acontece, por enquanto ele renderiza a mesma instância de componente e repassa todas as _props_ para o componente de saída, o que é inútil. Vamos então aprimorar o componente retornado, que deveria mostrar o componente _Loading_ quando o estado de _loading_ é _true_. Caso contrário, deveria mostrar o componente que recebeu como entrada. Uma renderização condicional é um grande caso de uso para um HOC.

{title="src/App.js",lang=javascript}
  # leanpub-start-insert
  const withLoading = (Component) => (props) =>
    props.isLoading
      ? <Loading />
      : <Component { ...props } />
  # leanpub-end-insert

Baseado na propriedade de _loading_, você pode aplicar a renderização condicional. A função irá retornar o componente _Loading_ ou o componente que recebeu via parâmetro.

Em geral, utilizar o operador _spread_ em um objeto pode ser bastante eficiente, como aqui com o objeto _props_. Veja a diferença no seguinte trecho de código:

{title="Code Playground",lang="javascript"}
  // antes, você teria que utilizar destructuring com as props, antes de passá-las
  const { foo, bar } = props;
  <SomeComponent foo={foo} bar={bar} />
  
  // você pode, no entanto, utilizar o operador spread para passar todas as propriedades do objeto
  <SomeComponent { ...props } />

Existe mais uma pequena coisa que você deveria evitar. Você passa todas as propriedades para o componente de entrada, incluindo `isLoading`, utilizando operador _spread_ no objeto. Contudo, o componente de _input_ poderia ignorar a propriedade `isLoading`. Você pode utilizar _rest destructuring_, de JavaScript ES6, para evitar isso.

{title="src/App.js",lang=javascript}
  # leanpub-start-insert
  const withLoading = (Component) => ({ isLoading, ...rest }) =>
    isLoading
      ? <Loading />
      : <Component { ...rest } />
  # leanpub-end-insert

Note que agora uma propriedade é extraída explicitamente do objeto, mantendo as outras agrupadas. Isto também funcionaria para múltiplas propriedades, inclusive. Você já deve ter lido mais à respeito de [destructuring][5].

Você agora pode usar o HOC em seu código JSX, no caso de exibir ou o botão “More”, ou o componente _Loading_. Este último já foi encapsulado no HOC, mas falta o componente de entrada. No caso de uso de exibir um componente _Button_ ou um componente _Loading_, _Button_ é o componente de entrada do HOC. A saída aprimorada é um componente _ButtonWithLoading_.

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

Finalmente, tudo foi bem definido. Como último passo, você deve utilizar o componente _ButtonWithLoading_, que recebe o estado de _loading_ como uma propriedade extra. Enquanto o HOC explicitamente usa esta propriedade, ele repassa todas as outras para o componente _Button_.

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

Quando você rodar seus testes novamente, irá notar que o _snapshot test_ para o componente _App_ falha. O _diff_ deverá aparentar mais ou menos como mostrado a seguir, na linha de comando:

{title=“Linha de Comando",lang="text"}
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

Mais uma vez, você tem a oportunidade consertar algo que possa estar errado no componente, ou aceitar o novo _snapshot_. Uma vez que você introduziu o componente _Loading_ neste capítulo, você deve aceitá-lo na interface interativa apresentada na linha de comando após o teste.

O uso de _higher-order components_ é uma técnica avançada em React. Eles têm múltiplos propósitos, como reuso, maior abstração, melhor composição de componentes e manipulação de _props_, estado e das suas _views_. Não se preocupe caso não entenda de imediato a utilização de HOCs. Leva algum tempo para se acostumar à eles.

Eu lhe encorajo a ler essa [gentil introdução a _higher-order components_][6]. Ela lhe provê outra abordagem para aprender, mostrando um jeito elegante de usá-los com um paradigma de programação funcional e resolvendo especificamente o problema de renderização condicional com _higher-order components_.

### Exercícios:

* Leia o artigo [A gentle introduction to higher-order components][7]
* Faça experiências com o HOC que você criou
* Pense a respeito de algum caso onde outro HOC poderia fazer sentido
  * Implemente o HOC, se este caso existir

## Ordenação Avançada

Você já implementou interações de busca do lado do cliente e do servidor. Uma vez que você tem um componente _Table_, faria sentido enriquecê-lo com interações mais avançadas. Que tal introduzir uma funcionalidade de ordenação (_sort_) para cada coluna, usando os seus cabeçalhos?

Seria possível escrever sua própria função de ordenação, mas eu pessoalmente prefiro usar uma biblioteca utilitária para tais casos. [Lodash][8] é uma dessas bibliotecas, mas você pode utilizar qualquer uma que lhe convir. Vamos instalar _Lodash_ e usá-la para a funcionalidade de ordenação.

{title=“Linha de Comando“,lang="text"}
  npm install lodash

Você pode agora importar a funcionalidade _sortBy_ de _Lodash_ em seu arquivo *src/App.js*.

{title="src/App.js",lang=javascript}
  import React, { Component } from 'react';
  import axios from 'axios';
  # leanpub-start-insert
  import { sortBy } from 'lodash';
  # leanpub-end-insert
  import './App.css';

Existem várias colunas em _Table_. Título, autor, comentários e pontos (_title, author, comments _e _points_). Você pode definir funções de ordenação, onde cada uma recebe uma lista e retorna uma lista ordenada para uma propriedade específica. Em adição a isto, você irá precisar uma função _default_, que não faz nenhuma ordenação, apenas retorna a lista desordenada. Este será o seu estado inicial.

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

Como você pode ver, duas das funções retornam uma lista inversamente ordenada. O motivo é que você desejará ver os items com o maior número de comentários ou de pontos no topo, ao invés de ver os valores mais baixos quando realizar a ordenação da lista.

O objeto `SORTS` lhe permite fazer referência a qualquer função de ordenação agora.

Mais uma vez, seu componente _App_ é responsável por armazenar o estado da operação _sort_. O estado inicial será o _default_, que acaba não ordenando nada e devolvendo a mesma lista como saída.

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

Quando você escolher uma `sortKey` diferente, digamos a chave `AUTHOR`, você irá ordenar a lista com a função mais apropriada do objeto `SORTS`.

Você pode agora definir um novo método de classe no seu componente _App_, que simplesmente define `sortKey` no estado local do seu componente. A `sortKey` poderá ser utilizada para obter a função de ordenação a ser aplicada em sua lista.

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

O próximo passo é passar o método e `sortKey` para seu componente _Table_.

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

O componente _Table_ é responsável por ordenar a lista. Ele recebe uma das funções `SORT` através da `sortKey` e passa a lista como entrada. Ele então continua iterando sobre a lista ordenada com a função _map_.

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

Em teoria, a lista deveria ser ordenada por alguma das funções. Mas, o valor _default_ é `NONE`, logo nada acontece ainda. Até agora, ninguém executa o método `onSort()` para mudar `sortKey`. Iremos estender a tabela com uma linha de cabeçalhos de coluna, que usa componentes _Sort_ para ordenar a lista em cada uma.

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

Cada componente _Sort_ recebe uma `sortKey` específica e a função `onSort()`. Internamente, ele chama o método com a `sortKey` para definir a chave específica.

{title="src/App.js",lang=javascript}
  const Sort = ({ sortKey, onSort, children }) =>
    <Button onClick={() => onSort(sortKey)}>
      {children}
    </Button>

Como você pode ver, o componente _Sort_ reutiliza seu componente genérico _Button_. Quando o botão for clicado, a `sortKey` passada irá ser definida com a ajuda do método `onSort()`. Agora você deve ser capaz de ordenar a lista quando clicar nos cabeçalhos das colunas.

Ainda é possível fazer uma pequena melhoria, para um visual melhor. Até então, o botão no _header_ da coluna parece um pouco bobo. Vamos dar ao botão no componente _Sort_ um `className` mais apropriado.

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

Deve parecer melhor agora. O próximo objetivo seria o de também implementar a ordenação inversa. A ordenação deveria ser invertida uma vez que você novamente clica no componente _Sort_. Primeiro, você precisa definir o estado booleano _reverse_. A ordenação poderá ser inversa, ou não.

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

Agora o seu, em seu método de ordenação, você pode verificar se a lista terá a ordenação invertida. Isto irá acontecer se o estado local `sortKey` tiver o mesmo valor que a `sortKey` recebida e o estado `isSortReverse` não já estiver definido com o valor _true_.

{title="src/App.js",lang=javascript}
  onSort(sortKey) {
  # leanpub-start-insert
    const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
    this.setState({ sortKey, isSortReverse });
  # leanpub-end-insert
  }

Novamente, você pode passar a _prop_ de ordenação invertida para o componente _Table_.

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

_Table_ precisará ter uma um bloco de escopo de função explícito, para computar os dados agora.

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

A ordenação invertida deverá funcionar.

Por último, mas não menos importante, você deverá lidar com uma questão de melhoria de experiência do usuário. Poderia o usuário distinguir por qual coluna está sendo feita a ordenação no momento? Até agora, isto não é possível. Vamos fornecer este _feedback_ visual.

Cada componente _Sort_ já recebe sua `sortKey` específica. Ela poderia ser utilizada par identificar a ordenação corrente. Você pode passar a `sortKey` do estado interno do componente como a chave de ordenação ativa para seu componente de Sort.

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

Agora, no seu componente _Sort_, você sabe qual ordenação está ativa, baseado na `sortKey` e na `activeSortKey`. Dê ao seu componente _Sort_ um atributo extra `className`, para o caso dele estar ativo, provendo assim um _feedback_ visual ao usuário.

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

A forma como definimos `sortClass` é um pouco bizarra, não acha? Existe uma pequena biblioteca que nos ajuda a consertar isto. Primeiro, você tem que instalá-la.

{title="Linha de Comando",lang="text"}
  npm install classnames

Segundo, você precisa importá-la no topo do arquivo *src/App.js*.

{title="src/App.js",lang=javascript}
  import React, { Component } from 'react';
  import axios from 'axios';
  import { sortBy } from 'lodash';
  # leanpub-start-insert
  import classNames from 'classnames';
  # leanpub-end-insert
  import './App.css';

Agora será possível usá-la para definir o `className` do seu componente com classes condicionais.

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

Novamente, quando você rodar os seus testes, verá _snapshot tests_ e agora também testes unitários falhando para o componente _Table_. Uma vez que você mudou novamente a representação do componente, pode acatar as mudanças de _snapshot_. Mas, você precisa consertar o teste unitário. Em seu arquivo *src/App.test.js*, você precisa fornecer uma `sortKey` e o valor booleano `isSortReverse` para o componente _Table_.

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

Mais uma vez, você precisará aceitar as mudanças de _snapshot_ para o componente _Table_, porque você forneceu novas _props_ para ele.

Finalmente, sua interação avançada de ordenação está completa.

### Exercícios:

* Use uma biblioteca como [Font Awesome][9] para indicar a ordenação (inversa ou não)
  * Poderia ser um ícone de seta para cima ou para baixo, ao lado do cabeçalho _Sort_
* Leia mais sobre a [biblioteca classnames][10]

{pagebreak}

Você aprendeu técnicas avançadas em componentes React! Vamos recapitular:

* React
  * o atributo _ref_ para referenciar _DOM nodes_
  * _higher-order components_ são um jeito comum de construir componentes avançados
  * implementação de interações avançadas em React
  * _classNames_ condicionais com uma biblioteca utilitária elegante
* ES6
  * uso de _rest destructuring_ para separar objetos e _arrays_

Você irá encontrar o código fonte no [repositório oficial][11].

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