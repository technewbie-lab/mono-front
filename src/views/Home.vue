<template>
  <v-app>
    <v-container>
      <h3>検索条件</h3>
      <v-row>
        <v-col cols="12" sm="3" md="3" lg="3">
          <v-text-field
            v-model="userName"
            label="User Name"
            type="text"
            prepend-icon="mdi-magnify"
          >
            <template v-slot:append-outer>
              <v-btn @click="search">Search</v-btn>
            </template>
          </v-text-field>
        </v-col>
      </v-row>
      <h3>検索結果</h3>
      <v-row>
        <v-col cols="12" sm="3" md="3">
          <v-simple-table>
            <thead>
              <tr>
                <td v-for="head in header" :key="head"> {{ head }}</td>
              </tr>
            </thead>
            <tbody>
              <tr v-for="user in userData" :key="user.id">
                <td>
                  <router-link :to="`/detail/${user.id}`">
                    {{ user.id }}
                  </router-link>
                </td>
                <td> {{ user.name }} </td>
              </tr>
            </tbody>
          </v-simple-table>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import axios from "axios";

@Component
export default class Home extends Vue {
  header: string[] = ['User Id', 'User Name'];
  userName = '';
  userData = [];

  async search () {
    const url = 'http://localhost:9000/users'
    const response = await axios.get(url, {
      params: {
        nameLike: this.userName
      }
    });
    this.userData = response.data.users;
  }
}
</script>
