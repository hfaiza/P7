<template>
  <section id="update-section">
    <h1>Mon profil</h1>
    <Form ref="form" />
    <p v-if="invalidInput">{{ invalidInput }}</p>
    <Button @click="sendForm" text="Mettre à jour" />
  </section>
</template>

<script lang="js">
import Form from '@/components/SignupForm.vue'
import Button from '@/components/Button.vue'
import store from '../store'

export default {
  name: 'UpdateProfile',
  components: {
    Form,
    Button
  },
  data () {
    return {
      userData: {},
      invalidInput: '',
      token: store.state.token,
      userId: store.state.userId
    }
  },
  created () {
    this.getUserData()
  },
  methods: {
    async getUserData () {
      try {
        const getData = await fetch(`${process.env.VUE_APP_URL_API}/users/${this.userId}`, {
          headers: { Authorization: `Bearer ${this.token}` }
        })
        const userData = await getData.json()
        this.userData = userData
        this.$refs.form.lastName = this.userData.last_name
        this.$refs.form.firstName = this.userData.first_name
        this.$refs.form.email = this.userData.email
      } catch (error) {
        console.log(error)
      }
    },
    async sendForm () {
      try {
        let userPassword = this.$refs.form.password
        if (userPassword.length === 0) {
          userPassword = '0'
        }
        const formData = new FormData()
        formData.append('image', this.$refs.form.file)
        formData.append('password', userPassword)
        formData.append('lastName', this.$refs.form.lastName)
        formData.append('firstName', this.$refs.form.firstName)
        const data = await fetch(`${process.env.VUE_APP_URL_API}/users/${this.userId}`, {
          method: 'PUT',
          headers: { Authorization: `Bearer ${this.token}` },
          body: formData
        })
        if ([400, 500].includes(data.status)) {
          const response = await data.json()
          this.invalidInput = `${response.error}`
        } else {
          this.$router.push({ name: 'UserProfile', params: { id: this.userId } })
        }
      } catch (error) {
        console.log(error)
      }
    }
  }
}
</script>

<style scoped lang="scss">
#update-section {
  margin: 3rem auto;
  padding: 0.5rem 1.5rem;
  border: solid 0.001rem #f9f7f7;
  background-color: #f2f2f2;
  border-radius: 1rem;
  box-shadow: 0 0 0.3rem #d3d3d3;

  @media (max-width: 1050px) {
    border: none;
    box-shadow: none;
    padding: 0.5rem;
  }
}

h1 {
  font-size: 2.5rem;
  margin: 0.5rem 0;

  @media (max-width: 1050px) {
    margin: -1rem auto 2rem auto;
  }
}

p {
  color: #c50404;
  border: solid 0.1rem #c50404;
  background-color: #fcf3f3;
  margin: 2rem 6rem 0 6rem;
  padding: 1rem 1rem;

  @media (max-width: 895px) {
    margin: 2rem 0.5rem 0 0.5rem;
  }
}
</style>
