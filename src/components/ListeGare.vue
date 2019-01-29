<template>
    <div class="container">
        <form>
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Départ</h2>
                    <input list="gares" v-model="gareStartSelected" v-on:input="getGaresStart(gareStartSelected)" id="gareStartSelected" name="gareStartSelected" class="form-control" placeholder="Choisir une gare de départ"/>
                    <datalist id="gares">
                        <option v-for="gare in garesByNameStart" :key="gare.id" :id="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Arivée</h2>
                    <input list="gares" v-model="gareEndSelected" v-on:input="getGaresEnd(gareEndSelected)" id="gareEndSelected" name="gareEndSelected" class="form-control" placeholder="Choisir une gare d'arrivé"/>
                    <datalist id="gares">
                        <option :id="gare.id" v-for="gare in garesByNameEnd" :key="gare.id" :value="gare.name"></option>
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
            garesByNameStart: [],
            garesByNameEnd: [],
            gareStartSelected: "",
            gareEndSelected: ""
        }
    },
    methods: {
        getGaresStart(name) {
            console.log(`#GetgaresStart(${name})`);
            this.garesByNameStart = this.filter(this.gares, name);
        },
        getGaresEnd(name) {
            console.log(`#GetgaresEnd(${name})`);
            this.garesByNameEnd = this.filter(this.gares, name);
        },
        getHoraires(depart, arrive) {

        },
        filter(arrayData, item) {
            arrayData.filter((item) => {
                return item.name.toLowerCase().indexOf(item.name.toLowerCase()) > -1;
            });
        }
        
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
                this.garesByNameStart = this.gares;
                this.garesByNameEnd = this.gares;
            });
        });
    }
}
</script>

<style scoped>

</style>
