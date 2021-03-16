<template>
  <section>
    <h2 class="my-2 font-weight-light text-center">Catégory :</h2>

  <form class="input-group">
      <input type="text" class="form-control" v-model="typeInput"/><button
        class="btn btn-lg btn-primary bi bi-upload"
        @click.prevent="addtoArrayFromInput"
      ></button>
    </form>
<div>
<h3>suggestions : </h3>

    <div class="my-2 images-small">
    <span v-for="type in existingType">
    <button class="btn btn-lg btn-danger "  @click="addRoArrayFromExisting(type)" >{{ type }}</button>
    </span>
    </div>
</div>
<div>
<h3>Sélectionnée : </h3>

    <div class="my-2 images-small">
    <span v-for="type in TypeArray">
    <button class="btn btn-lg btn-dark " @click=" DeleteFromArray(type)" >{{ type }}</button>
    </span>
    </div>
</div>
        <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copy" >copy</button>
    <div class="input-group">
      <textarea
        class="bg-dark text-warning form-control areaImage my-2"
        v-model="TypeArea"
        cols="30"
        rows="10"
      ></textarea>
    </div>
  </section>
</template>
<script>
import { ref } from "vue";

export default {
  name: "Type",
  props: {},
  setup() {

    const typeInput = ref(null);
    const TypeArray = ref([]);
    const TypeArea = ref(null)
const existingType = ref([])

fetch("https://falcon-admin.herokuapp.com/api/products")
      .then((res) => res.json())
      .then((result) => {
          let type = [];
          for (let i = 0; i < result.length; i++) {
            const e = result[i];
            for (let j = 0; j < e.type.length; j++) {
              const t = e.type[j];
              const v = type.includes(t);
              !v && (type = [...type, t]);
            }
          }
          existingType.value = type.sort()
          
        }
      );
      
const  generateArea = ()=>{
    console.log('TypeArray.value.:', TypeArray.value)
    
     TypeArea.value= "["
    for (let index = 0; index < TypeArray.value.length; index++) {
        const element = TypeArray.value[index];
        TypeArea.value+=  '\n "'+element + '"'
        if (TypeArray.value.length-1 != index ) {
            TypeArea.value+= ","
        }
    }
    TypeArea.value += "\n]"

}
const addtoArrayFromInput =()=>{
TypeArray.value =[...TypeArray.value ,typeInput.value] 
typeInput.value =null
generateArea()
}
const addRoArrayFromExisting=(type)=>{
TypeArray.value =[...TypeArray.value ,type] 
generateArea()
}
const DeleteFromArray=(type)=>{
TypeArray.value = TypeArray.value.filter(i=> i !=type)
generateArea()
}
    const copy = () => {navigator.clipboard.writeText(TypeArea.value);};
    return {
        typeInput,
TypeArray,
TypeArea,
existingType,

generateArea,
addtoArrayFromInput,
addRoArrayFromExisting,
DeleteFromArray,
copy,
    };
  },
};
</script>
