<template>
  <section class="my-4">
    <h2 class="my-2 font-weight-light text-center">Modeles</h2>
    <form class="input-group my-1" v-for="input in Modelinput"  >
      <input
        type="text"
        class="form-control"
        :value="input.content"
        @change="inputContent($event, input)"
      />
      <button class="btn btn-lg btn-primary bi bi-upload" @click.prevent="pushModal(input.id)"></button>
      
    </form>
    <button
      @click="addNewArrayModel"
      class="bi bi-folder-plus btn ttn-lg btn-outline-primary"
    ></button>

    <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copy" >copy</button>
    <div class="input-group">
      <textarea
        v-model="ModelArea"
        class="bg-dark text-warning form-control areaImage my-2"
        cols="30"
        rows="10"
        id="ModelArea"
      ></textarea>
    </div>
  </section>
</template>
<script>
import { ref } from "vue";
export default {
  name: "Models",
  props: {},
  setup() {
    const Modelinput = ref([{ id: Date.now(), content:null }]);
    const ModelArea = ref(null);
    const modelArray = ref([{ id: Modelinput.value[0].id, content: [] }]);

    const inputContent = (e, input) => {
      console.log('e:', e.target.value)
      for (let index = 0; index < Modelinput.value.length; index++) {
        const element = Modelinput.value[index];
        if (element.id == input.id) {
          Modelinput.value[index].content = e.target.value;
          return Modelinput.value[index].content;
        }
      }
    };
    
    const addNewArrayModel = () => {
      const id = Date.now();
      Modelinput.value = [...Modelinput.value, { id: id, content: "" }];
      modelArray.value = [...modelArray.value, { id: id, content: [] }];
    };
    const pushModal =(id)=>{
       console.log('id:', id)
       
       
 ModelArea.value  = "["
 
 for (let index = 0; index < modelArray.value .length; index++) {
   const element = modelArray.value [index];
   
   
   if(element.id == id){
     
     modelArray.value[index].content= [... modelArray.value[index].content , Modelinput.value[index].content]
     Modelinput.value[index].content = null;
   }
   
     console.log(' modelArray.value:',  modelArray.value)
 ModelArea.value  += " \n ["
 
 for (let i = 0; i < element.content.length; i++) {
   const el = element.content[i];
    ModelArea.value += '\n  "' + el + '"'
    if (element.content.length != i+1) {
        ModelArea.value += ","
    }
   
 }
      ModelArea.value += "\n ]"
      
    if (modelArray.value .length != index+1) {
        ModelArea.value += ","
}
 }
 
      ModelArea.value += "\n]"
    }
    
    const copy =()=>{
navigator.clipboard.writeText(ModelArea.value );
    }
    return {
      Modelinput,
      ModelArea,
      modelArray,
 
      inputContent,
      addNewArrayModel,
      pushModal,
      copy
    };
  },
};
</script>
