<template>
  <div class="container">
    <div class="flex justify-between items-center my-2">
      <p class="figure">Figure: 1</p>
      <div class="flex justify-between items-center mr-3">
        <div>
          <span class="price">Price:</span>
          <select v-model="currentPriceFilter" @change="applyFilters">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
            <option value="default">Default</option>
          </select>
        </div>
        <div>
          <span class="rating">Rating:</span>
          <select v-model="currentRatingFilter" @change="applyFilters">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
            <option value="default">Default</option>
          </select>
        </div>
      </div>
    </div>

    <table>
      <tr class="tr">
        <th>SI</th>
        <th>Name</th>
        <th>Rating</th>
        <th>Price</th>
        <th class="text-center">Action</th>
      </tr>
      <div v-if="loading">
        <p class="loading">Loading...</p>
      </div>

      <template v-for="item in paginatedItems">
        <tr :key="item.id" class="userItem">
          <td>{{ item.id }}</td>
          <td>{{ item.title }}</td>
          <td>{{ item.rating }}</td>
          <td>{{ item.price }}</td>
          <td class="text-center">
            <button
              class="btn btn-show"
              :class="{
                'btn-green': !isSelected(item),
                'btn-red': isSelected(item),
              }"
              @click="toggleDetails(item)"
            >
              {{ getButtonLabel(item) }}
            </button>
          </td>
        </tr>
        <tr v-if="selectedItem && selectedItem.id === item.id" class="userItem">
          <td colspan="5">
            <DetailsView
              class="mt-1"
              :item="selectedItem"
              @close="closeDetails"
            />
          </td>
        </tr>
      </template>
    </table>

    <div class="pagination text-center mt-1">
      <button
        class="pagination-btn"
        v-for="page in pageCount"
        :key="page"
        @click="changePage(page)"
      >
        {{ page }}
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import DetailsView from "../components/detailsPanel.vue";
import FilterButton from "../components/filtercomponent.vue";

export default {
  name: "IndexPage",
  components: {
    DetailsView,
    FilterButton,
  },
  data() {
    return {
      items: [],
      currentPage: 1,
      itemsPerPage: 5,
      selectedItem: null,
      currentPriceFilter: "default",
      currentRatingFilter: "default",
      loading: false,
    };
  },
  mounted() {
    this.getData();
  },
  computed: {
    paginatedItems() {
      const filteredItems = this.applyFilters();

      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      const endIndex = startIndex + this.itemsPerPage;
      return filteredItems.slice(startIndex, endIndex);
    },
    pageCount() {
      const filteredItems = this.applyFilters();
      return Math.ceil(filteredItems.length / this.itemsPerPage);
    },
  },
  methods: {
    async getData() {
      try {
        this.loading = true;
        const { data } = await axios.get("https://dummyjson.com/products");
        this.items = data.products;
        this.applyFilters(); // Apply initial filters
      } catch (error) {
        console.error(error);
      } finally {
        this.loading = false;
      }
    },
    changePage(page) {
      this.currentPage = page;
    },
    async toggleDetails(item) {
      if (this.selectedItem && this.selectedItem.id === item.id) {
        this.selectedItem = null;
      } else {
        try {
          const { data } = await axios.get(
            `https://dummyjson.com/products/${item.id}`
          );
          this.selectedItem = data;
        } catch (error) {
          console.error(error);
        }
      }
    },
    closeDetails() {
      this.selectedItem = null;
    },
    getButtonLabel(item) {
      return this.selectedItem && this.selectedItem.id === item.id
        ? "Close"
        : "Show";
    },
    isSelected(item) {
      return this.selectedItem && this.selectedItem.id === item.id;
    },
    applyFilters() {
      let filteredItems = [...this.items];

      if (this.currentPriceFilter === "asc") {
        filteredItems.sort((a, b) => a.price - b.price);
      } else if (this.currentPriceFilter === "desc") {
        filteredItems.sort((a, b) => b.price - a.price);
      }

      if (this.currentRatingFilter === "asc") {
        filteredItems.sort((a, b) => a.rating - b.rating);
      } else if (this.currentRatingFilter === "desc") {
        filteredItems.sort((a, b) => b.rating - a.rating);
      }

      return filteredItems;
    },
  },
};
</script>

<style scoped>
/* font-family: 'Merienda', cursive;
font-family: 'Open Sans', sans-serif;
font-family: 'Poppins', sans-serif;
font-family: 'Roboto', sans-serif;
font-family: 'Roboto Mono', monospace; */

* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
.flex {
  display: flex;
}
.items-center {
  align-items: center;
}
.justify-between {
  justify-content: space-between;
}
.btn-green {
  background-color: green !important;
}

.btn-red {
  background-color: hsl(45, 87%, 41%) !important;
}
.mt-1 {
  margin-top: 1rem;
}
.my-2 {
  margin: 2rem 0;
}
.mr-3 {
  margin-right: 3rem;
}
.text-center {
  text-align: center;
}
.btn {
  font-family: "Poppins", sans-serif !important;
  cursor: pointer;
  border: none;
  outline: none;
}
select {
  font-family: "Merienda", cursive;
  padding: 0.6rem 0.8rem !important;
  font-weight: 600;
  margin: 0 0.6rem;
  border: 1px solid #ddd !important;
}
.filter-btn {
  font-family: "Merienda", cursive;
  padding: 0.6rem 0.8rem !important;
  font-weight: 600;
  margin: 0 0.6rem;
  border: 1px solid #ddd !important;
}
.figure {
  font-size: 1.1rem;
  font-family: "Poppins", sans-serif;
  font-weight: 600;
}
.price,
.rating {
  font-family: "Poppins", sans-serif;
  font-weight: 600;
}
.btn-show {
  font-family: Arial, Helvetica, sans-serif;
  padding: 0.6rem 1.5rem;
  border-radius: 10px;
  color: white;
}
.loading {
  margin-top: 1rem;
  font-family: "Merienda", cursive;
  font-size: 1.2rem;
}
.pagination .pagination-btn {
  padding: 0.9rem;
}
.container {
  padding: 2rem;
  margin: auto;
  max-width: 1200px;
}

.btn {
  outline: none;
  border: none;
  background: transparent;
  cursor: pointer;
}

table {
  width: 100%;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  font-family: "Poppins", sans-serif !important;
  text-align: left;
  padding: 0.8rem !important;
  border: 1px solid #bdb0b0;
}
</style>
