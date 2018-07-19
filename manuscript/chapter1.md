# Introdução a React

Você pode estar se perguntando: Por que eu deveria aprender React, pra começo de conversa? Este capítulo é uma introdução ao assunto e pode lhe dar a resposta que procura. Você irá mergulhar no ecossistema de React, montando sua primeira aplicação sem a necessidade de nenhuma configuração customizada e, durante o processo, terá uma introdução à JSX e ReactDOM. Prepare-se para seus primeiros componentes React.

## Oi, meu nome é React.

**Por que você deveria se incomodar em aprender React?** Nos últimos anos, _single page applications_ ([SPA][1]) tornaram-se populares. _Frameworks_ como Angular, Ember e Backbone ajudaram desenvolvedores JavaScript a construir aplicações web modernas, indo além do que já se costumava fazer com jQuery e JavaScript puro. Existe uma ampla gama de _frameworks_ SPA e, olhando para suas respectivas datas de lançamento, a maioria pertence à primeira geração: Angular 2010, Backbone 2010 e Ember 2011. 

A primeira versão de React foi lançada em 2013 pelo Facebook. Não como um _framework_, mas como uma biblioteca. Ele é apenas o "V" do [MVC][2] (_model view controller_), que lhe permite renderizar componentes como elementos visualizáveis no _browser_. O ecossistema onde React se insere é que torna possível a construção de _single page applications_ .

E por que considerar o uso de React em detrimento à primeira geração de _frameworks_ SPA? Porque enquanto estes últimos tentam resolver várias coisas ao mesmo tempo, React apenas lhe ajuda a construir _views_. Uma biblioteca, não um _framework_, que segue uma simples idéia: a camada de visão é uma hierarquia de componentes.

Em React, você consegue manter o foco na criação das suas _views_ antes de introduzir outros aspectos na aplicação. Cada outro aspecto é como uma peça acoplável na sua SPA. Essa divisão em partes integráveis é essencial para construir uma aplicação mais madura, trazendo duas vantagens:

Primeiro, você pode aprender passo-a-passo cada parte, sem precisar se preocupar em entender tudo de só uma vez. Bem diferente de um _framework_, que lhe entrega todas as peças para montar desde o início. Este livro foca em React como o primeiro bloco da construção da aplicação. Outros eventualmente serão adicionados.

Segundo, todas as partes são substituíveis. Isso torna o ecossistema de React um lugar de inovação. Várias soluções competem entre si e você pode escolher a que mais se aplica a você e ao contexto em que irá utilizá-la.

_Frameworks_ SPA da primeira geração surgiram em um nível profissional e mais engessados. React permanece com um caráter mais inovador e é adotado por muitas empresas que estão na vanguarda do pensamento tecnológico, como [Airbnb, Netflix e (obviamente) Facebook][3]. Todas elas investem no futuro de React e estão contentes com a biblioteca e com tudo que a cerca.

React é uma das melhores escolhas para a construção de aplicações web atualmente. Apesar de cuidar apenas da camada de visão, [seu ecossistema forma um framework completo, flexível e intercambiável][4]. React tem uma API enxuta, um fantástico ecossistema e uma grande comunidade. Você pode ler sobre minhas experiências passadas em [Por que saí de Angular e fui para React?][5]. Recomendo fortemente que você entenda a razão do porquê você escolheria React e não outra biblioteca ou framework. Estamos todos ávidos pela experiência de seguir para onde React pode nos levar nos próximos anos.

### Exercícios

* Leia mais sobre [por que saí de Angular e fui para React][6]
* Leia mais sobre [O ecossistema flexível de React][7]
* Leia mais sobre [como aprender um _framework_][8]

## Pré-requisitos

Se você está migrando de um _framework_ SPA ou de uma biblioteca diferente, já deve estar familiarizado com o básico de desenvolvimento para a _web_. Mas, se está começando agora, deveria pelo menos se sentir confortável com HTML, CSS e JavaScript ES5 para aprender React. Este livro irá fazer uma transição suave para JavaScript ES6 e além. Encorajo você a entrar para o [Grupo no Slack][9] oficial para obter ajuda ou para ajudar outras pessoas.

### Editor e Terminal

E quanto ao ambiente de desenvolvimento?

Você precisa de um editor ou IDE e uma ferramenta de linha de comando (terminal). Se quiser, siga meu [guia de montagem de ambiente][10]. Ele foi feito para usuários de Mac OS, mas você pode encontrar ferramentas iguais ou equivalentes em outros sistemas operacionais. Existe também uma tonelada de artigos pela _web_ que irão lhe mostrar como configurar um ambiente de desenvolvimento de uma forma mais elaborada de acordo com o seu SO.

Opcionalmente, você pode utilizar o git e o GitHub quando estiver praticando os exercícios do livro, para guardar seus projetos e monitorar o progresso em seus repositórios. Segue um [pequeno guia][11] sobre como usar essas ferramentas. Mas, mais uma vez, não é obrigatório para acompanhar este guia e pode dar um pouco de trabalho caso você precise aprender tudo do começo. Se você é um novato no desenvolvimento _web_, pode pular esse passo e concentrar o foco nas partes essenciais ensinadas aqui.

### Node e NPM

Por último, mas não menos importante, você precisará ter [o node e o npm][12] instalados. Ambos são utilizados no gerenciamento de bibliotecas necessárias ao longo do caminho. Neste livro, você instalará pacotes node externos via npm (_node package manager_). Esses pacotes podem ser bibliotecas ou até _frameworks_ completos.

É possível verificar as versões de node e npm instaladas pela linha de comando. Caso você não obtenha nenhum resultado no terminal, significa que precisará instalar ambos antes de continuar. Abaixo, minhas versões no momento em que escrevia este livro:

{title="Linha de Comando",lang="text"}
  node --version
  *v8.3.0
  npm --version
  *v5.5.1

## node e npm

Esta seção do capítulo é um curso intensivo em node e npm. Ele não explora todas as funcionalidades, mas apresenta as ferramentas necessárias. Se você já está familiarizado com ambos, sinta-se livre para pular para o próximo assunto.

O **node package manager** (npm) possibilita a instalação de pacotes (**node packages**) externos pela linha de comando. Esses pacotes podem conter desde um conjunto de funções utilitárias até bibliotecas ou _frameworks_ completos e, quando adicionados, tornam-se dependências da sua aplicação. Você pode instalá-los tanto globalmente (no seu diretório global de pacotes node), quanto na pasta local do seu projeto.

Pacotes node globais são acessíveis de qualquer pasta no terminal e você só precisa fazer a instalação de cada pacote uma vez. Para tanto, digite em seu terminal:

{title="Linha de Comando",lang="text"}
  npm install -g <package>

A _flag_ `-g` diz ao npm para instalar o pacote globalmente. 

Já os pacotes locais são usados apenas na sua aplicação. React, por exemplo, por se tratar de uma biblioteca, será adicionado como um pacote local que poderá ser importado para uso interno. A instalação de qualquer pacote local via terminal é feita através do comando:

{title="Linha de Comando",lang="text"}
  npm install <package>

No caso específico de React, teríamos:

{title="Linha de Comando",lang="text"}
  npm install react

O pacote instalado irá aparecer automaticamente em uma pasta chamada *node\_modules/* e será listado no arquivo *package.json* juntamente com as outras dependências do projeto.

E como inicializar o projeto com a pasta *node\_modules/* e o arquivo *package.json*? Existe um comando npm para isso também. Quando executado, *package.json* é criado e somente com a existência desse arquivo no projeto é que é possível a instalação de novos pacotes locais.

{title="Linha de Comando",lang="text"}
  npm init -y

A _flag_ `-y` é um atalho para inicializar seu *package.json* com as configurações padrão. Caso não a use, você terá que informar alguns dados adicionais para configurar o arquivo. Feita a inicialização, você está apto a instalar novos pacotes com o comando `npm install <package>`.

Mais uma coisa sobre o *package.json*: O arquivo habilita você a compartilhar o projeto com outros desenvolvedores sem ter que incluir todos os pacotes node. Ele contém todas as referências dos pacotes utilizados no projeto, chamados de dependências. Todos podem copiar seu projeto sem baixar as dependências e simplesmente instalar todos os pacotes utilizando `npm install` na linha de comando. O _script_ considera todas as entradas listadas no *package.json* e instala cada uma na pasta *node\_modules/*.

Existe mais um comando npm sobre o qual eu gostaria de falar a respeito:

{title="Linha de Comando",lang="text"}
  npm install --save-dev <package>

A _flag_ `--save-dev` indica que o pacote node é usado apenas em ambiente de desenvolvimento, ou seja, não será usado em produção quando você implantar a aplicação no servidor. Mas, que tipo de pacote se enquadraria neste caso?

Imagine que você queira instalar um pacote para lhe ajudar a testar sua aplicação. Você precisa instalá-lo via npm, mas deseja evitar que seja adicionado no seu ambiente de produção. Atividades de teste devem acontecer durante o processo de desenvolvimento, não quando a aplicação já foi disponibilizada para o usuário e já deveria ter sido testada e estar funcionando plenamente. Este é apenas um dos casos onde você desejaria utilizar a flag `--save-dev`.

Encontraremos mais comandos npm pelo caminho. Mas, por enquanto, é suficiente.

Uma última coisa, que considero importante mencionar: Muitas pessoas optam por utilizar outro gerenciador de dependências para trabalhar com pacotes _node_ em suas aplicações. **Yarn** é um gerenciador que funciona de maneira muito similar ao **npm**. Ele tem a sua própria lista de comandos para executar as mesmas tarefas, mas você continua tendo acesso ao arquivo de pacotes do npm. Yarn nasceu para resolver alguns problemas que o npm não podia. Contudo, atualmente as duas ferramentas estão evoluindo realmente muito rápido e você pode escolher a que achar melhor.  

### Exercícios:

* Configurando um projeto npm qualquer:
  * Crie uma nova pasta com `mkdir <nome_da_pasta>`
  * Navegue para ela com `cd <nome_da_pasta>`
  * Execute `npm init -y` ou `npm init`
  * Instale um pacote local como React com `npm install react`
  * Olhe os conteúdos do arquivo *package.json* e da pasta *node\_modules/*
  * Descubra você mesmo como desinstalar o pacote *react*
* Leia mais sobre [npm][13]
* Leia mais sobre [yarn][14]

## Instalação

Existem muitas formas de começar a trabalhar com uma aplicação React.

A primeira delas é usar um CDN. Isso pode soar mais complicado do que realmente é, mas CDN é apenas a sigla para [content delivery network][15]. Muitas empresas possuem CDNs que hospedam arquivos publicamente para que as pessoas possam consumi-los. Esses arquivos podem ser de bibliotecas como React, já que toda a biblioteca é empacotada em um simples arquivo JavaScript *react.js*. Ele pode ser hospedado em algum lugar e você pode requisitá-lo em sua aplicação.

Como usar um CDN para começar a trabalhar com React? Simples. Você pode adicionar a tag `<script>` _inline_ no seu HTML, apontando para a url do CDN. Serão precisos dois arquivos (bibliotecas): *react* e *react-dom*.

{title="Code Playground",lang="javascript"}
  <script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>

Mas, eis a questão: Por que você deveria usar um CDN quando tem o npm para instalar pacotes como React?

Quando sua aplicação possui um arquivo *package.json* em uma pasta inicializada como projeto npm usando `npm init -y`, você pode instalar *react* e *react-dom* pelo terminal. É possível, inclusive, instalar múltiplos pacotes em apenas uma linha com o npm.

{title="Linha de Comando",lang="text"}
  npm install react react-dom

Essa é  uma abordagem frequentemente utilizada para adicionar React a uma aplicação existente, caso esta seja gerenciada com npm.

Mas, infelizmente, isso não é o bastante. Você teria que configurar também o [Babel][16] para fazer com que sua aplicação seja capaz de reconhecer JSX (a sintaxe React) e JavaScript ES6. O Babel "_transpila_" (tradução informal adotada pela comunidade para _transpile_) o seu código para que navegadores possam interpretar ES6 e JSX, pois nem todos conseguem fazê-lo naturalmente. Isso demanda muita configuração e uso de ferramentas, podendo ser aterrador para iniciantes em React lidarem com tudo isso.

Por esta razão, o Facebook introduziu *create-react-app* como uma solução de trabalho com React sem a necessidade de escrever configurações. O próximo capítulo lhe mostrará como montar sua aplicação utilizando essa ferramenta de inicialização (_bootstrap tool_).

* **Nota do Tradutor:** De agora em diante, ao longo do livro original, aparecerão cada vez mais termos como _bootstraping_ e _transpile_, que não possuem uma tradução exata na língua portuguesa. Além disso, em alguns casos, apesar de existir a palavra, o resultado da tradução de um termo técnico também pode soar estranho para estudantes e profissionais da área. Sendo assim, quando convir, manterei a palavra original e explicarei entre parênteses, se for necessário. Quando for possível e soar naturalmente, irei traduzir a sentença para algo de mesmo sentido no português.

### Exercícios:

* Leia mais a respeito de [instalações de React][17]

## _Setup_ sem nenhuma configuração

Em “The Road to learn React“, você usará [create-react-app][18] para montar a estrutura inicial da sua aplicação. Trata-se de um _kit_ de inicialização sem necessidade de configuração, de uso opcional, introduzido pelo Facebook em 2016. Cerca de [96% dos usuários de React recomendam-no para iniciantes][19]. Com o *create-react-app*, a configuração e a instrumentação se desenvolvem automaticamente em segundo plano, enquanto o seu foco permanece na implementação da aplicação.

Para começar, será necessário fazer a instalação global do pacote. Assim, ele sempre estará disponível na linha de comando para criar suas aplicações React.

{title="Linha de Comando",lang="text"}
  npm install -g create-react-app

Confira a versão do *create-react-app* para ter certeza de que a instalação ocorreu com sucesso:

{title="Linha de Comando",lang="text"}
  create-react-app --version
  *v1.4.1

Agora você pode criar sua primeira aplicação. Iremos chamá-la de *hackernews*, mas você pode escolher um nome diferente. O processo leva alguns segundos e, logo após terminar, navegue para a pasta:

{title="Linha de Comando",lang="text"}
  create-react-app hackernews
  cd hackernews

Você pode abrir a aplicação no editor de sua escolha. A estrutura a seguir (ou uma variação, dependendo da versão do _create-react-app_) lhe será apresentada:

{title="Estrutura de pastas",lang="text"}
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

Não tem problema se você não entender tudo desde o início. Eis uma descrição curta dos arquivos e pastas:

* **README.md:** A extensão .md indica que o arquivo é do tipo _markdown_, uma linguagem de marcação mais leve com uma sintaxe de formatação de texto. Muitos projetos com código-fonte incluem um arquivo *README.md* para passar as instruções iniciais. Quando eventualmente você sincronizar seu projeto em uma plataforma como o GitHub, o conteúdo do _README.md_ será exibido na página inicial do repositório. Por ter usado *create-react-app*, seu *README.md* terá conteúdo igual ao do [repositório do create-react-app no GitHub][20].

* **node\_modules/:** Contém todos os pacotes node instalados via npm pois, uma vez que foi utilizado o _create-react-app_, alguns módulos já foram instalados para você. Normalmente, você nunca irá manipular diretamente o conteúdo desta pasta, devendo instalar e desinstalar pacotes usando npm na linha de comando.

* **package.json:** Mostra uma lista de dependências de pacotes node e mais algumas outras configurações de projeto.

* **.gitignore:** Este arquivo indica todos os arquivos e pastas que não deveriam ser adicionados ao seu repositório quando utilizando git e que existirão apenas no seu projeto local. Um exemplo é a pasta *node\_modules/*. É suficiente compartilhar apenas o *package.json* com outras pessoas envolvidas para que elas sejam capazes de instalar sozinhas todas as dependências.

* **public/:** A pasta contém todos os arquivos do projeto quando este é preparado para produção. Todo o código que você escreveu na pasta *src/* será empacotado em um ou dois arquivos durante o _building_ e colocado na pasta _public_.

* **build/:** Esta pasta será criada quando você preparar o projeto para produção, contendo todos os arquivos que serão utilizados. Neste processo de preparação (_build_), todo o seu código nas pastas _src/_ e _public/_ será empacotado em alguns arquivos e colocados neste diretório.

* **manifest.json** e **registerServiceWorker.js:** Não se preocupe com estes arquivos por enquanto, pois não serão necessários neste projeto.   

No fim das contas, você não tem que alterar os arquivos e pastas mencionados acima. De início, tudo que você precisa está localizado na pasta _src/_. O foco principal fica com o arquivo *src/App.js*, que será usado para implementar sua aplicação. Mais tarde, você irá querer estruturá-la em múltiplos arquivos, cada um contendo seu próprio componente (ou mais de um).

Você encontrará um arquivo *src/App.test.js* (para seus testes) e um *src/index.js* como ponto de entrada para o "mundo React". Encontrará também os arquivos *src/index.css* e *src/App.css* para aplicar estilos aos componentes e à aplicação de uma forma geral. Se abrir qualquer dos dois, verá que ambos já trazem as definições de estilos padrão.

A aplicação *create-react-app* também é um projeto npm. Além de permitir que utilizemos npm para instalar e desinstalar pacotes node ao projeto, traz os seguintes _scripts_ npm para serem usados na linha de comando:

{title="Linha de Comando",lang="text"}
  // Roda a aplicação em http://localhost:3000
  npm start
  
  // Executa os testes
  npm test
  
  // Pepara a aplicação para produção
  npm run build

Os _scripts_ são definidos no *package.json*.
Seu "esqueleto" de aplicação React está agora criado. A parte excitante vem a seguir, com os exercícios, finalmente rodando sua aplicação no _browser_.

### Exercícios:

* Inicie sua aplicação com`npm start` e abra-a em seu navegador.
* Rode o _script_ interativo `npm test`
* Verifique o conteúdo da sua pasta *public/*, execute o _script_ `npm run build` e olhe novamente a pasta para ver quais arquivos foram adicionados (você pode removê-los se quiser, mas eles não causam nenhum problema)
* Familiarize-se com a estrutura de pastas
* Faça o mesmo com o conteúdo de cada arquivo
* Leia mais a respeito de [npm scripts e create-react-app][21]

## Introdução à JSX

Chegou o momento de você conhecer JSX, a sintaxe React. Como foi dito antes, *create-react-app* já montou uma estrutura inicial de aplicação para você. Todos os arquivos contêm alguma implementação _default_. 

Vamos ao código-fonte. Inicialmente, você irá trabalhar apenas com o arquivo *src/App.js*:

{title="src/App.js",lang=javascript}
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

Para que não se confunda com as declarações _import/export_ e com a palavra _class_, saiba que essas já são funcionalidades de JavaScript ES6. Iremos falar sobre isso mais tarde, neste mesmo capítulo.

No arquivo você tem uma ** classe de componente React ES6** (do inglês, um _class component_) de nome App. É a declaração de um componente. Basicamente, depois de o ter declarado, você poderá usá-lo como um elemento em qualquer lugar da sua aplicação. Será produzida uma **instância** do seu **componente**, ou, em outras palavras: o componente é instanciado.

O **elemento** retornado é especificado no método `render()`. Componentes são feitos de elementos. É importante entender as diferenças entre componente, instância e elemento.

Logo você verá que o componente App é instanciado, pois, se não o fosse, você não seria capaz de vê-lo renderizado no navegador. O componente é apenas a declaração, mas sem utilidade por si só. Você deve instanciá-lo em algum lugar usando JSX, assim: `<App />`.

O conteúdo do bloco _render_ se parece muito com HTML, mas é JSX. JSX lhe permite misturar HTML e JavaScript. É algo poderoso, mas confuso quando você já está acostumado a separar os dois. Por esta razão, é mais fácil começar com JSX usando apenas HTML básico, removendo qualquer outro conteúdo que possa ser uma distração no arquivo.

{title="src/App.js",lang=javascript}
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

Pronto. O método `render()` agora retorna apenas um HTML simples, sem JavaScript. Vamos definir o texto "_Welcome to the Road to learn React_" como uma variável, que pode ser usada dentro do seu JSX entre chaves.

{title="src/App.js",lang=javascript}
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

Deverá funcionar, quando você levantar sua aplicação novamente com `npm start` na linha de comando.

Você também deve ter notado o atributo `className`. Ele espelha o atributo `class` padrão de HTML. Por razões técnicas, JSX teve que substituir um punhado de atributos HTML. Você pode ver a lista completa em [atributos HTML suportados na documentação de React][22]. Eles seguem a convenção _camelCase_. No seu caminho aprendendo React, você irá se deparar com mais atributos específicos de JSX.

### Exercícios:

* Defina mais variáveis e as adicione ao seu código JSX
  * Use um objeto para representar um usuário com nome e sobrenome
  * Adicione as propriedades do objeto ao seu código JSX
* Leia mais sobre [JSX][23]
* Leia mais sobre [componentes, elementos e instâncias em React][24]

## ES6 const e let

Suponho que você tenha notado que declaramos a variável `helloWorld` com a palavra-chave `var`. JavaScript ES6 nos traz mais duas opções para declararmos variáveis: `const` e `let`. De agora em diante, você raramente irá encontrar `var` novamente.

Uma variável declarada com `const` não pode ter um novo valor atribuído a ela nem ser novamente declarada. Não pode ser modificada. Você adota o uso de estruturas de dados imutáveis quando utiliza `const`. Uma vez que sua estrutura de dados é definida, você não pode mais modificá-la.

{title="Code Playground",lang="javascript"}
  // não permitido
  const helloWorld = 'Welcome to the Road to learn React';
  helloWorld = 'Bye Bye React';

Uma variável definida com `let` pode ser modificada.

{title="Code Playground",lang="javascript"}
  // permitido
  let helloWorld = 'Welcome to the Road to learn React';
  helloWorld = 'Bye Bye React';

Você pode usar `let` quando achar de poderia precisar atribuir novo valor à variável.

Contudo, você deve ter cuidado com `const`. Uma variável declarada com `const` não pode ser modificada, mas, quando esta se trata de um _array_ ou objeto, os valores que ela armazena não são imutáveis e podem ser atualizados.

{title="Code Playground",lang="javascript"}
  // permitido
  const helloWorld = {
    text: 'Welcome to the Road to learn React'
  };
  helloWorld.text = 'Bye Bye React';

Então quando usar cada tipo de declaração? Existem diferentes opiniões sobre o melhor uso. Eu sugiro usar `const` sempre que possível, indicando que você quer manter sua estrutura de dados imutável, mesmo que os valores em objetos e _arrays_ possam ser modificados. Se você deliberadamente deseja modificar sua variável, use `let`.

Imutabilidade é uma característica que foi abraçada em React e no seu ecossistema. É por este motivo que`const` deveria ser sua escolha padrão quando definindo uma variável. Mais uma vez, os valores em objetos complexos ainda podem ser modificados. Tenha cuidado com esse comportamento.

Na sua aplicação, dê preferência a `const` sobre `var`.

{title="src/App.js",lang=javascript}
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

### Exercícios:

* Leia mais sobre [ES6 const][25]
* Leia mais sobre [ES6 let][26]
* Pesquise sobre imutabilidade de estruturas de dados
  * Descubra o porquê disso fazer sentido em geral em programação
  * Descubra porque é uma prática em React e em seu ecossistema

## ReactDOM

Antes de continuar trabalhando no componente App, você deve querer ver onde ele é utilizado, não é mesmo? Ele está localizado dentro do seu ponto de entrada no mundo React: o arquivo *src/index.js*.

{title="src/index.js",lang=javascript}
  import React from 'react';
  import ReactDOM from 'react-dom';
  import App from './App';
  import './index.css';
  
  ReactDOM.render(
    <App />,
    document.getElementById('root')
  );

Basicamente, `ReactDOM.render()` usa um _DOM node_ no HTML e o substitui com o seu JSX. Dessa forma, você pode facilmente integrar React em qualquer aplicação estranha à sua.

Não é proibido utilizar `ReactDOM.render()` muitas vezes na aplicação, você pode fazê-lo para habilitar o uso da sintaxe JSX, de um componente React, de múltiplos componentes React ou até uma aplicação inteira. Mas, numa aplicação React pura, você só usará este método uma vez, para carregar toda a sua árvore de componentes.

`ReactDOM.render()` espera dois argumentos. O primeiro é o código JSX que será renderizado. O segundo argumento especifica o lugar onde a aplicação React irá se acomodar em seu HTML. Ele espera um elemento com um `id='root'`. Abra o arquivo *public/index.html* você encontrará esse id como atributo de uma tag.

Na nossa implementação, `ReactDOM.render()` já recebe seu componente App. Contudo, não haveria problema se, no lugar, passássemos um simples código JSX, não sendo obrigatório que o argumento passado seja um componente instanciado.

{title="Code Playground",lang=javascript}
  ReactDOM.render(
    <h1>Hello React World</h1>,
    document.getElementById('root')
  );

### Exercícios:

* Abra o arquivo *public/index.html* e veja onde a aplicação React será alocada em seu HTML
* Leia mais a respeito da [renderização de elementos em React][27]

## _Hot Module Replacement_

Existe algo que você pode fazer no arquivo *src/index.js* para melhorar sua experiência de desenvolvimento. É opcional e não é necessário "enfiar goela abaixo" essa prática quando se está começando a aprender React.

Em uma aplicação criada com *create-react-app*,  o navegador atualiza a página exibida quando você altera o código fonte e isso já é uma vantagem. Faça o teste, alterando a variável `helloWorld` no seu arquivo *src/App.js*. A página será recarregada. Mas, existe uma maneira ainda melhor de fazê-lo.

_Hot Module Replacement_ - algo como "recarregamento de módulos em tempo real" - (ou HRM) é uma ferramenta que permite a atualização da aplicação em seu navegador, sem que este faça o recarregamento da página. Você pode facilmente ativar esse recurso, adicionando uma pequena configuração ao seu *src/index.js*:

{title="src/index.js",lang=javascript}
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

É só isso. Faça o teste novamente, alterando a variável `helloWorld` em seu *src/App.js*. O navegador não irá recarregar toda a página, mas a aplicação irá ser atualizada e mostrar a saída correta. 

HRM nos traz muitas vantagens:

Imagine que você está depurando o código fazendo chamadas `console.log()`. Como o navegador não atualiza mais a página todas as vezes que você altera e salva o código, as chamadas anteriores irão permanecer no console até que você não queira mais. Isso pode ajudar bastante no processo de depuração.

Em  uma aplicação que já está ficando grande, recarregamentos de página podem tirar sua produtividade. O tempo todo você tem que esperar que a página carregue novamente e isso pode demorar vários segundos em um app um pouco maior. HMR remove essa desvantagem.

Mas o maior benefício de usar HMR é o de que você consegue conservar o estado da aplicação por mais tempo. Imagine que você tem uma caixa de diálogo com uma sequência de passos e você está no passo 3 (procedimento bastante conhecido como um _wizard_). Sem HMR, ao realizar alterações no código-fonte, seu navegador automaticamente recarregará a página. Você terá que reiniciar o procedimento do passo 1 e navegar até o passo 3 para ver a modificação. Com HMR, sua janela permanece ativa no passo 3, mantendo o estado da aplicação mesmo depois das mudanças de código fonte. A aplicação em si recarrega, mas a página não.

### Exercícios:

* Mude o código-fonte do seu *src/App.js* algumas vezes para testar o uso de HMR
* Assista aos primeiros 10 minutos da apresentação [Live React: Hot Reloading with Time Travel][28], com Dan Abramov

## JavaScript dentro do código JSX

Voltemos ao componente App. Até então, você renderiza algumas variáveis primitivas em seu código JSX. Agora você irá começar a trabalhar com listas de itens. Inicialmente, a lista virá de uma amostra local de dados, mas depois você irá consultar os dados usando uma [API][29] externa, o que é muito mais empolgante.

Primeiro você precisa definir uma lista de itens.

{title="src/App.js",lang=javascript}
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

Esses dados irão refletir o modelo daqueles que iremos consultar mais tarde com a API. Um item da lista possui um título, uma url e um autor. Ele tem identificador, pontos (que indicam o quão popular é um artigo) e também um contador de comentários.

Com a lista em mãos, você agora pode usar a funcionalidade `map`, nativa de JavaScript, em seu código JSX. Ela lhe possibilita iterar sobre sua lista de itens e exibir seu conteúdo. Mais uma vez, você usará chaves para encapsular a expressão JavaScript no JSX.

{title="src/App.js",lang=javascript}
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

O uso de JavaScript dentro do HTML é algo muito poderoso em JSX. Normalmente, você teria utilizado `map` para converter uma lista de itens em outra lista de itens. Mas aqui você usa para converter uma lista de itens em elementos HTML.

Até então, apenas o `title` é exibido para cada item. Vamos adicionar mais propriedades:

{title="src/App.js",lang=javascript}
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

É possível enxergar como a função map é simplesmente invocada _inline_ no código JSX. Cada propriedade de item é exibida em uma tag `<span>`, com exceção da url, que colocamos no `href` da tag `<a>`.

React irá fazer todo o trabalho de exibir cada item. Contudo, você deve dar uma ajudá-lo a atingir todo o seu potencial e a ter uma melhor performance. Você deve dar um atributo `key` a cada elemento da lista. Essa é a forma de identificar que itens foram adicionados, modificados ou removidos quando a lista muda. Os itens do exemplo possuem um identificador que pode ser utilizado.

{title="src/App.js",lang=javascript}
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

Você deve se certificar de que o atributo key é um identificador único válido. Não cometa o erro de usar o índice do item no array, por exemplo. O índice não é um identificador estável, pois, quando a lista é reordenada, ficará difícil para o React identificar propriamente cada item.

{title="src/App.js",lang=javascript}
  // não faça isso
  {list.map(function(item, key) {
    return (
      <div key={key}>
        ...
      </div>
    );
  })}

Agora você está exibindo ambos os itens da lista. Inicie sua aplicação, abra o navegador e veja os dois sendo mostrados.

### Exercícios:

* Leia mais sobre [listas e _keys_ em React][30]
* Revise as [funcionalidades padrão de _arrays_ em JavaScript][31]
* Use mais expressões JavaScript no seu código JSX

## ES6 Arrow Functions

JavaScript ES6 introduziu _arrow functions_. Uma expressão com _arrow function_ é mais curta do que uma expressão com uma função convencional (utilizando a palavra `function`).

{title="Code Playground",lang="javascript"}
  // declaração com function
  function () { ... }
  
  // declaração com arrow function
  () => { ... }

Contudo, você precisa estar ciente das funcionalidades que essa sintaxe agrega. Uma delas é um comportamento diferente com com o objeto `this`. Uma função convencional sempre define seu próprio objeto `this`. _Arrow functions_ têm o objeto `this` do contexto que as contêm. Fique esperto quando utilizar `this` em funções definidas dessa forma.

Existe outro fato importante sobre _arrow functions_ com relação a parênteses. Você pode removê-los quando a função recebe apenas um argumento, mas precisa mantê-los quando recebe vários.

{title="Code Playground",lang="javascript"}
  // permitido
  item => { ... }
  
  // permitido
  (item) => { ... }
  
  // proibido
  item, key => { ... }
  
  // permitido
  (item, key) => { ... }

Olhemos a função `map`. Você pode reescrevê-la de forma mais concisa com uma _arrow function_ de ES6.

{title="src/App.js",lang=javascript}
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

Você também pode remover as chaves que delimitam o corpo da _arrow function_, pois este é conciso e o retorno é implícito. Se o fizer, você deve remover  também o `return`. Essa prática irá se repetir mais vezes pelo livro, então certifique-se que aprendeu a diferença entre um bloco de código como corpo da função e um corpo conciso de função quando estiver utilizando _arrow functions_.

{title="src/App.js",lang=javascript}
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

Seu código JSX parece mais conciso e legível agora. Ele omite a palavra-chave `function`, as chaves e o `return`. O desenvolvedor pode focar melhor nos detalhes da implementação.

### Exercícios:

* Leia mais a respeito de [ES6 _arrow functions_][32]

## Classes ES6

JavaScript ES6 introduziu classes, que são normalmente utilizadas em linguagens de programação orientadas a objetos. JavaScript sempre foi e ainda é muito flexível quanto aos seus paradigmas de programação. Você pode usar programação funcional e programação orientada a objetos lado a lado, quando mais apropriado for.

Apesar de React adotar programação funcional, por exemplo com estruturas de dados imutáveis, classes são usadas para declarar componentes. Elas são chamadas de componentes de classe de ES6 (_ES6 class components_). React mistura as boas partes de ambos os paradigmas de programação.

Consideremos a classe `Developer` a seguir, para que possamos examinar uma classe JavaScript ES6 sem ter que pensar sobre componentes.

{title="Code Playground",lang="javascript"}
  class Developer {
    constructor(firstname, lastname) {
      this.firstname = firstname;
      this.lastname = lastname;
    }
  
    getName() {
      return this.firstname + ' ' + this.lastname;
    }
  }

Uma classe tem um construtor para torná-la instanciável e ele pode receber argumentos para atribuí-los à instância da classe. Além disso, uma classe pode definir funções que, por estarem associadas, são chamadas de métodos. Geralmente, são chamados de métodos de classe.

`Developer` é apenas a declaração da classe. Você pode criar múltiplas instâncias invocando-a. É o mesmo raciocínio ao componente de classe de ES6, que tem uma declaração, mas que você precisa usá-lo em algum outro lugar para instanciá-lo.

Vejamos como você pode instanciar a classe e como pode utilizar seus métodos.

{title="Code Playground",lang="javascript"}
  const robin = new Developer('Robin', 'Wieruch');
  console.log(robin.getName());
  // saída: Robin Wieruch

React usa classes de JavaScript ES6 para componentes de classe ES6 e  você já utilizou um.

{title="src/App.js",lang=javascript}
  import React, { Component } from 'react';
  
  ...
  
  class App extends Component {
    render() {
      ...
    }
  }

A classe App herda de `Component`. Basicamente você declara o componente App, mas ele estende outro componente. O que isso significa? Em programação orientada a objetos, você tem o princípio da herança. Ele é usado para repassar funcionalidades de uma classe para outra.

App herda funcionalidades da classe `Component`. Ela é utilizada para transformar uma classe básica de ES6 em uma classe de componente ES6. Ela tem todas as funcionalidades que um componente em React precisa ter. O método render é uma dessas funcionalidades que você já utilizou. Você irá aprender sobre outros métodos de classes de componentes mais tarde.

A classe `Component` encapsula todos os detalhes de implementação de um componente React. Ela torna os desenvolvedores capazes de usar classes como componentes em React.

Os métodos que um `Component` React expõe são a sua interface pública. Um desses métodos tem que ser sobrescrito, os outros não. Você aprenderá sobre os outros futuramente quando o livro chegar nos métodos de ciclo de vida em capítulos posteriores. Mas o método `render()` deve ser sobrescrito, porque ele define a saída de um `Component` React.

Agora você sabe o básico sobre classes JavaScript de ES6 e como elas são usadas em React para tornarem-se componentes. Você aprenderá mais sobre os métodos de Component quando o livro for descrever os métodos de ciclo de vida de React.

### Exercícios:

* Leia mais sobre [classes ES6][33]

{pagebreak}

Você aprendeu a criar a estrutura inicial da sua própria aplicação React! Vamos recapitular os últimos tópicos:

* React
   * create-react-app inicializa a estrutura de uma aplicação React
   * JSX mistura HTML e JavaScript para definir a saída de componentes React em seus métodos render
   * Componentes, instâncias e elementos são coisas diferentes em React
   * `ReactDOM.render()` é o ponto de entrada de uma aplicação React e o gancho para o DOM
   * Funcionalidades nativas de JavaScript podem ser utilizadas em JSX
  * map pode ser usada para renderizar uma lista de itens como elementos HTML
* ES6
  * Declarações de variáveis com `const` e `let` podem ser usadas para casos de uso específicos
    * dê preferência ao uso de const ao invés de let em aplicações React
  * _Arrow functions_ podem ser usadas para manter a declaração de funções mais concisas
  * Classes são utilizadas para definir componentes em React através de herança

É prudente fazer um intervalo agora. Internalize o conhecimento adquirido e aplique-o por sua conta. Você pode brincar com o código fonte que escreveu até agora.

 O código-fonte está disponível no [repositório oficial][34].

[1]:	https://en.wikipedia.org/wiki/Single-page_application
[2]:	https://de.wikipedia.org/wiki/Model_View_Controller
[3]:	https://github.com/facebook/react/wiki/Sites-Using-React
[4]:	https://www.robinwieruch.de/essential-react-libraries-framework/
[5]:	https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/
[6]:	https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/
[7]:	https://www.robinwieruch.de/essential-react-libraries-framework/
[8]:	https://www.robinwieruch.de/how-to-learn-framework/
[9]:	https://slack-the-road-to-learn-react.wieruch.com/
[10]:	https://www.robinwieruch.de/developer-setup/
[11]:	https://www.robinwieruch.de/git-essential-commands/
[12]:	https://nodejs.org/en/
[13]:	https://docs.npmjs.com/
[14]:	https://yarnpkg.com/en/docs/
[15]:	https://en.wikipedia.org/wiki/Content_delivery_network
[16]:	http://babeljs.io/
[17]:	https://facebook.github.io/react/docs/installation.html
[18]:	https://github.com/facebookincubator/create-react-app
[19]:	https://twitter.com/dan_abramov/status/806985854099062785
[20]:	https://github.com/facebookincubator/create-react-app
[21]:	https://github.com/facebookincubator/create-react-app
[22]:	https://facebook.github.io/react/docs/dom-elements.html
[23]:	https://facebook.github.io/react/docs/introducing-jsx.html
[24]:	https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html
[25]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const
[26]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let
[27]:	https://facebook.github.io/react/docs/rendering-elements.html
[28]:	https://www.youtube.com/watch?v=xsSnOQynTHs
[29]:	https://www.robinwieruch.de/what-is-an-api-javascript/
[30]:	https://facebook.github.io/react/docs/lists-and-keys.html
[31]:	https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map
[32]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions
[33]:	https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes
[34]:	https://github.com/rwieruch/hackernews-client/tree/4.1