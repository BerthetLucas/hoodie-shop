<script>
import { HOODIES } from "../data/hoodies";

export default {
  name: "CardProduct",

  data() {
    return {
      hoodiesList: HOODIES,
      tab: [],
      selectedBrand: null,
      filterTab: [],
      priceLimite: null,
      itemQte:1
    };
  },
  methods: {
    brandFilter(item) {
      this.selectedBrand = item;
    },
    allBrand() {
      this.selectedBrand = "all";
    },
    priceFilter(item) {
      this.priceLimite = item;
    },
    addToCart(cartItem){
        this.$emit("add", [cartItem,this.itemQte])
    }
  },
  computed: {
    uniqueBrand() {
      for (let i = 0; i < this.hoodiesList.length; i++) {
        this.tab[i] = this.hoodiesList[i].brand;
      }

      let arrayWithDuplicates = this.tab;
      let arrayWithoutDuplicates = [...new Set(arrayWithDuplicates)];

      return arrayWithoutDuplicates;
    },

    brandFilterAction() {
      if (!this.selectedBrand || this.selectedBrand == "all") {
        this.filterTab = this.hoodiesList;
        this.priceLimite = false; 
      } else if (this.selectedBrand) {
        this.filterTab = this.hoodiesList;
        this.filterTab = this.filterTab.filter(
          (prod) => prod.brand === this.selectedBrand
        );
      } 
      
      if (this.priceLimite) {
        this.filterTab = this.filterTab.filter(
          (prod) => prod.price < this.priceLimite
        );
      }
      return this.filterTab;
    },
  },
  mounted() {
    this.uniqueBrand; // Attention appeler comme une donnée pas comme une fonction avec mes () à la fin
  },
};
</script>

<template>
  <div class="container mx-auto mt-4 w-2/3 bg-card-background rounded py-2">
    <div id="filters" class="px-4 mt-4 flex space-x-2 bg-white py-2">
      <span
        class="px-2 border-tertiary-contrast border-2 border-primary cursor-pointer rounded-[0.27rem] bg-tertiary-contrast hover:text-orange-300"
        @click="allBrand"
      >
        Reset Filter
      </span>
      <span
        class="px-2 border-tertiary-contrast border-2 border-primary cursor-pointer rounded-[0.27rem] hover:text-red-600"
        v-for="(item, index) in uniqueBrand"
        :key="index"
        @click="brandFilter(item)"
      >
        {{ item }}
      </span>

      <span
        class="px-2 border-tertiary-contrast border-2 border-primary cursor-pointer rounded-[0.27rem] hover:text-red-600"
        @click="priceFilter(100)"
      >
        &#60;100€
      </span>
    </div>

    <div id="item-list" class="px-4">
      <!-- item card -->
      <div
        id="item-card"
        class="flex w-full border-b-2 border-tertiary-contrast mt-6"
        v-for="(item, index) in brandFilterAction"
        :key="item.ref"
      >
        <div
          class="flex rounded-lg bg-card-background-light shadow-lg flex-row w-full"
        >
          <img
            class="h-96 w-full rounded-t-lg object-cover md:h-auto md:w-48 md:rounded-none md:rounded-l-lg"
            :src="'../src/assets/img/' + item.picture"
            alt=""
          />
          <div class="flex flex-col justify-start p-6">
            <h5 class="mb-2 text-2xl font-bold text-neutral-800">
              {{ item.name }}
              <span
                class="inline-block ml-4 bg-button-background border-button-text rounded-md border-2 border-primary px-6 py-2 text-sm font-bold uppercase leading-normal text-primary"
              >
                {{ item.price }}€
              </span>
            </h5>

            <p class="mb-4 text-base text-neutral-600">
              {{ item.description }}
            </p>
            <p class="text-xs text-neutral-600">
              <button
                type="button"
                class="inline-block rounded border-2 border-tertiary-contrast border-opacity-75 px-6 pt-2 pb-2 text-xs font-medium uppercase leading-normal text-info"
                v-for="(size, sizeIndex) in item.availableSize"
                :key="sizeIndex"
              >
                {{ size }}
              </button>
            </p>
            <div class="flex flex-row mt-12">
              <div class="basis-1/4 w-96">
                QTY
                <select v-model="itemQte">
                  <option value="0">0</option>
                  <option value="1" selected>1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                  <option value="5">5</option>
                  <option value="6">6</option>
                  <option value="7">7</option>
                  <option value="8">8</option>
                </select>
              </div>
              <div class="basis-1/4">
                <button
                  type="button"
                  class="inline-block rounded border-2 bg-tertiary-contrast text-card-background-light border-button-text px-6 pt-2 pb-2 text-xs font-medium uppercase leading-normal text-info w-44"
                    @click="addToCart(item)"
                >
                  ADD TO CART
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="ml-2 w-6 h-6 float-right"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M2.25 3h1.386c.51 0 .955.343 1.087.835l.383 1.437M7.5 14.25a3 3 0 00-3 3h15.75m-12.75-3h11.218c1.121-2.3 2.1-4.684 2.924-7.138a60.114 60.114 0 00-16.536-1.84M7.5 14.25L5.106 5.272M6 20.25a.75.75 0 11-1.5 0 .75.75 0 011.5 0zm12.75 0a.75.75 0 11-1.5 0 .75.75 0 011.5 0z"
                    />
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
