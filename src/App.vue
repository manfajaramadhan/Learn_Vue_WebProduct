<script setup>
import { ref, onMounted, computed } from "vue";
import "./style.css";

const products = ref([]);
const maximum = ref(200);
const cart = ref([]);

onMounted(() => {
  fetch("https://hplussport.com/api/products/order/price")
    .then((response) => response.json())
    .then((data) => {
      products.value = data;
    });
});

function addProduct(item) {
  cart.value.push(item);
}

const filteredProducts = computed(() => {
  return products.value.filter(
    (item) => Number(item.price) <= Number(maximum.value),
  );
});
</script>

<template>
  <div class="app">
    <nav class="navbar navbar-light bg-white shadow-sm px-4">
      <div class="navbar-brand fw-bold text-info">H+ Sport</div>

      <div class="d-flex align-items-center">
        <span class="me-2 fw-semibold">Cart</span>
        <span class="badge rounded-pill bg-info fs-6">
          {{ cart.length }}
        </span>
      </div>
    </nav>

    <main class="main-content">
      <div class="page-header">
        <h1>Sport Products</h1>
        <p>Find your favorite sports products</p>
      </div>

      <div class="filter-card">
        <div class="filter-header">
          <label for="maximum"> Maximum Price </label>

          <span> ${{ maximum }} </span>
        </div>

        <input
          id="maximum"
          type="range"
          class="form-range"
          min="0"
          max="200"
          step="1"
          v-model="maximum"
        />

        <div class="range-label">
          <small>$0</small>
          <small>$200</small>
        </div>

        <input
          type="number"
          class="form-control price-input"
          min="0"
          max="200"
          v-model="maximum"
        />
      </div>

      <div class="products-header">
        <h2>Products</h2>

        <span> {{ filteredProducts.length }} products found </span>
      </div>

      <div class="products-grid">
        <div v-for="item in filteredProducts" :key="item.id" class="item-card">
          <div class="item-image-wrapper">
            <img :src="item.image" :alt="item.name" class="item-image" />
          </div>

          <div class="item-body">
            <h3>
              {{ item.name }}
            </h3>

            <p>
              {{ item.description }}
            </p>

            <div class="item-footer">
              <span class="item-price"> ${{ item.price }} </span>

              <button class="btn btn-info text-white" @click="addProduct(item)">
                + Add
              </button>
            </div>
          </div>
        </div>
      </div>

      <div v-if="filteredProducts.length === 0" class="empty-state">
        <h3>No products found</h3>
        <p>Try increasing the maximum price.</p>
      </div>
    </main>
  </div>
</template>
