<template>
  <v-container align-center align-content-center>
    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <h1 class="title">Users</h1>
        <template v-for="(user, index) in users">
        <v-card :key="index" class="card_layout">
          <v-avatar
            :tile="false"
            size="120px"
            color="grey lighten-4"
          >
            <img :src="user.avatar" alt="avatar">
          </v-avatar>
          <div>
            <v-card-title>
              <span >{{user.first_name}} {{user.last_name}}</span>
            </v-card-title>
            <v-card-text>
              {{user.email}}
            </v-card-text>
            <v-card-actions>
              <v-btn color="primary" @click="editUser(user)">Edit</v-btn>
              <v-btn color="green" @click="addUser(user)">Create</v-btn>
              <v-btn color="error" @click="deleteUser(user)">Delete</v-btn>
            </v-card-actions>
          </div>
        </v-card>
        </template>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    name: 'Users',
    async created() {
      try {
          const localStorageUsers = localStorage.getItem('users')
          // let initialValue;
          if (!localStorageUsers) {
            let res = await fetch('https://reqres.in/api/users')
            let data = await res.json()
            this.data = data.data
            localStorage.setItem('users', JSON.stringify(this.data))
            } else {
            this.data = JSON.parse(localStorageUsers)
          }
        } catch (error) {
          console.log(error)
        }

    },
    computed: {
      users() {
        return this.data || [];
      }
    },
    methods: {
      editUser(user){
        this.$router.push({
          name: 'Users',
          params: {
            id: user.id
          }
        })
      },
      addUser(){
        this.$router.push({
          name: 'Users',
          params: {
            id: 'new'
          }
        })
      },
      deleteUser(user){
        let index = this.data.findIndex(item => item.id == user.id)
        this.data.splice(index, 1)
        localStorage.setItem('users', JSON.stringify(this.data))
      }
    },
    watch: {
    },
    data: () => ({
      data: []
    }),
  }
</script>
<style>
  .card_layout{
    display: flex;
    flex-direction: row;
    justify-content: space-evenly;
    align-content: center;
    align-items: center;
  }
  .title{
    font-size: 2rem !important;
    margin: .5em;
    font-weight: bold;
    color: #2c3e50;
    text-align: center;
  }

</style>
