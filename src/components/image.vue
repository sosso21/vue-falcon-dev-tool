<template>
  <section>
    <h2 class="my-2 font-weight-light text-center">Images</h2>
    <form class="input-group">
      <input type="text" class="form-control" v-model="imgInput" /><button
        class="btn btn-lg btn-primary bi bi-upload"
        @click.prevent="addImg"
      ></button>
    </form>

    <div class="my-2 images-small">
      <span v-for="img in imgArray">
        <img :src="img.img" :alt="img.img" @dblclick="deleteImg(img)" />
      </span>
    </div>
    <button class="btn btn-sm btn-outline-warning btn-absolut" @click="copy">copy</button>
    <div class="input-group">
      <textarea
        class="bg-dark text-warning form-control areaImage my-2"
        v-model="imgArea"
        cols="30"
        rows="10"
        id="imageArea"
      ></textarea>
    </div>
  </section>
</template>
<script>
import { ref } from "vue";
export default {
  name: "Image",
  props: {},
  setup() {
    const imgInput = ref(null);
    const imgArea = ref(null);
    const imgArray = ref([]);

    const addImg = () => {
      if (imgInput.value != null) {
        imgArray.value = [
          ...imgArray.value,
          { id: Date.now(), img: imgInput.value },
        ];
        imgInput.value = null;
      }

      let result = "[";
      for (let index = 0; index < imgArray.value.length; index++) {
        const element = imgArray.value[index];
        result += '\n " ' + element.img + '"';
        if (index != imgArray.value.length - 1) {
          result += ",";
        }
      }
      imgArea.value = result + " \n]";
    };
    const deleteImg = (elemnt) => {
      console.log("elemnt:", elemnt);
      imgArray.value = imgArray.value.filter((img) => img.id != elemnt.id);
      addImg();
    };
    const copy =()=>{
navigator.clipboard.writeText(imgArea.value );
    }
    return {
      imgArray,
      imgInput,
      imgArea,

      addImg,
      deleteImg,
      copy
    };
  },
};
</script>
