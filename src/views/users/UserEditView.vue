<script>
import axios from 'axios';
import { ref } from 'vue';

const formData = ref(null);

const getUserById = (userId) => {
  axios.get(`http://localhost:8080/v1/users/${userId}`).then(res => {
    formData.value = res.data;
  }).catch(err => {
    console.log(err);
  })
}

const editUser = (data) => {
  axios.put(`http://localhost:8080/v1/users/${data.id}`, data).then(res => {
    alert('User Edit successfully');
    formData.value = {
      username: '',
      password: '',
      email: '',
      firstName: '',
      lastName: '',
      age: 0,
      birthDate: ''
    };
  }).catch(err => {
    console.log(err);
  })
}

export default {
  name: 'UserEditView',
  setup() {
    return {
      formData,
      getUserById,
      editUser
    }
  },
  computed: {
    formattedBirthDate() {
      const [year, month, day] = this.formData.birthDate;
      return `${year}-${month.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
    }
  },
  created() {
    this.getUserById(this.$route.params.id);
  }
}

</script>

<template>
  <div class="form-user-create" style="display: flex; justify-content: center; align-items: center; height: 100vh; padding: 20px; box-sizing: border-box;"> 
    <div style="width: 80%; max-width: 400px;">
    <FormKit
      type="form"
      submit-label="Save"
      @submit="editUser"
    >
      <FormKit
        type="hidden"
        name="id"
        :value="formData.id"
      ></FormKit>
      <FormKit
        type="text"
        label="Username"
        name="username"
        :value="formData.username"
        placeholder="Enter username"
        validation="required|length:3,150"
        :validation-messages="{
          required: 'Username is required',
        }"
      />
      <FormKit
        type="password"
        label="Password"
        name="password"
        placeholder="Enter the new password"
        validation="required|length:3,150"
        :validation-messages="{
          required: 'Password is required',
        }"
      />
      <FormKit
        type="email"
        label="Email"
        name="email"
        :value="formData.email"
        placeholder="Enter email"
        validation="required|email"
        :validation-messages="{
          required: 'Email is required',
          email: 'Email must be a valid email'
        }"
      />
      <FormKit
        type="text"
        label="First Name"
        name="firstName"
        :value="formData.firstName"
        placeholder="Enter first name"
        validation="required|length:3,100"
        :validation-messages="{
          required: 'First name is required',
        }"
      />
      <FormKit
        type="text"
        label="Last Name"
        name="lastName"
        :value="formData.lastName"
        placeholder="Enter last name"
        validation="required|length:3,100"
        :validation-messages="{
          required: 'Last name is required',
        }"
      />
      <FormKit
        type="number"
        label="Age"
        name="age"
        :value="formData.age"
        placeholder="Enter age"
        validation="min:0|max:150"
        :validation-messages="{
          min: 'Age must be at least 0',
          max: 'Age must be less than 150'
        }"
      />
      <FormKit
        type="date"
        label="Birthdate"
        name="birthDate"
        :value="formattedBirthDate"
        placeholder="Enter birthdate"
      />
    </FormKit>
    <div style="display: flex;">
      <span>
        <FormKit
          type="button"
          label="Back"
          @click="$router.push('/users')"
        />
      </span>
    </div>
    <div style="display: none;">
      <pre wrap>{{ formData }}</pre>
    </div>
    </div>
  </div>
</template>

<style scoped>

</style>
