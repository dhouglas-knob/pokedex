<script setup>

    import { onMounted, ref } from 'vue';

    let carregamento = ref(true);

    let vetor = ref([]);

    let termoFiltragem = ref('');

    onMounted(async () => {

        for (let indice = 1; indice <= 151; indice++) {
            let requisicao = await fetch('https://pokeapi.co/api/v2/pokemon/'+indice)
            let pokemon = await requisicao.json();
            vetor.value.push(pokemon);
        }

        carregamento.value = false;

    });

    function filtrar() {
        return vetor.value.filter(obj => obj.name.toLowerCase().includes(termoFiltragem.value.toLowerCase()));
    }

</script>

<template>

    <div class="carregamento" v-if="carregamento">

        <img src="https://i.pinimg.com/originals/15/3c/fb/153cfb7dcfb406a368a3dc4e35e37efb.gif">

    </div>

    <main class="container" v-if="!carregamento">

        <div class="row">

            <div class="col-12">

                <input type="text" v-model="termoFiltragem" placeholder="Qual Pokémon você está procurando?" class="form-control pesquisa">

                <p v-if="filtrar().lenght ==0">Não foi encontrado nenhum Pokémon.</p>
                <p v-else-if="filtrar().lenght == 1">Foi encontrado apenas um Pokémon.</p>
                <p v-else>Foram encontrados {{ filtrar().length }} Pokémons.</p>

            </div>

        </div>

        <div class="row">

            <div class="col-12 col-sm-6 col-md-4 col-lg-3" v-for="v in filtrar()">

                <div class="card" :class="v.types[0].type.name">

                    <img :src="v.sprites.front_default">
                    <p>{{ v.name }}</p>

                </div>

            </div>

        </div>

    </main>

</template>