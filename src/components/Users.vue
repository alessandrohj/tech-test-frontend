<template>
    <div class="container">
        <h1>Users</h1>
        <div v-for="user in users" :key="user.id" class="users">
            <div v-bind:class="{'high':(user.highest === true)}">
            <img v-bind:src="user.picture" v-bind:alt="`${user.username}`">
            <h2>Username: {{user.username}}</h2>
            <h3 class="points">Points: {{user.score}}</h3>
            </div>
        </div>
        </div>
</template>

<script>
export default {
  data () {
      return { 
          users: [],
      }
  },
  mounted(){
      setInterval(()=>{
          fetch('http://localhost:3030/api/users')
          .then(resp => {
              if (resp.ok) return resp.json()
              else console.error('fetch error')
          })
          .then(({data}) => {
              let sortedData = data.sort((a,b) =>{
                return parseInt(b.score) - parseInt(a.score)
            })
                sortedData[0].highest = true
                console.log(data)
              this.users = sortedData
          })
          .catch(error => console.log(error))

      }, 1000)
  },
  methods: {
     highlight: ()=>{
         this.users.forEach(item => {
             if (item.highest === true) item.parentNode.classList.add('high')
         })
     },
  }
}
</script>

<style scoped>
.container {
    display: flex;
}
 .individual {
     padding: 1rem;
 }
  img {
    border-radius: 50%;
    border: 5px #333 solid;
    margin-bottom: 1rem;
  }

  .high{
    background-color: rgb(221, 230, 236);
  }

</style>