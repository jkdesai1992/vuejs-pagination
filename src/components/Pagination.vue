<template>
  <div class="row pro-result">
    <div class="col-xl-6 col-lg-6 col-md-6 col-12 pagination-style">
      <div class="pagination">
        <ul>
          <li>
            <button
              :disabled="pager.currentPage === 1"
              v-on:click="setPage(pager.currentPage - 1)"
            >Previous</button>
          </li>

          <li v-for="(page, i) in pager.pages" v-bind:key="i">
            <button
              :class="pager.currentPage === page ? 'active' : ''"
              v-on:click="setPage(page)"
            >{{page}}</button>
          </li>
          <li>
            <button
              :disabled="pager.currentPage === pager.totalPages"
              v-on:click="setPage(pager.currentPage + 1)"
            >Next</button>
          </li>
        </ul>
      </div>
    </div>
    <div class="col-xl-6 col-lg-6 col-md-6 col-12 pagination-style">
      <div class="show-result">
        <p>
          <span>Showing</span>
          <span>{{pager.currentPage}} - {{pager.totalPages}}</span>
          <span>of</span>
          <span>{{pager.totalPages}}</span>
          results
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  data() {
    return {
      pager: {},
      initialPage: 1
    };
  },
  props: {
    parentMethod: Function,
    items: Array,
    pageSize: Number
  },
  created() {
    debugger;
    this.setPage(this.initialPage);
  },
  methods: {
    setPage: function(page) {
      let { items, pageSize } = this;
      let pager = this.pager;

      // get new pager object for specified page
      pager = this.getPager(items.length, page, pageSize);

      // get new page of items from items array
      let pageOfItems = items.slice(pager.startIndex, pager.endIndex + 1);

      // update state
      this.pager = pager;

      // call change page function in parent component
      this.parentMethod(pageOfItems);
    },
    getPager: function(totalItems, currentPage, pageSize) {
      currentPage = currentPage || 1;

      // default page size is 10
      pageSize = pageSize || 10;

      // calculate total pages
      let totalPages = Math.ceil(totalItems / pageSize);

      let startPage, endPage;
      if (totalPages <= 5) {
        // less than 5 total pages so show all
        startPage = 1;
        endPage = totalPages;
      } else {
        // more than 5 total pages so calculate start and end pages
        if (currentPage <= 3) {
          startPage = 1;
          endPage = 5;
        } else if (currentPage + 2 >= totalPages) {
          startPage = totalPages - 4;
          endPage = totalPages;
        } else {
          startPage = currentPage - 2;
          endPage = currentPage + 2;
        }
      }

      // calculate start and end item indexes
      let startIndex = (currentPage - 1) * pageSize;
      let endIndex = Math.min(startIndex + pageSize - 1, totalItems - 1);

      // create an array of pages to ng-repeat in the pager control
      let pages = [...Array(endPage + 1 - startPage).keys()].map(
        i => startPage + i
      );

      // return object with all pager properties required by the view
      return {
        totalItems: totalItems,
        currentPage: currentPage,
        pageSize: pageSize,
        totalPages: totalPages,
        startPage: startPage,
        endPage: endPage,
        startIndex: startIndex,
        endIndex: endIndex,
        pages: pages
      };
    }
  }
};
</script>

<style scoped>
/*pagination-style*/
.pro-result {
  margin-top: 30px;
  align-items: center;
}
.pro-result .pagination-style:last-child {
  text-align: right;
}
.pagination-style .pagination ul {
  padding-left: 0;
  margin-bottom: 0px;
  display: flex;
}
.pagination-style .pagination ul li {
  list-style-type: none;
  display: inline-block;
  margin-right: 5px;
}
.pagination-style .pagination ul li button {
  color: #000;
  float: left;
  padding: 6px 12px;
  text-decoration: none;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
.pagination-style .pagination ul li button.active,
.pagination-style .pagination ul li button:hover {
  background-color: #13b5ea;
  border: 1px solid #13b5ea;
  color: #fff !important;
}

.pagination-style .show-result p {
  margin-bottom: 0px;
  color: #bbb;
}
</style>
