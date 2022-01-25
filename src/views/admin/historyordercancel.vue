<template>
  <div>
    <div style="background: #f5f5f5">
      <!-- --------------nav------------ -->
      <Navadmin />
      <!-- --------------nav------------ -->
      <br /><br /><br />
      <div style="padding: 10px">
        <div><b style="font-size: 16px">ประวัติการสั่งซื้อสินค้าที่ยกเลิก</b></div>
        <br />
      </div>
      <div>
        <div>
          <b-table
            show-empty
            responsive
            fixed
            stacked="md"
            :items="items1"
            :fields="fields1"
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
            <template v-slot:cell(status)>
              ยกเลิก
            </template>
          </b-table>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Navadmin from "../../components/Navadmin";
import axios from "axios";
import firebase from "firebase";
const api_url = require("../../../utilities/api");
export default {
  components: {
    Navadmin,
  },
  data() {
    return {
      items1: null,
      fields1: [
        { key: "img", label: "", class: "text-center" },
        { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
        { key: "TOTAL", label: "จำนวน", class: "text-center" },
        { key: "MEMBER_NAME", label: "ชื่อลูกค้า", class: "text-center" },
        {
          key: "MEMBER_ADDRESS",
          label: "ข้อมูลการจัดส่ง",
          class: "text-center",
        },
        { key: "status", label: "สถานะ", class: "text-center" },
      ],
    };
  },
  async created() {
    // สินค้าที่สั่งซื้อยกเลิก
    let order1 = await axios.post(
      `${api_url.api_url}/selectproductorderadmin5`,
      {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      }
    );
    this.items1 = order1.data;
    console.log(order1.data);
  },
  methods: {},
};
</script>