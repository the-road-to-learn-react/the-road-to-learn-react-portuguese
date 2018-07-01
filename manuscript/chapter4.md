# Organização do Código e Testes

O capítulo irá manter o foco em tópicos importantes para a manutenção do código em uma aplicação escalável. Você irá aprender sobre como organizar o código, visando adotar as melhores práticas de estruturação de arquivos de pastas. Outro aspecto são os testes, muito importantes para manter seu código robusto. O capítulo irá deixar um pouco de lado a aplicação prática que estamos desenvolvendo e explicará alguns destes tópicos para você de forma mais conceitual.

## _ES6 Modules_: _Import_ e _Export_

Em JavaScript ES6, você pode importar e exportar funcionalidades de módulos. Estas funcionalidades podem ser funções, classes, componentes, constantes e outros. Basicamente, tudo o que pode ser atribuído à uma variável. Módulos podem ser simples arquivos ou pastas inteiras com um arquivo _index_ como ponto de entrada.

No começo deste livro, depois que você inicializou sua aplicação com _create-react-app_, os arquivos gerados já tinham várias declarações de `import`e `export`. É chegada a hora de explicá-los.

As declarações de `import` e `export` facilitam o compartilhamento de código entre múltiplos arquivos. Antes, haviam diversas formas de atingir este objetivo em um ambiente JavaScript. Era uma verdadeira bagunça e desejava-se que existisse um padrão, ao invés de múltiplas abordagens para a mesma coisa. Agora, desde JavaScript ES6, este é o comportamento nativo.

Adicionalmente, elas abraçam o paradigma de \_code splitting, \_onde você distribui seu código por múltiplos arquivos para mantê-lo reutilizável e manutenível. Reutilizável porque você consegue importar um pedaço de código em múltiplos arquivos. Manutenível porque você tem uma única fonte onde você mantém um pedaço de código.

Pro fim, estas declarações lhe ajudam a pensar sobre o encapsulamento de código. Nem toda funcionalidade precisa ser exportada em um arquivo. Algumas deveriam ser utilizadas apenas no arquivo em que foram definidas. Os _exports_ de um arquivo são basicamente a sua API pública. Apenas as funcionalidades exportadas estão disponíveis para reuso em outro lugar, seguindo assim a boa prática de encapsulamento.

Vamos por a mão na massa. Como `import`e `export` funcionam? Os exemplo a seguir demonstram ambas as declarações compartilhando uma ou múltiplas variáveis entre dois arquivos. No final, esta abordagem pode escalar para múltiplos arquivos e poderia também compartilhar mais do que simples variáveis.

Você pode exportar uma ou múltiplas variáveis com o chamado “\_export\_ nomeado”.

{title="Code Playground: file1.js",lang="javascript"}
	const firstname = 'robin';
	const lastname = 'wieruch';
	
	export { firstname, lastname };
 
E importá-las em outro arquivo utilizando o caminho relativo para o primeiro.

{title="Code Playground: file2.js",lang="javascript"}
	import { firstname, lastname } from './file1.js';
	
	console.log(firstname);
	// output: robin

Você também pode importar, em um único objeto, todas as variáveis exportadas de outro arquivo.

{title="Code Playground: file2.js",lang="javascript"}
	import * as person from './file1.js';
	
	console.log(person.firstname);
	// output: robin

_Imports_ podem ter um _alias_. Por existir a possibilidade de você importar funcionalidades exportadas com o mesmo nome de arquivos diferentes, você pode utilizar o _alias_ para fornecer “apelidos” para elas.

{title="Code Playground: file2.js",lang="javascript"}
	import { firstname as foo } from './file1.js';
	
	console.log(foo);
	// output: robin

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
	// output: { firstname: 'robin', lastname: 'wieruch' }

Além disso, o nome dado para o módulo no _import_ pode diferir daquele que foi exportado com _default_. Ele pode até ser usado em conjunto com as declarações nomeadas de\_export\_ e _import_.

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
	// output: { firstname: 'robin', lastname: 'wieruch' }
	console.log(firstname, lastname);
	// output: robin wieruch

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

A seguir, eu irei propor várias estruturas de módulos que você **poderia** aplicar. Eu recomendaria aplicá-las como, um exercício, ao final do livro. Para manter o livro simples, eu não irei fazer a divisão de código e irei continuar os capítulos seguintes com o arquivo _src/App.js_.

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
	// bad practice, don't do it
	import SubmitButton from '../Buttons/SubmitButton';

Você agora sabe como poderia refatorar seu código-fonte em módulos, aplicando restrições de encapsulamento. Como eu disse antes, com fim de manter o livro simples, eu não farei estas mudanças aqui. Você deve refatorar quando terminar de ler este livro.

### Exercícios:

* refatore seu arquivo *src/App.js* em múltiplos módulos de componentes quando terminar de ler o livro

## Snapshot Tests with Jest

The book will not dive deeply into the topic of testing, but it shouldn't be unmentioned. Testing your code in programming is essential and should be seen as mandatory. You want to keep the quality of your code high and an assurance that everything works.

Perhaps you have heard about the testing pyramid. There are end-to-end tests, integration tests and unit tests. If you are not familiar with those, the book gives you a quick and basic overview. A unit test is used to test an isolated and small block of code. It can be a single function that is tested by an unit test. However, sometimes the units work well in isolation yet don't work in combination with other units. They need to be tested as a group of units. That's where integration tests can help out by covering whether units work well together. Last but not least, an end-to-end test is the simulation of a real user scenario. It could be an automated setup in a browser simulating the login flow of an user in a web application. While unit tests are fast and easy to write and to maintain, end-to-end tests are the opposite of this spectrum.

How many tests do I need of each type? You want to have many unit tests to cover your isolated functions. After that, you can have several integration tests to cover that the most important functions work in combination as expected. Last but not least, you might want to have only a few end-to-end tests to simulate critical scenarios in your web application. That's it for the general excursion in the world of testing.

So how do you apply this knowledge in testing your React application? The foundation for testing in React are component tests which can be generalized as unit tests and a part of it as snapshot tests. You will conduct unit tests for your components in the next chapter by using a library called Enzyme. In this chapter, you will focus on another kind of tests: snapshot tests. That's were Jest comes into play.

[Jest][3] is a JavaScript testing framework that is used at Facebook. In the React community, it is used for React component tests. Fortunately *create-react-app* already comes with Jest, so you don't need to worry about setting it up.

Let's start to test your first components. Before you can do that, you have to export the components, which you are going to test, from your *src/App.js* file. Afterward you can test them in a different file. You have learned about this in the code organization chapter.

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

In your *App.test.js* file, you will find a first test that came with *create-react-app*. It verifies that the App component would render without any errors.

{title="src/App.test.js",lang=javascript}
	import React from 'react';
	import ReactDOM from 'react-dom';
	import App from './App';
	
	it('renders without crashing', () => {
	  const div = document.createElement('div');
	  ReactDOM.render(<App />, div);
	  ReactDOM.unmountComponentAtNode(div);
	});

The "it"-block describes one test case. It comes with a test description and when you test it, it can either succeed or fail. Furthermore, you could wrap it into a "describe"-block that defines your test suite. A test suite could include a bunch of the "it"-blocks for one specific component. You will see those "describe"-blocks later on. Both blocks are used to separate and organize your test cases.

Note that the `it` function is acknowledged in the JavaScript community as the function where you run a single test. However, in Jest it is often found as an alias `test` function.

You can run your test cases by using the interactive *create-react-app* test script on the command line. You will get the output for all test cases on your command line interface.

{title="Command Line",lang="text"}
	npm test

Now Jest enables you to write snapshot tests. These tests make a snapshot of your rendered component and run this snapshot against future snapshots. When a future snapshot changes, you will get notified in the test. You can either accept the snapshot change, because you changed the component implementation on purpose, or deny the change and investigate for the error. It complements unit tests very well, because you only test the diffs of the rendered output. It doesn't add big maintenance costs, because you can simply accept changed snapshots when you changed something on purpose for the rendered output in your component.

Jest stores the snapshots in a folder. Only that way it can validate the diff against a future snapshot. Additionally, the snapshots can be shared across teams by having them in one folder.

Before writing your first snapshot test with Jest, you have to install an utility library.

{title="Command Line",lang="text"}
	npm install --save-dev react-test-renderer

Now you can extend the App component test with your first snapshot test. First, import the new functionality from the node package and wrap your previous "it"-block for the App component into a descriptive "describe"-block. In this case, the test suite is only for the App component.

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

Now you can implement your first snapshot test by using a "test"-block.

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
	    let tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	# leanpub-end-insert
	});

Run your tests again and see how the tests either succeed or fail. They should succeed. Once you change the output of the render block in your App component, the snapshot test should fail. Then you can decide to update the snapshot or investigate in your App component.

Basically the `renderer.create()` function creates a snapshot of your App component. It renders it virtually and stores the DOM into a snapshot. Afterward, the snapshot is expected to match the previous snapshot from when you ran your snapshot tests the last time. This way, you can assure that your DOM stays the same and doesn't change anything by accident.

Let's add more tests for our independent components. First, the Search component:

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
	    let tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	});
	# leanpub-end-insert

The Search component has two tests similar to the App component. The first test simply renders the Search component to the DOM and verifies that there is no error during the rendering process. If there would be an error, the test would break even though there isn't any assertion (e.g. expect, match, equal) in the test block. The second snapshot test is used to store a snapshot of the rendered component and to run it against a previous snapshot. It fails when the snapshot has changed.

Second, you can test the Button component whereas the same test rules as in the Search component apply.

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
	    let tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	});
	# leanpub-end-insert

Last but not least, the Table component that you can pass a bunch of initial props to render it with a sample list.

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
	    let tree = component.toJSON();
	    expect(tree).toMatchSnapshot();
	  });
	
	});
	# leanpub-end-insert

Snapshot tests usually stay pretty basic. You only want to cover that the component doesn't change its output. Once it changes the output, you have to decide if you accept the changes. Otherwise you have to fix the component when the output is not the desired output.

### Exercises:

* see how a snapshot test fails once you change your component's return value in the `render()` method
  * either accept or deny the snapshot change
* keep your snapshots tests up to date when the implementation of components change in next chapters
* read more about [Jest in React][4]

## Unit Tests with Enzyme

[Enzyme][5] is a testing utility by Airbnb to assert, manipulate and traverse your React components. You can use it to conduct unit tests to complement your snapshot tests in React.

Let's see how you can use enzyme. First you have to install it since it doesn't come by default with *create-react-app*. It comes also with an extension to use it in React.

{title="Command Line",lang="text"}
	npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16

Second, you need to include it in your test setup and initialize its Adapter for using it in React.

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

Now you can write your first unit test in the Table "describe"-block. You will use `shallow()` to render your component and assert that the Table has two items, because you pass it two list items. The assertion simply checks if the element has two elements with the class `table-row`.

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

Shallow renders the component without its child components. That way, you can make the test very dedicated to one component.

Enzyme has overall three rendering mechanisms in its API. You already know `shallow()`, but there also exist `mount()` and `render()`. Both instantiate instances of the parent component and all child components. Additionally `mount()` gives you access to the component lifecycle methods. But when to use which render mechanism? Here some rules of thumb:

* Always begin with a shallow test
* If `componentDidMount()` or `componentDidUpdate()` should be tested, use `mount()`
* If you want to test component lifecycle and children behavior, use `mount()`
* If you want to test a component's children rendering with less overhead than `mount()` and you are not interested in lifecycle methods, use `render()`

You could continue to unit test your components. But make sure to keep the tests simple and maintainable. Otherwise you will have to refactor them once you change your components. That's why Facebook introduced snapshot tests with Jest in the first place.

### Exercises:

* write a unit test with Enzyme for your Button component
* keep your unit tests up to date during the following chapters
* read more about [enzyme and its rendering API][6]
* read more about [testing React applications][7]

## Component Interface with PropTypes

You may know [TypeScript][8] or [Flow][9] to introduce a type interface to JavaScript. A typed language is less error prone, because the code gets validated based on its program text. Editors and other utilities can catch these errors before the program runs. It makes your program more robust.

In the book, you will not introduce Flow or TypeScript, but another neat way to check your types in components. React comes with a built-in type checker to prevent bugs. You can use PropTypes to describe your component interface. All the props that get passed from a parent component to a child component get validated based on the PropTypes interface assigned to the child component.

The chapter will show you how you can make all your components type safe with PropTypes. I will omit the changes for the following chapters, because they add unnecessary code refactorings. But you should keep and update them along the way to keep your components interface type safe.

First, you have to install a separate package for React.

{title="Command Line",lang="text"}
	npm install prop-types

Now, you can import the PropTypes.

{title="src/App.js",lang=javascript}
	import React, { Component } from 'react';
	import axios from 'axios';
	# leanpub-start-insert
	import PropTypes from 'prop-types';
	# leanpub-end-insert

Let's start to assign a props interface to the components:

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

Basically that's it. You take every argument from the function signature and assign a PropType to it. The basic PropTypes for primitives and complex objects are:

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

Additionally you have two more PropTypes to define a renderable fragment (node), e.g. a string, and a React element:

* PropTypes.node
* PropTypes.element

You already used the `node` PropType for the Button component. Overall there are more PropType definitions that you can read up on in the official React documentation.

At the moment all of the defined PropTypes for the Button are optional. The parameters can be null or undefined. But for several props you want to enforce that they are defined. You can make it a requirement that these props are passed to the component.

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

The `className` is not required, because it can default to an empty string. Next you will define a PropType interface for the Table component:

{title="src/App.js",lang=javascript}
	# leanpub-start-insert
	Table.propTypes = {
	  list: PropTypes.array.isRequired,
	  onDismiss: PropTypes.func.isRequired,
	};
	# leanpub-end-insert

You can define the content of an array PropType more explicitly:

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

Only the `objectID` is required, because you know that some of your code depends on it. The other properties are only displayed, thus they are not necessarily required. Moreover you cannot be sure that the Hacker News API has always a defined property for each object in the array.

That's it for PropTypes. But there is one more aspect. You can define default props in your component. Let's take again the Button component. The `className` property has an ES6 default parameter in the component signature.

{title="src/App.js",lang=javascript}
	const Button = ({
	  onClick,
	  className = '',
	  children
	}) =>
	  ...

You could replace it with the internal React default prop:

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

Same as the ES6 default parameter, the default prop ensures that the property is set to a default value when the parent component didn't specify it. The PropType type check happens after the default prop is evaluated.

If you run your tests again, you might see PropType errors for your components on your command line. It can happen because you didn't define all props for your components in the tests that are defined as required in your PropType definition. The tests themselves all pass correctly though. You can pass all required props to the components in your tests to avoid these errors.

### Exercises:

* define the PropType interface for the Search component
* add and update the PropType interfaces when you add and update components in the next chapters
* read more about [React PropTypes][10]

{pagebreak}

You have learned how to organize your code and how to test it! Let's recap the last chapters:

* React
  * PropTypes let you define type checks for components
  * Jest allows you to write snapshot tests for your components
  * Enzyme allows you to write unit tests for your components
* ES6
  * import and export statements help you to organize your code
* General
  * code organization allows you to scale your application with best practices

You can find the source code in the [official repository][11].

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
[11]:	https://github.com/the-road-to-learn-react/hackernews-client/tree/5.4