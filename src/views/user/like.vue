<template>
  <div style="background: #f9fafb">
    <!-- --------------nav------------ -->
    <Nav />
    <!-- --------------nav------------ -->
    <br />
    <div style="margin-top: 60px">
      <h5>สินค้าที่ถูกใจ</h5>
    </div>
    <div align="center">
      <b-row style="margin-right: 0px;margin-left: 0px;">
        <b-col
          v-for="item in product1"
          :key="item.PRODUCT_ID"
          cols="6"
          lg="2"
          style="padding: 5px"
        >
          <b-card-group
            deck
            img-top
            tag="article"
            style="max-width: 20rem"
            class="mb-2"
            border-variant="light"
          >
            <b-card style="padding: 0px" class="card55">
              <b-card-img @click="godetail(item)" :src="item.IMG"> </b-card-img>
              <b-card-text class="text1" style="padding: 20px">
                <div align="left">
                  <div style="line-height: 0.7">
                    <b style="font-size: 12px">{{ item.PRODUCT_NAME }}</b>
                  </div>
                </div>
                <div style="margin-top: 15px">
                  <b-row>
                    <b-col cols="6">
                      <div align="left">
                        <div style="line-height: 1.8">
                          <h6 style="font-size: 18px">฿{{ item.PRICE }}</h6>
                        </div>
                      </div>
                    </b-col>
                    <b-col cols="6">
                      <div v-if="item.checklike > 0" align="right">
                        <b-icon
                          icon="heart-fill"
                          style="width: 30px; height: 30px"
                          @click="Like(item)"
                          variant="danger"
                        ></b-icon>
                      </div>
                      <div v-else align="right">
                        <b-icon
                          icon="heart"
                          style="width: 30px; height: 30px"
                          @click="Like(item)"
                          variant="danger"
                        ></b-icon>
                      </div>
                    </b-col>
                  </b-row>
                </div>
              </b-card-text>
            </b-card>
          </b-card-group>
        </b-col>
      </b-row>
    </div>
    <br><br>
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
    product1: null,
  }),
  async created() {
    // สินค้าถูกใจ
    let p1 = await axios.post(`${api_url.api_url}/selectproductmemberlike`, {
      MEMBER_ID: localStorage.getItem("IDMEMBER"),
    });
    this.product1 = p1.data;
    // checklike
    var order1 = [];
    for (let index = 0; index < p1.data.length; index++) {
      const element = p1.data[index];
      console.log(element.PRODUCT_ID);
      order1.push(element);
      let check = await axios.post(`${api_url.api_url}/selectproductlike`, {
        PRODUCT_ID: element.PRODUCT_ID,
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      });
      console.log(check.data.length);
      order1[index].checklike = check.data.length;
    }
    console.log(order1);
    this.product1 = order1;
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
      // สินค้าถูกใจ
      let p1 = await axios.post(`${api_url.api_url}/selectproductmemberlike`, {
        MEMBER_ID: localStorage.getItem("IDMEMBER"),
      });
      this.product1 = p1.data;
      // checklike
      var order1 = [];
      for (let index = 0; index < p1.data.length; index++) {
        const element = p1.data[index];
        console.log(element.PRODUCT_ID);
        order1.push(element);
        let check = await axios.post(`${api_url.api_url}/selectproductlike`, {
          PRODUCT_ID: element.PRODUCT_ID,
          MEMBER_ID: localStorage.getItem("IDMEMBER"),
        });
        console.log(check.data.length);
        order1[index].checklike = check.data.length;
      }
      console.log(order1);
      this.product1 = order1;
    },
    Like(x) {
      console.log(x);
      if (x.checklike > 0) {
        axios
          .post(`${api_url.api_url}/updateLike`, {
            PRODUCT_ID: x.PRODUCT_ID,
          })
          .then((response) => {
            console.log(response.data);
            this.reset(response);
          });
      } else {
        axios
          .post(`${api_url.api_url}/insertLike`, {
            PRODUCT_ID: x.PRODUCT_ID,
            MEMBER_ID: localStorage.getItem("IDMEMBER"),
          })
          .then((response) => {
            console.log(response.data);
            this.reset(response);
          });
      }
    },
  },
};
</script>
<style>
/* CSS */
.button-61 {
  align-items: center;
  appearance: none;
  border-radius: 0.375rem;
  border-style: none;
  box-shadow: rgba(0, 0, 0, 0.2) 0 3px 1px -2px, rgba(0, 0, 0, 0.14) 0 2px 2px 0,
    rgba(0, 0, 0, 0.12) 0 1px 5px 0;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;

  padding: 8px 24px;
  font-size: 0.875rem;

  height: 30px;
  justify-content: center;
  letter-spacing: 0.0892857em;
  line-height: normal;
  min-width: 30px;
  outline: none;
  overflow: visible;
  padding: 0 16px;
  position: relative;
  text-align: center;
  text-decoration: none;
  text-transform: uppercase;
  transition: box-shadow 280ms cubic-bezier(0.4, 0, 0.2, 1);
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
  vertical-align: middle;
  will-change: transform, opacity;
}

.button-61:hover {
  box-shadow: rgba(0, 0, 0, 0.2) 0 2px 4px -1px, rgba(0, 0, 0, 0.14) 0 4px 5px 0,
    rgba(0, 0, 0, 0.12) 0 1px 10px 0;
}

.button-61:disabled {
  background-color: rgba(0, 0, 0, 0.12);
  box-shadow: rgba(0, 0, 0, 0.2) 0 0 0 0, rgba(0, 0, 0, 0.14) 0 0 0 0,
    rgba(0, 0, 0, 0.12) 0 0 0 0;
  color: rgba(0, 0, 0, 0.37);
  cursor: default;
  pointer-events: none;
}

.button-61:not(:disabled) {
  background-color: #2dce89;
}

.button-61:focus {
  box-shadow: rgba(0, 0, 0, 0.2) 0 2px 4px -1px, rgba(0, 0, 0, 0.14) 0 4px 5px 0,
    rgba(0, 0, 0, 0.12) 0 1px 10px 0;
}

.button-61:active {
  box-shadow: rgba(0, 0, 0, 0.2) 0 5px 5px -3px,
    rgba(0, 0, 0, 0.14) 0 8px 10px 1px, rgba(0, 0, 0, 0.12) 0 3px 14px 2px;
  background: #29b97b;
}

/* CSS */
.button-81 {
  background-color: #ffffff;
  border: 0 solid #e2e8f0;
  border-radius: 0.375rem;
  box-sizing: border-box;
  color: #2dce89;
  cursor: pointer;
  display: inline-block;
  width: 80px;

  font-size: 0.875rem;

  line-height: 0.25;
  padding: 0.9rem 0.1rem;
  text-align: center;
  text-decoration: none #0d172a solid;

  transition: all 0.1s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.25);
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

.button-81:hover {
  background-color: #f5365c;
  color: #ffffff;
}
.container {
  text-align: center;
  font-size: 20pt;
  font-family: "arial";
  margin-top: 200px;
}

.nav {
  /* position: relative; */
  /* position: fixed !important; */
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  padding: 0.2rem 0rem;
  width: 100%;

  z-index: 9999;
}
.nav-link {
  display: block;
  padding: 0.5rem 0.5rem !important;
}
p {
  margin-top: 0rem !important;
  margin-bottom: 0rem !important;
}
</style>