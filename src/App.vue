<template>
  <div id="app">
    <div class="user" :key="user.id.value" v-for="user in users">
      <div>
        <img class="img" :src="user.picture.large" />
      </div>
      <div>
        <p>{{ user.name.first }} {{ user.name.last }}</p>
        <ul></ul>
      </div>
    </div>
    <div ref="divAsTarget">
      <h3>Loading ...</h3>
    </div>/* below the user's list */
  </div>
</template>

<script>
import axios from "axios";
const options = {
  root: null,
  threshold: 0
};
export default {
  name: "app",
  data: () => {
    return {
      users: [],
      observer: null
    };
  },
  methods: {
    fetchUsers() {
      for (var i = 0; i < 5; i++) {
        axios.get(`https://randomuser.me/api/`).then(response => {
          this.users.push(response.data.results[0]);
          console.log(response);
        });
      }
    },
    callback(entries, observer) {
      console.log(entries)
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          this.fetchUsers();
        }
      });
    }
  },
  beforeMount() {
    this.fetchUsers();
  },
  mounted() {
    this.observer = new IntersectionObserver(this.callback, options);
    this.observer.observe(this.$refs.divAsTarget);
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
