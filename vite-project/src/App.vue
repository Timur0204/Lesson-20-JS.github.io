<script setup>

  const url = 'https://fakestoreapi.com/products';

  import { reactive, ref, onMounted, computed } from 'vue';

  import Card from './components/Card.vue';

  const products = reactive([]);

  onMounted(async () => {
    let data = await fetch(url);
        data = await data.json();
    for(let item of data){
      item.quantity = 0;
    }
    console.log(data);
    products.push(...data);
  });

  const search = ref('');

  const sort = ref('');

  const min = ref(0);

  const max = ref(1000);

  const totalQuantity = computed(() => products.reduce((acc, item) => acc + item.quantity, 0));
  const totalCost = computed(() => products.reduce((acc, item) => acc + item.quantity * item.price, 0));

  const productsToShow = computed(() => {
    let s = search.value.toLowerCase();
    let results = products.filter(item =>
      item.title.toLowerCase().includes(s) ||
      item.description.toLowerCase().includes(s)
    );

    results = results.filter(item => item.price >= min.value && item.price <= max.value);

    if(sort.value == 'up'){
      results.sort((a,b) => a.price - b.price);
    } else if(sort.value == 'down'){
      results.sort((a,b) => b.price - a.price);
    }

    return results;
  });

  const qtUp = (product) => {
    product.quantity++;
  }

  const qtDown = (product) => {
    if(product.quantity > 0){
      product.quantity--;
    }
  }

</script>

<template>
  <div class="container">
          <header class="mb-3 pb-2">
            <h1>Product</h1>

            <div class='text-right'>
                <input id='search' class='me-auto' type="text" placeholder="Search..." v-model="search">

                <div class="title">
                    <h2 class="h2">Options</h2>
                </div>
                <div class="range d-flex flex-column">
                    <input min="0" max="1000" type="range" id="minPrice" v-model="min">
                    <span>Min: {{ min }}$</span>
                    <input min="0" max="1000" type="range" id="maxPrice" v-model="max">
                    <span>Max: {{ max }}$</span>
                </div>                              
                

                <span class='mx-4 d-flex align-items-center'>
                    <label for='sort-up'>Price Up</label>
                    <input id='sort-up' class='mx-2' type="radio" value='up' name='sort' v-model="sort">
                </span>

                <span class='mx-4 d-flex align-items-center'>
                    <label for='sort-down'>Price Down</label>
                    <input id='sort-down' class='mx-2' type="radio" value='down' name='sort' v-model="sort">
                </span>
            </div>

            <h4 class="mb-0">
                <span class="px-2">Количество: ({{ totalQuantity }})</span>
                <span>Общая стоимость: ${{ totalCost.toFixed(2) }}</span>
              </h4>
        </header>
    <main>
        <div class="row row-cols-4">
            <Card
              v-for="item in productsToShow"
              :product="item"
              @quantity-up="qtUp"
              @quantity-down="qtDown"
            />
        </div>
    </main>
  </div>
</template>

<style scoped>

#app[v-cloak] {
  display: none;
}

#app {
  opacity: 1;
  transition: 1s;
}

.card-price {
    display: flex;
    justify-content: end;
   }

   header {
      border-bottom: 1px solid rgba(0, 0, 0, 1);
   }

   #search:focus {
    border: navy;
   }

   .range {
    width: 50vw;
   }

   .card {
    box-shadow: 0px 2px 4px 0px rgba(0, 0, 0, 0.5);
   }

   .burger {
    position: absolute;
    display: none;
   }

   @media(min-width: 764px) {
       img {
         max-height: 22vh;
       }

       .range {
        width: 15vw;
       }
       
   }

   @media(min-width: 1200px) {
      img {
        max-height: 35vh;
      }

      .card {
        transition: 0.5s;
       }
    
       .card:hover {
        transform: scaleX(1.1);
        z-index: 1;
       }
   
  }
</style>
