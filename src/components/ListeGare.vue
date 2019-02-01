<template>
    <div class="container">
        <!-- <form> -->
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Départ</h2>
                    <input list="gares" v-model="trajet.gareStart" v-on:input="filter(trajet.gareStart)" name="gareStartSelected" class="form-control" placeholder="Choisir une gare de départ"/>
                    <datalist id="gares">
                        <option v-for="gare in gares" :key="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Arivée</h2>
                    <input list="gares" v-model="trajet.gareEnd" v-on:input="filter(trajet.gareEnd)" name="gareEndSelected" class="form-control" placeholder="Choisir une gare d'arrivé"/>
                    <datalist id="gares">
                        <option :id="gare.id" v-for="gare in gares" :key="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Heure de départ</h2>
                    <input v-model="trajet.horaireStart" type="date" name="horaireStart" class="form-control" placeholder="Choisir un horaire de départ"/>
                </div>
            </div>
            <div class="row">
                <div class="col-12 form-group">
                    <button class="btn btn-primary" v-on:click="getHoraires(trajet.horaireStart)">Afficher les horaires</button>
                </div>
            </div>
        <!-- </form> -->
        <div class="list-horaires my-5" v-if="ui.listHoraires.state == true">
            <div class="row horaire-item">
                <div class="col-12">
                    <div class="horaire d-flex">
                        <span class="time">{{ trajet.horaireStart }}</span> 
                        <div class="d-flex flex-column">
                            <p><span class="label">Départ:</span> {{ trajet.gareStart }}<p>
                            <p><span class="label">Arrivé:</span> {{ trajet.gareEnd }}</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            records: [],
            gares: [],
            trajet: {
                gareStart: "",
                gareEnd: "",
                horaireStart: ""
            },
            ui: {
                listHoraires: {
                    // affiche ou non la liste des horaires
                    state: false
                }
            }
        }
    },
    methods: {
        getHoraires(horaireStart) {
            this.ui.listHoraires.state = !this.ui.listHoraires.state;
            console.log("horaireStart:", new Date(this.trajet.horaireStart));
            console.log("gareStart:", this.trajet.gareStart);
            console.log("gareEnd:", this.trajet.gareEnd);
        },
        filter(item) {
            this.gares.filter((item) => {
                return item.name.toLowerCase().indexOf(item.name.toLowerCase()) > -1;
            });
        }
        
    },
    created() {
        console.clear();
        this.$http.get("https://ressources.data.sncf.com/api/records/1.0/search/?dataset=referentiel-gares-voyageurs&rows=10&sort=intitule_gare&facet=agence_gare&facet=region_sncf&facet=unite_gare&facet=departement&facet=segment_drg").then((response) => {
            console.log("response", response);
            this.records = response.body.records;
            this.records.forEach(element => {
                this.gares.push(
                    {
                        id: element.fields.code_plate_forme,
                        name: element.fields.intitule_gare
                    }
                );
            });
        });
    }
}
</script>

<style scoped>

.horaire {
    background-color: #F5F5F5;
    border-radius: 5px;
    font-size: 1.2em;
}

.time {
    font-weight: bold;
    margin-right: 30px;
    padding: 20px 30px;
    background-color: #009CD3;
    color: white;  
}

.label {
    text-decoration: underline;
    font-size: 1.3em;
    margin-right: 20px;
}

.direction {
    color: #888;
}

</style>
