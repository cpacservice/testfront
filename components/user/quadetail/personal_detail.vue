<template>
  <div>
    <v-container class="content">
      <v-btn class="back" @click="refreshpage()">ย้อนกลับ</v-btn>
      <v-card full-height="900px">
        <div class="text-right" :style="{ color: 'blue' }">
          <v-btn @click="savetopdf()">
            <v-icon></v-icon>คลิกที่นี่
          </v-btn>
        </div>

        <div class="text-center">
          <h4 :style="{ paddingTop: '20px' }">
            <b>บริษัท ผลิตภัณฑ์และวัตถุก่อสร้าง จำกัด</b>
          </h4>

          <div>
            1516 ถ.ประชาราษฎร์ 1 แขวงวงศ์สว่าง เขตบางซื่อ กรุงเทพฯ 10800
            <br />โทร.02-555-5000 CPAC CALL CENTER 02-555-5555 Email:
            cpacinside@scg.com
          </div>
        </div>
        <!-- <div class="time">
        วันที่สั่งซื้อ : {{orderdate}}
        <br />
        เวลาที่สั่งซื้อ : {{ordertime}} นาที
        </div>-->

        <div class="text-center">
          <h5 :style="{ paddingTop: '20px' }">
            <b>ใบขอเสนอราคา</b>
          </h5>
        </div>
        <v-layout row>
          <v-flex xs8 :style="{ paddingLeft: '30px' }">
            <v-container>
              <b>ชื่อผู้ขอ :</b>
              {{ this.sendvalue.qPersonalName }}{{ this.sendvalue.qPersonalLast }}
              <br />
              <b>ที่อยู่ในการจัดส่ง :</b>
              {{ this.sendvalue.qPersonalAddressDelivery }}
              <br />
              <b>เบอร์โทร :</b>
              {{ this.sendvalue.qPersonalPhone }}
              <br />
              <b>อีเมล์ :</b>
              {{ this.sendvalue.qPersonalEmail }}
            </v-container>
          </v-flex>
          <v-flex xs4 :style="{ paddingLeft: '80px' }">
            <v-container>
              <b>หมายเลข :</b>
              {{ this.sendvalue.qPersonalId }}
              <br />
              <b>หมายเลขประจำตัวประชาชน :</b>
              {{ this.sendvalue.qPersonalIdcard }}
              <br />
              <b>วันที่สั่งซื้อ :</b>
              {{this.sendvalue.qPersonalDate}}
              <br />
              <b>เวลา :</b>
              {{ this.sendvalue.qPersonalTime }}
              <br />
              <b>หมายเลขผู้ใช้งาน :</b>
              {{ this.sendvalue.qPersonalUserid }}
            </v-container>
          </v-flex>
        </v-layout>

        <v-flex
          xs12
          :style="{
          paddingLeft: '70px',
          paddingRight: '70px',
          paddingTop: '20px',
          color: 'black'
        }"
          class="text-center"
        >
          <table :style="{ width: '100%', height: '50%'}">
            <tr>
              <th :style="{background:'cyan' }">สินค้า</th>
              <!-- <th>รูปสินค้า</th> -->
              <th :style="{background:'cyan' }">จำนวน</th>
            </tr>
            <tr>
              <td>{{ this.sendvalue.qPersonalProductname }}</td>
              <!-- <td>
                <img :src="item.productimage" />
              </td>-->
              <td>{{ this.sendvalue.qPersonalProductid }}</td>
            </tr>
          </table>
        </v-flex>
        <v-layout row>
          <v-flex xs6 :style="{ paddingLeft: '70px', paddingTop: '20px' }">
            <b>ท่านสามารถดูใบเสนอราคาของท่านได้ที่เมนู "ใบขอเสนอราคา"</b>
            <br />
          </v-flex>
          <v-flex></v-flex>
        </v-layout>
      </v-card>
    </v-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {},
      cartdetail: [{ to: "/admin/insertp" }],
    };
  },
  props: {
    sendvalue: {
      type: Object,
      default: () => {},
    },
  },
  watch: {
    show() {
      this.form = this.sendvalue;
    },
  },
  methods: {
    refreshpage() {
      window.location.href =
        window.location.pathname +
        window.location.search +
        window.location.hash;
    },
    savetopdf() {
      this.$router.push({
        name: "quatation-detail-quap",
        params: { quaid: this.sendvalue.qPersonalId },
      });
    },
  },
};
</script>

<style>
@media print {
  .text-right {
    visibility: hidden;
  }
  .back {
    visibility: hidden;
  }
  #section-to-print,
  #section-to-print * {
    visibility: visible;
  }
  #section-to-print {
    position: absolute;
    left: 0;
    top: 0;
  }
}
.time {
  float: right;
  padding: 15px;
}
.head {
  padding-left: 40%;
}
</style>