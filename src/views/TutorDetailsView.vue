<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute();
const tutor = ref(null);
const API_URL = 'http://localhost:3000';

async function carregarTutor() {
    const tutorId = route.params.id;

    const respostaTutor = await fetch(`${API_URL}/tutores/${tutorId}`)

    tutor.value = await respostaTutor.json();

    console.log('tutor', tutor.value);
}

onMounted(carregarTutor);
</script>

<template>
    <section v-if=" tutor == null">
        <p>Carregando.........</p>
    </section>

    <section v-else>
        <h1> Nome do Tutor: {{ tutor.nome }}</h1>
        <p> Telefone: {{ tutor.telefone }} </p>
    </section>
</template>