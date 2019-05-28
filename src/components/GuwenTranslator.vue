<template>
  <v-container>
    <v-layout row wrap>
      <v-flex xs12 lg4>
        <v-switch
            v-model="isNewToOld"
            :label="isNewToOldSwitch"
        ></v-switch>
      </v-flex>

      <v-flex xs12>
        <v-textarea
            v-model="source"
            :error-messages="sourceErrors"
            :label="sourcePlaceHolder"
            required
            @input="$v.source.$touch()"
            @blur="$v.source.$touch()"
        ></v-textarea>

      </v-flex>
      <v-flex xs12>
        <v-textarea
            v-model="translateResult"
            :label="translateResultPlaceHolder"
            readonly
        ></v-textarea>
      </v-flex>

      <v-flex xs12>
        <v-btn @click="translate" color="success">翻译</v-btn>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script lang="ts">
  import Vue from "vue";
  import {validationMixin} from "vuelidate";
  import {required, maxLength, minLength, sameAs} from "vuelidate/lib/validators";
  import config from "@/config.ts";
  import {get, post} from "@/helpers.ts";

  export default Vue.extend({
    name: "GuwenTranslator",
    mixins: [validationMixin],
    validations: {
      source: {required},
    },
    data: () => ({
      // Single
      source: "",
      translateResult: "",
      isNewToOld: false,
    }),
    computed: {
      isNewToOldSwitch() {
        if (this.isNewToOld) {
          return "现代文->文言文";
        } else {
          return "文言文->现代文";
        }
      },
      sourceErrors() {
        const errors: any[] = [];
        if (!this.$v.source!.$dirty) {
          return errors;
        }
        // !this.$v.keyword!.maxLength && errors.push("关键词长度不能超过 4");
        !this.$v.source!.required && errors.push("请输入待翻译内容");
        return errors;
      },
      sourcePlaceHolder() {
        if (this.isNewToOld) {
          return "请输入待翻译的现代文";
        } else {
          return "请输入待翻译的古文";
        }
      },
      translateResultPlaceHolder() {
        return "翻译结果";
      },
      translateType() {
        if (this.isNewToOld) {
          return "2";
        } else {
          return "1";
        }
      }
    },
    methods: {
      async translate() {
        this.$v.$touch();
        if (this.$v.source!.$invalid) {
          return;
        } else {
          this.translateResult = "";
          try {
            const response = await post<{
              result: boolean,
              code: number,
              msg: string,
              data: string,
            }>(config.baseUrl + "translate",
              {
                input: this.source,
                type: this.translateType,
              });
            console.log(response.parsedBody);
            if (response.parsedBody!.result) {
              this.showInfo(response.parsedBody!.msg);
              // console.log(response.parsedBody.data);
              this.translateResult = response.parsedBody.data;
            } else {
              this.showError(response.parsedBody!.msg);
            }
          } catch (response) {
            this.showError("出错啦！详细信息：" + response);
          }
        }
      },
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
  .fade-enter-active, .fade-leave-active
    transition: opacity .5s

  .fade-enter, .fade-leave-to
    opacity: 0
</style>
