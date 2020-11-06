<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn
        icon
        @click.stop="miniVariant = !miniVariant"
      >
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-btn
        icon
        @click.stop="clipped = !clipped"
      >
        <v-icon>mdi-application</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer
      :absolute="!fixed"
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Início',
          to: '/'
        },
        {
          icon: 'mdi-numeric-5',
          title: '5º Ano',
          to: '/turma/quinto-ano'
        },
        {
          icon: 'mdi-numeric-6',
          title: '6º Ano',
          to: '/turma/sexto-ano'
        },
        {
          icon: 'mdi-numeric-7',
          title: '7º Ano',
          to: '/turma/setimo-ano'
        },
        {
          icon: 'mdi-numeric-8',
          title: '8º Ano',
          to: '/turma/oitavo-ano'
        },
        {
          icon: 'mdi-numeric-9',
          title: '9º Ano',
          to: '/turma/nono-ano'
        },
        {
          icon: 'mdi-numeric-1',
          title: '1º Ano - EM',
          to: '/turma/primeiro-ano-em'
        },
        {
          icon: 'mdi-about',
          title: 'Sobre',
          to: '/sobre'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Controle de Classe'
    }
  }
}
</script>
