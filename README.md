# controle-de-classe-vue

App de Controle de Classe feito com Nuxt (Vue.JS), Vuetify e Firebase Firestore (banco não relacional). Nesta app o(a) professor(a) pode consultar as tabelas com os nomes, notas e médias dos alunos por série (do quinto ano ao primeiro ano do Ensino Médio). Os dados dos alunos são todos lidos e persistidos num banco de dados Firestore na nuvem. As operações (CRUD) possíveis são: leitura dos dados da nuvem, adição de novos alunos e notas, edição de nomes e notas e exclusão de alunos. A qualquer momento pode ser gerada e baixada uma planilha em formato .csv dos dados no app.

Demo: Em breve!

Futuras implementações: mais validações, login com firebase, cada usuário com sua coleção única no db, testes e etc.

Instruções para rodar localmente:

Para rodar esta aplicação é necessário criar um app no console do Firebase, criar um banco Firestore e adicionar o objeto firebaseConfig no módulo components/firebaseConfig.js e inicialize-o com firebase.initializeApp(firebaseConfig). Cada turma deve possuir a sua própria coleção no banco e devem ser nomeadas no formato 'quinto-ano', 'sexto-ano' e etc.

Tela do App com o menu lateral aberto:

![Tela 1](https://raw.githubusercontent.com/oliveira-vh/controle-de-classe-vue/master/Screenshot.png)

## Build Setup

```bash
# instalar dependências
$ npm install

# serve com hot reload
$ npm run dev

# build para produção e dar launch no servidor
$ npm run build
$ npm run start

# gerar projeto estático
$ npm run generate
```

Para mais informações chequem [Nuxt.js docs](https://nuxtjs.org).
