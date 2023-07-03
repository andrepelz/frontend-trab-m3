<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Imobiliária</title>
  <!-- Incluir bibliotecas e estilos necessários -->
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/element-ui/lib/theme-chalk/index.css">
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/element-ui/lib/index.js"></script>
  <style>
    /* Estilos personalizados */
    .mb--xl {
      margin-bottom: 20px;
    }
    .opacity--50 {
      opacity: 0.5;
    }
    .my--md {
      margin-top: 10px;
      margin-bottom: 10px;
    }
  </style>
</head>

<body>
  <div id="app">
    <el-tabs v-model="tabActive" @tab-click="fetch">
      <el-tab-pane label="Últimos Empréstimos" name="emprestimos">
        <div v-if="emprestimoList.length">
          <div v-for="emprestimo in emprestimoList" :key="emprestimo.id" class="mb--xl">
            <div>{{ emprestimo }}</div>
          </div>
        </div>
        <div v-else>
          <p class="opacity--50 my--md">( Sem empréstimos )</p>
        </div>
      </el-tab-pane>
      <el-tab-pane label="Últimos livros" name="livros">
        <div v-if="livroList.length">
          <div v-for="livro in livroList" :key="livro.id" class="mb--xl">
            <div>{{ livro }}</div>
          </div>
        </div>
        <div v-else>
          <p class="opacity--50 my--md">( Sem livros )</p>
        </div>
      </el-tab-pane>
      <el-tab-pane label="Últimos usuários" name="usuarios">
        <div v-if="usuarioList.length">
          <div v-for="usuario in usuarioList" :key="usuario.id" class="mb--xl">
            <div>{{ usuario }}</div>
          </div>
        </div>
        <div v-else>
          <p class="opacity--50 my--md">( Sem usuários )</p>
        </div>
      </el-tab-pane>
    </el-tabs>
  </div>

<script>
import { fetchUsuarios } from '@/modules/usuario/usuario.service';
import { fetchLivros } from '@/modules/livro/livro.service';
import { fetchEmprestimos } from '@/modules/emprestimo/emprestimo.service';

import BibliotecaUsuarioCard from '@/modules/usuario/components/UsuarioCard.vue';
import BibliotecaLivroCard from '@/modules/livro/components/LivroCard.vue';
import BibliotecaEmprestimoCard from '@/modules/emprestimo/components/EmprestimoCard.vue';

export default {
  name: 'BibliotecaHomeTabs',
  components: {
    BibliotecaUsuarioCard,
    BibliotecaLivroCard,
    BibliotecaEmprestimoCard,
  },
  data() {
    return {
      tabActive: 'emprestimos',
      usuarioList: [],
      livroList: [],
      emprestimoList: [],
    };
  },
  mounted() {
    this.fetch();
  },
  methods: {
    fetch() {
      if (this.tabActive === 'emprestimos') {
        this.fetchEmprestimos();
      } else if (this.tabActive === 'livros') {
        this.fetchLivros();
      } else if (this.tabActive === 'usuarios') {
        this.fetchUsuarios();
      }
    },
    fetchLivros() {
      fetchLivros()
        .then(({ data }) => {
          this.livroList = data.data;
        })
        .catch(() => {
          this.livroList = [];
        });
    },
    fetchUsuarios() {
      fetchUsuarios()
        .then(({ data }) => {
          this.usuarioList = data.data;
        })
        .catch(() => {
          this.usuarioList = [];
        });
    },
    fetchEmprestimos() {
      fetchEmprestimos()
        .then(({ data }) => {
          this.emprestimoList = data.data;
        })
        .catch(() => {
          this.emprestimoList = [];
        });
    },
  },
};
</script>
