<template>
    <div class="container">
        <form>
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Départ</h2>
                    <input list="gares" v-model="gare_start" v-on:input="this.getGares(gare_start)" id="gare_start" name="gare_start" class="form-control" placeholder="Choisir une gare de départ"/>
                    <datalist id="gares">
                        <option v-for="gare in gares" :key="gare.id" :id="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Arivée</h2>
                    <input list="gares" v-model="gare_end" id="gare_end" name="gare_end" class="form-control" placeholder="Choisir une gare d'arrivé"/>
                    <datalist id="gares">
                        <option :id="gare.id" v-for="gare in gares" :key="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            <div class="row">
                <div class="col-12 form-group">
                    <button class="btn btn-primary" v-on:submit.prevent>Afficher les horaires</button>
                </div>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            records: [],
            gares: [],
            gare_start: "",
            gare_end: ""
        }
    },
    methods: {
        getGares(name) {
            console.log(`#Getgares(${name})`);
        },
        getHoraires(depart, arrive) {

        },
        
    },
    created() {
        fetch("https://ressources.data.sncf.com/api/records/1.0/search/?dataset=referentiel-gares-voyageurs&rows=10&sort=intitule_gare&facet=agence_gare&facet=region_sncf&facet=unite_gare&facet=departement&facet=segment_drg").then((response) => {
            response.json().then((data) => {
                console.log(data);
                this.records = data.records;
                this.records.forEach(element => {
                    this.gares.push(
                        {
                            id: element.fields.code_plate_forme,
                            name: element.fields.intitule_gare
                        }
                    );
                });
            });
        });
    }
}
</script>

<style scoped>

</style>
