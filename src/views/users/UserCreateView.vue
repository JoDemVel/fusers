<script>
import axios from 'axios';
import { ref } from 'vue';

const formData = ref(null);

const createUser = () => {
  axios.post('http://localhost:8080/v1/users', formData.value).then(res => {
    alert('User created successfully');
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

</script>

<template>
  <div class="form-user-create" style="display: flex; justify-content: center; align-items: center; height: 100vh; padding: 20px; box-sizing: border-box;"> 
    <div style="width: 80%; max-width: 400px;">
    <FormKit
      type="form"
      submit-label="Create User"
      @submit="createUser"
    >
      <FormKit
        type="text"
        label="Username"
        name="username"
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
        placeholder="Enter password"
        validation="required|length:3,150"
        :validation-messages="{
          required: 'Password is required',
        }"
      />
      <FormKit
        type="email"
        label="Email"
        name="email"
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
    <pre wrap>{{ formData }}</pre>
    </div>
  </div>
</template>

<style scoped>

</style>
