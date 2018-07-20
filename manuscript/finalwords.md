# Outline

Foi-se o último capítulo do livro. Espero que você tenha gostado da leitura e que ele tenha ajudado a deixar React um pouco mais popular no seu conceito. Se você gostou do livro, compartilhe-o como uma forma de aprender React com seus amigos. Ele pode ser dado a quem você quiser. Também significaria muito para mim se você puder dedicar 5 minutos do seu tempo, para escrever um _review_ sobre ele na [Amazon][1] ou no [Goodreads][2].

**Então, onde devo ir, agora que li este livro?** Você pode estender a aplicação por conta própria ou até tentar construir o seu primeiro projeto React. Antes de mergulhar em outro livro, curso ou tutorial, você deveria colocar a mão na massa com um projeto. Faça-o durante uma semana, coloque-o em produção em algum lugar e [me avise][3] para divulgá-lo. Eu estou curioso sobre o que você irá construir depois de ter consumido o livro.

Se você está procurando ir mais além com sua aplicação, posso recomendar vários caminhos distintos de aprendizado, uma vez que você utilizou apenas React puro neste livro:

* **Gerenciamento de Estado:** Você utilizou `setState()` e `this.state` de React para gerenciar e acessar o estado local de componentes. Este é o jeito ideal de começar. Contudo, em uma aplicação maior, você irá testar os [limites do estado local de componentes React][4]. Você pode, portanto, utilizar uma biblioteca de terceiros para gerenciamento de estados como [Redux or MobX][5]. Na plataforma de cursos [Road to React][6], você irá encontrar o curso "Taming the State in React", que ensina o gerenciamento avançado de estado em React, com Redux e MobX. Ele disponibiliza um e-book, mas eu recomendo a todos que mergulhem fundo no código-fonte e nos _screencasts_ que o acompanham. Se você gostou deste livro, definitivamente deveria levar o Taming the State in React.

* **Conexão com um Banco de Dados e/ou Autenticação:** Em uma aplicação React que está crescendo, você pode, eventualmente, querer persistir dados. Eles devem ser armazenados em um banco de dados para que possam sobreviver entre diferentes sessões de um navegador e serem compartilhados entre diferentes usuários da sua aplicação. A forma mais simples de introduzir um banco de dados é utilizando Firebase. Neste [abrangente tutorial][7], você irá encontrar um passo a passo sobre como utilizar autenticação Firebase (sign up, sign in, sign out, …) em React. Além disso, você irá utilizar o banco de dados Firebase para armazenar entidades do usuário. Depois disso, ficará a seu critério armazenar ou não mais dados no banco de dados da sua aplicação.

* **Adicionando ferramentas com Webpack e Babel:** Neste livro, você utilizou *create-react-app* para configurar sua aplicação. Em algum momento futuro, quando você já tiver aprendido React, pode querer conhecer melhor o ferramental que o cerca, que lhe habilita a configurar seu próprio projeto sem o _create-react-app_. Eu recomendo seguir com um _setup_ mínimo com [Webpack and Babel][8]. Depois, você pode aplicar mais ferramentas por sua própria conta. Por exemplo, você poderia [usar ESLint][9] para seguir um estilo de código unificado em sua aplicação.

* **Sintaxe de Componentes React:** As possibilidades e melhores práticas de implementação de componentes React evoluem com o tempo. Você encontrará formas de escrever seus componentes React, especialmente componentes de classe em outros materiais de aprendizado. Você pode baixar [esse repositório do GitHub][10] para descobrir um jeito alternativo de escrever componentes de classe React. No futuro, você poderá escrevê-los ainda mais concisos utilizando declarações de campos de classe.

* **Outros Projetos:** Depois de aprender React puro, sempre é bom aplicar os conhecimentos adquiridos antes em seus projetos, antes de partir para aprender algo novo. Você poderia escrever seu próprio jogo da velha ou uma simples calculadora em React. Existe uma abundância de tutorials por aí, que usam apenas React para construir coisas excitantes. Confira os meus, sobre [a rolagem de uma lista paginada infinita][11], [um _showcase _ de tweets][12] ou [conectando sua aplicação React ao _Stripe_ para efetuar cobranças em dinheiro][13]. Experimente essas mini aplicações para ficar confortável com React.

* **Componentes de UI:** Não cometa o erro de investir muito cedo no aprendizado de uma bibliotecas de componentes de UI no seu projeto. Primeiro, você deveria aprender como implementar e usar um _dropdown_, um _checkbox_ ou uma caixa de diálogo em React com elementos de HTML puro, do zero. A maior parte desses componentes irá gerenciar o seu próprio estado local. Uma _checkbox_ precisa saber quando está marcada ou não. Desta forma, você deveria implementá-los como componentes controlados. Depois que passar por todas as implementações base, você pode se iniciar no uso de uma biblioteca de componentes de UI, que lhe dá _checkboxes_ e caixas de diálogo como componentes React.

* **Organização de Código:** Na sua jornada lendo o livro, você se deparou com um capítulo sobre organização de código. Você pode aplicar essas mudanças agora, se ainda não o fez. Organizar seus componentes em arquivos e pastas estruturados (módulos). Ademais, isto lhe ajudaria a entender e aprender princípios de separação, reusabilidade e manutenabilidade de código, além do projeto (_design_) de API de módulos.

* **Testes:** O livro apenas arranhou a superfície da disciplina de testes. Se você não é familiarizado com o tópico em geral, pode mergulhar mais fundo nos conceitos de testes unitários e de integração, especialmente no contexto de aplicações React. Em termos de implementação, eu recomendaria continuar com _Enzyme_ e _Jest_ para refinar sua técnica escrevendo testes de unidade e _snapshot tests_ em React.

* **_Routing_:** Você pode implementar o roteamento de páginas para sua aplicação com [react-router][14]. Até então, você só possui uma página na aplicação. React Router lhe ajuda a ter várias páginas através de múltiplas URLs. Quando você adiciona o roteamento de páginas à sua aplicação, nenhuma requisição à um servidor é feita para obter a próxima página. O _router_ irá fazer todo o seu trabalho do lado do cliente. Suje suas mãos, então, adicionando _routing_ à sua aplicação.

* **Checagem de Tipos:** Em um capítulo, você utilizou React _PropTypes_ para definir interfaces de componentes. É uma boa prática comum para prevenir _bugs_. Mas, _PropTypes_ só são checados em tempo de execução. Você pode ir um passo além, adicionando a checagem estática de tipos em tempo de compilação. [TypeScript][15] é uma abordagem popular mas, no ecossistema de React, as pessoas geralmente utilizam [Flow][16]. Eu recomento que você faça um teste com Flow, se estiver interessado a fazer com que suas aplicações sejam mais robustas.

* **React Native:** [React Native][17] lhe leva ao mundo dos dispositivos móveis. Você pode aplicar seus novos conhecimentos em React para entregar aplicativos iOS e Android. A curva de aprendizado de React Native, uma vez que você já sabe React, não deve ser muito íngrime. Ambos compartilham dos mesmos princípios. Você apenas irá encontrar componentes de _layout_ diferentes no mundo _mobile_, em relação aos que usa em aplicações web.

Em geral, eu o convido a visitar meu [website][18], para encontrar mais tópicos interessantes sobre desenvolvimento web e engenharia de software. Você pode [assinar minha Newsletter][19] para receber novidades sobre artigos, livros e cursos. Além disso, a plataforma de cursos [Road to React][20] oferece mais cursos avançados sobre o ecossistema de React. Vá lá e confira!

Por último, mas não menos importante, espero encontrar mais [Patrons][21] (patrocinadores) aptos a patrocinar meu conteúdo. Existem muito estudantes por aí que não podem dar conta de pagar por conteúdo educacional. Por este motivo, eu disponibilizo boa parte do meu conteúdo de forma gratuita. Apoiando meu trabalho como um Patron, garante que eu posso continuar meus esforços em prover educação gratuita.

Mais uma vez, se você gostou do livro, peço-lhe que reserve um momento para pensar em alguém que seria uma boa indicação para aprender React. Vá até esta pessoa e compartilhe o livro, significaria muito para mim. Ele é feito para ser passado para outras pessoas e irá melhorar ao longo do tempo, à medida que mais pessoas o lêem e compartilham suas opiniões de _feedback_ comigo. Espero também ver seu _feedback_, sua avaliação ou comentário, também!

Muito obrigado por ler **The Road to learn React**.

Robin

[1]:	https://www.amazon.com/dp/B077HJFCQX
[2]:	https://www.goodreads.com/book/show/37503118-the-road-to-learn-react
[3]:	https://twitter.com/rwieruch
[4]:	https://www.robinwieruch.de/learn-react-before-using-redux/
[5]:	https://www.robinwieruch.de/redux-mobx-confusion/
[6]:	https://roadtoreact.com/
[7]:	https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial/
[8]:	https://www.robinwieruch.de/minimal-react-webpack-babel-setup/
[9]:	https://www.robinwieruch.de/react-eslint-webpack-babel/
[10]:	https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax
[11]:	https://www.robinwieruch.de/react-paginated-list/
[12]:	https://www.robinwieruch.de/react-svg-patterns/
[13]:	https://www.robinwieruch.de/react-express-stripe-payment/
[14]:	https://github.com/ReactTraining/react-router
[15]:	https://www.typescriptlang.org/
[16]:	https://flowtype.org/
[17]:	https://facebook.github.io/react-native/
[18]:	https://www.robinwieruch.de
[19]:	https://www.getrevue.co/profile/rwieruch
[20]:	https://roadtoreact.com
[21]:	https://www.patreon.com/rwieruch