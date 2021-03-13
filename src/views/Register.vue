<template>
  <div>
    <v-row class="mt-10 mb-10">
      <v-col justify="center" align="center">
        <h3>ユーザ登録</h3>
      </v-col>
    </v-row>
    <v-container>
      <Form @submit="register" pageType="register">
        Register
      </Form>
    </v-container>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import axios from "axios";
import Form from "../components/Form.vue";

@Component({ components: { Form } })
export default class Register extends Vue {
  async register(name: string) {
    const answer = confirm(`登録してもよろしいですか？`);
    if (answer) {
      const url = "http://localhost:8080/users/create";
      const params = {
        name: name,
      };
      const response = await axios.post(url, params);
      if (response.data.code === 200) {
        this.$router.push("/");
      }
    }
  }
}
</script>
