<template>
  <div class="login container-fluid bg-fundo">
    <div class="row">
      <div class="col-4">
        <div class="logo text-center">
          <img
            :src="require('../assets/img/login-icon.png')"
            alt="logo"
            class="logo-icon"
          />
          <div class="logo-text">CoolPost</div>
          <hr />
          <p>Publish your articles, thoughts, ideas.</p>
        </div>
      </div>
      <div class="col-8 centralizar">
        <div>
          <h1 class="title text-center">Sing In</h1>
          <hr class="line-singin" />
        </div>
        <form autocomplete="off" @submit.prevent="login" class="login-form">
          <input
            type="text"
            v-model="user.name"
            id="username"
            placeholder="Username"
          />
          <input
            type="password"
            v-model="user.zipcode"
            id="password"
            placeholder="Password"
          />
          <button>Login</button>
        </form>
      </div>
    </div>
    <div class="pessoas"></div>
  </div>
</template>

<script>
import api from '../services/api';

export default {
  name: 'login',
  data() {
    return {
      user: {
        name: 'Bret',
        zipcode: '92998-3874',
      },
      id: null,
    };
  },
  methods: {
    mounted() {
      if (localStorage.id) {
        this.id = localStorage.id;
      }
    },

    login: async function() {
      try {
        let { data: user } = await api.get('users?username=' + this.user.name);

        if (
          user.length &&
          user[0].username === this.user.name &&
          user[0].address.zipcode === this.user.zipcode
        ) {
          this.id = user[0].id;

          localStorage.id = JSON.stringify(this.id);
          localStorage.user = JSON.stringify(user[0]);
          this.$router.push('/');
        } else {
          alert('Usuario nao existe ou senha incorreto');
        }
      } catch (Erro) {
        console.log('Erro', Erro);
      }
    },
  },
};
</script>

<style lang="css">
.bg-fundo {
  height: 100vh;
  background-image: url(../assets/img/background.png);
  background-repeat: no-repeat;
  background-position: left;
  background-size: contain;
  position: fixed;
}
.pessoas {
  width: 439px;
  height: 500px;
  background-image: url(../assets/img/login-img.png);
  background-repeat: no-repeat;
  background-position: left bottom;
  background-size: contain;
  position: fixed;
  bottom: 0;
}
.centralizar {
  height: 100vh;
  padding-top: 10%;
  display: flex;
  flex-direction: column;
}
.line-singin {
  width: 500px;
  color: #b5a2a2;
}
.login-form {
  width: 400px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  margin: 0 auto;
}
.login-form input {
  height: 50px;
  color: #746d6d;
  border: 0.5px solid #a591b6;
  border-left: 6px solid #a591b6;
  padding: 5px 8px;
  margin-bottom: 25px;
}
.login-form input:focus {
  border-color: #b5a2a2;
}
.login-form button {
  height: 50px;
  background-color: #a591b6;
  border: 0;
  color: #ffffff;
  padding: 6px;
  font-weight: bold;
  margin-top: 10px;
  box-shadow: 2px 2px 5px #a591b6;
}
.logo-icon {
  width: 90px;
  margin: 0 auto;
}
.logo-text {
  font-size: 40px;
  font-weight: 900;
  color: #ffffff;
}
.logo {
  color: #ffffff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 5rem;
  width: max-content;
}
.logo hr {
  background-color: #ffffff;
  width: 350px;
  margin-top: 0;
}
.logo p {
  font-size: 20px;
}
</style>
