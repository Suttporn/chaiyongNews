<template>
  <div style="background: #f5f5f5">
    <!-- --------------nav------------ -->
    <Nav />
    <!-- --------------nav------------ -->
    <br />
    <div style="margin-top: 60px">
      <h5>ประวัติการสั่งซื้อ</h5>
    </div>
    <div>
      <b-tabs content-class="mt-3" fill>
        <b-tab title="การสั่งซื้อ" active>
          <div>
            <b-table
              show-empty
              responsive
              fixed
              stacked="md"
              :items="items"
              :fields="fields"
              :filter="filter"
              :filterIncludedFields="filterOn"
              :head-variant="dark"
            >
              <template v-slot:cell(img)="row">
                <b-img
                  rounded="0"
                  style="width: 100px"
                  :src="row.item.IMG"
                ></b-img>
              </template>
              <template v-slot:cell(status)="row">
                <div v-if="row.item.ORDERSTATUS == '1'">รอดำเนินการ</div>
                <div v-if="row.item.ORDERSTATUS == '2'">กำลังจัดส่ง</div>
                <div v-if="row.item.ORDERSTATUS == '3'">
                  จัดส่งเเล้ว
                  <br />จัดส่งโดย : {{ row.item.KERRY }} <br />เลขพัสดุ :
                  {{ row.item.PERCEL_NEMBER }}
                  <br />
                  <b-button @click="UpdateOrder(row.item)" variant="warning"
                    >ได้รับสินค้าเรียบร้อยเเล้ว</b-button
                  >
                </div>
              </template>
            </b-table>
          </div>
        </b-tab>
        <b-tab title="สำเร็จ">
          <div>
            <b-table
              show-empty
              responsive
              fixed
              stacked="md"
              :items="itemss"
              :fields="fieldss"
              :filter="filter"
              :filterIncludedFields="filterOn"
              :head-variant="dark"
            >
              <template v-slot:cell(img)="row">
                <b-img
                  rounded="0"
                  style="width: 100px"
                  :src="row.item.IMG"
                ></b-img>
              </template>
              <template v-slot:cell(status)> สำเร็จ </template>
            </b-table>
          </div>
        </b-tab>
      </b-tabs>
    </div>
    <br /><br />
    <div class="footerr">
      <b-row>
        <b-col cols="4">
          <div style="padding-top: 10px">
            <b-icon
              @click="backindex()"
              icon="house-fill"
              variant="light"
              font-scale="1.5"
            ></b-icon>
          </div>
          <font color="#FFFFFF">สินค้า</font>
        </b-col>

        <b-col cols="4">
          <div style="padding-top: 10px">
            <b-icon
              @click="backlike()"
              icon="heart-fill"
              variant="light"
              font-scale="1.5"
            ></b-icon>
          </div>
          <font color="#FFFFFF">ถูกใจ</font>
        </b-col>

        <b-col cols="4" @click="goshopping()">
          <div style="padding-top: 10px">
            <b-icon
              icon="basket-fill"
              variant="light"
              font-scale="1.5"
            ></b-icon>
          </div>
          <font color="#FFFFFF">รถเข็น</font>
        </b-col>
      </b-row>
    </div>
  </div>
</template>

<script>
import Nav from "../../components/Nav";
import axios from "axios";
import firebase from "firebase";
const api_url = require("../../../utilities/api");
export default {
  components: {
    Nav,
  },
  data: () => ({
    items: null,
    fields: [
      { key: "img", label: "", class: "text-center" },
      { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
      { key: "TOTAL", label: "จำนวน", class: "text-center" },
      { key: "TOTAL_PRICE", label: "ราคารวม", class: "text-center" },
      { key: "status", label: "สถานะ", class: "text-center" },
    ],
    itemss: null,
    fieldss: [
      { key: "img", label: "", class: "text-center" },
      { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
      { key: "TOTAL", label: "จำนวน", class: "text-center" },
      { key: "TOTAL_PRICE", label: "ราคารวม", class: "text-center" },
      { key: "status", label: "สถานะ", class: "text-center" },
    ],
  }),
  async created() {
    // สินค้าที่สั่งซื้อ
    let order1 = await axios.post(`${api_url.api_url}/selectproductorder1`, {
      MEMBER_ID: localStorage.getItem("IDMEMBER"),
    });
    this.items = order1.data;
    console.log(order1.data);
    // สำเร็จ
    let order2 = await axios.post(`${api_url.api_url}/selectproductorder2`, {
      MEMBER_ID: localStorage.getItem("IDMEMBER"),
    });
    this.itemss = order2.data;
    console.log(order2.data);
  },
  methods: {
    backindex() {
      this.$router.push({ path: "/" });
    },
    backlike() {
      this.$router.push({ path: "/userlike" });
    },
    goshopping() {
      this.$router.push({ path: "/usershopping" });
    },
    async reset() {
      // สินค้าที่สั่งซื้อ
      let order1 = await axios.post(`${api_url.api_url}/selectproductorder1`, {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      });
      this.items = order1.data;
      console.log(order1.data);
      // สำเร็จ
      let order2 = await axios.post(`${api_url.api_url}/selectproductorder2`, {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      });
      this.itemss = order2.data;
      console.log(order2.data);
    },
    async UpdateOrder(x) {
      console.log(x);
      let update = await axios.post(`${api_url.api_url}/updateStatusOrder`, {
        ORDER_ID: x.ORDER_ID,
        STATUS: "4",
      });
      console.log(update.data);
      this.reset();
    },
  },
};
</script>

<style>
</style>