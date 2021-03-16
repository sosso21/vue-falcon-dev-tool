<template>
  <section>
    <h2 class="my-2 font-weight-light text-center">Prix</h2>

    <div>
      <div class="form-group row my-1">
        <label for="price" class="col-sm-2 col-form-label">Prix :</label>
        <div class="col-sm-10">
          <input
            @change="calcArray"
            type="number"
            class="form-control"
            id="price"
            v-model="priceInput"
          />
        </div>
      </div>
      <div class="form-group row my-1">
        <label for="reduction" class="col-sm-2 col-form-label"
          >Promo (%) :</label
        >
        <div class="col-sm-10">
          <input
            @change="calcArray"
            type="number"
            class="form-control"
            id="reduction"
            v-model="reduction"
          />
        </div>
      </div>
      <div class="form-group row my-1">
        <label for="marge" class="col-sm-2 col-form-label">Marge (%) :</label>
        <div class="col-sm-10">
          <input
            @change="calcArray"
            type="number"
            class="form-control"
            id="marge"
            v-model="marge"
          />
        </div>
      </div>
      
      <div class="form-group row my-1">
        <label for="StockPrice" class="col-sm-2 col-form-label">Prix de gros :</label>
        <div class="col-sm-10">
          <input
            @change="calcArray"
            type="number"
            class="form-control"
            id="StockPrice"
            v-model="stockPrice"
          />
        </div>
      </div>
    </div>
    <div>
    <h3 class="my-2">Estimation du prix de vente</h3>
      <p>  ce produit sera potentiellement vendu entre : 
       
         Maximum : <strong class="fs-2"> {{ Math.round(estimation.max * 100) / 100 }}<sup>€</sup> </strong>
        et  Minimum : <strong class="fs-2"> {{ Math.round(estimation.min * 100) / 100 }}<sup>€</sup></strong>
      </p>
      <p>
      Le Bénéfices potentielles seront de entre : 
         Maximum : <strong class="fs-2"> {{ Math.round((estimation.max * 100) / 100  - stockPrice)}}<sup>€</sup> </strong>
         soit {{  Math.round((1-(stockPrice/estimation.max ))*100) }}% ,
        et  Minimum : <strong class="fs-2"> {{ Math.round((estimation.min * 100) / 100 )-stockPrice }}<sup>€</sup></strong>
         soit {{  Math.round((1-(stockPrice/estimation.min ))*100) }}%
        
      </p>
      
    </div>
    <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copy">
      copy
    </button>
    <div class="input-group">
      <textarea
        class="bg-dark text-warning form-control areaImage my-2"
        v-model="PriceArea"
        cols="30"
        rows="10"
      ></textarea>
    </div>
  </section>
</template>
<script>
import { ref } from "vue";

export default {
  name: "Price",
  props: {},
  setup() {
    const priceInput = ref(99.99);
    const reduction = ref(0);
    const stockPrice = ref(1)

    const marge = ref(20);
    const estimation = ref({ min: 79.99, max: 99.99 });
    const PriceArea = ref(null);

    const calcArray = () => {
      let promo = reduction.value == 0 ? false : reduction.value;
      PriceArea.value = "[\n " + priceInput.value + ",\n " + promo + "\n]";
 estimation.value={
     min: priceInput.value * (1 - ((+reduction.value + +marge.value) / 100)),
        max: priceInput.value * (1 - reduction.value / 100),
      };
       
       
    };

    const copy = () => {navigator.clipboard.writeText(PriceArea.value);};
    return {
      priceInput,
      PriceArea,
      reduction,
      marge,
      estimation,
      stockPrice,

      copy,
      calcArray,
    };
  },
};
</script>
