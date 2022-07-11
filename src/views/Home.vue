<template>
  <Users :users="data"/>
</template>
<script>
import Users from '../components/Users.vue';
export default {
  name: 'Home',
  components: {
    Users,
  },
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
  data: () => ({
    data: []
  }),
};
</script>
