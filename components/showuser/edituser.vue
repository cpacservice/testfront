<template>
  <div>
    <v-snackbar
      :color="coloralert"
      v-model="alertstatus"
      :timeout="timeout"
      top=""
    >
      {{ alertMessage }}
    </v-snackbar>
    <v-dialog v-model="show" max-width="500px" persistent>
      <v-card>
        <v-card-title>
          <span class="headline"></span>
        </v-card-title>

        <v-card-text>
          <v-container>
            อัพเดตสถานะสมาชิก
            <v-row>
              <v-col cols="12" sm="6" md="6">
                <v-select
                  v-model="form.userStatus"
                  :items="status"
                  label="สถานะสมาชิก"
                  persistent-hint
                  return-object
                  single-line
                  required
                ></v-select>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="updatestatus()"
            >บันทึก</v-btn
          >
          <v-btn
            color="blue darken-1"
            rounded
            :style="{ color: 'white' }"
            @click="() => $emit('close')"
            >ยกเลิก</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  props: {
    toggle: {
      type: Boolean,
      default: false
    },
    sendvalue: {
      type: Object,
      default: () => {}
    }
  },
  data() {
    return {
      coloralert: "",
      alertstatus: false,
      alertMessage: "",
      timeout: 2000,
      dialog: false,
      form: {
        email: "",
        userid: "",
        userStatus: ""
      },

      status: ["active", "inactive"]
    };
  },
  watch: {
    show() {
      this.form = this.sendvalue;
    }
  },
  computed: {
    show() {
      return this.toggle;
    }
  },

  methods: {
    async updatestatus() {
      //update สถานะ
      this.dialog = true;
      console.log("check", this.form.email);
      let res = await this.$http.post("/users/updatestatus", {
        email: this.form.email,
        userStatus: this.form.userStatus
      });
      if (!res.data.ok) {
        this.coloralert = "red";
        (this.alertstatus = true),
          (this.alertMessage = "กรุณากรอกข้อมูลให้ครบถ้วน");
      } else {
        this.coloralert = "green";
        (this.alertstatus = true),
          (this.alertMessage = "บันทึกข้อมูลเรียบร้อย");
        this.$emit("close");
      }
    },
    close() {
      this.dialog = false;
    }
  }
};
</script>

<style></style>
