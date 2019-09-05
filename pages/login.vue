<template>
  <v-card class="elevation-12">
    <v-toolbar dark color="primary">
      <v-toolbar-title>ログイン</v-toolbar-title>
    </v-toolbar>
    <v-card-text>
      <v-form v-model="valid">
        <v-select
          label="お名前"
          item-text="name"
          item-value="id"
          :items="accounts('ALL')"
          :value="id" 
          @input="$store.dispatch('login/id', $event)"
        />
        <v-text-field 
        v-model="user"
        label="ユーザーID"
        :rules="[v => !!v  || '何か入力してください']"
        required/>
        <v-text-field
        v-model="password"
        label="パスワード"
        :rules="[v => !!v  || '何か入力してください']"
        type="password"
        required/>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-spacer />
      <v-btn nuxt to="/signin" color="success">
        口座開設
      </v-btn>
      <v-btn :disabled="!valid" nuxt @click="login" color="primary">
        ログイン
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import { mapGetters } from "vuex";
import md5 from "blueimp-md5";


export default {
  layout: "login",
  data: () => ({
    valid: false,
    user: null,
    password: null,
  }),
  computed: {
    ...mapGetters("accounts", ["accounts"]),
    ...mapGetters("login", ["id"]),
  },
  methods: {
    login(){
      const account = this.accounts("ALL").find(a => a.user === this.user);
      if(!account){
        alert("ユーザーIDが違います")
      }else if(md5(this.password) !== account.password){
        alert("パスワードが違います")
      }else{
        this.$router.push("/")
      }
    }
  }
};
</script>
