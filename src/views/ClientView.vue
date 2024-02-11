<template>
    <main>
        <div>
            <h1>Les clients</h1>
        </div> 

        <div>
            <table>
                <caption>Liste de clients</caption>
                <tr>
                    <th>Code</th>
                    <th>Societe</th>
                    <th>Contact</th>
                    <th>Ville</th>
                </tr>

                <tr v-if="data.listeClients.length === 0">
                    <td colspan="4">Veuillez patienter, chargement des catégories...</td>
                </tr>
         
                <tr v-for="client in data.listeClients" :key="client.code">
                    <td>{{ client.code }}</td>
                    <td>{{ client.societe }}</td>
                    <td>{{ client.contact }}</td>
                    <td>{{ client.ville }}</td>
                </tr>

                <tr>
                    <th>Début</th>
                    <th>Page précédente</th>
                    <th>Page suivante</th>
                    <th>Fin</th>
                </tr>

                <tr v-if="data.listeFonctionnalites.length === 0">
                    <td colspan="4">Chargement fonctionnalités</td>
                </tr>
         
                <tr v-for="fonctionnalites in data.listeFonctionnalites" :key="fonctionnalites.code"> 
                    <td>{{ fonctionnalites.debut }}</td>
                    <td>{{ fonctionnalites.pagePre }}</td>
                    <td>{{ fonctionnalites.pageSuiv }}</td>
                    <td>{{ fonctionnalites.fin }}</td>

                </tr>
            </table>

        </div>
    </main>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import { doAjaxRequest } from "@/api";

let data = reactive({
    listeClients: []
});

function showError(error) {
    console.log("Erreur : status %d", error.status)
    console.log(error.body);
    alert(error.message);
}
function chargeClients() {
    doAjaxRequest("/api/clients")
        .then((json) => {
            data.listeClients = json._embedded.client;
        })
        .catch(showError);
}

function chargeFonctionnalites() {
    doAjaxRequest("/api/fonctionnalites")
        .then((json) => {
            data.listeFonctionnalites = json._embedded.fonctionnalites;
        })
        .catch(showError);
}

onMounted(chargeClients);
onMounted(chargeFonctionnalites);
</script>

<style scoped>
td,
th {
    border: 1px solid #ddd;
    padding: 8px;
}

th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #232623;
    color: rgb(255, 255, 255);
}
</style>
