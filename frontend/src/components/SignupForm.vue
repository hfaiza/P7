<template>
  <form>
    <div>
      <label for="lastName">Nom :</label>
      <input v-model="lastName" id="lastName" placeholder="Grose" type="text" autocomplete="on" required />
    </div>
    <div>
      <label for="firstName">Prénom :</label>
      <input v-model="firstName" id="firstName" placeholder="Hannah" type="text" autocomplete="on" required />
    </div>
    <div>
      <label for="email">Adresse e-mail :</label>
      <input
        v-model="email"
        id="email"
        placeholder="hannah.grose@groupomania.com"
        type="email"
        autocomplete="on"
        :required="signup"
        :disabled="updateProfile"
      />
    </div>
    <div>
      <label for="password">Mot de passe :</label>
      <input v-model="password" id="password" placeholder="••••••••" type="password" autocomplete="on" required />
    </div>
    <div class="icon">
      <label for="picture">Photo de profil :</label>
      <button @click="uploadFile" type="button">Charger une image (optionnel)</button>
      <input
        ref="file"
        v-on:change="handleFileUpload()"
        id="picture"
        type="file"
        accept="image/jpeg, image/png"
      />
    </div>
    <p v-if="invalidInput">{{ invalidInput }}</p>
  </form>
</template>

<script lang="js">
export default ({
  name: 'SignupForm',
  data () {
    return {
      lastName: '',
      firstName: '',
      email: '',
      password: '',
      file: '',
      invalidInput: ''
    }
  },
  computed: {
    signup () {
      return this.$route.name === 'Signup'
    },
    updateProfile () {
      return this.$route.name === 'UpdateProfile'
    }
  },
  methods: {
    uploadFile () {
      document.getElementById('picture').click()
    },
    handleFileUpload () {
      const filetype = this.$refs.file.files[0].type
      if (['image/png', 'image/jpeg'].includes(filetype)) {
        this.file = this.$refs.file.files[0]
      } else {
        this.invalidInput = 'Seuls les formats JPG, JPEG et PNG sont acceptés.'
      }
    }
  }
})
</script>

<style scoped lang="scss">
form {
  div {
    display: flex;
    flex-direction: column;
    padding: 0 6rem;

    @media (max-width: 895px) {
      padding: 0 0.5rem;
    }
  }

  label {
    text-align: left;
    text-transform: uppercase;
    font-weight: bold;
    padding: 0.5rem;
  }

  input {
    background-color: #f2f2f2;
    border: solid 0.12rem #091f43;
    border-radius: 3rem;
    height: 2rem;
    margin-bottom: 1rem;
    font-size: 1rem;
    padding-left: 1rem;

    @media (max-width: 1050px) {
      background-color: #fff;
    }
  }
}

.icon {
  justify-content: space-between;
  flex-direction: row;
  margin: 0.5rem 0;

  @media (max-width: 1050px) {
    flex-direction: column;
  }

  button {
    border: none;
    border-radius: 3rem;
    background-color: #585757;
    width: 77%;
    cursor: pointer;
    color: white;
    font-weight: bold;
    text-transform: uppercase;
    font-size: 1rem;
    box-shadow: 0 0 0.5rem #adabab;
    transition: all 0.2s ease-in-out;

    &:hover {
      box-shadow: 0 0 1rem #adabab;
    }

    @media (max-width: 1050px) {
      width: 100%;
      padding: 0.5rem;
      margin-top: 0.5rem;
    }
  }
}

input[type="file"] {
  display: none;
}

:disabled {
  color: rgb(73, 73, 73);
  background-color: #dddbdb;
}

p {
  color: #c50404;
  border: solid 0.1rem #c50404;
  background-color: #fcf3f3;
  margin: 2rem 6rem 0 6rem;
  padding: 1rem;

  @media (max-width: 895px) {
    margin: 2rem 0.5rem 0 0.5rem;
  }
}
</style>
