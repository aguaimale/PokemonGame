<template>
    <div class="score">
        <h3>Puntaje:{{ puntaje }} </h3>
    </div>
</template>

<script>
export default{
    props: {
        puntaje:Number,
    }

}

</script>

<style scoped>
.score{
    
    display: flex;
    flex-direction: row;
    justify-content: center;
    

}

</style>