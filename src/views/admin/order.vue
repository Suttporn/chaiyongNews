<template>
  <div>
    <div style="background: #f5f5f5">
      <!-- --------------nav------------ -->
      <Navadmin />
      <!-- --------------nav------------ -->
      <br /><br /><br />
      <div style="padding: 10px">
        <div><b style="font-size: 16px">ออเดอร์สินค้า </b></div>
        <br />
      </div>
      <div>
        <b-tabs content-class="mt-3" fill>
          <b-tab :title="'(' + items1.length + ') ' + 'การสั่งซื้อ'" active>
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
                <template v-slot:cell(status)="row">
                  <b-button
                    style="margin: 5px"
                    @click="UpdateOrder1(row.item, 1)"
                    variant="warning"
                    >ยืนยันออเดอร์</b-button
                  >
                  <b-button
                    style="margin: 5px"
                    @click="UpdateOrder1(row.item, 2)"
                    variant="danger"
                    >ยกเลิก</b-button
                  >
                </template>
                <template v-slot:cell(receipt)="row">
                  <b-button
                    variant="warning"
                    @click="receipts(row.item, row.index, $event.target)"
                    class="mr-2"
                    >ดู</b-button
                  >
                </template>
              </b-table>
              <!-- Info modal 1 -->
              <b-modal
                :id="infoModal2.id"
                ref="modal-2"
                :title="infoModal2.title"
                hide-footer
              >
              <div v-if="this.items1.length > 0">
                <center>
                  <b-img
                    style="width: 100%"
                    :src="this.items1[this.md2].RECEIPT_IMG"
                  ></b-img>
                </center>
                 </div>
              </b-modal>
            </div>
          </b-tab>
          <b-tab :title="'(' + items2.length + ') ' + 'ที่ต้องจัดส่ง'">
            <div>
              <b-table
                show-empty
                responsive
                fixed
                stacked="md"
                :items="items2"
                :fields="fields2"
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
                  <b-button
                    variant="warning"
                    @click="detail(row.item, row.index, $event.target)"
                    class="mr-2"
                    >การจัดส่ง</b-button
                  >
                </template>
              </b-table>
              <!-- Info modal 1 -->
              <b-modal
                :id="infoModal1.id"
                ref="modal-1"
                :title="infoModal1.title"
                hide-footer
              >
                จัดส่งโดย :
                <b-form-select v-model="KERRY">
                  <b-form-select-option value="Kerry Express"
                    >Kerry Express</b-form-select-option
                  >
                  <b-form-select-option value="Flash Express"
                    >Flash Express</b-form-select-option
                  >
                </b-form-select>
                เลขพัสดุ :
                <b-form-input
                  v-model="PERCEL_NEMBER"
                  placeholder="เลขพัสดุ"
                ></b-form-input>
                <br />
                {{ this.items2[this.md1].ORDER_ID }}
                <div align="right">
                  <b-button @click="UpdateOrder2()" variant="warning"
                    >ยืนยันออเดอร์</b-button
                  >
                </div>
              </b-modal>
            </div>
          </b-tab>
          <b-tab :title="'(' + items3.length + ') ' + 'จัดส่งเเล้ว'">
            <div>
              <b-table
                show-empty
                responsive
                fixed
                stacked="md"
                :items="items3"
                :fields="fields3"
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
                <template v-slot:cell(status)> รอการยืนยันรับสินค้า </template>
              </b-table>
            </div>
          </b-tab>
        </b-tabs>
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
      PERCEL_NEMBER: "",
      KERRY: "",
      items1: null,
      fields1: [
        { key: "img", label: "", class: "text-center" },
        { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
        { key: "TOTAL", label: "จำนวน", class: "text-center" },
        { key: "MEMBER_NAME", label: "ชื่อลูกค้า", class: "text-center" },
        { key: "receipt", label: "การชำระเงิน", class: "text-center" },
        {
          key: "MEMBER_ADDRESS",
          label: "ข้อมูลการจัดส่ง",
          class: "text-center",
        },
        { key: "status", label: "ปุ่มเปลี่ยนสถานะ", class: "text-center" },
      ],
      items2: null,
      fields2: [
        { key: "img", label: "", class: "text-center" },
        { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
        { key: "TOTAL", label: "จำนวน", class: "text-center" },
        { key: "MEMBER_NAME", label: "ชื่อลูกค้า", class: "text-center" },
        {
          key: "MEMBER_ADDRESS",
          label: "ข้อมูลการจัดส่ง",
          class: "text-center",
        },
        { key: "status", label: "ปุ่มเปลี่ยนสถานะ", class: "text-center" },
      ],
      items3: null,
      fields3: [
        { key: "img", label: "", class: "text-center" },
        { key: "PRODUCT_NAME", label: "ชื่อสินค้า", class: "text-center" },
        { key: "TOTAL", label: "จำนวน", class: "text-center" },
        { key: "MEMBER_NAME", label: "ชื่อลูกค้า", class: "text-center" },
        {
          key: "MEMBER_ADDRESS",
          label: "ข้อมูลการจัดส่ง",
          class: "text-center",
        },
        { key: "status", label: "ปุ่มเปลี่ยนสถานะ", class: "text-center" },
      ],
      md1: 0,
      infoModal1: {
        id: "info-modal1",
        title: "",
        content: "",
      },
      md2: 0,
      infoModal2: {
        id: "info-modal2",
        title: "",
        content: "",
      },
    };
  },
  async created() {
    // สินค้าที่สั่งซื้อ
    let order1 = await axios.post(
      `${api_url.api_url}/selectproductorderadmin1`,
      {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      }
    );
    this.items1 = order1.data;
    console.log(order1.data);
    // ที่ต้องจัดส่ง
    let order2 = await axios.post(
      `${api_url.api_url}/selectproductorderadmin2`,
      {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      }
    );
    this.items2 = order2.data;
    console.log(order2.data);
    // จัดส่งเเล้ว
    let order3 = await axios.post(
      `${api_url.api_url}/selectproductorderadmin3`,
      {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      }
    );
    this.items3 = order3.data;
    console.log(order3.data);
  },
  methods: {
    async reset() {
      // สินค้าที่สั่งซื้อ
      let order1 = await axios.post(
        `${api_url.api_url}/selectproductorderadmin1`,
        {
          MEMBER_ID: localStorage.getItem("IDMEMBER"),
        }
      );
      this.items1 = order1.data;
      console.log(order1.data);
      // ที่ต้องจัดส่ง
      let order2 = await axios.post(
        `${api_url.api_url}/selectproductorderadmin2`,
        {
          MEMBER_ID: localStorage.getItem("IDMEMBER"),
        }
      );
      this.items2 = order2.data;
      console.log(order2.data);
      // จัดส่งเเล้ว
      let order3 = await axios.post(
        `${api_url.api_url}/selectproductorderadmin3`,
        {
          MEMBER_ID: localStorage.getItem("IDMEMBER"),
        }
      );
      this.items3 = order3.data;
      console.log(order3.data);
    },
    async UpdateOrder1(a, b) {
      console.log(a);
      console.log(b);
      if (b == 1) {
        //ยืนยัน
        let update = await axios.post(`${api_url.api_url}/updateStatusOrder`, {
          ORDER_ID: a.ORDER_ID,
          STATUS: "2",
        });
        console.log(update.data);
        this.reset();
      } else {
        //ยกเลิก
        let update = await axios.post(`${api_url.api_url}/updateStatusOrder`, {
          ORDER_ID: a.ORDER_ID,
          STATUS: "5",
        });
        console.log(update.data);
        this.reset();
      }
    },
    async UpdateOrder2() {
      console.log(this.items2[this.md1].ORDER_ID);
      let update = await axios.post(`${api_url.api_url}/updateStatusOrder3`, {
        ORDER_ID: this.items2[this.md1].ORDER_ID,
        STATUS: "3",
        PERCEL_NEMBER: this.PERCEL_NEMBER,
        KERRY: this.KERRY,
      });
      console.log(update.data);
      this.$refs["modal-1"].hide();
      this.reset();
    },
    detail(item, index, button) {
      this.md1 = index;
      console.log(index);
      this.infoModal1.title = item.name;
      this.infoModal1.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal1.id, button);
    },
    receipts(item, index, button) {
      this.md2 = index;
      console.log(index);
      this.infoModal2.title = item.name;
      this.infoModal2.content = JSON.stringify(item, null, 2);
      this.$root.$emit("bv::show::modal", this.infoModal2.id, button);
    },
  },
};
</script>