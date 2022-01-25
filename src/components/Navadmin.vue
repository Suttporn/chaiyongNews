<template>
  <div>
    <b-navbar
      toggleable="lg"
      type="dark"
      style="z-index: 1; background-color: rgb(52, 86, 139)"
    >
      <b-navbar-brand href="#">ชัยยงค์การช่าง</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item @click="costomer()">ข้อมูลลูกค้า</b-nav-item>
          <b-nav-item @click="product()">ข้อมูลสินค้า</b-nav-item>
          <b-nav-item @click="order()">ออเดอร์สินค้า</b-nav-item>
          <b-nav-item-dropdown text="ประวัติ" right>
            <b-dropdown-item  @click="historyorder()">ประวัติการสั่งซื้อ</b-dropdown-item>
            <b-dropdown-item @click="historyordercancel()">ประวัติการยกเลิกออเดอร์</b-dropdown-item>
          </b-nav-item-dropdown>
          <b-nav-item href="#">dashboard</b-nav-item>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content>
              <em>{{ NAMEUSER }}</em>
            </template>
            <!-- <b-dropdown-item href="#">โปรไฟล์</b-dropdown-item> -->
            <b-dropdown-item @click="logouts">ออกจากระบบ</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>
<script>
import axios from "axios";
const api_url = require("../../utilities/api");
export default {
  name: "Nav",
  data: () => ({
    NAMEUSER: "",
    checklogin: "",
    img: "",
  }),
  mounted() {
    this.NAMEUSER = localStorage.getItem("USER");
    this.checklogin = localStorage.getItem("login");
  },
  methods: {
    costomer() {
      this.$router.push({ path: "/admincustomer" });
    },
    product() {
      this.$router.push({ path: "/adminproduct" });
    },
    order() {
      this.$router.push({ path: "/adminorder" });
    },
    historyorder(){
        this.$router.push({ path: "/adminhistoryorder" });
    },
    historyordercancel(){
        this.$router.push({ path: "/adminhistoryordercancel" });
    },
    logouts() {
      console.log("logout");
      this.$router.push({ path: "/" });
      localStorage.clear();
    },
  },
};
</script>

<style>
</style>