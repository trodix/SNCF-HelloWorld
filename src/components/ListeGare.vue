<template>
    <div class="container">
            <div class="alert alert-danger" v-if="ui.invalidForm.state">
                <p>Invalid input, please check the form</p>
            </div>
        <!-- <form> -->
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Départ</h2>
                    <input list="gares" v-model="trajet.gare.start.name" v-on:input="filter(trajet.gare.start.name)" autocomplete="off" name="gareStartSelected" class="form-control" placeholder="Choisir une gare de départ"/>
                    <datalist id="gares">
                        <option v-for="gare in gares" :key="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Arivée</h2>
                    <input list="gares" v-model="trajet.gare.end.name" v-on:input="filter(trajet.gare.end.name)" autocomplete="off" name="gareEndSelected" class="form-control" placeholder="Choisir une gare d'arrivé"/>
                    <datalist id="gares">
                        <option :id="gare.id" v-for="gare in gares" :key="gare.id" :value="gare.name"></option>
                    </datalist>
                </div>
            </div>
            <div class="row">
                <div class="col-12 form-group">
                    <h2>Heure de départ</h2>
                    <input v-model="trajet.horaire.start" type="date" name="horaireStart" class="form-control" autocomplete="off" placeholder="Choisir un horaire de départ"/>
                </div>
            </div>
            <div class="row">
                <div class="col-12 form-group">
                    <button class="btn btn-primary" v-on:click="getHoraires(trajet.horaire.start)">Afficher les horaires</button>
                </div>
            </div>
        <!-- </form> -->
        <div class="list-horaires my-5" v-if="ui.listHoraires.state == true">
            <div class="row horaire-item">
                <div class="col-12">
                    <div class="horaire d-flex">
                        <span class="time">{{ trajet.horaire.start }}</span> 
                        <div class="d-flex flex-column">
                            <p><span class="label">Départ:</span> {{ trajet.gare.start.name }}<p>
                            <p><span class="label">Arrivé:</span> {{ trajet.gare.end.name }}</p>
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
                gare: {
                    start: {
                        name: ""
                    },
                    end: {
                        name: ""
                    }
                },
                horaire: {
                    start: "",
                    list: []
                }
            },
            ui: {
                listHoraires: {
                    // affiche ou non la liste des horaires
                    state: false
                },
                invalidForm: {
                    // affiche un message d'erreur si le formulaire est invalide
                    state: false
                }
            }
        }
    },
    methods: {
        getHoraires(horaire) {
            if(!this.checkForm()) { return false; }
            this.ui.invalidForm.state = false;
            this.ui.listHoraires.state = !this.ui.listHoraires.state;
            console.log("horaireStart:", new Date(this.trajet.horaire.start));
            console.log("gareStart:", this.trajet.gare.start);
            console.log("gareEnd:", this.trajet.gare.end);
            this.$http.get('').then((response) => {
                this.trajet.horaire.list = response.body;
            });
            return true;
        },
        filter(item) {
            setTimeout(() => {
                this.$http.get("https://ressources.data.sncf.com/api/records/1.0/search/?dataset=referentiel-gares-voyageurs&q="+item+"&sort=intitule_gare&facet=agence_gare&facet=region_sncf&facet=unite_gare&facet=departement&facet=segment_drg").then((response) => {
                    this.records = response.body.records;
                    this.gares = [];
                    this.records.forEach(element => {
                        this.gares.push(
                            {
                                id: element.recordid,
                                name: element.fields.intitule_gare
                            }
                        );
                    });
                });
            }, 1000);
            this.gares.filter((item) => {
                return item.name.toLowerCase().indexOf(item.name.toLowerCase()) > -1;
            });
        },
        checkForm() {
            // if(!this.filter(this.trajet.gare.start) 
            //     || !this.filter(this.trajet.gare.end) 
            //     || new Date(this.trajet.horaire.start) == 'Invalid Date') {
            //         this.ui.invalidForm.state = true;
            //         console.log("Formulaire invalide");
            //         return false;
            //     }
            //     console.log("Formulaire valide");
            return true;
        }
        
    },
    created() {
        console.clear();
        this.$http.get("https://ressources.data.sncf.com/api/records/1.0/search/?dataset=referentiel-gares-voyageurs&sort=intitule_gare&facet=agence_gare&facet=region_sncf&facet=unite_gare&facet=departement&facet=segment_drg").then((response) => {
            console.log("response", response);
            this.records = response.body.records;
            this.records.forEach(element => {
                this.gares.push(
                    {
                        id: element.recordid,
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
