<template>
  <div>
    <loading :toggle="loadingme" />
    <normaledit :toggle="a" @close="a = false" :sendvalue="send" />
    <normaldelete
      :deletetoggle="b"
      @closed="b = false"
      :delete_id="id_delete"
    />
    <div v-if="open">
      <normaldetail :sendvalue="send" />
    </div>
    <div v-else>
      <v-card class="color">
        <v-card-title>
          ขอเสนอราคาทั่วไป
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="ค้นหา"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-spacer></v-spacer>
        <div row>
          <v-flex xs12>
            <v-data-table
              class="text-center"
              :headers="headers"
              :search="search"
              :items="show"
            >
              <template v-slot:item.edit="{ item }">
                <v-btn @click="ToggleDetail(item)" class="center">
                  <v-icon>mdi-information</v-icon>
                </v-btn>
                <v-btn @click="modalToggle(item)" class="center">
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
                <v-btn @click="ToggleDelete(item)" class="center">
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
              </template>
              <template v-slot:item.qNormalStatus="{ item }">
                <div v-if="item.qNormalStatus === 'กำลังดำเนินการ'">
                  <v-btn color="yellow">{{ item.qNormalStatus }}</v-btn>
                </div>
                <div v-else-if="item.qNormalStatus === 'สำเร็จ'">
                  <v-btn color="green">{{ item.qNormalStatus }}</v-btn>
                </div>
                <div v-else-if="item.qNormalStatus === 'รอการติดต่อกลับ'">
                  <v-btn color="orange">{{ item.qNormalStatus }}</v-btn>
                </div>
                <div v-else-if="item.qNormalStatus === 'ข้อมูลไม่ครบถ้วน'">
                  <v-btn color="red">{{ item.qNormalStatus }}</v-btn>
                </div> </template
              >-->
              <!-- <template v-slot:item.orderid="{ item }">
              <nuxt-link
                :to="{  name: 'admin-showorder-detail',
        params: {orderq:item.orderid}}"
              >
                <v-list-item-title>{{item.orderid}}</v-list-item-title>
              </nuxt-link>
            </template>
            <template v-slot:item.edit="{ item }">
              <v-btn @click="modalToggle(item)" class="center">
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-btn @click="ToggleDelete(item)" class="center">
                <v-icon>mdi-delete</v-icon>
              </v-btn>
            </template>
            <template v-slot:item.orderStatus="{ item }">
              <div v-if="item.orderStatus ==='รอการชำระเงิน' ">
                <v-btn color="yellow">{{item.orderStatus}}</v-btn>
              </div>
              <div v-else-if="item.orderStatus ==='กำลังจัดส่ง' ">
                <v-btn color="orange">{{item.orderStatus}}</v-btn>
              </div>
              <div v-else-if="item.orderStatus ==='สำเร็จ' ">
                <v-btn color="green">{{item.orderStatus}}</v-btn>
              </div>
              <div v-else-if="item.orderStatus ==='ยกเลิก' ">
                <v-btn color="red">{{item.orderStatus}}</v-btn>
              </div>
              </template>-->
            </v-data-table>
          </v-flex>
        </div>
      </v-card>
    </div>
  </div>
</template>

<script>
import normaldetail from "../../components/showqua/normal_detail";
import normaledit from "../../components/quaedit/normal";
import normaldelete from "../../components/quadelete/normal";
import loading from "@/components/loading/loading";
export default {
  components: {
    normaldetail,
    normaledit,
    normaldelete,
    loading
  },
  data() {
    return {
      loadingme: false,
      open: false,
      a: false,
      b: false,
      id_delete: 0,
      show: [],
      search: "",
      send: {},
      headers: [
        {
          text: "หมายเลข",
          align: "start",
          sortable: true,
          value: "qNormalId"
        },
        { text: "ชื่อผู้สั่ง", value: "qNormalName" },
        { text: "นามสกุล", value: "qNormalLast" },
        { text: "วันที่ขอใบเสนอราคา", value: "qNormalDate" },
        { text: "สถานะ", value: "qNormalStatus" },
        { text: "แก้ไข", value: "edit" },
        { text: "ตอบกลับ", value: "reply" }
      ]
    };
  },
  async created() {
    //ปกป้องเสริมส่วนนี้มาให้
    this.loadingme = true;
    let res = await this.$http.get("/q_normal");

    this.show = res.data.Normal;
    if (res.data.ok) {
      this.loadingme = false;
    }
  },
  methods: {
    modalToggle(item) {
      this.send = item;
      this.a = true;
    },
    ToggleDelete(pid) {
      this.id_delete = pid.qNormalId;
      this.b = true;
    },
    ToggleDetail(item) {
      this.$router.push({
        name: "quatation-detail-quan",
        params: { quaid: item.qNormalId }
      });
    }
  }
};
</script>

<style>
table,
th,
td {
  border: 1px solid black;
  border-collapse: collapse;
}
.center {
  text-align: center;
}
</style>
