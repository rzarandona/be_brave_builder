<template>
  <div>
    <NavigationBar />
    <main class="container">
      <div class="step">
        <p>Thanks for waiting! Here's your preview.</p>
        <div
          :style="{ 'background-image': 'url(' + preview_url + ')' }"
          class="preview"
        ></div>
      </div>
      <!-- <a target="_blank" :href="inner_pdf_url">
        <button class="submit">DEV : VIEW PDF</button></a
      >
      <br /><br />
      <button @click="submit" class="submit">SUBMIT TO PUREPRINT</button> -->
      <button @click="proceedToCheckout" class="submit">
        PROCEED TO CHECKOUT
      </button>
    </main>
  </div>
</template>

<script>
import NavigationBar from "../components/NavigationBar.vue";
import crypto from "crypto-js";
import axios from "axios";
import qs from "qs";

export default {
  computed: {
    preview_url() {
      return this.$store.state.preview_url;
    },
    inner_pdf_url() {
      return this.$store.state.inner_pdf_url;
    },
    outer_pdf_url() {
      return this.$store.state.outer_pdf_url;
    },
    source_id() {
      return this.$store.state.source_id;
    },
    purchaser_email() {
      return this.$store.state.purchaser_email;
    },
    cover_type() {
      return this.$store.state.cover_type;
    },
    sku() {
      if (this.cover_type == "hardback") return "hectorspost_hardback_210x210";
      return "hectorspost_softback_210x210";
    },
  },
  methods: {
    submit() {
      let data = {
        destination: {
          name: "pureprint",
        },
        orderData: {
          sourceOrderId: this.source_id,
          customerName: "hectorspost",
          items: [
            {
              barcode: this.source_id,
              shipmentIndex: 0,
              sourceItemId: this.source_id,
              sku: "hectorspost_staging",
              quantity: 1,
              unitCost: 0.0,
              components: [
                {
                  path: this.inner_pdf_url,
                  fetch: true,
                  localFile: false,
                  code: "text",
                },
                {
                  path: this.outer_pdf_url,
                  fetch: true,
                  localFile: false,
                  code: "cover",
                },
              ],
            },
          ],
          shipments: [
            {
              shipmentIndex: 0,
              shipTo: {
                name: "Test Tester ",
                companyName: "Test Company",
                address1: "Do Not Ship",
                town: "DO NOT SHIP",
                postcode: "1ES TE1",
                isoCountry: "GB",
              },
              shipByDate: "2021-09-09T13:15:25.7654838+01:00",
              canShipEarly: false,
              returnAddress: {
                name: "Test",
                companyName: "Pureprint Group",
                address1: "Beon House, Bellbrook Park",
                town: "Uckfield",
                postcode: "TN22 1PL",
                isoCountry: "GB",
              },
              carrier: {
                alias: "shippingtest",
              },
              dispatchAlert: "",
            },
          ],
          tags: ["0"],
        },
      };

      let method = "POST";
      let path = "/api/order";
      let timestamp = Math.floor(Date.now() / 1000);
      let secret = "a3b2f495e1dc171a45d686747a9478d9cdbe1ed646d25791";
      let token = "1916514645615";

      let stringToSign = method + " " + path + " " + timestamp;
      let signature = crypto
        .HmacSHA1(stringToSign, secret)
        .toString(crypto.enc.Hex);
      let authHeader = token + ":" + signature;

      let headers = {
        "x-oneflow-authorization": authHeader,
        Accept: "application/json",
        "x-oneflow-date": timestamp,
      };
      axios
        .post("https://pro-api.oneflowcloud.com/api/order", data, {
          headers: headers,
        })
        .then((res) => {
          console.log(res);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    proceedToCheckout() {
      axios
        .post(
          "https://bebraveapi.hectorspost.com/save_submission.php",
          qs.stringify({
            email: this.purchaser_email,
            outer_pdf_url: this.outer_pdf_url,
            inner_pdf_url: this.inner_pdf_url,
            preview_url: this.preview_url,
            cover_type: this.cover_type,
            sku: this.sku,
          })
        )
        .then((res) => {
          console.log(res);
          let product_id = 937;
          if (this.cover_type == "hardback") {
            product_id = 635;
          }
          window.location.replace(
            `https://hectorspost.com/checkout/?add-to-cart=${product_id}&session_id=` +
              res.data
          );
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  components: {
    NavigationBar,
  },
};
</script>

<style scoped>
.preview {
  height: 543.5px;
  background-size: 111%;
  background-position: top;
}
a {
  text-decoration: none;
  color: white;
}
</style>
