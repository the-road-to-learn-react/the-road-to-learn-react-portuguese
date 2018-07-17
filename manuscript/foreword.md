# Prefácio

"O Caminho para aprender React" ensina os fundamentos de React. Ao longo do livro, você irá construir uma aplicação de verdade em React puro, sem fazer uso de ferramentas complicadas. Tudo será explicado, da configuração do projeto, até a sua implantação em um servidor.

O livro traz referências à exercícios e materiais de leitura adicional em cada capítulo. Depois de lê-lo, você será capaz de construir suas próprias aplicações em React. Os recursos são mantidos atualizados por mim e pela comunidade.

Minha intenção neste livro é oferecer uma base sólida, antes de mergulhar em um ecossistema mais amplo. Poucas ferramentas e gerenciamento externo de estado, mas muita informação a respeito de React. Ele explica conceitos gerais, padrões e melhores práticas em uma aplicação React do mundo real.

Você irá aprender a construir sua própria versão do Hacker News. Ela cobre funcionalidades como paginação, _client-side caching_ e operações de busca e ordenação. Além disso, irá paulatinamente fazer a transição de JavaScript ES5 para JavaScript ES6. Espero que este livro transmita meu entusiasmo por React e JavaScript e ajude você a ser iniciado.

{pagebreak}

# Sobre o Author

Robin Wieruch é um engenheiro de software alemão, que se dedica a aprender e ensinar programação em JavaScript. Depois de obter seu mestrado em ciência da computação, ele continuou estudando por conta própria, diariamente. Ganhou experiência no mundo das _startups_, onde ele utilizou excessivamente JavaScript, não só quando estava trabalhando, mas também no seu tempo livre. Isso lhe deu a oportunidade de ensinar os outros sobre estes tópicos.

Por alguns anos, Robin trabalhou ao lado de um grande time de engenheiros, em uma companhia chamada [Small Improvements][1], desenvolvendo aplicações de larga escala. Esta companhia oferece um produto no formato SaaS, que permite que clientes dêem o seu _feedback_ para empresas, desenvolvida usando JavaScript na camada de _frontend_ e Java na de _backend_. No _frontend_, a primeira funcionalidade foi escrita em Java com o _Wicket Framework_ e jQuery. Quando a primeira geração de SPAs tornou-se popular, a companhia migrou para Angular 1.x. Depois de utilizar Angular por mais de dois anos, tornou-se claro que esta não era a melhor solução da época para se trabalhar com aplicações fortemente baseadas em estado. Por esta razão, a empresa saltou para React e Redux, o que lhe permitiu operar em larga escala com sucesso.

Durante seu tempo na empresa, Robin escreveu artigos sobre desenvolvimento _web_ regularmente no seu _website_. Ele recebeu ótimo _feedback_ das pessoas sobre eles e isso lhe permitiu melhorar seu estilo de escrita e ensino. Artigo após artigo, Robin evoluiu sua habilidade de ensinar para outras pessoas. O primeiro tinha sido montado com muitas coisas juntas, o que sobrecarregava os estudantes. Mas, com o tempo, ele foi aperfeiçoando-se, focando e ensinando apenas um assunto.

Atualmente, Robin é um professor autônomo. Para ele, ver seus alunos prosperarem com os objetivos claros e os ciclos curtos de _feedback_, providos por ele, é algo que lhe completa. Esta é, afinal, uma coisa que você provavelmente aprenderia em uma empresa especializada em _feedback_, não é mesmo? Mas, se ele também não estivesse codificando, não seria capaz de ensinar nada. Por isso, ele investe o tempo que lhe sobra em programação. Você pode encontrar mais informações sobre Robin e formas de apoiar o seu trabalho em seu [website][2].

{pagebreak}

# Depoimentos

**[Muhammad Kashif][3]:** "O Caminho para Aprender React é um livro único, que eu recomendo para qualquer estudante ou profissional interessado em aprender react, do nível básico ao avançado. Ele é recheado de dicas pertinentes e técnicas difíceis de achar em qualquer outro lugar, notavelmente completo no seu uso de exemplos e referências para modelos de problemas, eu tenho 17 anos de experiência em desenvolvimento de aplicações web e desktop, e antes de ler este livro eu estava tendo problemas em aprender react, mas este livro funciona como mágica."

**[Andre Vargas][4]:** "O Caminho para Aprender React de Robin Wieruch é um livro impressionante! Muito do que eu aprendi sobre React e até ES6 foi através dele!"

**[Nicholas Hunt-Walker, Instrutor de Python na Seattle Coding School][5]:** "Esse é um dos livros mais bem escritos e informativos com o qual eu já trabalhei. Uma sólida introdução à React & ES6."

**[Austin Green][6]:** "Obrigado, realmente amei o livro. Mistura perfeita para aprender React, ES6, e conceitos avançados de programação."

**[Nicole Ferguson][7]:** "Estou fazendo o curso O Caminho para Aprender React de Robin este final de semana & eu quase que me sinto culpada por me divertir tanto."

**[Karan][8]:** "Acabo de terminar o seu Caminho para React. Melhor livro do mundo para iniciantes em React e JS. Exposição elegante de ES. Felicitações! :)"

**[Eric Priou][9]:** "O Caminho para aprender React de Robin Wieruch é leitura obrigatória, simples e concisa para React e JavaScript."

**Desenvolvedor Novato:** "Acabo de terminar o livro mesmo sendo um desenvolvedor iniciante, obrigado por ter trabalhado nisso. Foi fácil de acompanhar e eu me sinto confiante para começar um novo _app_ do zero nos próximos dias. O livro se mostrou muito melhor que o tutorial oficial do React.js que eu tentei anteriormente (e que não pude completar devido à escassez de detalhes). Os exercícios ao final de cada seção foram muito gratificantes."

**Estudante:** "O melhor livro para começar a aprender ReactJS. O projeto acompanha os conceitos aprendidos o que ajuda a pegar o assunto. Descobri que 'Codifique e aprenda' é o melhor jeito de dominar programação e este livro faz exatamente isso."

**[Thomas Lockney][10]:** "Introdução sólida a React que não tenta ser muito abrangente. Eu queria apenas provar para entender de que se tratava e este livro me entregou exatamente isso. Não segui todas as notas de rodapé para aprender a respeito das novas características de ES6 que havia perdido ("_I wouldn't say I've been missing it, Bob._"). Porém, tenho certeza que aqueles de vocês que pararam e são aplicados para segui-las, provavelmente aprenderão muito mais do que apenas o que o livro ensina."

{pagebreak}

# Educação para Crianças

Este livro é _open source_ e deveria permitir que qualquer um aprenda React. Contudo, nem todos são privilegiados para fazer uso de recursos _open source_,  porque nem todo mundo é educado primariamente na língua inglesa. Mesmo que este livro seja um "pague o quanto quiser", eu gostaria de utilizá-lo para apoiar projetos que ensinam inglês para crianças no mundo em desenvolvimento.

* 11 a 18 de Abril de 2017, [Giving Back, By Learning React][11]

{pagebreak}

# FAQ

**Como recebo atualizações?** Você pode [assinar][12] a newsletter ou me seguir no [Twitter][13] para estar por dentro das atualizações. Uma vez que você obtenha uma cópia do livro, esta permanecerá atualizada até que uma nova edição seja lançada. Quando isso acontecer, você precisará fazer o _download_ novamente.

**O livro usa a versão mais recente de React?** O livro sempre é atualizado quando uma nova versão de React é lançada. De modo geral, livros ficam obsoletos logo após o lançamento. Mas, como este livro é de publicação própria, eu posso atualizá-lo quando quiser.

**Ele aborda Redux?** Não. Para isso, eu escrevi um segundo livro. "O Caminho para aprender React" deve lhe fornecer uma base sólida antes de você mergulhar em tópicos avançados. A implementação da aplicação de exemplo no livro irá mostrar que você não precisa de Redux para construir uma aplicação em React. Após ter terminado o livro, você deverá estar apto a implementar uma aplicação consistente sem Redux. Você então poderá ler meu segundo livro e aprender [Redux][14].

**Ele usa JavaScript ES6?** Sim, mas não se preocupe. Você se sairá bem se já for familiarizado com JavaScript ES5. Todas as características de ES6, que eu descrevo nesta jornada para aprender React, irão fazer a transição de ES5 para ES6 no livro. Todas serão explicadas ao longo do caminho. O livro não ensina apenas React, mas também todas as funcionalidades de JavaScript ES6 que são úteis para React.

**Você irá adicionar capítulos no futuro?** Você pode ver no "Registro de Mudanças" as atualizações mais significativas que já aconteceram. Também foram feitas outras melhorias não listadas. Em geral, depende da comunidade dizer quando eu devo continuar a trabalhar no livro. Se este tiver uma boa aceitação, eu irei providenciar mais capítulos e melhorar o material mais antigo. Continuarei mantendo o conteúdo atualizado com as melhores práticas, padrões e conceitos mais recentes.

**Quais são os formatos de leitura disponíveis?** Além dos formatos .pdf, .epub e .mobi, você pode ler o livro em _markdown_ puro no [GitHub][15]. Em geral, eu recomendo a leitura em formato adequado, caso contrário trechos de código irão ter quebras de linha indesejadas.

**Como posso obter ajuda enquanto leio o livro?** Existe um [Grupo no Slack][16] para pessoas que estão lendo o livro e você pode entrar no canal para obter ajuda ou ajudar outras pessoas. Afinal de contas, ajudar os outros também pode melhorar o seu aprendizado.

**Existe alguma seção de solução de problemas?** Se você se deparar com problemas, por favor, junte-se ao grupo no Slack. Você também pode verificar os [_issues_ abertos no GitHub][17] para o livro. Talvez seu problema até já tenha sido levantado e você poderá achar a solução. Caso contrário, não hesite em abrir um novo _issue_ onde você pode explicar sua dificuldade, talvez fornecer uma captura de tela e mais alguns detalhes (ex.: página do livro, versão do _node_). Eu tento enviar todas as correções nas próximas edições do livro.

**Posso ajudar a melhorar o livro?** Sim. Você pode ter um impacto direto com suas idéias e [contribuições no GitHub][18]. Não tenho a pretensão de ser um especialista nem de escrever em inglês nativo. Eu agradeceria muito a sua ajuda.

**Por que posso pagar quanto quiser pelo livro?** Eu me dediquei bastante a este trabalho e irei continuar a fazê-lo no futuro. Meu desejo é alcançar o máximo de pessoas, todos devem estar aptos a aprender React. Mas existe a possibilidade de você pagar, caso possa. Além disso, [o livro é uma tentativa de apoiar projetos que educam crianças em países em desenvolvimento][19]. Você também pode fazer a diferença.

**Posso apoiar o projeto?** Sim. Sinta-se livre para entrar em contato. Eu dedico bastante tempo a tutoriais _open source_ e recursos de aprendizagem. Você pode visitar minha página [sobre mim][20]. Eu também adoraria ter você como meu [Patron no Patreon][21].

**Sou encorajado a participar ativamente?** Sim. Quero que você pare um momento para pensar em alguém que seria um bom candidato a aprender React. A pessoa pode já ter mostrado interesse, ou estar no meio do aprendizado ou ainda nem ter despertado a vontade de aprender ainda. Aborde essa pessoa e compartilhe o livro. Significaria muito para mim. Ele foi feito com a intenção de ser distribuído.

{pagebreak}

# Registro de Mudanças

**10 de janeiro de 2017:**

* [v2 Pull Request][22]
* ainda mais amigável para iniciantes
* 37% mais conteúdo
* 30% de conteúdo melhorado
* 13 novos (ou melhorados) capítulos
* 140 páginas de material
* [+ curso interativo do livro em educative.io][23]

**08 de março de 2017:**

* [v3 Pull Request][24]
* 20% mais conteúdo
* 25% de conteúdo melhorado
* 9 novos capítulos
* 170 páginas de material

**15 de abril de 2017:**

* atualização para React 15.5

**5 de julho de 2017:**

* atualização para node 8.1.3
* atualização para npm 5.0.4
* atualização para create-react-app 1.3.3

**17 de outubro de 2017:**

* atualização para node 8.3.0
* atualização para npm 5.5.1
* atualização para create-react-app 1.4.1
* atualização para React 16
* [v4 Pull Request][25]
* 15% mais conteúdo
* 15% de conteúdo melhorado
* 3 novos capítulos (_Bindings_, Tratamento de Eventos, Tratamento de Erros)
* 190+ páginas de material
* [+9 Projetos com código-fonte][26]

**17 de fevereiro de 2018:**

* atualização para node 8.9.4
* atualização para npm 5.6.0
* atualização pra create-react-app 1.5.1
* [v5 Pull Request][27]
* mais formas de aprendizado
* material de leitura extra
* 1 novo capítulo (Axios ao invés de Fetch)

{pagebreak}

# Como ler o livro?

Esta é uma tentativa minha de lhe ensinar enquanto você escreve uma aplicação. É um guia prático, não uma referência sobre React. Você irá escrever uma versão própria do Hacker News que interage com uma API de verdade. Entre os vários tópicos interessantes, estão o gerenciamento de estado em React, _caching_ e operações como ordenações e buscas. Durante o processo, você irá aprender as melhores práticas e padrões em React.

O livro também lhe provê uma transição de JavaScript ES5 para JavaScript ES6. React adota muitas funcionalidades do JavaScript ES6 e eu gostaria de lhe mostrar como utilizá-las.

Em geral, um capítulo irá continuar de onde o anterior parou, ensinando-lhe alguma coisa nova. Não se apresse para terminar o livro. Você deve internalizar o conhecimento em cada passo. Você também pode aplicar suas próprias implementações e ler mais a respeito do tópico em questão. Ao final de cada capítulo, disponibilizo alguns exercícios e material de leitura. Se você quer aprender React, eu recomendo fortemente que leia o material extra e que "mele as mãos" com os exercícios. Sinta-se confortável com o que aprendeu antes de passar para o próximo assunto.

No fim, você irá ter uma aplicação React completa em produção e eu tenho grande interesse em ver seus resultados. Portanto, sinta-se livre para entrar em contato quando terminar. O último capítulo irá lhe prover várias opções para continuar sua jornada. Você irá encontrar muitos tópicos relacionados a React no [meu website pessoal][28].

Como você está lendo este livro, suponho que seja novo em React. Isso é ótimo. Espero receber o seu _feedback_ sobre como posso melhorar o material para que qualquer um possa aprender. Você pode fazer a diferença diretamente no [GitHub][29] ou falar comigo no [Twitter][30].

{pagebreak}

[1]:	https://www.small-improvements.com/
[2]:	https://www.robinwieruch.de/about
[3]:	https://twitter.com/appsdevpk/status/848625244956901376
[4]:	https://twitter.com/andrevar66/status/853789166987038720
[5]:	https://twitter.com/nhuntwalker/status/845730837823840256
[6]:	https://twitter.com/AustinGreen/status/845321540627521536
[7]:	https://twitter.com/nicoleffe/status/833488391148822528
[8]:	https://twitter.com/kvss1992/status/889197346344493056
[9]:	https://twitter.com/erixtekila/status/840875459730657283
[10]:	https://www.goodreads.com/review/show/1880673388
[11]:	https://www.robinwieruch.de/giving-back-by-learning-react/
[12]:	https://www.getrevue.co/profile/rwieruch
[13]:	https://twitter.com/rwieruch
[14]:	https://roadtoreact.com/course-details?courseId=TAMING_THE_STATE
[15]:	https://github.com/rwieruch/the-road-to-learn-react
[16]:	https://slack-the-road-to-learn-react.wieruch.com/
[17]:	https://github.com/rwieruch/the-road-to-learn-react/issues
[18]:	https://github.com/rwieruch/the-road-to-learn-react
[19]:	https://www.robinwieruch.de/giving-back-by-learning-react/
[20]:	https://www.robinwieruch.de/about/
[21]:	https://www.patreon.com/rwieruch
[22]:	https://github.com/rwieruch/the-road-to-learn-react/pull/18
[23]:	https://www.educative.io/collection/5740745361195008/5676830073815040
[24]:	https://github.com/rwieruch/the-road-to-learn-react/pull/34
[25]:	https://github.com/rwieruch/the-road-to-learn-react/pull/72
[26]:	https://roadtoreact.com/course-details?courseId=THE_ROAD_TO_LEARN_REACT
[27]:	https://github.com/the-road-to-learn-react/the-road-to-learn-react/pull/105
[28]:	https://www.robinwieruch.de/
[29]:	https://github.com/rwieruch/the-road-to-learn-react
[30]:	https://twitter.com/rwieruch