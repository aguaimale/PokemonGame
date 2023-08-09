<template>
    <div class="vidas">

        <div class="pokebola" v-for="(vida, index) in vidas" :key="index" :class="{ 'perdida': index >= vidasRestantes }">
            <img src="../assets/pokeball-pokemon-svgrepo-com.svg" alt="vida">
        </div>
    </div>
</template>
  
<script>
export default {
    props: {
        vidas: Number,
        vidasRestantes: Number
    }

};
</script>
  
<style scoped>
.vidas {
    display: flex;
    flex-direction: row;
    justify-content: flex-end;
    margin-left: 149px;
}

.pokebola {
    width: 50px;
    height: 50px;
    transition: transform 0.5s, opacity 0.5s;
}

.perdida {
    opacity: 0;
    pointer-events: none;
    display: none;
}

img {
    width: 65%;
}
</style>
  