<template>
  <div id="app">
    <v-app>
      <v-app-bar color="primary" app dark>
        <v-toolbar-title>Mono-Training</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-toolbar-items>
          <v-btn text to="/">ホーム</v-btn>
          <v-btn text to="/register">ユーザ登録</v-btn>
        </v-toolbar-items>
      </v-app-bar>
      <v-main>
        <router-view />
      </v-main>
    </v-app>
  </div>
</template>
