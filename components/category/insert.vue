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
    <v-btn @click="dialog = true" color="primary" dark class="mb-2"
      >+ เพิ่มประเภทสินค้า</v-btn
    >
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline"></span>
        </v-card-title>

        <v-card-text>
          <v-container>
            เพิ่มประเภทสินค้าสินค้า
            <v-row>
              <v-col cols="12" sm="4" md="6">
                <v-text-field
                  v-model="categoryname"
                  required
                  label="ชื่อประเภทสินค้า"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="save">บันทึก</v-btn>
          <v-btn
            color="blue darken-1"
            rounded
            :style="{ color: 'white' }"
            @click="close"
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
    label: {
      type: String,
      default: "Label"
    }
  },
  data() {
    return {
      coloralert: "",
      alertstatus: false,
      alertMessage: "",
      timeout: 2000,

      dialog: false,
      categories: [],
      categoryname: ""
    };
  },
  async created() {
    //ปกป้องเสริมส่วนนี้มาให้
    let res = await this.$http.get("/categories");
    console.log("test cate", res.data);
  },
  methods: {
    async save() {
      this.dialog = true;
      let res = await this.$http.post("/categories/insert", {
        categoryid: this.$route.query.categoryid,
        categoryname: this.categoryname
      });
      if (!res.data.ok) {
        this.coloralert = "red";
        (this.alertstatus = true),
          (this.alertMessage = "กรุณากรอกข้อมูลให้ครบถ้วน");
      } else {
        this.coloralert = "green";
        (this.alertstatus = true), (this.alertMessage = "เพิ่มข้อมูลถูกต้อง");
        this.close();
      }
    },
    close() {
      this.dialog = false;
    }
  }
};
</script>

<style></style>
