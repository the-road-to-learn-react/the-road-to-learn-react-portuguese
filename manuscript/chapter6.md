# Gerenciamento de Estado em React (e além)

Você já aprendeu o básico sobre gerenciamento de estado em React nos capítulos anteriores. Este capítulo “cava” um pouco mais fundo neste tópico. Você aprenderá as melhores práticas, como aplicá-las e por que você deveria considerar utilizar uma biblioteca de terceiros para gerenciar o estado de uma aplicação.

## Realocando o Estado

Apenas _App_ é um componente _stateful_ (com estado) em sua aplicação. Ele lida com um bastante estado e lógica em seus métodos de classe. Talvez você tenha notado que você passa muitas propriedades para o componente _Table_ e muitas delas só são utilizadas nele mesmo. Podemos afirmar, então, que não faz sentido que _App_ tenha conhecimento sobre elas.

A funcionalidade de ordenação, como um todo, só é utilizada no componente _Table_. Você pode movê-la para ele, _App_ não precisa dela para nada. O processo de refatoração onde um pedaço do estado é movido de um componente para o outro é conhecido como realocação de estado (_lifting state_). No seu caso, você quer mover o estado que não é utilizado no componente _App_ para o componente _Table_. O estado move-se para baixo, do componente pai para o filho.

Com o intuito de lidar com estado local e métodos de classe no componente _Table_, ele deverá se tornar um componente de classe ES6. A refatoração de um _stateless functional component_ para um componente de classe é um processo simples.

_Table_ como um _stateless functional component_:

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

_Table_ como um componente de classe ES6:

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Uma vez que você deseja trabalhar com estado e métodos no seu componentes, precisa adicionar também um construtor e o estado inicial.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Você pode agora mover o estado e os métodos de classe relacionados com a funcionalidade de ordenação do componente _App_ para o componente _Table_.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Não se esqueça de remover o estado que foi movido e o método `onSort()` do componente _App_.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Além disso, você pode fazer com que a API do componente _Table_ fique mais enxuta. Remova as _props_ que não são mais utilizadas, pois são valores tratados internamente no componente agora.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Agora, você pode utilizar método `onSort()` e o estado interno no seu componente _Table_.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Sua aplicação deve continuar funcionando. Mas, você fez uma refatoração crucial, movendo funcionalidade e estado para outro componente. Outros componentes ficaram menos pesados, assim como a assinatura do componente _Table_, pois ele lida internamente com a funcionalidade de ordenação.

O processo de realocação de estado pode acontecer no sentido oposto, da mesma forma: de um componente filho, para componente pai. Podemos chamar este processo de “promoção de estado” (_lifting state up_). Imagine que você estava lidando com o estado interno em um componente filho. Agora, você precisa atender à um requisito exibindo o estado também no componente pai. Você teria que promover este estado para o componente pai. Mas, iremos mais além. Imagine que você deseja exibir o mesmo estado em componentes “irmãos” (dois componentes com o mesmo componente pai). Mais uma vez, você terá que promover o estado para o componente pai, que irá tratar dele e expô-lo para ambos os componentes filhos.

### Exercícios:

* Leia mais sobre [realocação de estado em React][1]
* Leia mais sobre realocação do estado em [aprenda React antes de utilizar Redux][2]

## Revisitando: setState()

Até então, você utilizou `setState()` para gerenciar o estado interno de componentes. Você pode passar um objeto para a função, onde o estado interno pode ser parcialmente atualizado.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState({ foo: bar });
~~~~~~~~

Mas, `setState()` não recebe apenas um objeto. Em uma segunda versão, você pode passar uma função que atualiza o estado.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  ...
});
~~~~~~~~

Por que você iria querer isto? Em um caso de uso crítico, onde faz sentido usar uma função ao invés de um objeto. É quando você atualiza o estado dependendo do estado ou de _props_ anteriores. Se não fizer desta forma, podem ocorrer _bugs_ relacionados ao gerenciamento interno do estado.

Mas e por que utilizar um objeto no lugar de uma função causaria _bugs_ quando há dependência de estado e _props_ anteriores? Porque o método `setState()` de React é assíncrono. React processa as chamadas de `setState()` em lote e pode acontecer de o estado ou _props_ serem modificados no meio da execução da sua própria chamada de `setState()`.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const { fooCount } = this.state;
const { barCount } = this.props;
this.setState({ count: fooCount + barCount });
~~~~~~~~

Imagine que `fooCount` e `barCount`, aqui estado e _props_, são modificados de forma assíncrona em outro ponto no momento em que você chama `setState()` aqui. Em uma aplicação que está ganhando maior escala, você tem mais de uma chamada de `setState()`. Uma vez que `setState()` é assíncrono, você fica dependendo dos valores atuais dos estados.

Com a abordagem funcional, a função em `setState()` é um _callback_ que irá operar sobre o estado e as _props_ que existiam no momento da sua execução.  Mesmo `setState()` sendo assíncrono, este será o comportamento.

{title="Code Playground",lang="javascript"}
~~~~~~~~
this.setState((prevState, props) => {
  const { fooCount } = prevState;
  const { barCount } = props;
  return { count: fooCount + barCount };
});
~~~~~~~~

Agora, voltemos para o seu código, para consertar este comportamento. Vamos juntos fazê-lo em um lugar onde `setState()` é utilizado e depende do estado e das _props_. Você depois estará apto a aplicar a correção em outros lugares.

O método `setSearchTopStories()` depende do estado anterior e é, assim, um exemplo perfeito para utilizarmos uma função ao invés de um objeto em `setState()`. No momento, o trecho de código se parece com esse a seguir:

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Você extrai valores de _state_, mas o atualiza de forma assíncrona dependendo do estado anterior. É possível, então, utilizar a abordagem funcional para prevenir _bugs_ causados por um estado viciado.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;

# leanpub-start-insert
  this.setState(prevState => {
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

Você pode mover o bloco inteiro implementado para dentro da função. A única modificação necessária é operar com `prevState` ao invés de `this.state`.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

Isso irá resolver o problema do estado viciado. Existe ainda uma coisa que pode ser melhorada. Uma vez que você tem uma função, ela pode ser extraída, em nome de uma melhor legibilidade. Esta é mais uma vantagem de se utilizar uma função e não um objeto. A função pode existir até fora do componente, mas você tem que usar uma _higher-order function_ para passar o resultado para ela.

No fim das contas você quer atualizar o estado com base o resultado obtido da API:

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
~~~~~~~~

A função `updateSearchTopStoriesState()` precisa retornar uma função. Ela é uma _higher-order function_ e pode ser definida fora do componente _App_. Note como a assinatura pode ser ligeiramente diferente agora.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
~~~~~~~~

É isto. A abordagem funcional de `setState()` resolve potenciais _bugs_ e ainda aumenta a legibilidade e manutenabilidade do seu código. Torna-se também mais estável fora do componente _App_. Como exercício, você pode exportá-la e escrever um teste para ela.

### Exercício:

* Leia mais sobre [uso correto do estado em React][3]
* Exporte _updateSearchTopStoriesState_ no seu arquivo
 * Escreva um teste para, o qual passa o _payload_ (hist, page) e um “estado anterior”, com um _expect_ para um novo estado
* refatore seus métodos `setState()` para a abordagem funcional
  * mas, somente quando fizer sentido, por ele depender de estado ou _props_
* Rode seus testes novamente e verifique se tudo foi atualizado

## _Taming the State_

Os capítulos anteriores lhe mostraram que o gerenciamento de estado pode ser um tópico crucial em aplicações de larga escala. Em geral, não apenas React, mas uma grande quantidade de _frameworks_ SPA gastam bastante energia com o assunto. As aplicações ficaram ainda mais complexas em anos recentes e um dos maiores desafios para aplicações _web_, nos dias de hoje, é o de **domar** (do inglês “_tame_”) e controlar o estado.

Comparado com outras soluções, React já deu um grande passo à frente. O fluxo unidirecional de dados e uma API simples para gerenciar estado em um componente são indispensáveis. Estes conceitos tornam mais leve a tarefa de raciocinar sobre seu estado e as mudanças sobre ele. É mais fácil pensar a nível de components e, até certo ponto, de aplicação também.

Em uma aplicação que está crescendo, fica mais difícil raciocinar sobre mudanças de estado. Você pode acabar introduzindo _bugs_ operando sobre um estado viciado, quando passa um objeto para `setState()`. Você precisa ficar realocando o estado para lá e para cá, por necessidade de compartilhamento, além de ficar escondendo estados desnecessários em componentes. Pode acontecer de um componente precisar promover um estado, porque o componente irmão depende dele. Talvez este esteja muito longe na árvore de componentes e você terá que compartilhar o estado por toda ela. No fim, componentes têm um grande envolvimento no gerenciamento de estados. Mas, a responsabilidade maior de componentes deveria ser a de representar a UI, não é mesmo?

Por todos estes motivos é que existem soluções independentes para cuidar do gerenciamento de estado. Estas soluções não são utilizadas apenas em React, mas são elas que fazer do seu ecossistema um lugar tão poderoso. Você pode usar diferentes soluções para resolver seus problemas. Já pode ter ouvido falar das bibliotecas [Redux][4] e [MobX][5]. Você pode utilizar qualquer uma delas em sua aplicação React. Elas trazem extensões, [react-redux][6] e [mobx-react][7] (respectivamente) para integrá-las na camada de visão de React.

Redux e MobX estão fora do escopo deste livro. Quando você terminar de lê-lo, irá ser guiado sobre como pode continuar a aprender React e seu ecossistema. Um dos caminhos que pode ser seguido é o aprendizado de Redux. Mas, antes de você mergulhar no tópico de gerenciamento externo de estado, recomendo que leia este [artigo][8]. Ele visa dar-lhe um melhor entendimento sobre como aprender este assunto.

### Exercícios:

* Leia mais sobre [gerenciamento externo de estado e como aprendê-lo][9]
* Confira meu segundo _e-book_ sobre [gerenciamento de estado em React][10]

{pagebreak}

Você aprendeu técnicas avançadas de gerenciamento de estado em React! Vamos recapitular:

* React
  * realocação do estado para cima e para baixo, para componentes mais adequados
    * setState pode usar uma função para prevenir _bugs_ relacionados a um estado viciado
    * soluções externas existentes que lhe ajudam a domar o estado (_tame the state_)

Você pode encontrar o código-fonte no [repositório oficial][11].

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