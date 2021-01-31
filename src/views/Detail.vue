<template>
  <v-app>
    <v-container>
      <v-row justify="center" align="center">
        <v-col cols="12">
          <v-card max-width="300">
            <v-list-item>
              <v-list-item-avatar>
                <v-icon x-large> mdi-account-circle </v-icon>
              </v-list-item-avatar>
              <v-list-item-content>
                <v-list-item-subtitle>{{ userId }}</v-list-item-subtitle>
                <v-list-item-title class="title">
                  {{ userName }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";

@Component
export default class UserDetail extends Vue {
  userId = "";
  userName = "";

  // インスタンスライフサイクルフックによる割り込み処理
  async created() {
    const url = "http://localhost:9000/users";
    this.userId = this.$route.params.id;
    const response = await axios.get(url, {
      params: {
        id: this.userId,
      },
    });
    this.userName = response.data.users[0].name;
  }
}
</script>