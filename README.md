# controle-de-classe-nuxt

App de Controle de Classe feito com Nuxt (Vue.JS), Vuetify e Firebase (banco não relacional). Nesta app o(a) professor(a) pode consultar as tabelas com os nomes, notas e médias dos alunos por série (do quinto ano ao primeiro ano do Ensino Médio). Os dados dos alunos são todos lidos e persistidos num banco de dados Firestore na nuvem. As operações (CRUD) possíveis são: leitura dos dados da nuvem, adição de novos alunos e notas, edição de nomes e notas e exclusão de alunos.

Demo: Em breve!

Futuras implementações: mais validações, login com firebase, cada usuário com sua coleção única no db, testes, geração de .csv.

## Build Setup

```bash
# install dependencies
$ npm install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm run start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).
