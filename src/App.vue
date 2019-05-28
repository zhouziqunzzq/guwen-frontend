<template>
  <v-app>
    <v-toolbar app color="indigo darken-3" dark>
      <v-toolbar-side-icon @click="showDrawer = !showDrawer"></v-toolbar-side-icon>
      <v-toolbar-title class="headline text-uppercase">
        <span>神经网络古文翻译</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn v-for="item in navItems"
             :key="item.title"
             :to="item.link"
             flat
             class="hidden-sm-and-down"
      >
        {{ item.title }}
      </v-btn>
    </v-toolbar>

    <v-navigation-drawer
        app absolute temporary
        v-model="showDrawer"
    >
      <v-toolbar flat>
        <v-list>
          <v-list-tile>
            <v-list-tile-title class="title">
              神经网络古文翻译
            </v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-toolbar>
      <v-divider></v-divider>
      <v-list dense class="pt-0">
        <v-list-tile
            v-for="item in navItems"
            :key="item.title"
            :to="item.link"
        >
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>

    <v-snackbar
        v-model="snackbar.show"
        :color="snackbar.color"
        :timeout="3000"
    >
      {{ snackbar.msg }}
      <v-btn
          dark
          flat
          @click="$store.commit('hideSnackbar')"
      >
        关闭
      </v-btn>
    </v-snackbar>

    <v-content>
      <keep-alive>
        <router-view></router-view>
      </keep-alive>
    </v-content>
  </v-app>
</template>

<script lang="ts">
  import HelloWorld from "./views/Home.vue";
  import Vue from "vue";
  import {mapState} from "vuex";

  export default Vue.extend({
    name: "App",
    components: {
      //
    },
    data() {
      return {
        showDrawer: false,
      };
    },
    computed: {
      ...mapState([
        "snackbar",
        "user",
      ]),
      navItems() {
        const items = [{title: "主页", link: "/", icon: "dashboard"}];
        items.push({title: "翻译", link: "/translate", icon: "translate"});
        items.push({title: "关于", link: "/about", icon: "question_answer"});
        return items;
      }
    }
  });
</script>
