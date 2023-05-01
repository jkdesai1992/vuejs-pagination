<template>
  <div class="hello">
    <div>
      <table>
        <thead>
          <tr>
            <th>Product Title</th>
            <th>Price</th>
            <th>Rating</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in finalDataList" v-bind:key="index" class="pro-list-info">
            <td>{{item.productTitle}}</td>
            <td>${{item.salePrice}}</td>
            <td>
              <Rating :ratingStars="item.rating"/>
            </td>
          </tr>
        </tbody>
      </table>
      <Pagination :items="allProductList" :pageSize="8" :parentMethod="this.parentMethodData"/>
    </div>
  </div>
</template>

<script>
import Rating from "./Rating";
import Pagination from "./Pagination";
import productData from "../ProductListJson";

export default {
  name: "ProductList",
  components: { Pagination, Rating },
  props: {
    msg: String
  },
  data() {
    return {
      finalDataList: [],
      allProductList: []
    };
  },
  created() {
    this.allProductList = productData.productDataList;
  },
  methods: {
    parentMethodData: function(listData) {
      this.finalDataList = listData;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}
</style>
