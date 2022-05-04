<template>
  <div style="background: #f5f5f5">
    <!-- --------------nav------------ -->
    <Navadmin />
    <!-- --------------nav------------ -->
    <br /><br /><br />
    <div align="left" style="padding: 10px">
      <b-row>
        <b-col cols="12" lg="1"> เลือกปี : </b-col>
        <b-col cols="12" lg="2">
          <b-form-select
            @change="select()"
            v-model="YEAR"
            :options="year"
            class="mb-1"
          >
          </b-form-select>
        </b-col>
        <b-col cols="12" lg="1"> เลือกเดือน : </b-col>
        <b-col cols="12" lg="2">
          <b-form-select
            @change="select()"
            v-model="MONTH"
            :options="month"
            class="mb-1"
          >
          </b-form-select>
        </b-col>
      </b-row>
      <div>
        <b-row>
          <b-col cols="12" lg="3">
            <b-card class="text-center" style="margin: 10px">
              <div>
                <b
                  >สินค้าที่ขาย
                  <b style="color: rgb(22 118 5)">{{ dash1 }}</b> ชิ้น</b
                >
              </div>
            </b-card>
            <b-card class="text-center" style="margin: 10px">
              <div>
                <b
                  >สินค้าที่ถูกยกเลิก
                  <b style="color: red">{{ dash3 }}</b> ชิ้น</b
                >
              </div>
            </b-card>
          </b-col>
          <b-col cols="12" lg="3">
            <b-card class="text-center" style="margin: 10px">
              <div>
                <b
                  >จำนวนเงินที่ได้ทั้งหมด
                  <b style="color: rgb(22 118 5)">{{ dash2 }}</b> บาท</b
                >
              </div>
            </b-card>
          </b-col>
          <b-col cols="12" lg="6">
            <b-card class="text-center" style="margin: 10px">
              <div align="left">
                <b>ลำดับสินค้าขายดี</b>
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
                ></b-table>
              </div>
            </b-card>
          </b-col>
        </b-row>
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
      YEAR: "",
      year: [
        { value: "", text: "ทั้งหมด" },
        { value: "2022", text: "ปี 2565" },
        { value: "2023", text: "ปี 2566" },
        { value: "2024", text: "ปี 2567" },
        { value: "2025", text: "ปี 2568" },
        { value: "2026", text: "ปี 2569" },
        { value: "2027", text: "ปี 2570" },
        { value: "2028", text: "ปี 2571" },
        { value: "2029", text: "ปี 2572" },
      ],
      MONTH: "",
      month: [
        { value: "", text: "ทั้งหมด" },
        { value: "1", text: "มกราคม" },
        { value: "2", text: "กุมภาพันธ์" },
        { value: "3", text: "มีนาคม" },
        { value: "4", text: "เมษายน" },
        { value: "5", text: "พฤษภาคม" },
        { value: "6", text: "มิถุนายน" },
        { value: "7", text: "กรกฎาคม" },
        { value: "8", text: "สิงหาคม" },
        { value: "9", text: "กันยายน" },
        { value: "10", text: "ตุลาคม" },
        { value: "11", text: "พฤศจิกายน" },
        { value: "12", text: "ธันวาคม" },
      ],
      dash1: null,
      dash2: null,
      dash3: null,
      items: null,
      fields: [
        { key: "PRODUCT_ID", label: "รหัสสินค้า", class: "text-center" },
        { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
        { key: "TOTAL", label: "จำนวนสินค้า", class: "text-center" },
      ],
    };
  },
  async mounted() {
    await axios
      .post(`${api_url.api_url}/Dashboard1`, {
        YEAR: this.YEAR,
        MONTH: this.MONTH,
      })
      .then((response) => {
        console.log(response.data[0].TOTAL);
        this.dash1 = response.data[0].TOTAL;
      });
    await axios
      .post(`${api_url.api_url}/Dashboard2`, {
        YEAR: this.YEAR,
        MONTH: this.MONTH,
      })
      .then((response) => {
        console.log(response.data);
        this.dash2 = response.data[0].TOTAL_PRICE;
      });
    await axios
      .post(`${api_url.api_url}/Dashboard3`, {
        YEAR: this.YEAR,
        MONTH: this.MONTH,
      })
      .then((response) => {
        console.log(response.data);
        this.dash3 = response.data[0].TOTALCANCEL;
      });
    await axios
      .post(`${api_url.api_url}/Dashboard4`, {
        YEAR: this.YEAR,
        MONTH: this.MONTH,
      })
      .then((response) => {
        console.log(response.data);
        this.items = response.data;
      });
  },
  methods: {
    async select() {
      await axios
        .post(`${api_url.api_url}/Dashboard1`, {
          YEAR: this.YEAR,
          MONTH: this.MONTH,
        })
        .then((response) => {
          console.log(response.data);
          if (response.data[0].TOTAL != null) {
              this.dash1 = response.data[0].TOTAL;
          }else{
              this.dash1 = 0;
          }
          
        });
      await axios
        .post(`${api_url.api_url}/Dashboard2`, {
          YEAR: this.YEAR,
          MONTH: this.MONTH,
        })
        .then((response) => {
          console.log(response.data);
          if (response.data[0].TOTAL_PRICE != null) {
              this.dash2 = response.data[0].TOTAL_PRICE;
          }else{
              this.dash2 = 0;
          }
         
        });
      await axios
        .post(`${api_url.api_url}/Dashboard3`, {
          YEAR: this.YEAR,
          MONTH: this.MONTH,
        })
        .then((response) => {
          console.log(response.data);
           if (response.data[0].TOTALCANCEL != null) {
              this.dash3 = response.data[0].TOTALCANCEL;
          }else{
              this.dash3 = 0;
          }
          
        });
      await axios
        .post(`${api_url.api_url}/Dashboard4`, {
          YEAR: this.YEAR,
          MONTH: this.MONTH,
        })
        .then((response) => {
          console.log(response.data);
          this.items = response.data;
        });
    },
  },
};
</script>

<style>
</style>