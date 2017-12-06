<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <input type="text" class="nameInput" v-model="name"/>
      <input type="submit" class="nameSubmit" @click="submit"/>
    </div>
    <div>
      <ul>
        <li v-for="person in people">{{ person.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'HelloWorld',
    data () {
      return {
        msg: 'Acando fagmøte 1 demo',
        name: '',
        people: []
      }
    },
    methods: {
      submit () {
        console.log('name: ' + this.name)
        axios.post('http://localhost:8081/addPerson', {name: this.name}).then(resp => {
          // console.log('person added, id: ' + resp.data.id)
          console.log('person created: ' + JSON.stringify(resp.data))
          this.msg = resp.data.name + ' is added to database'
          this.getPeople()
          this.name = ''
        })
      },
      getPeople () {
        axios.get('http://localhost:8081/people').then(resp => {
          this.people = resp.data
        })
      }
    },
    mounted () {
      console.log('mounted...')
      axios.get('http://localhost:8081/hello').then(response => {
        this.msg = response.data
      })
      this.getPeople()
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
