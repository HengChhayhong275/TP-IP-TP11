<template>
  <main>
    <div class="wrapper">
      <div class="side-bar">
        <ul>
          <li v-for="category in categories" :key="category._id">
            <h4 class="pointer">
              {{ category.name }}
            </h4>
            <ul>
              <li v-for="p in category.items" :key="p._id">
                <span>{{ p.name }}</span>
              </li>
            </ul>
          </li>
        </ul>
      </div>

      <div class="products">
        <ProductCard
          v-for="pro in products"
          :key="pro._id"
          :products="pro"
        ></ProductCard>
      </div>
    </div>
  </main>
</template>


<script>
import ProductCard from "./card.vue";
export default {
  components: { ProductCard },

  data() {
    return {
      item: [],
      products: [],
      categories: [],
    };
  },

  async mounted() {
    const requestOptions = {
      method: "GET",
      credentials: "include",
      headers: {
        "Content-Type": "application/json",
        "Access-Control-Allow-Credentials": true,
      },
      body: JSON.stringify(),
    };

    const response_item = await fetch(
      "http://localhost:3001/item/all",
      requestOptions
    );

    const response_product = await fetch(
      "http://localhost:3001/product/all",
      requestOptions
    );

    const data_product = await response_product.json();

    console.log("data_product: ", data_product);
    this.products = data_product;

    //post data to api
    const response = await fetch(
      "http://localhost:3001/category/all",
      requestOptions
    );

    const data = await response.json();
    this.categories = data;
  },
  methods: {
    // async created() {},
  },
};
</script>


<style scoped>
.wrapper {
  display: flex;
  position: relative;
}

.side-bar {
  position: absolute;
  top: 0;
  left: 0;
  height: 100vh;
  border: 1px solid;
  width: 200px;
}

.products {
  padding-top: 20px;
  padding-left: 20px;
  padding-right: 20px;
  padding-bottom: 20px;
  margin-left: 220px;
  border: 1px solid;
  height: 100vh;
  display: grid;
  width: 100%;
  overflow-y: auto;
  justify-items: center;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}

.pointer {
  cursor: pointer;
}

@media (min-width: 768px) {
  .products {
    grid-template-columns: repeat(3, 1fr);
    grid-gap: 20px;
  }
}
</style>
