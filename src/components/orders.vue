<template>
  <div class="center">
    <button class="base-btn add-btn" @click="$emit('addOrder')">新增</button>
    <table>
      <thead>
        <tr>
          <th :key="item" v-for="item in data.header">{{setTableHeader(item)}}</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr :key="item.id" v-for="(item, index) in data.body">
          <th :key="key" v-for="(val, key) in item" v-if="key !== 'id' && key !== 'isEdit'">
            <textarea
              ref="textarea"
              rows="1"
              v-if="key === 'notes'"
              :disabled="!data.body[index].isEdit"
              @keyup="autosize"
              v-model="data.body[index][key]"
            ></textarea>
            <input
              v-else
              :type="setInputType(key)"
              :disabled="!data.body[index].isEdit"
              v-model="data.body[index][key]"
            />
          </th>
          <th>
            <button
              class="base-btn edit-btn"
              v-if="!data.body[index].isEdit"
              @click="onEditHandler(data.body[index], true)"
            >編輯</button>
            <button
              class="base-btn cancel-btn"
              v-else
              @click="onEditHandler(data.body[index], false)"
            >確定</button>
            <button class="base-btn delete-btn" @click="onDeleteHandler(data.body[index])">刪除</button>
          </th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    data: {
      type: Object,
      default: () => ({
        screenWidth: document.body.clientWidth,
        header: [],
        body: [
          {
            id: null,
            isEdit: false,
            name: "",
            quantity: null,
            price: null,
            notes: ""
          }
        ]
      })
    }
  },
  methods: {
    serHeight() {
      this.$refs["textarea"].forEach(item => {
        const { scrollHeight } = item;
        item.style.height = `${scrollHeight}px`;
      });
    },
    autosize({ target }) {
      const { scrollHeight } = target;
      target.style.height = `${scrollHeight}px`;
    },
    setInputType(key) {
      switch (key) {
        case "name":
          return "產品";
        case "quantity":
          return "number";
        case "price":
          return "金額";
        case "notes":
          return "";
        default:
          return "text";
      }
    },
    setTableHeader(key) {
      switch (key) {
        case "name":
          return "產品";
        case "quantity":
          return "數量";
        case "price":
          return "金額";
        case "notes":
          return "備註";
        default:
      }
    },
    onEditHandler(orderItem, isEdit) {
      this.$emit("onEdit", { ...orderItem, isEdit });
    },
    onDeleteHandler(orderItem) {
      this.$emit("onDelete", { ...orderItem });
    }
  },
  watch: {
    screenWidth() {
      this.serHeight();
    }
  },
  mounted() {
    this.serHeight();
    window.addEventListener(
      "resize",
      () => {
        this.screenWidth = document.body.clientWidth;
      },
      false
    );
  },
  destroyed() {
    window.removeEventListener("resize", () => {}, false);
  }
};
</script>

<style >
table {
  border-collapse: collapse;
  border-spacing: 0;
  clear: both;
  border: 1px solid #cbcbcb;
}
table,
th,
td {
  width: 100%;
  table-layout: fixed;
}

td,
th {
  padding: 0.5em 0.25em;
  border-left: 1px solid #cbcbcb;
}

thead {
  color: #000;
  background-color: #e0e0e0;
}
tbody {
  color: #777;
}
tbody > tr {
  border-bottom: 1px solid #cbcbcb;
}
tfoot {
  color: #777;
}

input,
textarea {
  width: 100%;
  min-height: 20px;
  border-radius: 3px;
  border: 1px inset #777;
  box-sizing: border-box;
}
input {
  padding-left: 0.15em;
  padding-right: 0.15em;
}
textarea {
  display: block;
  height: 100%;
  overflow: hidden;
  resize: none;
}
input[disabled],
textarea[disabled] {
  text-align: center;
  background-color: #fff;
  border-style: none;
}
.center {
  width: 100%;
  max-width: 500px;
  margin: auto;
}
.base-btn {
  color: #fff;
  min-height: 30px;
  border-style: solid;
  border-width: 1px;
  border-radius: 3px;
  font-size: 13px;
  opacity: 0.75;
  cursor: pointer;
}
.base-btn:hover {
  opacity: 1;
}
.add-btn {
  color: #777;
  float: right;
  border-color: #777;
  background-color: #fff;
  margin-bottom: 5px;
}
.edit-btn {
  border-color: #1890ff;
  background-color: #1890ff;
}
.cancel-btn {
  border-color: #13ce66;
  background-color: #13ce66;
}
.delete-btn {
  margin-left: 5px;
  border-color: #ff4949;
  background-color: #ff4949;
}

@media screen and (max-width: 768px) {
  .center {
    width: 90%;
    margin: auto;
  }
  .delete-btn {
    margin-left: 0px;
  }

  textarea {
    overflow-y: auto;
  }
}
</style>





