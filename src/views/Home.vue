<template>
  <v-container>
    <v-row>
      <v-col align="left" class="mt-10">
        <h3>検索条件</h3>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4">
        <v-text-field
          v-model="userName"
          label="User Name"
          type="text"
          prepend-inner-icon="mdi-magnify"
          outlined
          dense
          id="inputName"
        >
        </v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4" align="right" class="pt-0">
        <v-btn @click="search" id="search">Search</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col align="left">
        <h3>検索結果</h3>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="6">
        <v-simple-table>
          <thead>
            <tr>
              <td v-for="head in header" :key="head">{{ head }}</td>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in userData" :key="user.id">
              <td>{{ user.id }}</td>
              <td>{{ user.name }}</td>
              <td>
                <router-link
                  :to="{
                    name: 'detail',
                    params: { id: user.id },
                  }"
                >
                  <v-icon id="edit-icon">mdi-account-edit</v-icon>
                </router-link>
                <v-btn text icon @click="deleteUser(index, user.id)">
                  <v-icon id="delete-icon">mdi-trash-can</v-icon>
                </v-btn>
              </td>
            </tr>
          </tbody>
        </v-simple-table>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";

@Component
export default class Home extends Vue {
  header: string[] = ["User Id", "User Name", "Operation"];
  userName = "";
  userData = [];

  async search() {
    const url = "http://localhost:8080/users";
    const response = await axios.get(url, {
      params: {
        name: this.userName,
      },
    });
    this.userData = response.data.users;
  }

  async deleteUser(index: number, id: number) {
    const answer = confirm(`本当にID:${id}のユーザーを削除しますか`);
    if (answer) {
      const url = `http://localhost:8080/users/delete/${id}`;
      const response = await axios.get(url);
      if (response.data.code === 200) {
        this.userData.splice(index, 1);
      }
    }
  }
}
</script>

<style lang="scss" scoped>
a {
  text-decoration: none;
}

h3 {
  padding: 0.25em 0.5em;
  color: #494949;
  background: transparent;
  border-left: solid 5px #7db4e6;
}
</style>
