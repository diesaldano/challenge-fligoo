<template>
  <v-container align-center align-content-center >
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 >
        <h1 class="title">Create {{this.$route.params.id}}</h1>
        <v-form @submit.prevent ref="form" lazy-validation v-model="valid">
          <v-text-field
            v-model="name"
            type="text"
            placeholder="username"
            label="Username"
            :rules="rules.name"
          />
          <v-text-field
            v-model="last_name"
            type="text"
            label="Last Name"
            :rules="rules.last_name"
          /> 
          <v-text-field
            v-model="email"
            type="email"
            label="E-mail"
            :rules="rules.email"
          /> 
          <v-btn color="primary" @click="create">
            Create
          </v-btn>
        </v-form>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
export default {
  name: 'AddUser',
  computed: {
    avatar() {
      return `https://ui-avatars.com/api/?name=${this.first_name}+${this.last_name}&size=150`
    }
  },
  methods: {
    create(){
      this.$refs.form.validate()
      if(this.valid){
        let user = {
          id: Math.floor( Math.random() * 100 ),
          email: this.email,
          first_name: this.name,
          last_name: this.last_name,
          avatar: this.avatar,
        }
        this.$emit('addUser', user)
        this.$router.push('/')
      }

    },
  },
  data() {
    return {
      name: '',
      last_name: '',
      email: '',
      valid: false,
      user: null,
      rules: {
        email: [
          v => !!v || 'E-mail is required',
          v => /.+@.+\..+/.test(v) || 'E-mail must be valid'
        ],
        name: [
          (value) => !!value || 'Username is required',
          (value) => (value && value.length <= 10) || 'Username must be less than 10 characters',          
        ],
        last_name: [
          (value) => !!value || 'Last Name is required',
          (value) => (value && value.length <= 10) || 'Last Name must be less than 10 characters',          
        ],
      }
    }
  },
}
</script>