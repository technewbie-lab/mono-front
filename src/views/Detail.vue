<template>
  <v-container>
    <v-row class="mt-10 mb-10">
      <v-col justify="center" align="center">
        <h3>ユーザー詳細</h3>
      </v-col>
    </v-row>
    <Form
      @submit="update"
      pageType="detail"
      isEdit="true"
      :userId="userId"
      :name="userName"
    >
      UPDATE
    </Form>
  </v-container>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import axios from "axios";
import Form from "../components/Form.vue";

@Component({ components: { Form } })
export default class UserDetail extends Vue {
  userId = "";
  userName = "";

  async created() {
    const url = "http://localhost:8080/users";
    this.userId = this.$route.params.id;
    const response = await axios.get(url, {
      params: {
        id: this.userId,
      },
    });
    this.userName = response.data.users[0].name;
  }
  async update(name: string) {
    const answer = confirm(`更新してもよろしいですか？`);
    if (answer) {
      const url = "http://localhost:8080/users/update";
      const params = {
        id: this.userId,
        name: name,
      };
      const response = await axios.post(url, params);
      if (response.data.code === 200) {
        this.userName = name;
      }
    }
  }
}
</script>
