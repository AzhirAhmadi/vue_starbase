<template>
    <div class="col-md-4" @click="switchCharacter">
        <div class="character-card">
            <div class="card-block">
                <h4 class="card-title">{{character.name}}</h4>
                <p class="card-text">Height: {{character.height}}</p>
                <p class="card-text">Mass: {{character.mass}}</p>
                <p class="card-text">Hair Color: {{character.hair_color}}</p>
                <p class="card-text">Skin Color: {{character.skin_color}}</p>
                <p class="card-text">Eye Color: {{character.eye_color}}</p>
                <p class="card-text">Gender: {{character.gender}}</p>
                <p class="card-text">Home World: {{planet.name}}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['id'],
    data (){
        return{
            character: {},
            planet: {}
        }
    },
    methods: {
        fetchPlanet(url) {
            fetch(url,{
                method: 'GET'
            }).then(response => response.json())
            .then(json => this.planet = json)
        },
        fetchCharacter(id) {
            fetch(`https://swapi.co/api/people/${id}/`,{
                method: 'GET'
            }).then(response => response.json())
            .then(json => this.character = json)
            .then(json => this.fetchPlanet(json.homeworld))
        },
        switchCharacter(){
            let random_id
            do{
                random_id = Math.floor(Math.random()*83)+1
            }while (random_id == 17)
            this.fetchCharacter(random_id)
        }
    },
    created() {
        this.fetchCharacter(this.id)
    }
}
</script>