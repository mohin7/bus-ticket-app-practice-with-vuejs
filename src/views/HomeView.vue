<template>
  <div class="booking-app">
    <h1 class="app-title">Ticket Booking App</h1>
    <div class="ticket-app">
      <div class="confirmed-dialog" v-if="confirmed">
        <h3>Ticket confirmed!</h3>
        <hr />
        <br />
        <table class="selected-seats">
          <tr>
            <th>Passenger Name</th>
            <td>{{ uname }}</td>
          </tr>

          <tr>
            <th>Passenger Contact</th>
            <td>{{ mobile }}</td>
          </tr>
          <tr>
            <th>Seats:</th>
            <td>
              <div>
                <div v-for="(seat, i) in selectedSeat" :key="i">
                  {{ seat.name }}
                </div>
              </div>
            </td>
          </tr>
          <tr>
            <th>Total Cost</th>
            <td>Tk. {{ cost }}</td>
          </tr>
        </table>

        <br />

        <button class="confirm-button" @click="resetData">Book Again</button>
      </div>
      <div class="ticket-app__top">
        <div class="seat-states">
          <div
            class="seat-state"
            v-for="(value, key, idx) in seatStates"
            :key="idx"
          >
            <div
              class="seat-state__color"
              :style="{ backgroundColor: value.color }"
            ></div>
            <div class="seat-state__text">{{ value.text }}</div>
          </div>
        </div>
      </div>
      <div class="ticket-app__bottom">
        <div class="ticket-app__left">
          <div class="bus">
            <div class="bus__top">
              <div class="bus__door">Door</div>
              <div class="bus__driver">Driver</div>
            </div>

            <div class="seats">
              <div
                class="seat"
                v-for="(seat, idx) in seats"
                :key="idx"
                :class="{
                  'seat--sold': seat.type === 'sold',
                  'seat--booked': seat.type === 'booked',
                  'seat--selected': seat.type === 'selected',
                }"
                @click="selectSeat(idx)"
              >
                {{ seat.name }}
              </div>
            </div>
          </div>
        </div>

        <div class="ticket-app__instruction" v-if="selectedSeat < 1">
          No seats selected <br />
          Select some seats first
        </div>

        <div class="ticket-app__right" v-else>
          <div class="cart">
            <strong>Selected Seats</strong>
            <table class="selected-seats">
              <thead>
                <tr>
                  <th>Seat</th>
                  <th>Price</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(seat, idx) in selectedSeat" :key="idx">
                  <td>{{ seat.name }}</td>
                  <td>Tk. {{ seat.price }}</td>
                </tr>

                <tr v-if="appliedCoupon !== null">
                  <th>Discount</th>
                  <th>Tk. -{{ appliedCoupon.discount }}</th>
                </tr>

                <tr>
                  <th>Total</th>
                  <th>Tk. {{ cost }}</th>
                </tr>
              </tbody>
            </table>
            <p style="display: flex">
              Ex. Coupon: <mark>OFF100</mark> and <mark>OFF200</mark>
            </p>
            <p v-if="appliedCoupon === null">
              Have a coupon?
              <input
                type="text"
                v-model="couponCode"
                placeholder="COUPON CODE"
              />
            </p>

            <p v-else>Applied Coupon : {{ appliedCoupon.code }}</p>
          </div>

          <div class="info">
            <input type="text" v-model="uname" placeholder="Name" />
            <input type="text" v-model="mobile" m placeholder="Mobile" />
          </div>

          <button class="confirm-button" @click="confirm()">Confirm</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      uname: "",
      mobile: "",
      confirmed: false,
      seatStates: {
        sold: {
          text: "Sold",
          color: "#ff0000",
        },
        available: {
          text: "Available",
          color: "#fff",
        },
        booked: {
          text: "Booked",
          color: "#dddddd",
        },
        selected: {
          text: "Selected",
          color: "#00ff00",
        },
      },
      seats: [
        {
          name: "A1",
          type: "available",
          price: 500,
        },
        {
          name: "A2",
          type: "available",
          price: 500,
        },
        {
          name: "A3",
          type: "available",
          price: 500,
        },
        {
          name: "A4",
          type: "available",
          price: 500,
        },
        {
          name: "B1",
          type: "available",
          price: 450,
        },
        {
          name: "B2",
          type: "available",
          price: 450,
        },
        {
          name: "B3",
          type: "available",
          price: 450,
        },
        {
          name: "B4",
          type: "available",
          price: 450,
        },
        {
          name: "C1",
          type: "sold",
          price: 500,
        },
        {
          name: "C2",
          type: "sold",
          price: 500,
        },
        {
          name: "C3",
          type: "sold",
          price: 500,
        },
        {
          name: "C4",
          type: "sold",
          price: 500,
        },
        {
          name: "D1",
          type: "available",
          price: 400,
        },
        {
          name: "D2",
          type: "available",
          price: 400,
        },
        {
          name: "D3",
          type: "available",
          price: 400,
        },
        {
          name: "D4",
          type: "available",
          price: 400,
        },
        {
          name: "E1",
          type: "available",
          price: 300,
        },
        {
          name: "E2",
          type: "available",
          price: 300,
        },
        {
          name: "E3",
          type: "booked",
          price: 300,
        },
        {
          name: "E4",
          type: "booked",
          price: 300,
        },
        {
          name: "F1",
          type: "available",
          price: 300,
        },
        {
          name: "F2",
          type: "available",
          price: 300,
        },
        {
          name: "F3",
          type: "available",
          price: 300,
        },
        {
          name: "F4",
          type: "available",
          price: 300,
        },
      ],
      couponCode: "",
      appliedCoupon: null,
      coupons: [
        {
          code: "OFF100",
          discount: 100,
        },
        {
          code: "OFF200",
          discount: 200,
        },
      ],
    };
  },
  computed: {
    selectedSeat() {
      let ss = this.seats.filter((seat) => {
        return seat.type === "selected";
      });
      return ss;
    },
    cost() {
      let price = 0;
      this.selectedSeat.forEach((seat) => {
        price = price + seat.price;
      });

      if (this.appliedCoupon !== null) {
        price = price - this.appliedCoupon.discount;
      }
      return price;
    },
  },
  watch: {
    couponCode(newVal) {
      if (newVal.length === 6) {
        let searchCoupons = this.coupons.filter((item) => {
          return item.code === newVal;
        });
        console.log(searchCoupons);
        if (searchCoupons.length === 1) {
          this.appliedCoupon = searchCoupons[0];
          this.couponCode = "";
        } else {
          alert("Coupon Code Not Valid");
        }
      }
    },
  },
  methods: {
    selectSeat(i) {
      let selectedSeatItem = this.seats[i];
      if (
        selectedSeatItem.type === "sold" ||
        selectedSeatItem.type === "booked"
      ) {
        alert("You cannot select this seats");
        return;
      }

      if (
        selectedSeatItem.type === "available" &&
        this.selectedSeat.length >= 4
      ) {
        alert("You cannot more than 4 seats");
        return;
      }
      selectedSeatItem.type =
        selectedSeatItem.type === "selected" ? "available" : "selected";
    },
    confirm() {
      if (this.uname !== "" || this.mobile !== "") {
        this.confirmed = true;
      } else {
        alert("Please enter your valid name and phone");
      }
    },
    resetData() {
      this.uname = "";
      this.mobile = "";
      this.appliedCoupon = null;
      this.confirmed = false;
      this.seats.forEach((seat) => {
        if (seat.type === "selected") {
          seat.type = "sold";
        }
      });
    },
  },
};
</script>
<style lang=""></style>
