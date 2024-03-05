<script setup>

    const props = defineProps({
        product: Object
    });

    const emit = defineEmits(['quantityUp', 'quantityDown'])

    const inc = () => emit('quantityUp', props.product);
    const dec = () => emit('quantityDown', props.product);    

</script>

<template>
    <main class="d-flex flex-wrap justify-content-center">
                <div class="card m-1">
                    <img :src="product.image" class="card-img-top" alt="...">
                    <div class="card-body">
                      <h5 class="card-title">{{ product.title }}</h5>
                      <p class="card-text">{{ product.description }}</p>
                      <p class="card-price">{{ product.price }}$</p>
                        <div class="d-flex align-items-center justify-content-between">
                        <button class="btn btn-primary" @click="dec">-1</button>
                        <span>{{ product.quantity }}</span>
                        <button class="btn btn-primary" @click="inc">+1</button>
                        </div>
                    </div>
                </div>
        </main>
</template>

<style scoped>
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