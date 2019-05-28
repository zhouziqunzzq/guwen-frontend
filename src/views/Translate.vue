<template>
  <v-container>
    <v-tabs
        color="indigo darken-4"
        dark
        slider-color="cyan"
        grow
    >
      <v-tab ripple>古文翻译</v-tab>
      <v-tab-item>
        <GuwenTranslator/>
      </v-tab-item>

    </v-tabs>
  </v-container>
</template>

<script lang="ts">
  import Vue from "vue";
  import GuwenTranslator from "@/components/GuwenTranslator.vue";

  export default Vue.extend({
    name: "translate",
    components: {
      GuwenTranslator,
    },
    data: () => ({
      //
    }),
    computed: {
      //
    },
    methods: {
      //
      showInfo(msg: string) {
        this.$store.dispatch("showInfo", msg);
      },
      showError(msg: string) {
        this.$store.dispatch("showError", msg);
      },
    },
  });
</script>

<style scoped lang="stylus">

</style>
