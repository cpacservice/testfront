<template>
  <v-app class="bg">
        <loading :toggle="loadingme" />
    <v-alert border="top" :color="coloralert" dark :value="regisstatus">{{
      alertMessage
    }}</v-alert>
    <v-layout row>
      <v-flex xs12>
        <v-container class="fill-height" fluid>
          <v-row align="center" justify="center">
            <v-col cols="12" sm="8" md="4">
              <v-card class="elevation-12" height="500px">
                <v-toolbar
                  flat
                  color="cyan"
                  height="80px"
                  :style="{ color: 'white' }"
                  >* เข้าสู่ระบบ *</v-toolbar
                >

                <v-card-text>
                  <v-form class="form">
                    <v-text-field
                      label="อีเมล์"
                      type="text"
                      v-model="email"
                    ></v-text-field>

                    <v-text-field
                      id="password"
                      label="รหัสผ่าน"
                      type="password"
                      v-model="password"
                    ></v-text-field>
                    <div class="forgot">
                      <nuxt-link to="forgetpassword">ลืมรหัสผ่าน ?</nuxt-link>
                    </div>
                  </v-form>
                </v-card-text>

                <v-flex xs12 class="text-center">
                  <v-btn
                    color="cyan"
                    rounded
                    width="200px"
                    :style="{ color: 'white' }"
                    @click="LoginSubmit"
                    >เข้าสู่ระบบ</v-btn
                  >
                </v-flex>

                <v-divider class="mx-10"></v-divider>
                <v-card-text class="gotosignup">
                  ยังไม่มีบัญชีใช่มั้ย ?
                  <nuxt-link to="register">สมัครสมาชิก</nuxt-link>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-flex>
    </v-layout>
  </v-app>
</template>

<script>
import loading from "@/components/loading/loading";
export default {
    components: {

    loading
  },
  layout: "login_layout",
  data() {
    return {
       loadingme: false,
      regisstatus: false,
      alertMessage: "",
      coloralert: "",
      iconalert: "",
      email: "",
      password: ""
    };
  },
  methods: {
    async LoginSubmit() {
       this.loadingme = true;
      try {
        const response = await this.$auth.loginWith("local", {
          data: { email: this.email, password: this.password }
        });
        console.log(response);
        this.loadingme = false;
        this.regisstatus = true;
        (this.coloralert = "green lighten-2"),
          (this.iconalert = "mdi mdi-checkbox-marked-circle"),
          (this.alertMessage = "เข้าสู่ระบบสำเร็จ");
      } catch (err) {
           this.loadingme = false;
        this.regisstatus = true;
        (this.coloralert = "red lighten-2"),
          (this.iconalert = "mdi-alert-circle"),
          (this.alertMessage =
            "เข้าสู่ระบบไม่สำเร็จ กรุณาตรวจสอบความถูกต้องอีกครั้ง"),
          window.scrollTo(0, 0);
      }
    }
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Kanit:wght@200&display=swap");
.form {
  /* padding: 40px; */
  padding-left: 40px;
  padding-right: 40px;
  padding-bottom: 40px;
}
.action {
  padding-left: 125px;
}
.forgot {
  float: right;
}
.gotosignup {
  padding-left: 200px;
  /* padding-right: 15px; */
}
.logo {
  background-image: url("../../assets/image/cpac.png");
}
.h5 {
  text-align: center;
  margin: auto;
}
</style>
