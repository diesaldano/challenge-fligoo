<template>
  <EditUser v-if="this.$route.params.id != 'new'" @editUser="editUser($event)" @addUser="addUser($event)" />
  <AddUser v-else @addUser="addUser($event)" />
</template>
<script>
import AddUser from '../components/AddUser.vue';
import EditUser from '../components/EditUser.vue';
export default {
  name: 'Users',
  components: {
    AddUser,
    EditUser,
  },
  methods: {
    editUser($event){
      let users = JSON.parse(localStorage.getItem('users'))
      let index = users.findIndex(user => user.id == $event.id)
      users[index] = $event
      localStorage.setItem('users', JSON.stringify(users))
    },
    addUser($event){
      let users = JSON.parse(localStorage.getItem('users'))
      localStorage.setItem('users', JSON.stringify([...users, $event]))
    },
  },
  data: () => ({
    //
  }),
};
</script>
