<script setup>
import WelcomeItem from "./WelcomeItem.vue";
import DocumentationIcon from "./icons/IconDocumentation.vue";
import EcosystemIcon from "./icons/IconEcosystem.vue";
import CommunityIcon from "./icons/IconCommunity.vue";
</script>

<script>
export default {
  data() {
    return {
      resultContent: "",
    };
  },

  methods: {
    async getData() {
      let cep = document.getElementById("cep");

      if (cep.value == "") return;
      if (cep.value.length < 8) return;

      let url = `https://viacep.com.br/ws/` + cep.value + `/json/`;

      await fetch(url, {
        method: "GET",
      })
        .then(async (rs) => {
          let res = await rs.json();
          this.resultContent =
            res.logradouro +
            ", " +
            res.bairro +
            ", " +
            res.localidade +
            " - " +
            res.uf;
        })
        .catch(function (err) {
          this.btnLimpar();
        });
    },

    btnConsultar() {
      const btn = document.getElementById("btnConsultar");
      btn.addEventListener("click", (e) => {
        this.getData();
      });
    },

    btnLimpar() {
      let cep = document.getElementById("cep");
      this.resultContent = cep.value = "";
    },

    isNumber: function (evt) {
      evt = evt ? evt : window.event;
      var charCode = evt.which ? evt.which : evt.keyCode;
      if (
        charCode > 31 &&
        (charCode < 48 || charCode > 57) &&
        charCode !== 46
      ) {
        evt.preventDefault();
      }
      else {
        return true;
      }
    },
  },

  watch() {
    this.btnConsultar();
    this.btnLimpar();
  },
};
</script>

<template>
  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Modo de utilização</template>
    <p>Digite um CEP e receberá o retorno do endereço, caso seja encontrado.</p>
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <EcosystemIcon />
    </template>
    <template #heading>Busca Rápida ViaCEP</template>

    <div class="form-group">
      <label for="exampleFormControlInput1">Digite o CEP: </label>
      <input
        type="tel"
        class="form-control"
        style="width: 70px"
        maxlength="8"
        id="cep"
        required
        @keypress="isNumber($event)"
      /><br />
      <button
        type="button"
        class="btn btn-primary"
        id="btnConsultar"
        @click="getData">
        Consultar
      </button>
      <button
        type="button"
        class="btn btn-primary"
        id="btnLimpar"
        @click="btnLimpar">
        Limpar
      </button>
      <br />
      <small class="form-text text-muted">Retorno da consulta:
        <b><span id="resultApi">{{ resultContent }}</span></b>
      </small>
    </div>
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <CommunityIcon />
    </template>
    <template #heading>Contato</template>

    <p>
      Por favor, dê uma [ Stars ] no GitHub nessa proposta de consumo de API.
    </p>
    <a href="evaldolbj@gmail.com" target="_blank" rel="noopener">evaldolbj@gmail.com</a>
    meu contato direto, ou
    <a href="http://www.evaldolopes.com.br/" target="_blank" rel="noopener">www.evaldolopes.com.br</a>.
  </WelcomeItem>
</template>
