<script>
import axios from 'axios';
export default {
  name: 'UserView',
  data() {
    return {
      users: []
    }
  },
  methods: {
    getUsers() {
      axios.get('http://localhost:8080/v1/users?detailed=true').then(res => {
        this.users = res.data;
      }).catch(err => {
        console.log(err);
      })
    },

    deleteUserById(userId) {
      if (confirm('Are you sure you want to delete this user?')) {
        axios.delete(`http://localhost:8080/v1/users/${userId}`).then(res => {
          alert('User deleted successfully');
          this.getUsers();
        }).catch(err => {
          console.log(err);
        })
      }
    }
  },
  mounted() {
    this.getUsers();
  }
}
</script>

<template>
  <div class="busers">
    <div class="card">
      <div class="card-header">
        <h4>
          Users
          <RouterLink to="/users/create" class="btn btn-primary float end">
            Add User
          </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table table-bordered">
          <thead>
            <tr>
              <th>Nro.</th>
              <th>Id</th>
              <th>Username</th>
              <th>Email</th>
              <th>First Name</th>
              <th>Last Name</th>
              <th>Age</th>
              <th>Birthdate</th>
            </tr>
          </thead>
          <tbody v-if="users.length > 0">
            <tr v-for="(user, index) in users" :key="index">
              <td>{{ index + 1 }}</td>
              <td>{{ user.id }}</td>
              <td>{{ user.username }}</td>
              <td>{{ user.email }}</td>
              <td>{{ user.firstName }}</td>
              <td>{{ user.lastName }}</td>
              <td>{{ user.age }}</td>
              <td>{{ user.birthDate }}</td>
              <td>
                <RouterLink :to="{path: '/users/'+user.id+'/edit'}" class="btn btn-primary">
                  Edit
                </RouterLink>
                <button type="button" @click="deleteUserById(user.id)" class="btn btn-danger">
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr>
              <td colspan="7" class="text-center">No data</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
