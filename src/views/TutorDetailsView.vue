<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const tutor = ref(null);
const API_URL = 'http://localhost:3000';
const pets = ref([]);
const petTutor = ref([]);
const router = useRoute();






async function carregarTutor() {
    const tutorId = route.params.id;

    const respostaTutor = await fetch(`${API_URL}/tutores/${tutorId}`)

    tutor.value = await respostaTutor.json();

    const respostaPets = await fetch(`${API_URL}/pets`);
        pets.value = await respostaPets.json();

        for (const pet of pets.value) {
        if (pet.tutorId === tutor.value.id) {
            petTutor.value.push(pet); 
        }
    }

    const novoPet = ref({
        nome: '',
        especie: '',
        tutorId: tutor.value.id,
    });

    async function salvarPet() {
  await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),
  });

  // redirecionar para a tela de listagem de Pets
  router.push('/tutores/:id');
}


    console.log('tutor', tutor.value);
}

// function petsDoTutor() {
//    
//    for(const pet of pets.value){
//        if (pet.tutorId === tutor.value.id) {
//            petTutor.value.push(pet.value);
//        }
//    }
//  return petTutor;
// } 

onMounted(carregarTutor);
</script>

<template>
    <section v-if=" tutor == null">
        <p>Carregando.........</p>
    </section>

    <section v-else>
        <section>
            <h1> Nome do Tutor: {{ tutor.nome }}</h1>
            <p> Telefone: {{ tutor.telefone }} </p>
        </section>

        <section>
            <table class="table table-striped table-hover">
            <thead>
                <th>id</th>
                <th>Nome</th>
                <th>Espécie</th>
                <th>Ações</th>
            </thead>
            <tbody>
                <tr v-for="pet in petTutor" :key="pet.id">
                    <td>{{ pet.id }}</td>
                    <td>{{ pet.nome }}</td>
                    <td>{{ pet.especie }}</td>

                    <td> 
                        <RouterLink :to="`/pets/${pet.id}`"> Visualizar </RouterLink>
                    </td>
                </tr>
            </tbody>
        </table>
        </section>

        <section>
            <form class="row" @submit.prevent="salvarPet()">

                <div class="col-md-6">
                <label for="nome" class="form-label">
                    Nome do Pet:
                </label>
                <input type="text" class="form-control" id="nome" v-model="novoPet.nome" required />
                </div>



                <div class="col-12 d-flex gap-2">
                <button class="btn btn-success" type="submit">
                    Salvar Pet
                </button>
                </div>
            </form>
        </section>
    </section>
    
</template>