<script>
import { VueElement } from "vue";

export default {
  data() {
    return {
      lists: [],
      userData: {
        first_name: "",
        last_name: "",
        email: "",
        id: "",
      },
      isActive: null,
      indexUser: {
        first_name: "",
        last_name: "",
        email: "",
        avatar: "",
      },
    };
  },
  methods: {
    createUser() {
      this.lists.lenghts + 1;
      axios
        .post("https://reqres.in/api/users", this.userData)
        .then((response) => console.log(response.data))
        .then(this.lists.push(this.userData));
    },
    deleteUser(k) {
      axios
        .delete("https://reqres.in/api/users")
        .then((response) => this.lists.splice(k, 1))
        .then((response) => console.log(response));
    },
    openWindow() {
      this.isActive = true;
    },
    closeWindow() {
      this.isActive = false;
    },
    idUser(id) {
      const idD = id;
      this.indexUser.first_name = this.lists[idD].first_name;
      this.indexUser.last_name = this.lists[idD].last_name;
      this.indexUser.email = this.lists[idD].email;
      this.indexUser.avatar = this.lists[idD].avatar;
      console.log();
    },
  },
  mounted() {
    axios
      .get("https://reqres.in/api/users")
      .then((response) => (this.lists = response.data.data))
      .then((response) => console.log(response.data));
  },
};
</script>

<template>
  <header class="header">
    <h1>Info List</h1>
  </header>
  <main class="main">
    <form @submit.prevent="createUser" class="form">
      <div class="form__inputs">
        <label for="first_name">First name</label>
        <input
          id="first_name"
          name="first_name"
          v-model="userData.first_name"
          type="text"
        />
      </div>
      <div class="form__inputs">
        <label for="last__name">Last Name</label>
        <input
          id="last_name"
          name="last_name"
          v-model="userData.last_name"
          type="text"
        />
      </div>
      <div class="form__inputs">
        <label for="email">Email</label>
        <input id="email" name="email" v-model="userData.email" type="text" />
      </div>
      <button v-on:click="submit" class="btn">Submit</button>
    </form>

    <div class="list__container">
      <div v-for="(list, index) in lists" :key="list.id" id="list" class="list">
        <div class="list__user">
          <img class="avatar" v-bind:src="list.avatar" alt="" />
          <p class="name" @click="openWindow(), idUser(index)">
            {{ list.first_name }}
          </p>
          <p>{{ list.last_name }}</p>
          <p>{{ list.email }}</p>
          <button class="btn__delete" v-on:click="deleteUser(index)">
            Delete
          </button>
        </div>
      </div>
    </div>

    <div :class="{ active: isActive }" class="modal">
      <div class="info">
        <img class="avatarModal" v-bind:src="indexUser.avatar" alt="" />
        <p>
          {{ indexUser.first_name }}
        </p>
        <p>{{ indexUser.last_name }}</p>
        <p>{{ indexUser.email }}</p>
        <button class="closeBtn" @click="closeWindow()">CLose</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Ubuntu:ital,wght@0,300;0,400;0,500;0,700;1,700&display=swap");
body {
  padding: none;
  margin: 0;
}
.header {
  width: 100%;
  height: 80px;
  background-color: #155c9f;
  padding-left: 50px;

  font-family: "Ubuntu", sans-serif;
  color: #ffffff;
  font-size: 24px;
}

.main {
  width: 100%;
  height: 1200px;
  background-color: #b0d2f1;
}

.modal {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* position: absolute; */
  position: fixed;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  z-index: -1;
  background-color: rgba(0, 0, 0, 0.445);
  transition: all 0.4s ease 0s;
}

.info {
  width: 500px;
  height: 300px;
  background-color: #ffffff;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;

  border-radius: 10px;

  font-family: "Ubuntu", sans-serif;
  color: #000000;
  font-size: 24px;
}

.active {
  opacity: 1;
  z-index: 2;
  transition: all 0.4s ease 0s;
}

.form {
  margin: 0 auto;
  width: 500px;
  height: 300px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  font-family: "Ubuntu", sans-serif;
  color: #ffffff;
  font-size: 24px;
}

.form__inputs {
  width: 100%;
  height: 100px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

input {
  height: 40px;
  width: 300px;
  border-radius: 10px;
}

.list__container {
  margin: 0 auto;
  margin-top: 50px;
  width: 800px;
  height: 800px;
}
.list {
  padding-left: 10px;
  padding-right: 10px;
  margin-bottom: 10px;
  width: 800px;
  height: 90px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;

  background: #465194;
  border-radius: 10px;

  font-family: "Ubuntu", sans-serif;
  color: #ffffff;
  font-size: 16px;
}

.list__user {
  width: 100%;
  height: 90px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
}

.name {
  text-decoration: underline;
  cursor: pointer;
}

.avatar {
  width: 80px;
  height: 80px;
  border-radius: 50px;
}

.avatarModal {
  width: 120px;
  height: 120px;
  border-radius: 50px;
}

.btn {
  width: 200px;
  height: 80px;
  border-radius: 10px;

  background-color: #6fcc85;
  font-family: "Ubuntu", sans-serif;
  color: #ffffff;
  font-size: 16px;
}

.btn__delete {
  width: 80px;
  height: 40px;
  background-color: #cc6f6f;
  border-radius: 10px;

  font-family: "Ubuntu", sans-serif;
  color: #ffffff;
  font-size: 16px;
}

.closeBtn {
  width: 80px;
  height: 40px;
  background-color: #cc6f6f;
  border-radius: 10px;

  font-family: "Ubuntu", sans-serif;
  color: #ffffff;
  font-size: 16px;
}
</style>
