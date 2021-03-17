<template>
  <section>
  <h1 class="my-2">
    Réseaux Sociaux
  </h1>

  <form class="input-group">
      <input type="text" class="form-control" v-model="idProduct" placeholder="id du produit"/><button
        class="btn btn-lg btn-primary bi bi-search"
        @click.prevent="searchProduct"
      ></button>
    </form>
    <div>
    <h2 class="my-2 font-weight-light text-center">infos :</h2>
    <pre>
    nom : {{ ProductApi.name}}
    prix (sans reduction): {{ProductApi.price[0]}}
    reduction: {{ProductApi.price[1] ? ProductApi.price[1]  : 0 }}%
    prix (avec reduction): {{ProductApi.price[0] * (1-(ProductApi.price[1]/100) ) }}


    </pre>

    </div>

    <div>
    <h2 class="my-2 font-weight-light text-center"><i class="bi bi-facebook"></i> Facebook :</h2>
    
    </div>

        <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copyFb" >copy</button>
    <div class="input-group">
      <textarea
        class="bg-dark text-warning form-control areaImage my-2"
        v-model="FbArea"
        cols="30"
        rows="10"
      ></textarea>
    </div>

    <h2 class="my-2 font-weight-light text-center"><i class="bi bi-instagram"></i> Instagram :</h2>

        <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copyInsta" >copy</button>
    <div class="input-group">
      <textarea
        class="bg-dark text-warning form-control areaImage my-2"
        v-model="InstArea"
        cols="30"
        rows="10"
      ></textarea>
    </div>
    <div>
    
    <h2 class="my-2 font-weight-light text-center"><i class="bi bi-image"></i> Umages :</h2>
    <button class="btn btn-danger btn-lg my-4" @click="seePromo = !seePromo">{{seePromo ? "voir":"chacher"}} les promo </button>
    <div class="imgSocial" v-if="ProductApi.image[0]" >
    
    <span  v-for="image in ProductApi.image" >
       <i v-show="ProductApi.price && seePromo">-{{ProductApi.price[1]}} % </i>
    <img :src="image" alt="image">
    
    </span>
    </div>
    </div>
  </section>
</template>
<script>
import { ref } from "vue";

export default {
  name: "Social",
  props: {},
  setup() {
    const idProduct = ref(null)
    const ProductApi=ref({
      name:null,
description:null,
price:[],
image:[],
type:[],
models:[],
type:[]

    })
    const FbArea = ref(null);
    const InstArea = ref(null)
    let seePromo =ref(true)

const generatePostInsta=()=>{
   let text  = FbArea.value
   
let keyWirdarray = []
    ProductApi.value.type.map(a=> a.replaceAll("-", " ").replaceAll(',' , " ").replaceAll("." , " ").split(" ").filter(i=> i != " ").map(i=> keyWirdarray =[...keyWirdarray , i]  ))
    ProductApi.value.models.map(e=> e.map(a=> a.replaceAll("-", " ").replaceAll(',' , " ").replaceAll("." , " ").split(" ").filter(i=> i != " ").map(i=> keyWirdarray =[...keyWirdarray , i] )))
ProductApi.value.name.replaceAll("-", " ").replaceAll(',' , " ").replaceAll("." , " ").split(" ").filter(i=> i != " ").map(i=> keyWirdarray =[...keyWirdarray , i] )

  keyWirdarray = keyWirdarray.filter((v, i, a) => a.indexOf(v) === i)
text += "\n \n "
 keyWirdarray.map (i=> text += " #"+ i )

 text += '\n'
 text+= "#falcon #shop #shopping #likeforlike  #instagram #promo #shopping #solg #collection #influance #mode follow4follow"
   
   InstArea.value= text
}
const generatePostFB=() =>{
   let text  = ""
   ProductApi.value.price[1] ? (text  += "⚠️ -"+ ProductApi.value.price[1] +"% : " ) : ( text += " :D ")
text += "👉🏻 "+ ProductApi.value.name +"\n"
text += "Prix : "+ Math.round((ProductApi.value.price[1] ? (ProductApi.value.price[0] *( 1- (ProductApi.value.price[1] /100) )) : ProductApi.value.price[0])*100)/100
 text += "\n 👉🏻commandez le Dès maintient sur: https://falcon-com.web.app?produit="+ idProduct.value
 text += " \n 🛒Découvrez nos nombreux produits et collections ainsi que nos incroyables offres sur : https://falcon-com.web.app"

FbArea.value =text

}
 const searchProduct=()=>{
   console.log('ProductApi:', ProductApi)

fetch("https://falcon-admin.herokuapp.com/api/products/"+idProduct.value)
      .then((res) => res.json())
      .then((result) => {
        ProductApi.value =result;
        if (ProductApi.value.name != null ) {
          generatePostFB()
          generatePostInsta()
        }
        
        });
 }
    const copyInsta = () => {navigator.clipboard.writeText(InstArea.value)};
    const copyFb = () => {navigator.clipboard.writeText(FbArea.value)};
    return {
      idProduct,
ProductApi,
FbArea,
InstArea,
seePromo,

searchProduct,
copyInsta,
copyFb
    };
  },
};
</script>
