<template>
<div>
  <v-data-table
    :headers="headers"
    :items="alunos"
    hide-default-footer
    disable-pagination
    :loading="loading"
    loading-text="Carregando..."
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title class="titulo-turma">{{tituloDaTurma}}</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              Novo Aluno
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.nome"
                      label="Nome"
                      :rules="[rules.required]"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.nota1"
                      label="Nota 1"
                      type="number"
                      :rules="[rules.interval]"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.nota2"
                      label="Nota 2"
                      type="number"
                      :rules="[rules.interval]"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.nota3"
                      label="Nota 3"
                      type="number"
                      :rules="[rules.interval]"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.nota4"
                      label="Nota 4"
                      type="number"
                      :rules="[rules.interval]"
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancelar
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="setAlunos"
              >
                Salvar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="400px">
          <v-card>
            <v-card-title class="headline">Tem certeza que deseja excluir?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="getItemToDelete(item)"
      >
        mdi-delete
      </v-icon>
    </template>
  </v-data-table>

 <vue-json-to-csv
    :json-data="alunos"
    :labels="{ name: { title: 'Nome' }, nota1: { title: 'Nota 1' }, nota2: { title: 'Nota 2' }, nota3: { title: 'Nota 3' }, nota4: { title: 'Nota 4' }, media: { title: 'Média' }}"
    :csv-title="`${turma}`"
    class="d-flex justify-end mt-3"
    >
    <v-btn
      color="primary"
    >
      Gerar Planilha
    </v-btn>
</vue-json-to-csv>

</div>
</template>

<script>
/* eslint-disable */
import VueJsonToCsv from 'vue-json-to-csv'
import firebase from '../../components/firebaseConfig';
const db = firebase.firestore();
export default {
components: {
    VueJsonToCsv
  },
async asyncData({params}){
    const turma = params.turma
    return { turma }
},
data: () => ({
dialog: false,
dialogDelete: false,
headers: [
      {
        text: 'Alunos',
        align: 'start',
        sortable: true,
        value: 'name',
      },
      { text: 'Nota 1', value: 'nota1' },
      { text: 'Nota 2', value: 'nota2' },
      { text: 'Nota 3', value: 'nota3' },
      { text: 'Nota 4', value: 'nota4' },
      { text: 'Média', value:  'media' },
      { text: 'Ações', value: 'actions', sortable: false }
    ],
alunos: [],
loading: true,
editedIndex: -1,
tituloDaTurma: '',
itemIdToDelete: '',
editedItem: {
  nome: '',
  nota1: 0,
  nota2: 0,
  nota3: 0,
  nota4: 0
},
  defaultItem: {
    nome: '',
    nota1: 0,
    nota2: 0,
    nota3: 0,
    nota4: 0
  },
  rules :{
      required: value => !!value || 'Nome obrigatório.',
      interval: value => (value <= 10 && value >=0) || 'Nota de 0 a 10' ,
    },
}),
computed: {
  formTitle() {
    return this.editedIndex === -1 ? 'Novo Aluno' : 'Editar Aluno'
  },
},
watch: {
  dialog(val) {
    val || this.close()
  },
  dialogDelete (val) {
    val || this.closeDelete()
  },
},
created () {
  this.getAlunos()
},
methods: {
getAlunos() {
  db.collection(`${this.turma}`)
  .get()
  .then((querySnapshot) => {
    querySnapshot.forEach((doc) => {
      this.alunos.push({
        id: doc.id,
        name: doc.data().nome,
        nota1: doc.data().nota1,
        nota2: doc.data().nota2,
        nota3: doc.data().nota3,
        nota4: doc.data().nota4,
        media: (doc.data().nota1 + doc.data().nota2 + doc.data().nota3 + doc.data().nota4)/4
      });
    });
    console.log(this.alunos)
    this.loading = false;
    this.tituloDaTurma = this.turma.replace('-', ' ');
  })
  .catch((error) => {
    console.log("Error getting documents: ", error);
  });
},
editItem(item) {
  this.editedIndex = item.id
  //console.log(item)
  this.editedItem = { 
    id: item.id,
    nome: item.name,
    nota1: item.nota1,
    nota2: item.nota2,
    nota3: item.nota4,
    nota4: item.nota4
    }
  //console.log(this.editedItem)
  this.dialog = true
},
getItemToDelete(item) {
  //console.log(item.id)
  this.editedIndex = item.id
  this.dialogDelete = true
},
deleteItemConfirm() {
  db.collection(`${this.turma}`)
    .doc(`${this.editedIndex}`)
    .delete()
    .then(() => {
      this.editedIndex = -1
    })
    .catch((error) => {
      alert("Erro ao excluir. Favor tentar novamente!")
    });
    this.closeDelete()
    this.alunos = [];
    this.getAlunos();
},
close() {
  this.dialog = false
  this.$nextTick(() => {
    this.editedItem = Object.assign({}, this.defaultItem)
    this.editedIndex = -1
  })
},
closeDelete() {
  this.dialogDelete = false
  this.$nextTick(() => {
    this.editedItem = Object.assign({}, this.defaultItem)
    this.editedIndex = -1
  })
},
setAlunos() {
  if(this.editedItem.id === undefined){
  db.collection(`${this.turma}`)
    .add({ 
      nome: this.editedItem.nome,
      nota1: parseInt(this.editedItem.nota1),
      nota2: parseInt(this.editedItem.nota2),
      nota3: parseInt(this.editedItem.nota3),
      nota4: parseInt(this.editedItem.nota4)
      })
      .then(() => {
          alert("Cadastro feito com sucesso!")
          this.close();
          this.alunos = [];
          this.getAlunos();
      })
      .catch((error) => {
          alert("Erro ao criar novo aluno. Favor tentar novamente.")
    });
  } else {
      db.collection(`${this.turma}`)
        .doc(this.editedItem.id)
        .update({
          nome: this.editedItem.nome,
          nota1: parseInt(this.editedItem.nota1),
          nota2: parseInt(this.editedItem.nota2),
          nota3: parseInt(this.editedItem.nota3),
          nota4: parseInt(this.editedItem.nota4)
        })
        .then(() => {
          alert("Atualização feita com sucesso!")
          this.close();
          this.alunos = [];
          this.getAlunos();
        })
        .catch((error) => {
          alert("Erro ao atualizar. Favor tentar novamente.")
        });
  }
},
},
}
</script>

<style scoped>
  .titulo-turma{
    text-transform:capitalize;
  }
</style>