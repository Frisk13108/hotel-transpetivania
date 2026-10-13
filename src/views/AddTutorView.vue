<script setup>
import { onMounted, ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const tutores = ref([]);
const API_URL = 'http://localhost:3000';

const novoTutor = ref({
  nome: '',
  telefone: '',
});

async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);

  tutores.value = await resposta.json();
  console.table(tutores.value);
}

async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });


  router.push('/tutores');
}

onMounted(carregarTutores)
</script>

<template>
    <div>
        <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastrar Tutor</h1>
      <p class="text-body-secondary mb-0">Preencha os dados para cadastrar um novo tutor.</p>
    </header>

    <h2>
        Dados do Tutor
    </h2>

    <form
      class="row"
      @submit.prevent="salvarTutor"
    >

    <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
        >
          Nome:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoTutor.nome"
          required
        />
      </div>

      <div class="col-md-6">
        <label
          for="telefone"
          class="form-label"
        >
          Telefone / Celular / Whatsapp:
        </label>
        <input
          type="tel"
          class="form-control"
          id="telefone"
          v-model="novoTutor.telefone"
          required
        />
      </div>

      <div class="col-12 d-flex gap-2">
        <button
          class="btn btn-success"
          type="submit"
        >
          Cadastrar Tutor
        </button>
      </div>
    </form>
    </div>
</template>