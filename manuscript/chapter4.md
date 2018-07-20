# Organização do Código e Testes

O capítulo irá manter o foco em tópicos importantes para a manutenção do código em uma aplicação escalável. Você irá aprender sobre como organizá-lo, visando adotar as melhores práticas de estruturação de arquivos e pastas. Outro aspecto são os testes, muito importantes para manter seu código robusto. Finalmente, você irá aprender sobre uma ferramenta muito útil para depurar suas aplicações React. O capítulo irá deixar um pouco de lado a aplicação prática que estamos desenvolvendo e explicará alguns destes tópicos para você de forma mais conceitual. 

## _ES6 Modules_: _Import_ e _Export_

Em JavaScript ES6, você pode importar e exportar funcionalidades de módulos. Estas funcionalidades podem ser funções, classes, componentes, constantes e outros. Basicamente, tudo o que pode ser atribuído à uma variável. Módulos podem ser simples arquivos ou pastas inteiras com um arquivo _index_ como ponto de entrada.

No começo deste livro, depois que você inicializou sua aplicação com _create-react-app_, os arquivos gerados já tinham várias declarações de `import`e `export`. É chegada a hora de explicá-los.

As declarações de `import` e `export` facilitam o compartilhamento de código entre múltiplos arquivos. Antes, haviam diversas formas de atingir este objetivo em um ambiente JavaScript. Era uma verdadeira bagunça e desejava-se que existisse um padrão, ao invés de múltiplas abordagens para a mesma coisa. Agora, desde JavaScript ES6, este é o comportamento nativo.

Adicionalmente, elas abraçam o paradigma de _code splitting_, onde você distribui seu código por múltiplos arquivos para mantê-lo reutilizável e manutenível. Reutilizável porque você consegue importar um pedaço de código em múltiplos arquivos. Manutenível porque você tem uma única fonte onde mantém o mesmo pedaço de código.

Pro fim, estas declarações lhe ajudam a pensar sobre o encapsulamento de código. Nem toda funcionalidade precisa ser exportada em um arquivo, algumas deveriam ser utilizadas apenas onde foram definidas. Os _exports_ de um arquivo são basicamente a sua API pública. Apenas as funcionalidades exportadas estão disponíveis para reuso em outro lugar, seguindo assim a boa prática de encapsulamento.

Vamos colocar a mão na massa. Como `import`e `export` funcionam? Os exemplo a seguir demonstram ambas as declarações, compartilhando uma ou múltiplas variáveis entre dois arquivos. No final, esta abordagem pode escalar para múltiplos arquivos e poderia também compartilhar mais do que simples variáveis.

Você pode exportar uma ou múltiplas variáveis com o chamado “_export_ nomeado” (_named export_).

{title="Code Playground: file1.js",lang="javascript"}
	const firstname = 'robin';
	const lastname = 'wieruch';
	
	export { firstname, lastname };
 
E importá-las em outro arquivo utilizando o caminho relativo para o primeiro.

{title="Code Playground: file2.js",lang="javascript"}
	import { firstname, lastname } from './file1.js';
	
	console.log(firstname);
	// saída: robin

Você também pode importar, em um único objeto, todas as variáveis exportadas de outro arquivo.

{title="Code Playground: file2.js",lang="javascript"}
	import * as person from './file1.js';
	
	console.log(person.firstname);
	// saída: robin

_Imports_ podem ter um _alias_. Por existir a possibilidade de você importar funcionalidades exportadas com o mesmo nome de arquivos diferentes, você pode utilizar o _alias_ para fornecer “apelidos” para elas.

{title="Code Playground: file2.js",lang="javascript"}
	import { firstname as foo } from './file1.js';
	
	console.log(foo);
	// saída: robin

Por último, mas muito importante, existe a declaração `default`, que pode ser utilizada em alguns casos, como:

* exportar e importar uma única funcionalidade
* para destacar a funcionalidade principal sendo exportada na API em um módulo
* para ter uma funcionalidade padrão de importação

{title="Code Playground: file1.js",lang="javascript"}
	const robin = {
	  firstname: 'robin',
	  lastname: 'wieruch',
	};
	
	export default robin;

Quando importando um módulo exportado com a declaração _default_, você pode dispensar o uso de chaves no _import_.

{title="Code Playground: file2.js",lang="javascript"}
	import developer from './file1.js';
	
	console.log(developer);
	// saída: { firstname: 'robin', lastname: 'wieruch' }

Além disso, o nome dado para o módulo no _import_ pode diferir daquele que foi exportado com _default_. Ele pode até ser usado em conjunto com as declarações nomeadas de _export_ e _import_.

{title="Code Playground: file1.js",lang="javascript"}
	const firstname = 'robin';
	const lastname = 'wieruch';
	
	const person = {
	  firstname,
	  lastname,
	};
	
	export {
	  firstname,
	  lastname,
	};
	
	export default person;

{title="Code Playground: file2.js",lang="javascript"}
	import developer, { firstname, lastname } from './file1.js';
	
	console.log(developer);
	// saída: { firstname: 'robin', lastname: 'wieruch' }
	console.log(firstname, lastname);
	// saída: robin wieruch

Em _exports_ nomeados, você pode poupar linhas adicionais, exportando diretamente as variáveis.

{title="Code Playground: file1.js",lang="javascript"}
	export const firstname = 'robin';
	export const lastname = 'wieruch';

Essas são as funcionalidades principais de _ES6 modules_. Elas lhe ajudam a organizar e manter o seu código, projetando APIs com módulos reutilizáveis. Você também pode exportar e importar funcionalidades para testá-las, o que será feito em um dos capítulos a seguir.

### Exercícios:

* Leia mais sobre [ES6 import][1]
* Leia mais sobre [ES6 export][2]

## Organização de Código com _ES6 Modules_

Você pode estar se perguntando: Por que nós não seguimos as melhores práticas de divisão de código para o arquivo _src/App.js_? No arquivo, nós já temos múltiplos componentes que poderiam ter sido definidos em seus próprios arquivos/pastas (módulos). Para o propósito de aprendizado de React, é prático manter estas coisas em um só lugar. Mas, uma vez que a sua aplicação cresce, você deve considerar dividir estes componentes em múltiplos módulos. Somente desta forma sua aplicação será escalável.

A seguir, eu irei propor várias estruturas de módulos que você **poderia** aplicar. Eu recomendaria aplicá-las como um exercício, ao final do livro. Para mantê-lo simples, eu não irei fazer a divisão de código e irei continuar os capítulos seguintes com o arquivo _src/App.js_.

Uma possível estrutura de módulo seria:

{title=“Estrutura de Pastas“,lang="text"}
	src/
	  index.js
	  index.css
	  App.js
	  App.test.js
	  App.css
	  Button.js
	  Button.test.js
	  Button.css
	  Table.js
	  Table.test.js
	  Table.css
	  Search.js
	  Search.test.js
	  Search.css

Ela separa os componentes em seus próprios arquivos, mas não aparenta ser muito promissora. Note quantos arquivos com nomes duplicados, diferindo apenas nas extensões.

Outra estrutura de módulos seria:

{title=“Estrutura de Pastas“,lang="text"}
	src/
	  index.js
	  index.css
	  App/
	    index.js
	    test.js
	    index.css
	  Button/
	    index.js
	    test.js
	    index.css
	  Table/
	    index.js
	    test.js
	    index.css
	  Search/
	    index.js
	    test.js
	    index.css

Ela parece mais limpa que a anterior. Dar o nome “index” a um arquivo o coloca como o ponto de entrada de uma parta. É uma convenção de nome comumente utilizada, mas não significa que você não possa utilizar os nomes que quiser. Nesta estrutura de módulos, um componente é definido pela sua declaração em um arquivo JavaScript, mas também pelo seu estilo e seus testes.

Outro passo que poderia ser dado é o de extrair as constantes do componente _App_. Estas constantes foram utilizadas para compor a URL da API Hacker News.

{title="Folder Structure",lang="text"}
	src/
	  index.js
	  index.css
	# leanpub-start-insert
	  constants/
	    index.js
	  components/
	# leanpub-end-insert
	    App/
	      index.js
	      test.js
	      index.css
	    Button/
	      index.js
	      test.js
	      index.css
	    ...

Os módulos naturalmente se dividem em _src/constants/_ e _src/components/_. O arquivo _src/constants/index.js_ pareceria-se com algo assim:

{title="Code Playground: src/constants/index.js",lang="javascript"}
	export const DEFAULT_QUERY = 'redux';
	export const DEFAULT_HPP = '100';
	export const PATH_BASE = 'https://hn.algolia.com/api/v1';
	export const PATH_SEARCH = '/search';
	export const PARAM_SEARCH = 'query=';
	export const PARAM_PAGE = 'page=';
	export const PARAM_HPP = 'hitsPerPage=';

O arquivo _App/index.js_ poderia importar estas variáveis para utilizá-las.

{title="Code Playground: src/components/App/index.js",lang=javascript}
	import {
	  DEFAULT_QUERY,
	  DEFAULT_HPP,
	  PATH_BASE,
	  PATH_SEARCH,
	  PARAM_SEARCH,
	  PARAM_PAGE,
	  PARAM_HPP,
	} from '../../constants/index.js';
	
	...

Quando você utiliza a convenção de nome _index.js_, pode omitir o nome do arquivo do caminho relativo no _import_.

{title="Code Playground: src/components/App/index.js",lang=javascript}
	import {
	  DEFAULT_QUERY,
	  DEFAULT_HPP,
	  PATH_BASE,
	  PATH_SEARCH,
	  PARAM_SEARCH,
	  PARAM_PAGE,
	  PARAM_HPP,
	# leanpub-start-insert
	} from '../../constants';
	# leanpub-end-insert
	
	...

No entanto, o que está realmente por trás da convenção de nomes _index.js_? Ela foi introduzida no mundo _node.js_. O arquivo _index_ é o ponto de entrada de um módulo, descrevendo sua API pública. Módulos externos somente são permitidos de usar o arquivo _index.js_ para importar código compartilhado pelo módulo. Considere a seguinte estrutura de módulos, feita para demonstrar o que foi falado aqui:

{title=“Estrutura de Pastas“,lang="text"}
	src/
	  index.js
	  App/
	    index.js
	  Buttons/
	    index.js
	    SubmitButton.js
	    SaveButton.js
	    CancelButton.js

A pasta _Buttons/_ tem múltiplos componentes de botões definidos em arquivos distintos. Cada arquivo pode utilizar `export default` para tornar o componente específico disponível para _Buttons/index.js_. O arquivo _Buttons/index.js_ importa todas as diferentes representações de botões e as exporta como a API pública do módulo.

{title="Code Playground: src/Buttons/index.js",lang="javascript"}
	import SubmitButton from './SubmitButton';
	import SaveButton from './SaveButton';
	import CancelButton from './CancelButton';
	
	export {
	  SubmitButton,
	  SaveButton,
	  CancelButton,
	};

Agora, *src/App/index.js* pode importar os botões desta API pública localizada em *index.js*.

{title="Code Playground: src/App/index.js",lang=javascript}
	import {
	  SubmitButton,
	  SaveButton,
	  CancelButton
	} from '../Buttons';

Se seguirmos esta restrição, seria então uma má prática acessar outros arquivos diretamente, sem ser através do _index.js_ do módulo. Quebraria as regras de encapsulamento.

{title="Code Playground: src/App/index.js",lang=javascript}
	// má prática, não fazer
	import SubmitButton from '../Buttons/SubmitButton';

Você agora sabe como poderia refatorar seu código-fonte em módulos, aplicando restrições de encapsulamento. Como eu disse antes, com a intenção de manter o livro simples, eu não farei estas mudanças aqui. Você deve refatorar quando terminar a leitura do último capítulo.

### Exercícios:

* refatore seu arquivo *src/App.js* em múltiplos módulos de componentes quando terminar de ler o livro

## _Snapshot Tests_ com Jest

Este livro não irá mergulhar fundo no tópico de testes, mas estes não poderiam deixar de ser mencionados. Em programação, testar o código é essencial e deveria ser encarado como algo obrigatório. Você com certeza quer manter alta a qualidade do seu código e com a garantia de que tudo funciona.

Talvez você tenha ouvido falar sobre a pirâmide de testes. Existem testes de ponta a ponta, testes de integração e testes unitários. Se você não está familiarizado com eles, o livro lhe dará uma rápida e básica noção geral.

Um teste unitário é utilizado para testar isoladamente um pequeno bloco de código. Pode ser uma única função que é testada por um teste deste tipo. Contudo, às vezes as partes isoladas funcionam bem, mas não funcionam como esperado quando combinadas. Elas precisam ser testadas como um grupo de unidades. É onde entram os testes de integração, que ajudam a ir aonde os testes unitários não chegam. Por último, mas não menos importante, testes ponta a ponta são uma simulação de um cenário real de usuário. Poderia ser, por exemplo, um _setupi_ automatizado em um _browser_ simulando o fluxo de _login_ de um usuário em uma aplicação web. Enquanto testes unitários são rápidos e fáceis de escrever e manter, testes de ponta a ponta estão do lado oposto deste espectro.

De quantos testes de cada tipo eu preciso? Você irá querer ter muitos testes unitários para cobrir suas funções isoladamente. Depois disso, você poderá ter vários testes de integração para cobrir o uso combinado das mais importantes funções. Por fim, você pode querer ter apenas alguns testes ponta-a-ponta para simular cenários críticos em suas aplicações web. Isso é tudo para nossa excursão geral no mundo dos testes.

Então, como você aplica este conhecimento sobre testes em sua aplicação React? A base para testar em React são os testes de componentes, que podem ser generalizados como testes de unidade e uma parte deles como _snapshot tests_. Você irá criar testes de unidade para seus componentes no próximo capítulo, utilizando uma biblioteca chamada _Enzyme_. Neste capítulo, você irá focar em outro tipo: _snapshot testes_. É quando _Jest_ entra em cena.

[Jest][3] é um framework JavaScript de testes utilizado no Facebook. Na comunidade React, ele é usado para testar componentes. Felizmente, _create-react-app_ já embute Jest e você não precisa se preocupar em configurá-lo.

Comecemos os testes dos seus primeiros componentes. Antes de fazê-lo, precisará exportar os componentes que estão em _src/App.js_ e que irá testar. Assim, poderá testá-los em um arquivo separado. Você aprendeu como fazer a exportação no capítulo sobre organização de código.

{title="src/App.js",lang=javascript}
	...
	
	class App extends Component {
	  ...
	}
	
	...
	
	export default App;
	
	# leanpub-start-insert
	export {
	  Button,
	  Search,
	  Table,
	};
	# leanpub-end-insert

Em seu arquivo _App.test.js_, você encontrará um primeiro teste que veio do _create-react-app_. Ele verifica se o componente _App_ é renderizado sem erros.

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	import App from './App';
	
	it('renders without crashing', () => {
	  const div = document.createElement('div');
	  ReactDOM.render(<App />, div);
	  ReactDOM.unmountComponentAtNode(div);
	});

O bloco “it” descreve um caso de teste. Ele traz uma descrição do teste e, quando executado, pode ter sucesso ou falhar. Além disso, ele poderia ser colocado dentro de um bloco “describe” que define sua suíte de testes. Sua suíte de testes poderia incluir um punhado de blocos “it” para um componente específico. Mais tarde, você verá mais sobre o bloco “describe”. Ambos os blocos são utilizados para separar e organizar seus casos de teste.

Note que a função `it` é reconhecida na comunidade JavaScript como a função onde você roda um único teste. Contudo, em Jest ela é frequentemente encontrada sob o _alias_ de `test`.

Você pode rodar seus casos de testes utilizando o script _test_ do _create-react-app_ na linha de comando. Você irá obter a saída para todos os casos de testes nesta mesma interface de linha de comando.

{title=“Linha de Comando“,lang="text"}
	npm test


Jest lhe permite escrever _snapshot tests_. Eles fazem uma fotografia (_snapshot_) do seu componente e a comparam com futuras fotografias. Quando um _snapshot_ futuro muda, você será notificado no teste. Você pode aceitar a mudança, porque realmente mudou a implementação do componente de propósito, ou rejeitar a mudança e investigar o que causou o erro. É um ótimo complemento para testes unitários, porque você testa apenas as diferenças entre as renderizações. Não adiciona grandes custos de manutenção, porque você pode simplesmente aceitar os _snapshots_ que mudaram quando você altera algo conscientemente.

Jest guarda os _snapshots_ (ou fotografias) em uma pasta. Somente desta forma ele consegue validar as diferenças nas comparações com _snapshots_ futuros. Isso ainda permite que eles sejam compartilhados pelo time.

Antes de escrever o seu primeiro _snapshot test_ com Jest, você tem que instalar uma biblioteca utilitária:

{title=“Linha de Comando“,lang="text"}
	npm install --save-dev react-test-renderer

Agora você pode extender o teste do componente _App_ como seu primeiro _snapshot test_. Primeiro, importe a nova funcionalidade da biblioteca recém adicionada e envolva seu bloco “it” em um bloco “describe”.  Neste caso, a suíte de testes trata apenas do componente _App_.

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	# leanpub-start-insert
	import renderer from 'react-test-renderer';
	# leanpub-end-insert
	import App from './App';
	
	# leanpub-start-insert
	describe('App', () => {
	
	# leanpub-end-insert
	  it('renders without crashing', () => {
	    const div = document.createElement('div');
	    ReactDOM.render(<App />, div);
	    ReactDOM.unmountComponentAtNode(div);
	  });
	# leanpub-start-insert
	
	});
	# leanpub-end-insert

Implemente agora seu primeiro _snapshot test_ utilizando um bloco “test”.

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	import renderer from 'react-test-renderer';
	import App from './App';
	
	describe('App', () => {
	
	  it('renders without crashing', () => {
	    const div = document.createElement('div');
	    ReactDOM.render(<App />, div);
	    ReactDOM.unmountComponentAtNode(div);
	  });
	
	# leanpub-start-insert
	  test('has a valid snapshot', () => {
	    const component = renderer.create(
	      <App />
	    );
	    const tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	# leanpub-end-insert
	});

Rode seus testes novamente e veja se eles têm sucesso ou se falham. Eles devem ter sucesso. Se você mudar a saída do bloco “render” em seu componente _App_, o _snapshot test_ deve falhar. Então você pode decidir atualizar o _snapshot_ ou investigar o que aconteceu em seu componente _App_.

Basicamente, a função `renderer.create()` cria um _snapshot_ do seu componente App. Ele o renderiza virtualmente e armazena uma fotografia do DOM. Depois, espera-se que essa fotografia (ou _snapshot_) coincida com outras anteriores, de quando você rodou seus _snapshot tests_ da última vez. Desta forma, você pode assegurar que o seu DOM permanecerá o mesmo, sem mudanças acidentais.

Vamos adicionar mais testes para nossos componentes independentes. Primeiro, o componente _Search_:

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	import renderer from 'react-test-renderer';
	# leanpub-start-insert
	import App, { Search } from './App';
	# leanpub-end-insert
	
	...
	
	# leanpub-start-insert
	describe('Search', () => {
	
	  it('renders without crashing', () => {
	    const div = document.createElement('div');
	    ReactDOM.render(<Search>Search</Search>, div);
	    ReactDOM.unmountComponentAtNode(div);
	  });
	
	  test('has a valid snapshot', () => {
	    const component = renderer.create(
	      <Search>Search</Search>
	    );
	    const tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	});
	# leanpub-end-insert

O componente _Search_ tem dois testes similares aos do componente _App_. O primeiro simplesmente renderia _Search_ no DOM e verifica que não existe erro no processo de renderização.  Se um erro ocorrer, o teste quebrará mesmo que não exista nenhuma assertiva (_expect, match, equal_) no bloco de teste. O segundo _snapshot test_ é utilizado para armazenar o _snapshot_ do componente renderizado e rodá-lo em comparação aos anteriores. Ele falha quando o _snapshot_ muda.

Segundo, você pode testar o componente _Button_ aplicando estas mesmas regras do componente _Search_.

{title="src/App.test.js",lang=javascript}
	...
	# leanpub-start-insert
	import App, { Search, Button } from './App';
	# leanpub-end-insert
	
	...
	
	# leanpub-start-insert
	describe('Button', () => {
	
	  it('renders without crashing', () => {
	    const div = document.createElement('div');
	    ReactDOM.render(<Button>Give Me More</Button>, div);
	    ReactDOM.unmountComponentAtNode(div);
	  });
	
	  test('has a valid snapshot', () => {
	    const component = renderer.create(
	      <Button>Give Me More</Button>
	    );
	    const tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	});
	# leanpub-end-insert

Por fim, o componente _Table_, para o qual você passa um punhado de _props_ contendo uma amostra de dados para renderizá-lo.

{title="src/App.test.js",lang=javascript}
	...
	# leanpub-start-insert
	import App, { Search, Button, Table } from './App';
	# leanpub-end-insert
	
	...
	
	# leanpub-start-insert
	describe('Table', () => {
	
	  const props = {
	    list: [
	      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
	      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
	    ],
	  };
	
	  it('renders without crashing', () => {
	    const div = document.createElement('div');
	    ReactDOM.render(<Table { ...props } />, div);
	  });
	
	  test('has a valid snapshot', () => {
	    const component = renderer.create(
	      <Table { ...props } />
	    );
	    const tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	});
	# leanpub-end-insert

_Snapshot tests_ geralmente são bem básicos. Você só quer cobrir o caso de o componente ter ou não mudado. Uma vez que mudou, você decide se aceita as mudanças. Caso não, você tem que consertar a modificação indesejada.

### Exercícios:

* Veja como um _snapshot test_ falha quando você muda o retorno do seu componente no método `render()`
  * Aceite ou negue a mudança de _snapshot_
* Mantenha seus _snapshot tests_ atualizados quando implementar mudanças nos componentes nos próximos capítulos
* Leia mais sobre [Jest em React][4]

## Testes de Unidade com Enzyme

[Enzyme][5] é um utilitário de testes, lançado pelo Airbnb, para percorrer, manipular  e realizar assertivas com seus componentes React. Você pode utilizá-lo para conduzir testes unitários, complementando seus _snapshot tests_ em React.

Vejamos como podemos utilizar _enzyme_. Primeiro, você precisa instalar a biblioteca, uma vez que não ela vem por padrão com _create-react-app_. Ele possui também uma extensão para ser utilizado em React.

{title=“Linha de Comando“,lang="text"}
	npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16

Segundo, você precisa incluir a biblioteca no seu _setup_ de testes e inicializar o seu _Adapter_ para ser usado com React.

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	import renderer from 'react-test-renderer';
	# leanpub-start-insert
	import Enzyme from 'enzyme';
	import Adapter from 'enzyme-adapter-react-16';
	# leanpub-end-insert
	import App, { Search, Button, Table } from './App';
	
	# leanpub-start-insert
	Enzyme.configure({ adapter: new Adapter() });
	# leanpub-end-insert

Agora, você pode escrever seu primeiro teste unitário no bloco “describe” de _Table_. Você irá usar `shallow()` para renderizar seu componente e verificar que _Table_ tem dois itens, de acordo com os dois itens de lista que você passou para ele. A verificação simplesmente checa se o elemento renderizado possui dois outros elementos com a classe `table-row`.

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	import renderer from 'react-test-renderer';
	# leanpub-start-insert
	import Enzyme, { shallow } from 'enzyme';
	# leanpub-end-insert
	import Adapter from 'enzyme-adapter-react-16';
	import App, { Search, Button, Table } from './App';
	
	...
	
	describe('Table', () => {
	
	  const props = {
	    list: [
	      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
	      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
	    ],
	  };
	
	  ...
	
	# leanpub-start-insert
	  it('shows two items in list', () => {
	    const element = shallow(
	      <Table { ...props } />
	    );
	
	    expect(element.find('.table-row').length).toBe(2);
	  });
	# leanpub-end-insert
	
	});

_Shallow_ renderiza o componente sem renderizar seus componentes filhos. Assim, você pode fazer o teste verdadeiramente dedicado a ele.

_Enzyme_ tem, ao todo, três mecanismos de renderização em sua API. Você já conhece `shallow()`, mas também existem `mount()` e `render()`. Ambos criam instâncias do componente pai e também dos seus filhos. A diferença é que `mount()` lhe dá acesso aos métodos de ciclo de vida do componente. Quando, então, utilizar cada mecanismo de renderização? Seguem algumas regras:

* Sempre comece com um teste _shallow_
* Se `componentDidMount()` ou `componentDidUpdate()` precisam ser testados, use `mount()`
* Se você quiser testar o ciclo de vida do componente e o comportamento dos seus filhos, use `mount()`
* Se você quiser testar a renderização dos filhos de um componente com menos _overhead_ do que quando se usa `mount()`, use `render()`

Você pode continuar escrevendo testes de unidade para seus componentes. Mas, faça por onde manter os testes simples e manuteníveis. Caso contrário, você irá acabar tendo que refatorá-los todas as vezes que seus componentes mudarem. O Facebook já introduziu _snapshot tests_, com Jest, para este propósito.

### Exercícios:

* Escreva um teste unitário com _Enzyme_ para o seu componente _Button_
* Mantenha seus testes unitários atualizados ao longo dos próximos capítulos
* Leia mais sobre [enzyme e sua API de renderização][6]
* Leia mais sobre [testes de aplicações React][7]

## Interface de Componente com _PropTypes_

Você pode conhecer [TypeScript][8] ou [Flow][9] como meios de introduzir uma interface de tipos para JavaScript. Uma linguagem com tipos é menos propensa a erros, porque o código é validado com base no texto. Editores e outros utilitários podem capturar estes erros antes mesmo do programa ser executado e seu programa torna-se mais robusto.

Neste livro, você não irá trabalhar com _Flow_ ou _TypeScript_, mas com outra forma bastante organizada de checagem de tipos em seus componentes. React traz, nativamente, uma checagem de tipos para prevenir bugs. Você pode utilizar _PropTypes_ para descrever a interface do seu componente. Todas as _props_ passadas de um componente pai para um componente filho são validadas com base na interface definida no componente filho.

Este capítulo irá lhe mostrar como tornar seus componentes mais seguros com _PropTypes_. Não irei manter estas mudanças nos capítulos seguintes, porque elas irão causar refatorações desnecessárias de código, com a evolução dos exemplos. Mas, você deveria mantê-las e atualizá-las ao longo do caminho, para manter a interface dos seus componentes segura em relação à tipos.

Primeiro, você tem que instalar um pacote separado para React.

{title="Linha de Comando",lang="text"}
	npm install prop-types

Agora, você pode importar _PropTypes_.

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	import axios from 'axios';
	# leanpub-start-insert
	import PropTypes from 'prop-types';
	# leanpub-end-insert

Vamos começar a definir uma interface de _props_ nos nossos componentes:

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
	
	# leanpub-start-insert
	Button.propTypes = {
	  onClick: PropTypes.func,
	  className: PropTypes.string,
	  children: PropTypes.node,
	};
	# leanpub-end-insert

Basicamente, é isto. Você atribui um _PropType_ para cada argumento na assinatura da função. Os _PropTypes_ básicos para tipos primitivos e objetos complexos são:

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

Adicionalmente, você tem mais dois _PropTypes_ que podem ser usados para definir um fragmento renderizável (node) e um elemento React.

* PropTypes.node
* PropTypes.element

Você já utilizou o _PropType_ `node` para o componente _Button_. Existem, no total, mais definições de _PropTypes_ que você pode ler na documentação oficial de React.

No momento, todos os _PropTypes_ definidos para _Button_ são opcionais. Os parâmetros podem receber _null_ ou _undefined_. Mas, em alguns casos, você quer forçar a definição de algumas _props_. Você pode fazer com que seja obrigatório que essas _props_ sejam passadas para o componente.

{title="src/App.js",lang=javascript}
	Button.propTypes = {
	# leanpub-start-insert
	  onClick: PropTypes.func.isRequired,
	# leanpub-end-insert
	  className: PropTypes.string,
	# leanpub-start-insert
	  children: PropTypes.node.isRequired,
	# leanpub-end-insert
	};

O parâmetro `className` não é obrigatório, porque ele pode simplesmente obter o valor _default_ de _string_ vazia.

O próximo passo é definir uma interface com _PropTypes_ para o componente _Table_:

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	Table.propTypes = {
	  list: PropTypes.array.isRequired,
	  onDismiss: PropTypes.func.isRequired,
	};
	# leanpub-end-insert

Você pode definir o conteúdo de um _PropType_ do tipo _array_ mais explicitamente:

{title="src/App.js",lang=javascript}
	Table.propTypes = {
	  list: PropTypes.arrayOf(
	    PropTypes.shape({
	      objectID: PropTypes.string.isRequired,
	      author: PropTypes.string,
	      url: PropTypes.string,
	      num_comments: PropTypes.number,
	      points: PropTypes.number,
	    })
	  ).isRequired,
	  onDismiss: PropTypes.func.isRequired,
	};

Apenas `objectID` é obrigatório, porque você sabe que alguma parte do seu código depende dele. As outras propriedades são apenas exibidas, não sendo necessariamente requeridas. Além disso, você não tem plena certeza de que a API Hacker News tem sempre uma propriedade definida para cada objeto no _array_.

Isso era tudo que tínhamos para falar, sobre _PropTypes_. Só falta mais um aspecto a ser mencionado: Você pode definir _props default_ em seu componente. Tomemos como exemplo o componente _Button_ novamente. A propriedade `className`, na assinatura do componente, tem um parâmetro _default_ (de ES6).

{title="src/App.js",lang=javascript}
	const Button = ({
	  onClick,
	  className = '',
	  children
	}) =>
	  ...

Você pode substituí-lo por uma _prop default_ de React:

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	const Button = ({
	  onClick,
	  className,
	  children
	}) =>
	# leanpub-end-insert
	  <button
	    onClick={onClick}
	    className={className}
	    type="button"
	  >
	    {children}
	  </button>
	
	# leanpub-start-insert
	Button.defaultProps = {
	  className: '',
	};
	# leanpub-end-insert

Assim como o parâmetro _default_ de ES6, a _default prop_ garante que a propriedade é definida com um valor padrão quando o componente pai não informa um. A checagem de tipo de _PropType_ é feita **depois** que a _default prop_ é avaliada.

Se você rodar seus testes novamente, verá que erros de _PropType_ aparecerão na linha de comando para seu componente. Isso pode ocorrer porque você não definiu todas as propriedades para seus componentes nos testes que foram definidos, algumas delas foram marcadas como obrigatórias na definição de PropTypes. Os testes, no entanto, passam sem problemas. Se quiser, você poderia passar todas as propriedades obrigatórias em seus testes, para evitar estes erros.

### Exercícios:

* Defina a interface de _PropTypes_ para o componente _Search_
* Adicione e atualize as interfaces de _PropTypes_ quando você adicionar ou alterar componentes nos próximos capítulos
* Leia mais sobre [React PropTypes][10]

## Depuração com _React Developer Tools_

Esta última seção apresenta uma ferramenta muito útil, geralmente utilizada para analisar e depurar aplicações React. **React Developer Tools** lhe permite inspecionar a hierarquia, as _props_ e  estado local de componentes, estando disponível como uma extensão (para Chrome e Firefox) e como uma aplicação _standalone_ (que funciona com qualquer outro ambiente). Uma vez instalada, o ícone da extensão irá acender quando você carregar _websites_ que estejam utilizando React.  Em páginas assim, você poderá ver uma aba chamada “React” nas ferramentas de desenvolvedor do seu navegador.

Vamos testá-la com a sua aplicação Hacker News. Na maioria dos navegadores, uma forma rápida de abrir as ferramentas de desenvolvedor é clicando com o botão direito do mouse na página e, depois, selecionar “Inspect” (ou “Inspecionar”). Faça isso com a sua aplicação carregada, depois clique na aba “React”. Você deverá ver a hierarquia de elementos, sendo `App` o elemento raiz (_root element_). Se você expandir a árvore, achará também as instâncias dos componentes `Search`, `Table` e `Button`. 

A extensão mostra, no painel lateral do lado direito, o estado local e as _props_ de componente para o elemento selecionado. Por exemplo, se você clicar em `App`, verá que ele ainda não possui _props_, mas já possui um estado local. Uma técnica de depuração bastante simples é a de monitorar as mudanças de estado da aplicação causadas pela interação do usuário.

Primeiro, irá querer marcar a opção “Highlight Updates” (geralmente localizada acima da árvore de elementos). Segundo, você pode digitar um termo de busca diferente no campo de _input_ da aplicação. Como você poderá ver, apenas `searchTerm` será modificado no estado do componentes. Você até já sabia que isto aconteceria desta forma, mas agora pode ver de verdade que ocorreu como planejado.

Finalmente, pressione o botão “Search”. O estado `searchKey` irá ser imediatamente alterado para o mesmo valor de `searchTerm` e o objeto de retorno da requisição irá ser adicionado à `results` poucos segundos depois. A natureza assíncrona do seu código está agora visível aos seus olhos.

Por último, se você clicar com o botão direito do mouse em qualquer elemento, um menu de contexto irá aparecer com várias opções. Por exemplo, você pode copiar as _props_ ou o nome de um elemento, achar o nó correspondente a ele no DOM ou até saltar para o código-fonte da aplicação direto no navegador. Esta última opção é bem útil, uma vez que lhe permite inserir _breakpoints_ e depurar as suas funções JavaScript.

### Exercícios:

* Instale a extensão [React Developer Tools][11] no navegador de sua escolha
	* Rode a sua aplicação clone do Hacker News e a analise utilizando a extensão
	* Faça experiências mudando o estado e as _props_ de componentes
	* Observe o que acontece quando você dispara requisições assíncronas
	* Faça múltiplas requisições, incluindo algumas repetidas. Observe o mecanismo de _cache_ em funcionamento
* Leia sobre [como depurar suas funções JavaScript no navegador][12] 

{pagebreak}

Você aprendeu como organizar o seu código e como testá-lo. Vamos recapitular:

* React
	* _PropTypes_ lhe permite definir checagem de tipos para componentes
	* Jest lhe permite escrever _snapshot tests_ para seus componentes
	* Enzyme lhe permite escrever testes unitários para seus componentes
	* React Developer Tools is a helpful debugging tool
* ES6
	* Declarações _import_ e _export_ lhe ajudam a organizar o seu código
* Geral
	* Uma melhor organização de código lhe permite escalar sua aplicação com as melhores práticas

Você irá achar o código-fonte deste capítulo no [repositório oficial][13].

[1]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import
[2]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export
[3]:	https://facebook.github.io/jest/
[4]:	https://facebook.github.io/jest/docs/tutorial-react.html
[5]:	https://github.com/airbnb/enzyme
[6]:	https://github.com/airbnb/enzyme
[7]:	https://www.robinwieruch.de/react-testing-tutorial
[8]:	https://www.typescriptlang.org/
[9]:	https://flowtype.org/
[10]:	https://reactjs.org/docs/typechecking-with-proptypes.html
[11]:	https://github.com/facebook/react-devtools
[12]:	https://developers.google.com/web/tools/chrome-devtools/javascript/
[13]:	https://github.com/the-road-to-learn-react/hackernews-client/tree/5.4