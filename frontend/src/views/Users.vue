<template>
  <section>
    <h1>Membres</h1>
    <ul v-for="user of users" :key="user.id">
      <li>
        <router-link style="text-decoration: none;" :to="{ name: 'UserProfile', params: { id: user.user_id } }">
          <span>
            <img
              class="profile-pic"
              :src="user.user_picture"
              :alt="`Photo de profil de ${user.first_name} ${user.last_name}.`"
            />
            <h2>{{ user.first_name }} {{ user.last_name }}</h2>
            <img class="logo" :src="require(`@/assets/pale-logo.png`)" alt="Logo de Groupomania." />
          </span>
        </router-link>
      </li>
    </ul>
  </section>
</template>

<script lang="js">
import store from '../store'

export default ({
  name: 'Users',
  data () {
    return {
      users: [],
      token: store.state.token
    }
  },
  created () {
    this.getUsers()
  },
  methods: {
    async getUsers () {
      try {
        const getData = await fetch(`${process.env.VUE_APP_URL_API}/users`, {
          headers: { Authorization: `Bearer ${this.token}` }
        })
        const users = await getData.json()
        this.users = users
      } catch (error) {
        console.log(error)
      }
    }
  }
})
</script>

<style scoped lang="scss">
section {
  margin-bottom: 3rem;
}

h1 {
  font-size: 2.5rem;
}

h2 {
  font-size: 1.5rem;
  font-weight: bold;

  @media (max-width: 769px) {
    font-size: 1.3rem;
    padding-left: 1rem;
  }
}

ul {
  list-style-type: none;
  padding-left: 0;
}

span {
  background-color: #fff;
  text-decoration: none;
  color: #091f43;
  display: flex;
  align-items: center;
  height: 6rem;
  margin: 1rem 7rem;
  border-radius: 5rem;
  box-shadow: 0 0 0.5rem #d3d3d3;
  transition: all 0.2s ease-in-out;

  &:hover {
    color: #fd2d01;
    transform: scale(1.02);
  }

  @media (max-width: 1050px) {
    margin: 1rem;
  }

  @media (max-width: 769px) {
    border-radius: 0;
    box-shadow: 0 0 0.2rem #d3d3d3;
  }
}

span > p,
span > img {
  top: 5rem;
}

.profile-pic {
  border-radius: 50%;
  height: 4rem;
  width: 4rem;
  object-fit: cover;
  border: solid 0.2rem #091f43;
  margin: 0 3rem;

  @media (max-width: 769px) {
    margin: 1.5rem;
  }
}

.logo {
  height: 4rem;
  text-align: right;
  margin-left: auto;
  margin-right: 3rem;

  @media (max-width: 769px) {
    display: none;
  }
}
</style>
