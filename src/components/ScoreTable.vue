<template>
    <div class="tablecontainer">
        <h4>Tablero de posiciones</h4>
        <table>
            <thead>
                <tr>
                    <th>Jugador</th>
                    <th>Puntos</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(jugador, index) in puntajes" :key="index">
                    <td>{{ jugador.nombre }}</td>
                    <td>{{ jugador.puntaje }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
  
<script>
export default {
    props: {
        puntajes: Array  
    }
};
</script>
  
<style scoped>
.tablecontainer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-left: 100px;
    padding-right: 100px;

}

table {
    border-collapse: collapse;
    width: 80%;
    margin-top: 10px;
}

th,
td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
}

th {
    background-color: #f2f2f2;
}
</style>
  