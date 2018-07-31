# Etapas Finais para Produção

Os últimos capítulos irão mostrar como implantar sua aplicação em produção. Você irá utilizar o serviço grátis de hospedagem _Heroku_. No meio do caminho, você irá aprender mais sobre o _create-react-app_.

## Ejetando

O passo (e o conhecimento) a seguir **não é realmente necessário** para que sua aplicação seja implantada em produção. Mesmo assim, quero explicá-lo para você. _create-react-app_ traz uma funcionalidade para deixá-lo extensível e também prevenir amarrações, que podem ocorrer quando você compra uma tecnologia que não lhe dá possibilidades de livrar-se dela no futuro. Felizmente, _create-react-app_ possui está válvula de escape com "eject".

No seu arquivo _package.json_, você encontrará os _scripts_ para iniciar (_start_), testar (_test_) e construir (_build_) sua aplicação. Um último _script_ é _eject_. Uma vez que você use ele, não existe caminho de volta. **É uma operação definitiva. Uma vez que você ejetou sua aplicação, não é possível voltar atrás!** Se você apenas iniciou o aprendizado de React, não faz muito sentido abandonar o ambiente conveniente provido pelo _create-react-app_.

Se você executar `npm run eject`, o comando irá copiar todas as configurações e dependências para o seu _package.json_ e para uma nova pasta _config/_. Você converteria o projeto inteiro em uma configuração customizada, com ferramentas que incluem _Babel_ e _Webpack_. No final, você teria total controle sobre elas.

A documentação oficial recomenda *create-react-app* para projetos pequenos e médios. Não se sinta obrigado a utilizar o comando "eject" nas suas aplicações.

### Exercícios:

* Leia mais sobre [eject][1]

## Implante sua Aplicação

Nenhuma aplicação deveria permanecer para sempre em _localhost_, você deve querer publicá-la. _Heroku_ é uma plataforma em forma de serviço onde você pode hospedar seus _apps_. Ele oferece uma suave integração com React, sendo possível implantar uma aplicação criada com _create-react-app_ em minutos, sem configuração alguma, seguindo a mesma filosofia desta ferramenta.

Você precisa cumprir dois passos antes de poder implantar sua aplicação no _Heroku_:

* instalar a [Heroku CLI][2]
* criar uma [conta gratuita no Heroku][3]

Se você possui o _Homebrew_ no seu computador, poderá instalar a Heroku CLI direto da linha de comando:

{title="Linha de Comando",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Agora, você pode utilizar _git_ e _Heroku CLI_ para implantar a sua aplicação.

{title="Linha de Comando",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

É o bastante. Espero que sua aplicação esteja rodando agora. Se tiver algum problema, você pode recorrer aos seguintes recursos:

* [Git and GitHub Essentials][4]
* [Deploying React with Zero Configuration][5]
* [Heroku Buildpack for create-react-app][6]

[1]:	https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup
[2]:	https://devcenter.heroku.com/articles/heroku-command-line
[3]:	https://www.heroku.com/
[4]:	https://www.robinwieruch.de/git-essential-commands/
[5]:	https://blog.heroku.com/deploying-react-with-zero-configuration
[6]:	https://github.com/mars/create-react-app-buildpack