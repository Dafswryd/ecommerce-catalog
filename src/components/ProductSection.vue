<template>
  <div class="magenta">
    <div
      :class="
        product.category === 'men\'s clothing'
          ? 'background-container-men'
          : product.category === 'women\'s clothing'
          ? 'background-container-women'
          : 'background-container-unavailable'
      "
    ></div>
    <div class="section">
      <div v-if="loading" class="loader">
        <div class="spinner"></div>
      </div>
      <div v-else class="container">
        <div
          v-if="
            product.category === 'men\'s clothing' ||
            product.category === 'women\'s clothing'
          "
          class="image-section"
        >
          <img
            v-if="
              product.category === 'men\'s clothing' ||
              product.category === 'women\'s clothing'
            "
            :src="product.image"
            alt="Image"
            class="image"
          />
        </div>
        <div v-else class="image-sad">
          <div class="sad">
            <p class="paragraf">This product is unavailable to show</p>
            <button class="button-next-unavailable" @click="nextProduct">
              Next Product
            </button>
          </div>
          <!-- <img src="../assets/images/sad-face.png" alt="" class="sad" /> -->
        </div>

        <div class="description-section">
          <h1
            v-if="
              product.category === 'men\'s clothing' ||
              product.category === 'women\'s clothing'
            "
            :class="
              product.category === 'men\'s clothing'
                ? 'title-product-men'
                : 'title-product-women'
            "
          >
            {{ product.title }}
          </h1>
          <h1 v-else class="title-product-unavailable" style="display: none">
            {{ product.title }}
          </h1>
          <div
            v-if="
              product.category === 'men\'s clothing' ||
              product.category === 'women\'s clothing'
            "
            class="header-content"
          >
            <div>
              <p>{{ product.category }}</p>
            </div>
            <div class="rating">
              <p>{{ product.rating.rate }}/5</p>
              <div class="rating-ui">
                <div
                  v-for="i in 5"
                  :key="i"
                  :class="{
                    'circle-men': product.category === 'men\'s clothing',
                    'circle-women': product.category === 'women\'s clothing',
                    'circle-filled': i <= product.rating.rate,
                    'empty-circle': i > product.rating.rate,
                  }"
                ></div>
              </div>
            </div>
          </div>
          <div v-else style="display: none" class="header-content"></div>
          <p
            v-if="
              product.category === 'men\'s clothing' ||
              product.category === 'women\'s clothing'
            "
            class="description"
          >
            {{ product.description }}
          </p>
          <p
            v-if="
              product.category === 'men\'s clothing' ||
              product.category === 'women\'s clothing'
            "
            :class="
              product.category === 'men\'s clothing'
                ? 'price-men'
                : 'price-women'
            "
          >
            ${{ product.price }}
          </p>

          <div class="button-section">
            <button
              v-if="
                product.category === 'men\'s clothing' ||
                product.category === 'women\'s clothing'
              "
              :class="
                product.category === 'men\'s clothing'
                  ? 'button-buy-men'
                  : 'button-buy-women'
              "
            >
              Buy now
            </button>

            <button
              @click="nextProduct"
              v-if="
                product.category === 'men\'s clothing' ||
                product.category === 'women\'s clothing'
              "
              :class="
                product.category === 'men\'s clothing'
                  ? 'button-next-men'
                  : 'button-next-women'
              "
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const totalNumberOfProducts = 20;
export default {
  name: "ProductSection",
  data: function () {
    return {
      loading: true,
      product: {},
    };
  },
  mounted() {
    this.fetchProduct();
  },
  methods: {
    fetchProduct() {
      fetch("https://fakestoreapi.com/products/1")
        .then((response) => {
          if (!response.ok) {
            throw new Error("Network response was not ok");
          }
          return response.json();
        })
        .then((data) => {
          this.product = data;
          this.loading = false;
        })
        .catch((error) => {
          console.error("Error fetching product:", error);
          this.loading = false;
        });
    },
    nextProduct() {
      this.loading = true;
      const nextProductId = this.product.id + 1;
      this.fetchNextProduct(nextProductId);
    },

    
    fetchNextProduct(productId) {
      fetch(`https://fakestoreapi.com/products/${productId}`)
        .then((response) => {
          if (!response.ok) {
            throw new Error("Network response was not ok");
          }
          return response.json();
        })
        .then((data) => {
          this.product = data;
          this.loading = false;

          if (this.product.id === totalNumberOfProducts) {
            this.fetchProduct();
          }
        })
        .catch((error) => {
          console.error("Error fetching product:", error);
        });
    },
  },
};
</script>

<style scoped src="../assets/styles/ProductSection.css"></style>
