<template>
  <div class="search">
    <h1>Search</h1>
    <v-text-field solo dense type="text" v-model="name" />
    <p>{{ name }}</p>
    <h1>Users</h1>
    <table class="users">
      <thead>
        <th v-for="(header, index) in headers" :key="index">
          {{ header.text }}
        </th>
      </thead>
      <tbody>
        <tr v-for="(item, index) in filteredUsers" :key="index">
          <td>{{ item.name }}</td>
          <td>{{ item.address }}</td>
          <td>{{ item.city }}</td>
          <td>{{ item.email }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      items: [],
      headers: [
        { text: "Name", value: "name" },
        { text: "Address", value: "calories" },
        { text: "City", value: "fat" },
        { text: "Email", value: "carbs" },
      ],
      name: "",
    };
  },
  methods: {
    async getUsers() {
      await this.$axios.get("http://localhost:3000/users").then((response) => {
        this.items = response.data;
      });
    },
    logUsers() {
      console.log(this.items);
    },
  },
  created() {
    this.getUsers();
    this.logUsers();
  },
  computed: {
    filteredUsers() {
      return this.items.filter((item) => {
        return item.name.toLowerCase().includes(this.name);
      });
    },
  },
};
</script>

<style>
.users {
  width: 100%;
}
.users thead,
.users tr {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
}
.users td {
  text-align: center;
}
</style>
