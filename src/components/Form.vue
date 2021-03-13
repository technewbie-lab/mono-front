<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <v-row justify="center">
      <v-col cols="4">
        <v-row>
          <v-col>
            <v-text-field
              v-if="isDetail"
              :value="userId"
              label="Id"
              required
              filled
              readonly
              id="id"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row class="mb-10">
          <v-col>
            <v-text-field
              :value="name"
              v-model="name"
              label="Name"
              required
              :filled="isEditMode"
              :readonly="isEditMode"
              :rules="[required]"
              id="name"
            ></v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-btn v-if="isEditMode" @click="edit" color="success" id="edit">
            Edit
          </v-btn>
          <v-btn v-else @click="submit" color="primary" id="submit">
            <slot></slot>
          </v-btn>
          <v-btn @click="back" class="ml-5" id="back">
            Back
          </v-btn>
        </v-row>
      </v-col>
    </v-row>
  </v-form>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Form extends Vue {
  @Prop({ default: 0 })
  userId!: number;
  @Prop({ default: "" })
  name!: string;
  @Prop({ default: "" })
  pageType!: string;
  @Prop({ default: false })
  isEdit!: boolean;

  // validation
  valid = true;
  required: any = (value: any) => !!value || "Name is required";

  get isDetail() {
    return this.pageType === "detail";
  }

  get isEditMode() {
    return this.pageType === "detail" && this.isEdit;
  }

  edit() {
    this.isEdit = !this.isEdit;
  }

  submit() {
    const result = (this.$refs.form as Vue & {
      validate: () => boolean;
    }).validate();

    // validation error
    if (!result) return;

    this.$emit("submit", this.name);
  }

  back() {
    this.$router.push("/");
  }
}
</script>
