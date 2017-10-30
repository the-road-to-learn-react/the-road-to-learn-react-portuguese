# Introdução a React

Você pode estar se perguntando: Por que eu deveria aprender React, pra começo de conversa? Este capítulo é uma introdução ao assunto e pode lhe dar a resposta que procura. Você irá mergulhar no ecossistema de React, montando sua primeira aplicação sem a necessidade de nenhuma configuração e, durante o processo, terá uma introdução à JSX e ReactDOM.

Prepare-se para seus primeiros componentes React.

## Oi, meu nome é React.

**Por que você deveria se incomodar em aprender React?** Nos últimos anos, _single page applications_ ([SPA][1]) tornaram-se populares. _Frameworks_ como Angular, Ember e Backbone ajudaram desenvolvedores JavaScript a construir aplicações web modernas, indo além do que já se costumava fazer com jQuery e JavaScript puro. Existe uma ampla gama de _frameworks_ SPA e, olhando para suas respectivas datas de lançamento, a maioria pertence à primeira geração: Angular 2010, Backbone 2010 e Ember 2011. 

A primeira versão de React foi lançada em 2013 pelo Facebook. Não como um _framework_, mas como uma biblioteca. Ele é apenas o "V" do [MVC][2] (_model view controller_), que lhe permite renderizar componentes como elementos visualizáveis no _browser_. O ecossistema onde React se insere é que torna possível a construção de _single page applications_ .

E por que considerar o uso de React em detrimento à primeira geração de _frameworks_ SPA? Porque enquanto estes últimos tentam resolver várias coisas ao mesmo tempo, React apenas lhe ajuda a construir _views_. Uma biblioteca, não um _framework_, que segue uma simples idéia: a camada de visão é uma hierarquia de componentes.

Em React, você consegue manter o foco na criação das suas _views_ antes de introduzir outros aspectos na aplicação. Cada outro aspecto é como uma peça acoplável na sua SPA. Essa divisão em partes integráveis é essencial para construir uma aplicação mais madura, trazendo duas vantagens:

Primeiro, você pode aprender passo-a-passo cada parte, sem precisar se preocupar em entender tudo de só uma vez. Bem diferente de um _framework_, que lhe entrega todas as peças para montar desde o início. Este livro foca em React como o primeiro bloco da construção da aplicação. Outros eventualmente serão adicionados.

Segundo, todas as partes são substituíveis. Isso torna o ecossistema de React um lugar de inovação. Várias soluções competem entre si e você pode escolher a que mais se aplica a você e ao contexto em que irá utilizá-la.

_Frameworks_ SPA da primeira geração surgiram com um nível mais profissional e mais engessados. React permanece com um caráter mais inovador e é adotado por muitas empresas que estão na vanguarda do pensamento tecnológico, como [Airbnb, Netflix e (obviamente) Facebook][3]. Todas elas investem no futuro de React e estão contentes com a biblioteca e com tudo que a cerca.

React é, provavelmente, uma das melhores escolhas para a construção de aplicações web atualmente. Apesar de cuidar apenas da camada de visão, [seu ecossistema forma um framework completo, flexível e intercambiável][4]. Em suma, React tem uma API enxuta, um fantástico ecossistema e uma grande comunidade. Você pode ler sobre minhas experiências passadas em [Por que saí de Angular e fui para React?][5].

Recomendo fortemente que você entenda a razão do porquê você escolheria React e não outra biblioteca ou framework. Estamos todos ávidos pela experiência de seguir para onde React pode nos levar nos próximos anos.

### Exercícios

* ler sobre [Por que saí de Angular e fui para React?][6]
* ler sobre [O ecossistema flexível de React][7]

## Pré-requisitos

Se você está migrando de um _framework_ SPA ou de uma biblioteca diferente, já deve estar familiarizado com o básico de desenvolvimento para a _web_. Mas, se está começando agora, deveria pelo menos se sentir confortável com HTML, CSS e JavaScript ES5 para aprender React. Este livro irá fazer uma transição suave para JavaScript ES6 e além. Encorajo você a entrar para o [Grupo no Slack][8] oficial para obter ajuda ou para ajudar outras pessoas.

### Editor e Terminal

E quanto ao ambiente de desenvolvimento?

Você precisa de um editor ou IDE e uma ferramenta de linha de comando (terminal). Se quiser, siga meu[guia de montagem de ambiente][9]. Ele foi feito para usuários de Mac OS, mas você pode encontrar ferramentas iguais ou equivalentes em outros sistemas operacionais. Existe também uma tonelada de artigos pela _web_ que irão lhe mostrar como configurar um ambiente de desenvolvimento de uma forma mais elaborada de acordo com o seu SO.

Opcionalmente, você pode utilizar o git e o GitHub quando estiver praticando os exercícios do livro, para guardar seus projetos e monitorar o progresso em seus repositórios. Segue um [pequeno guia][10] sobre como usar essas ferramentas. Mas, mais uma vez, não é obrigatório para acompanhar este guia e pode dar um pouco de trabalho caso você precise aprender tudo do começo. Se você é um novato no desenvolvimento _web_, pode pular esse passo e concentrar o foco nas partes essenciais ensinadas aqui.

### Node e NPM

Por último, mas não menos importante, você precisará ter [o node e o npm][11] instalados. Ambos são utilizados no gerenciamento de bibliotecas necessárias ao longo do caminho. Neste livro, você instalará pacotes node externos via npm (_node package manager_). Esses pacotes podem ser bibliotecas ou até _frameworks_ completos.

É possível verificar as versões de node e npm instaladas pela linha de comando. Caso você não obtenha nenhum resultado no terminal, significa que precisará instalar ambos antes de continuar. Abaixo, minhas versões no momento em que escrevia este livro:

{title="Linha de Comando",lang="text"}
~~~~~~~~
node --version
*v8.3.0
npm --version
*v5.5.1
~~~~~~~~

## node e npm

Este capítulo lhe submete a um pequeno curso intensivo em node e npm. Não explora todas as funcionalidades, mas dá todas as ferramentas necessárias. Se você já está familiarizado com ambos, sinta-se livre para pular para o próximo assunto.

O **node package manager** (npm) lhe possibilita instalar pacotes (**node packages**) externos pela linha de comando. Esses pacotes podem ser desde um conjunto de funções utilitárias até bibliotecas ou _frameworks_ completos. Quando adicionados, tornam-se dependências da sua aplicação. Você pode instalá-los tanto globalmente (no seu diretório global de pacotes node), quanto na pasta local do seu projeto.

Pacotes node globais são acessíveis de qualquer pasta no terminal e você precisa fazer a instalação apenas uma vez. Para tanto, digite em seu terminal:

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

A _flag_ `-g` diz ao npm para instalar o pacote globalmente. 

Pacotes locais são usados apenas na sua aplicação. Por exemplo, por tratar-se de uma biblioteca, React será adicionado como um pacote local que poderá ser importado para uso na sua aplicação. A instalação de qualquer pacotes local via terminal é feita através do comando:

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

No caso específico de React, teríamos:

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

O pacote instalado irá aparecer automaticamente em uma pasta chamada *node\_modules/* e será listado no arquivo *package.json* juntamente com as outras dependências do projeto.

Como inicializar o projeto com a pasta *node\_modules/* e o arquivo *package.json*? Existe um comando npm para isso. Quando executado, *package.json* é criado e somente com a existência desse arquivo no projeto é que é possível a instalação de novos pacotes locais via npm.

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

A _flag_ `-y` é um atalho para inicializar seu *package.json* com as configurações padrão. Caso não a use, você terá que decidir como configurar o arquivo. Feita a inicialização, você estará apto a instalar novos pacotes com o comando `npm install <package>`.

Mais uma coisa sobre o *package.json*: O arquivo habilita você a compartilhar o projeto com outros desenvolvedores sem ter que incluir todos os pacotes node. Ele contém todas as referências dos pacotes utilizados no projeto, chamados de dependências. Todos podem copiar seu projeto sem baixar as dependências e simplesmente instalar todos os pacotes utilizando `npm install` na linha de comando. O _script_ considera todas as dependências listadas no *package.json* e instala cada uma na pasta *node\_modules/*.

Quanto aos comandos do npm, gostaria de falar apenas sobre mais um:

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

A _flag_ `--save-dev` indica que o pacote node é usado apenas em ambiente de desenvolvimento, ou seja, não será usado em produção quando você implantar a aplicação no servidor. Mas, que tipo de pacote se enquadraria neste caso?

Imagine que você queira testar sua aplicação com a ajuda de um pacote node. Você precisa instalá-lo via npm, mas deseja evitar que seja adicionado no seu ambiente de produção. Atividades de teste devem acontecer durante o processo de desenvolvimento, não quando a aplicação já foi disponibilizada para o usuário. A este ponto, já deve ter sido testada e estar funcionando plenamente. Este exemplo é apenas um dos casos onde você desejaria utilizar a flag `--save-dev`.

Encontraremos mais comandos npm pelo caminho. Mas, por enquanto, esses serão suficientes.

### Exercícios:

* Configurando um projeto npm qualquer:
  * crie uma nova pasta com `mkdir <nome_da_pasta>`
  * navegue para ela com `cd <nome_da_pasta>`
  * execute `npm init -y` ou `npm init`
  * instale um pacote local como o React com `npm install react`
  * inspecione o arquivo *package.json* e a pasta *node\_modules/*
  * descubra você mesmo como desinstalar o pacote *react*
* leia mais sobre [npm][12]

## Instalação

Existem muitas formas de iniciar uma aplicação React.

A primeira é usar um CDN. Isso pode soar mais complicado do que realmente é, mas CDN é apenas a sigla para [content delivery network][13]. Muitas empresas possuem CDNs que hospedam arquivos publicamente para que as pessoas possam consumi-los. Esses arquivos podem ser de bibliotecas, como React, já que toda a biblioteca é empacotada em um simples arquivo JavaScript *react.js*. Ele pode ser hospedado em algum lugar e você pode requisitá-lo em sua aplicação.

Como usar um CDN para começar a trabalhar com React? Você pode adicionar a tag `<script>` _inline_ no seu HTML, apontando para a url do CDN. Você irá precisar de dois arquivos (duas bibliotecas): *react* e *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

Mas, eis a questão: por que você deveria usar um CDN quando tem o npm para instalar pacotes como React?

Quando sua aplicação possui um arquivo *package.json* em uma pasta inicializada como projeto npm usando `npm init -y`, você pode instalar *react* e *react-dom* pelo terminal. É possível, inclusive, instalar múltiplos pacotes em apenas uma linha com o npm.

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

Essa abordagem é frequentemente usada para adicionar React a uma aplicação existente, se esta for gerenciada com npm.

Mas isso não é o bastante, infelizmente. Você teria que configurar também o [Babel][14] para fazer com que sua aplicação seja capaz de reconhecer JSX (a sintaxe React) e JavaScript ES6. Babel "_transpila_" (do inglês _transpile_, tradução informal adotada pela comunidade) o seu código para que navegadores possam interpretar ES6 e JSX, pois nem todos conseguem fazê-lo naturalmente. Isso demanda muita configuração e uso de ferramentas, podendo ser aterrador para iniciantes em React ligar com tudo isso.

Por esta razão, o Facebook introduziu *create-react-app* como uma solução de trabalho com React sem a necessidade de configurações. O próximo capítulo lhe mostrará como montar sua aplicação utilizando essa ferramenta de inicialização (_bootstrap tool_).

* **Nota do Tradutor:** De agora em diante, ao longo do livro original, aparecerão cada vez mais termos como _bootstraping_ e _transpile_, que não possuem uma tradução exata na língua portuguesa. Além disso, em alguns casos, apesar de existir a palavra, o resultado da tradução de um termo técnico também pode soar estranho para estudantes e profissionais da área. Sendo assim, quando convir, manterei a palavra original e explicarei entre parênteses, se for necessário. Quando for possível e soar naturalmente, irei traduzir a sentença para algo de mesmo sentido no português.

### Exercícios:

* Leia mais a respeito de [instalações de React][15]

## _Setup_ sem nenhuma configuração

Em "O Caminho para aprender React", você usará [create-react-app][16] para montar a estrutura inicial da sua aplicação. Trata-se de um _kit_ de inicialização sem necessidade de configuração, de uso opcional, introduzido pelo Facebook em 2016. Cerca de [96% das pessoas perguntadas recomendam-no para iniciantes][17]. Com o *create-react-app*, a configuração e a instrumentação se desenvolvem automaticamente em segundo plano, enquanto o foco permanece na implementação da aplicação.

Para começar, será necessário fazer a instalação global do pacote. Assim, ele sempre estará disponível na linha de comando para criar suas aplicações React.

{title="Linha de Comando",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~

Confira a versão do *create-react-app* para ter certeza de que a instalação ocorreu com sucesso:

{title="Linha de Comando",lang="text"}
~~~~~~~~
create-react-app --version
*v1.4.1
~~~~~~~~

Agora você pode criar sua primeira aplicação. Iremos chamá-la de *hackernews*, mas você pode escolher um nome diferente. O processo leva alguns segundos e, logo após terminar, navegue para a pasta:

{title="Linha de Comando",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Agora você pode abrir a aplicação no editor de sua escolha. A estrutura a seguir (ou uma variação, dependendo da versão do _create-react-app_) lhe será apresentada:

{title="Estrutura de pastas",lang="text"}
~~~~~~~~
hackernews/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
~~~~~~~~

Não tem problema se você não entender tudo desde o início. Uma descrição curta dos arquivos e pastas:

* **README.md:** A extensão .md indica que o arquivo é do tipo _markdown_, usado como uma linguagem de marcação mais leve, com uma sintaxe de formatação de texto. Muitos projetos com código-fonte incluem um arquivo *README.md* para passar as instruções iniciais. Quando eventualmente você sincronizar seu projeto em uma plataforma como o GitHub, o conteúdo do _README.md_ será exibido na página inicial do repositório. Por ter usado *create-react-app*, seu *README.md* terá conteúdo igual ao do [repositório do create-react-app no GitHub][18].

* **node\_modules/:** Contém todos os pacotes node que foram instalados via npm. Uma vez que foi utilizado o _create-react-app_, alguns módulos já foram instalados para você. Normalmente, você nunca irá manipular diretamente o conteúdo desta pasta, devendo instalar e desinstalar pacotes usando o npm na linha de comando.

* **package.json:** Mostra uma lista de dependências de pacotes node e mais algumas outras configurações de projeto.

* **.gitignore:** Este arquivo indica todos os arquivos e pastas que não deveriam ser adicionados ao seu repositório quando utilizando git e que existirão apenas no seu projeto local. Um exemplo é a pasta *node\_modules/*. É suficiente compartilhar apenas o *package.json* com outras pessoas envolvidas para que elas sejam capazes de instalar sozinhas todas as dependências.

* **public/:** A pasta contém todos os arquivos do projeto quando este é preparado para produção. Todo o código que você escreveu na pasta *src/* será empacotado em um ou dois arquivos durante o _building_ e colocado na pasta _public_.

No fim das contas, você não tem que alterar os arquivos e pastas mencionados. De início, tudo que você precisa está localizado na pasta _src/_. O foco principal fica com o arquivo *src/App.js*, que será usado para implementar sua aplicação. Mais tarde, você deve querer estruturá-la em múltiplos arquivos, cada um contendo seu próprio (ou alguns) componente.

Você encontrará um arquivo *src/App.test.js* (para seus testes) e um *src/index.js* como ponto de entrada para o "mundo React". Encontrará também os arquivos *src/index.css* e *src/App.css* para aplicar estilos aos componentes e à aplicação de uma forma geral. Se abrir qualquer dos dois, verá que ambos já trazem as definições de um estilo padrão.

A aplicação *create-react-app* é um projeto npm. Além de permitir que utilizemos npm para instalar e desinstalar pacotes node ao projeto, traz os seguintes _scripts_ npm para serem usados na linha de comando:

{title="Linha de Comando",lang="text"}
~~~~~~~~
// Roda a aplicação em http://localhost:3000
npm start

// Executa os testes
npm test

// Pepara a aplicação para produção
npm run build
~~~~~~~~

Os _scripts_ são definidos no *package.json*.
Seu "esqueleto" de aplicação React está agora inicializado. A parte excitante vem agora com os exercícios, finalmente rodando sua aplicação no _browser_.

### Exercícios:

* Inicie sua aplicação com`npm start` e abra-a em seu navegador.
* Rode o _script_ interativo `npm test`
* Verifique o conteúdo da sua pasta *public/*, execute o _script_ `npm run build` e olhe novamente a pasta para ver que arquivos foram adicionados (você pode removê-los se quiser, mas eles não causam nenhum problema)
* Familiarize-se com a estrutura de pastas
* Faça o mesmo com o conteúdo de cada arquivo
* Leia mais a respeito de [the npm scripts e create-react-app][19]

## Introdução à JSX

Chegou o momento de você conhecer JSX, a sintaxe React. Como foi dito antes, *create-react-app* já montou uma estrutura inicial de aplicação para você. Todos os arquivos contêm alguma implementação _default_. 

Vamos ao código-fonte. Inicialmente, você irá trabalhar apenas com o arquivo *src/App.js*:

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Para que não se confunda com as declarações _import/export_ e com a palavra _class_, saiba que essas já são funcionalidades de JavaScript ES6. Iremos falar sobre isso novamente, mais tarde, neste mesmo capítulo.

No arquivo você tem uma ** classe de componente React ES6** (do inglês, _class component_) de nome App. É a declaração de um componente. Basicamente, depois de o ter declarado, você pode usá-lo como um elemento em qualquer lugar da sua aplicação. Será produzida uma **instância** do seu **componente**, ou, em outras palavras: o componente é instanciado.

O **elemento** retornado é especificado no método `render()` e os componentes são feitos de elementos. É importante entender as diferenças entre componente, instância e elemento.

Logo você verá que o componente App é instanciado, pois, se não o fosse, você não seria capaz de vê-lo renderizado no navegador. O componente é apenas a declaração, mas sem utilidade por si só. Você deve instanciá-lo em algum lugar usando JSX, assim: `<App />`.

O conteúdo do bloco _render_ se parece muito com HTML, mas é JSX. JSX lhe permite misturar HTML e JavaScript. É algo poderoso, mas confuso quando você já está acostumado a separar os dois. Por esta razão, é mais fácil começar com JSX usando apenas HTML básico, removendo qualquer outro conteúdo que possa ser uma distração no arquivo.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Welcome to the Road to learn React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Pronto. O método `render()` agora retorna apenas um HTML simples, sem JavaScript. Vamos definir o texto "_Welcome to the Road to learn React_" como uma variável, que pode ser usada dentro do seu JSX entre chaves.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
# leanpub-start-insert
        <h2>{helloWorld}</h2>
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Deve funcionar, quando você levantar sua aplicação novamente com `npm start` na linha de comando.

Você também deve ter notado o atributo `className`. Ele espelha o atributo `class` padrão de HTML. Por razões técnicas, JSX teve que substituir um punhado de atributos HTML. Você pode ver a lista completa em [atributos HTML suportados na documentação de React][20]. Eles seguem a convenção _camelCase_. No seu caminho aprendendo React, você irá se deparar com mais atributos específicos de JSX.

### Exercícios:

* Defina mais variáveis e as adicione ao seu código JSX
  * Use um objeto para representar um usuário com nome e sobrenome
  * Adicione as propriedades do objeto ao seu código JSX
* Leia mais sobre [JSX][21]
* Leia mais sobre [componentes, elementos e instâncias em React][22]

## ES6 const and let

I guess you noticed that we declared the variable `helloWorld` with a `var` statement. JavaScript ES6 comes with two more options to declare your variables: `const` and `let`. In JavaScript ES6, you will rarely find `var` anymore.

A variable declared with `const` cannot be re-assigned or re-declared. It cannot get mutated (changed, modified). You embrace immutable data structures by using it. Once the data structure is defined, you cannot change it.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// not allowed
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

A variable declared with `let` can get mutated.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

You would use it when you would need to re-assign a variable.

However, you have to be careful with `const`. A variable declared with `const` cannot get modified. But when the variable is an array or object, the value it holds can get updated. The value it holds is not immutable.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

But when to use each declaration? There are different opinions about the usage. I suggest using `const` whenever you can. It indicates that you want to keep your data structure immutable even though values in objects and arrays can get modified. If you want to modify your variable, you can use `let`.

Immutability is embraced in React and its ecosystem. That's why `const` should be your default choice when you define a variable. Still, in complex objects the values within can get modified. Be careful about this behavior.

In your application, you should use `const` over `var`.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

### Exercises:

* read more about [ES6 const][23]
* read more about [ES6 let][24]
* research more about immutable data structures
  * why do they make sense in programming in general
  * why are they used in React and its ecosystem

## ReactDOM

Before you continue with the App component, you might want to see where it is used. It is located in your entry point to the React world: the *src/index.js* file.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~~

Basically `ReactDOM.render()` uses a DOM node in your HTML to replace it with your JSX. That's how you can easily integrate React in every foreign application. It is not forbidden to use `ReactDOM.render()` multiple times across your application. You can use it at multiple places to bootstrap simple JSX syntax, a React component, multiple React components or a whole application. But in plain React application you will only use it once to bootstrap your whole component tree.

`ReactDOM.render()` expects two arguments. The first argument is JSX that gets rendered. The second argument specifies the place where the React application hooks into your HTML. It expects an element with an `id='root'`. You can open your *public/index.html* file to find the id attribute.

In the implementation `ReactDOM.render()` already takes your App component. However, it would be fine to pass simpler JSX as long as it is JSX. It doesn't have to be an instantiation of a component.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

### Exercises:

* open the *public/index.html* to see where the React applications hooks into your HTML
* read more about [rendering elements in React][25]

## Hot Module Replacement

There is one thing that you can do in the *src/index.js* file to improve your development experience as a developer. But it is optional and shouldn't overwhelm you in the beginning when learning React.

In *create-react-app* it is already an advantage that the browser automatically refreshes the page when you change your source code. Try it by changing the `helloWorld` variable in your *src/App.js* file. The browser should refresh the page. But there is a better way of doing it.

Hot Module Replacement (HMR) is a tool to reload your application in the browser. The browser doesn't perform a page refresh. You can easily activate it in *create-react-app*. In your *src/index.js*, your entry point to React, you have to add one little configuration.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

# leanpub-start-insert
if (module.hot) {
  module.hot.accept();
}
# leanpub-end-insert
~~~~~~~~

That's it. Try again to change the `helloWorld` variable in your *src/App.js* file. The browser shouldn't perform a page refresh, but the application reloads and shows the correct output. HMR comes with multiple advantages:

Imagine you are debugging your code with `console.log()` statements. These statements will stay in your developer console, even though you change your code, because the browser doesn't refresh the page anymore. That can be convenient for debugging purposes.

In a growing application a page refresh delays your productivity. You have to wait until the page loads. A page reload can take several seconds in a large application. HMR takes away this disadvantage.

The biggest benefit is that you can keep the application state with HMR. Imagine you have a dialog in your application with multiple steps and you are at step 3. Basically it is a wizard. Without HMR you would change the source code and your browser refreshes the page. You would have to open the dialog again and would have to navigate from step 1 to step 3. With HMR your dialog stays open at step 3. It keeps the application state even though the source code changes. The application itself reloads, but not the page.

### Exercises:

* change your *src/App.js* source code a few times to see HMR in action
* watch the first 10 minutes of [Live React: Hot Reloading with Time Travel][26] by Dan Abramov

## Complex JavaScript in JSX

Let's get back to your App component. So far you rendered some primitive variables in your JSX. Now you will start to render a list of items. The list will be sample data in the beginning, but later you will fetch the data from an external [API][27]. That will be far more exciting.

First you have to define the list of items.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

The sample data will reflect the data we will fetch later on from the API. An item in the list has a title, an url and an author. Additionally it comes with an identifier, points (which indicate how popular an article is) and a count of comments.

Now you can use the built-in JavaScript `map` functionality in your JSX. It enables you to iterate over your list of items to display them. Again you will use curly braces to encapsulate the JavaScript expression in your JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return <div>{item.title}</div>;
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Using JavaScript in HTML is pretty powerful in JSX. Usually you might have used `map` to convert one list of items to another list of items. This time you use `map` to convert a list of items to HTML elements.

So far, only the `title` will be displayed for each item. Let's display some more of the item properties.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return (
            <div>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

You can see how the map function is simply inlined in your JSX. Each item property is displayed in a `<span>` tag. Moreover the url property of the item is used in the `href` attribute of the anchor tag.

React will do all the work for you and display each item. But you should add one helper for React to embrace its full potential and improve its performance. You have to assign a key attribute to each list element. That way React is able to identify added, changed and removed items when the list changes. The sample list items come with an identifier already.

{title="src/App.js",lang=javascript}
~~~~~~~~
{list.map(function(item) {
  return (
# leanpub-start-insert
    <div key={item.objectID}>
# leanpub-end-insert
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

You should make sure that the key attribute is a stable identifier. Don't make the mistake of using index of the item in the array. The array index isn't stable at all. For instance, when the list changes its order, React will have a hard time identifying the items properly.

{title="src/App.js",lang=javascript}
~~~~~~~~
// don't do this
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

You are displaying both list items now. You can start your app, open your browser and see both items of the list displayed.

### Exercises:

* read more about [React lists and keys][28]
* recap the [standard built-in array functionalities in JavaScript][29]
* use more JavaScript expressions on your own in JSX

## ES6 Arrow Functions

JavaScript ES6 introduced arrow functions. An arrow function expression is shorter than a function expression.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function expression
function () { ... }

// arrow function expression
() => { ... }
~~~~~~~~

But you have to be aware of its functionalities. One of them is a different behavior with the `this` object. A function expression always defines its own `this` object. Arrow function expressions still have the `this` object of the enclosing context. Don't get confused when using `this` in an arrow function.

There is another valuable fact about arrow functions regarding the parenthesis. You can remove the parenthesis when the function gets only one argument, but have to keep them when it gets multiple arguments.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// allowed
item => { ... }

// allowed
(item) => { ... }

// not allowed
item, key => { ... }

// allowed
(item, key) => { ... }
~~~~~~~~

However, let's have a look at the `map` function. You can write it more concisely with an ES6 arrow function.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item => {
# leanpub-end-insert
  return (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

Additionally, you can remove the *block body*, meaning the curly braces, of the ES6 arrow function. In a *concise body* an implicit return is attached. Thus you can remove the return statement. That will happen more often in the book, so be sure to understand the difference between a block body and a concise body when using arrow functions.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item =>
# leanpub-end-insert
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
# leanpub-start-insert
)}
# leanpub-end-insert
~~~~~~~~

Your JSX looks more concise and readable now. It omits the function statement, the curly braces and the return statement. Instead a developer can focus on the implementation details.

### Exercises:

* read more about [ES6 arrow functions][30]

## ES6 Classes

JavaScript ES6 introduced classes. A class is commonly used in object-oriented programming languages. JavaScript was and is very flexible in its programming paradigms. You can do functional programming and object-oriented programming side by side for their particular use cases.

Even though React embraces functional programming, for instance with immutable data structures, classes are used to declare components. They are called ES6 class components. React mixes the good parts of both programming paradigms.

Let's consider the following Developer class to examine a JavaScript ES6 class without thinking about a component.

{title="Code Playground",lang="javascript"}
~~~~~~~~
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
~~~~~~~~

A class has a constructor to make it instantiable. The constructor can take arguments to assign it to the class instance. Additionally a class can define functions. Because the function is associated with a class, it is called a method. Often it is referenced as a class method.

The Developer class is only the class declaration. You can create multiple instances of the class by invoking it. It is similar to the ES6 class component, that has a declaration, but you have to use it somewhere else to instantiate it.

Let's see how you can instantiate the class and how you can use its methods.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React uses JavaScript ES6 classes for ES6 class components. You already used one ES6 class component.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

The App class extends from `Component`. Basically you declare the App component, but it extends from another component. What does extend mean? In object-oriented programming you have the principle of inheritance. It is used to pass over functionalities from one class to another class.

The App class extends functionality from the Component class. To be more specific, it inherits functionalities from the Component class. The Component class is used to extend a basic ES6 class to a ES6 component class. It has all the functionalities that a component in React needs to have. The render method is one of these functionalities that you have already used. You will learn about other component class methods later on.

The `Component` class encapsulates all the implementation details of a React component. It enables developers to use classes as components in React.

The methods a React `Component` exposes is the public interface. One of these methods has to be overridden, the others don't need to be overridden. You will learn about the latter ones when the book arrives at lifecycle methods in a later chapter. The `render()` method has to be overridden, because it defines the output of a React `Component`. It has to be defined.

Now you know the basics around JavaScript ES6 classes and how they are used in React to extend them to components. You will learn more about the Component methods when the book describes React lifecycle methods.

### Exercises:

* read more about [ES6 classes][31]

{pagebreak}

You have learned to bootstrap your own React application! Let's recap the last chapters:

* React
  * create-react-app bootstraps a React application
  * JSX mixes up HTML and JavaScript to define the output of React components in their render methods
  * components, instances and elements are different things in React
  * `ReactDOM.render()` is an entry point for a React application to hook React into the DOM
  * built-in JavaScript functionalities can be used in JSX
	* map can be used to render a list of items as HTML elements
* ES6
  * variable declarations with `const` and `let` can be used for specific use cases
	* use const over let in React applications
  * arrow functions can be used to keep your functions concise
  * classes are used to define components in React by extending them

It makes sense to take a break at this point. Internalize the learnings and apply them on your own. You can experiment with the source code you have written so far.

You can find the source code in the [official repository][32].

[1]:	https://en.wikipedia.org/wiki/Single-page_application
[2]:	https://de.wikipedia.org/wiki/Model_View_Controller
[3]:	https://github.com/facebook/react/wiki/Sites-Using-React
[4]:	https://www.robinwieruch.de/essential-react-libraries-framework/
[5]:	https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/
[6]:	https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/
[7]:	https://www.robinwieruch.de/essential-react-libraries-framework/
[8]:	https://slack-the-road-to-learn-react.wieruch.com/
[9]:	https://www.robinwieruch.de/developer-setup/
[10]:	https://www.robinwieruch.de/git-essential-commands/
[11]:	https://nodejs.org/en/
[12]:	https://docs.npmjs.com/
[13]:	https://en.wikipedia.org/wiki/Content_delivery_network
[14]:	http://babeljs.io/
[15]:	https://facebook.github.io/react/docs/installation.html
[16]:	https://github.com/facebookincubator/create-react-app
[17]:	https://twitter.com/dan_abramov/status/806985854099062785
[18]:	https://github.com/facebookincubator/create-react-app
[19]:	https://github.com/facebookincubator/create-react-app
[20]:	https://facebook.github.io/react/docs/dom-elements.html
[21]:	https://facebook.github.io/react/docs/introducing-jsx.html
[22]:	https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html
[23]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const
[24]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let
[25]:	https://facebook.github.io/react/docs/rendering-elements.html
[26]:	https://www.youtube.com/watch?v=xsSnOQynTHs
[27]:	https://www.robinwieruch.de/what-is-an-api-javascript/
[28]:	https://facebook.github.io/react/docs/lists-and-keys.html
[29]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map
[30]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions
[31]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes
[32]:	https://github.com/rwieruch/hackernews-client/tree/4.1