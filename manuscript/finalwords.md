# Outline

Foi-se o último capítulo do livro. Espero que você tenha gostado da leitura e que ele tenha ajudado a deixar React um pouco mais popular no seu conceito. Se você gostou do livro, compartilhe-o como uma forma de aprender React com seus amigos. Ele pode ser dado a quem você quiser. Também significaria muito para mim se você puder dedicar 5 minutos do seu tempo, para escrever um _review_ sobre ele na [Amazon][1] ou no [Goodreads][2].

**Então, onde devo ir, agora que li este livro?** Você pode estender a aplicação por conta própria ou até tentar construir o seu primeiro projeto React. Antes de mergulhar em outro livro, curso ou tutorial, você deveria colocar a mão na massa com um projeto. Faça-o durante uma semana, coloque-o em produção em algum lugar e [me avise][3] para divulgá-lo. Eu estou curioso sobre o que você irá construir depois de ter consumido o livro.

Se você está procurando ir mais além com sua aplicação, posso recomendar vários caminhos distintos de aprendizado, uma vez que você utilizou apenas React puro neste livro:

* **Gerenciamento de Estado:** Você utilizou `setState()` e `this.state` de React para gerenciar e acessar o estado local de componentes. Este é o jeito ideal de começar. Contudo, em uma aplicação maior, você irá testar os [limites do estado local de componentes React][4]. Você pode, portanto, utilizar uma biblioteca de terceiros para gerenciamento de estados como [Redux or MobX][5]. Na plataforma de cursos [Road to React][6], você irá encontrar o curso “Taming the State in React”, que ensina o gerenciamento avançado de estado em React, com Redux e MobX. Ele disponibiliza um e-book, mas eu recomendo a todos que mergulhem fundo no código-fonte e nos _screencasts_ que o acompanham. Se você gostou deste livro, definitivamente deveria levar o Taming the State in React.

* **Conexão com um Banco de Dados e/ou Autenticação:** Em uma aplicação React que está crescendo, você pode, eventualmente, querer persistir dados. Eles devem ser armazenados em um banco de dados para que possam sobreviver entre diferentes sessões de um navegador e serem compartilhados entre diferentes usuários da sua aplicação. A forma mais simples de introduzir um banco de dados é utilizando Firebase. Neste [abrangente tutorial][7], você irá encontrar um passo a passo sobre como utilizar autenticação Firebase (sign up, sign in, sign out, …) em React. Além disso, você irá utilizar o banco de dados Firebase para armazenar entidades do usuário. Depois disso, ficará a seu critério armazenar ou não mais dados no banco de dados da sua aplicação.

* **Adicionando ferramentas com Webpack e Babel:** In the book you have used *create-react-app* to set up your application. At some point, when you have learned React, you might want to learn the tooling around it. It enables you to setup your own project without *create-react-app*. I can recommend to follow a minimal setup with [Webpack and Babel][8]. Afterward, you can apply more tooling on your own. For instance, you could [use ESLint][9] to follow a unified code style in your application.

* **React Component Syntax:** The possibilities and best practices to implement React components evolve over time. You will find many ways to write your React components, especially React class components, in other learning resources. You can checkout [this GitHub repository][10] to find out about an alternative way to write React class components. By using the class field declarations, you can write them even more concise in the future.

* **Other Projects:** After learning plain React, it is always good to apply the learnings first in your own projects before learning something new. You could write your own tic-tac-toe game or a simple calculator in React. There are plenty of tutorials out there that use only React to build something exciting. Check out mine about building [a paginated and infinite scrolling list][11], [showcasing tweets on a Twitter wall][12] or [connecting your React application to Stripe for charging money][13]. Experiment with these mini applications to get comfortable in React.

* **UI Components:** You shouldn't make the mistake to introduce too early a UI component library in your project. First, you should learn how to implement and use a dropdown, checkbox or dialog in React with standard HTML elements from scratch. The major part of these components will manage their own local state. A checkbox has to know whether it is checked or not checked. Thus you should implement them as controlled components. After you went through all the foundational implementations, you can introduce a UI component library which gives you checkboxes and dialogs as React components.

* **Code Organization:** On your way reading the book, you came across one chapter about code organization. You could apply these changes now, if you haven't done it yet. It will organize your components in structured files and folders (modules). In addition, it helps you to understand and learn the principles of code splitting, reusability, maintainability and module API design. Eventually your application will grow in size and you will need to structure it in modules. So it's better you get started now.

* **Testing:** The book only scratched the surface of testing. If you are not familiar with the general topic, you could dive deeper into the concepts of unit testing and integration testing, especially in context of React applications. On an implementation level, I would recommend to stick to Enzyme and Jest in order to refine your approach of testing with unit tests and snapshot tests in React.

* **Routing:** You can implement routing for your application with [react-router][14]. So far, you only have one page in your application. React Router helps you to have multiple pages across multiple URLs. When you introduce routing to your application, you don't make any requests to your web server to request the next page. The router will do everything for you on the client-side. So get your hands dirty and introduce routing in your application.

* **Type Checking:** In one chapter, you have used React PropTypes to define component interfaces. It is a general good practice to prevent bugs. But the PropTypes are only checked on runtime. You can go one step further to introduce static type checking on compile time. [TypeScript][15] is one popular approach. But in the React ecosystem, people often use [Flow][16]. I can recommend to give Flow a shot, if you are interested to make your application more robust.

* **React Native:** [React Native][17] brings your application on mobile devices. You can apply your learnings from React to ship iOS and Android applications. The learning curve, once you have learned React, shouldn't be steep in React Native. Both share the same principles. You will only encounter different layout components on mobile than you are used to in web applications.

In general, I invite you to visit my [website][18] to find more interesting topics about web development and software engineering. You can [subscribe to my Newsletter][19] to get updates about articles, books, and courses. Furthermore, the course platform [Road to React][20] offers more advanced courses to learn about the React ecosystem. You should check it out!

Last but not least, I hope to find more [Patrons][21] who are able to support my content. There are many students out there who cannot afford to pay for educational content. That's why I put lots of my content out there for free. By supporting me in my doings as being my Patron, I can sustain these efforts to educate others for free.

Once again, if you liked the book, I want you to take a moment to think about a person who would be a good match to learn React. Reach out to that person and share the book. It would mean a lot to me. The book is intended to be given to others. It will improve over time when more people read it and share their feedback with me. I hope to see your feedback, review or rating as well!

Thank you a lot for reading the Road to learn React.

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