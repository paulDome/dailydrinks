<template>
  <div id="app">
    <header>
      <img alt="17 logo" class="logo" src="./assets/logo.png" />
      <h1 class="title">飲料訂購單</h1>
    </header>
    <Orders
      :data="orders"
      @addOrder="addHandler"
      @onEdit="onEditHandler"
      @onDelete="onDeleteHandler"
    />
  </div>
</template>

<script>
import Orders from "./components/orders.vue";

export default {
  data() {
    return {
      orders: {
        header: ["name", "quantity", "price", "notes"],
        body: [
          {
            id: 1,
            isEdit: false,
            name: "珍珠奶茶",
            quantity: 1,
            price: 35,
            notes: "去冰、無糖"
          },
          {
            id: 2,
            isEdit: false,
            name: "紅茶",
            quantity: 1,
            price: 30,
            notes: ""
          },
          {
            id: 3,
            isEdit: false,
            name: "綠茶",
            quantity: 1,
            price: 25,
            notes: "無糖"
          },
          {
            id: 4,
            isEdit: false,
            name: "綠茶",
            quantity: 1,
            price: 25,
            notes: "無糖"
          }
        ]
      }
    };
  },
  computed: {
    isHasBlankOrde() {
      return this.orders.body.some(
        item =>
          item.name === "" && item.quantity === null && item.price === null
      );
    }
  },
  components: {
    Orders
  },
  methods: {
    addHandler() {
      const _orderList = this.orders.body.length;
      const _newOrder = {
        id: _orderList + 1,
        isEdit: true,
        name: "",
        quantity: null,
        price: null,
        notes: ""
      };
      if (!this.isHasBlankOrde) {
        // reset other orders edig status
        this.orders.body
          .filter((item, index) => index !== _orderList)
          .forEach((item, index) =>
            this.$set(this.orders.body[index], "isEdit", false)
          );
        this.$set(this.orders.body, _orderList, _newOrder);
      }
    },
    onEditHandler({ name, quantity, price, id, isEdit }) {
      const _index = this.orders.body.findIndex(item => item.id === id);
      this.orders.body.forEach((item, index) =>
        this.$set(this.orders.body[index], "isEdit", false)
      );
      if (name === "" && quantity === null && price === null) {
        this.onDeleteHandler(id);
      } else {
        if (this.isHasBlankOrde) this.onClearBlankOrde();
        this.$set(this.orders.body[_index], "isEdit", isEdit);
      }
    },
    onDeleteHandler({ id }) {
      const _index = this.orders.body.findIndex(item => item.id === id);
      if (this.isHasBlankOrde) this.onClearBlankOrde();
      this.$delete(this.orders.body, _index);
    },
    onClearBlankOrde() {
      const _index = this.orders.body.findIndex(
        item =>
          item.name === "" && item.quantity === null && item.price === null
      );
      this.$delete(this.orders.body, _index);
    }
  }
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}
img {
  vertical-align: bottom;
}
header {
  padding-top: 10%;
}
#app {
  width: 100%;
  height: 100%;
  background-color: #fff;
}
.title {
  margin: 0;
  text-align: center;
  padding-top: 15px;
  padding-bottom: 15px;
}
.logo {
  display: block;
  width: 100%;
  max-width: 120px;
  margin: auto;
}
.banner {
  width: 100%;
}
</style>
