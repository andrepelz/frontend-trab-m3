<template>
  <el-main>
    <biblioteca-row>
      <biblioteca-col
        class="text-center mb-3"
        :sm="12"
        :offset="6">
        <img
          class="mb-3"
          src="C:\Users\T-GAMER\OneDrive\Área de Trabalho\Uou\imobiliaria.png"
          alt=""
          width="80" />
        <biblioteca-header size="md">Bem-vindo!</biblioteca-header>
        <small>Por favor, informe os seus dados</small>
      </biblioteca-col>
    </biblioteca-row>
    <biblioteca-row>
      <biblioteca-col
        class="text-center mb-3"
        :sm="12"
        :offset="6">
        <biblioteca-form v-slot="{ validate }" :submit="validateUsuario">
          <biblioteca-row>
            <biblioteca-col
              class="text-center mb-3">
              <biblioteca-input
                v-model="info.nome"
                type="text"
                name="nome"
                rules="required"
                placeholder="Nome"
                :focus="true" />
            </biblioteca-col>
          </biblioteca-row>
          <biblioteca-row>
            <biblioteca-col
              class="text-center mb-3">
              <biblioteca-input
                v-model="info.email"
                type="email"
                name="e-mail"
                rules="required|email"
                placeholder="E-mail" />
            </biblioteca-col>
          </biblioteca-row>
          <biblioteca-row>
            <biblioteca-col
              class="text-center mb-3">
              <biblioteca-input
                v-model="info.cpf"
                type="text"
                name="cpf"
                rules="required"
                placeholder="CPF" />
            </biblioteca-col>
          </biblioteca-row>
          <biblioteca-row>
            <biblioteca-col class="text-center mb-3">
              <select v-model="info.tipoUsuario" name="tipo-usuario" required>
                <option value="" disabled>Selecione</option>
                <option value="proprietario">Proprietário</option>
                <option value="corretor">Corretor</option>
              </select>
            </biblioteca-col>
          </biblioteca-row>
          <biblioteca-row>
            <biblioteca-col
              class="text-center mb-3"
              :sm="12">
              <biblioteca-input
                v-model="info.senha"
                name="senha"
                type="password"
                rules="required|min:6"
                placeholder="Senha*" />
            </biblioteca-col>
            <biblioteca-col
              class="text-center mb-3"
              :sm="12">
              <biblioteca-input
                v-model="info.confirmar_senha"
                name="confirmar senha"
                type="password"
                rules="required|min:6"
                placeholder="Confirmar senha*" />
            </biblioteca-col>
          </biblioteca-row>
          <biblioteca-row>
            <biblioteca-col
              class="text-center mb-3">
              <biblioteca-button
                native-type="submit"
                class="btn btn-dark"
                @click="validate">
                Confirmar
              </biblioteca-button>
            </biblioteca-col>
          </biblioteca-row>
        </biblioteca-form>
      </biblioteca-col>
    </biblioteca-row>
    <biblioteca-row>
      <biblioteca-col
        class="text-center mb-3"
        :sm="12"
        :offset="6">
        <div class="d-grid text-center">
          <p>
            Já possui conta?
            <router-link class="color--primary" to="/login">Login</router-link>
          </p>
        </div>
      </biblioteca-col>
    </biblioteca-row>
  </el-main>
</template>

<script>
import * as authStore from '@/modules/auth/auth.store';
import { registrar } from '@/modules/auth/auth.service';
import { toastError } from '@/services/toastService';
import { goToBasePage } from '@/router/route.service';

export default {
  name: 'BibliotecaRegistrarPage',
  provide() {
    const infoRegistrarVm = {};
    Object.defineProperty(infoRegistrarVm, 'info', {
      get: () => this.info,
    });
    return { infoRegistrarVm };
  },
  data() {
    return {
      info: {
        nome: null,
        email: null,
        senha: null,
        confirmar_senha: null,
      },
    };
  },
  beforeCreate() {
    if (authStore.getters.getToken()) {
      goToBasePage();
    }
  },
  methods: {
    onRegistrar() {
      registrar(this.info)
        .then(goToBasePage)
        .catch(err => {
          if (err.response.data.detail === 'EMAIL_DUPLICADO') {
            toastError('E-mail duplicado');
          }
        });
    },
    validateUsuario() {
      if (this.info.senha !== this.info.confirmar_senha) {
        toastError('As senhas não são iguais');
      } else {
        this.onRegistrar();
      }
    },
  },
};
</script>
<style scoped>
span {
  width: 620px;
}
button {
  width:150px;
}
</style>
