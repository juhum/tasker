<template>
  <div class="signup-container">
    <h1>Sign Up</h1>
    <form @submit.prevent="submitForm" class="signup-form">
      <div class="form-field">
        <label for="username">Username</label>
        <input type="text" id="username" v-model="username" class="input-field">
      </div>

      <div class="form-field">
        <label for="password">Password</label>
        <input type="password" id="password" v-model="password" class="input-field">
      </div>

      <div class="form-field">
        <label for="password2">Confirm Password</label>
        <input type="password" id="password2" v-model="password2" class="input-field">
      </div>

      <div v-if="errors.length" class="error-notification">
        <p v-for="error in errors" :key="error">{{ error }}</p>
      </div>

      <div class="form-field">
        <button type="submit" class="signup-btn">Sign Up</button>
      </div>
    </form>

    <hr>

    <router-link to="/login" class="login-link">Already have an account?</router-link>
  </div>
</template>


<script>
import axios from 'axios'
export default {
    name: "SignUpView",
    data (){
        return {
            username: '',
            password: '',
            password2: '',
            errors: []
        }
    },
    mounted(){
    document.title = "Sign Up"
    },
    methods: {
        submitForm(){
            this.errors = []

            if (this.username === ''){
                this.errors.push("Username is missing")
            }

            if (this.password === ''){
                this.errors.push("Password is too short")
            }

            if (this.password !== this.password2){
                this.errors.push("Passwords doesn\'t match")
            }

            if (!this.errors.length){
                const formData = {
                    username: this.username,
                    password: this.password
                }

                axios
                .post("/api/v1/users/", formData)
                .then(response =>{
                    this.registrationMessage = 'Registration successful!';
                    this.$router.push('/login')
                }).catch(error =>{
                    if (error.response){
                        for (const property in error.response.data) { 
                            this.errors.push(`${property}: ${error.response.data[property]}`)
                        }
                        console.log(JSON.stringify(error.response.data))
                    } else if(error.message) {
                        this.errors.push('Something went wrong. Please try again')
                        console.log(JSON.stringify(error))
                    }
                })
                
                
            }
        }
    }
}
</script>


<style scoped>
.signup-container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.signup-form {
  display: flex;
  flex-direction: column;
}

.form-field {
  margin-bottom: 10px;
}

.label {
  font-weight: bold;
}

.input-field {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
  width: 100%;
}

.error-notification {
  margin-top: 10px;
  background-color: red;
  padding: 10px;
  border: 1px solid #ff9999;
  border-radius: 3px;
}

.signup-btn {
  padding: 8px 16px;
  background-color: #121212;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.signup-btn:hover {
  background-color: black;
}

.login-link {
  display: block;
  margin-top: 20px;
  text-align: center;
  color: #007bff;
  text-decoration: none;
}

.login-link:hover {
  text-decoration: underline;
}
</style>