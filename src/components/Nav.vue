<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
      <a @click="changeonglet('arrays')" class="navbar-brand" href="#"
        >Tableaux</a
      >
      <a @click="changeonglet('social')" class="navbar-brand" href="#"
        >Social</a
      >
    </div>
  </nav>
</template>

<script>
export default {
  name: "Nav",
  props: {},
  setup(props,ctx) {
    const changeonglet = (onglet) => {
      
        ctx.emit("change",onglet)
    };

    return {
      changeonglet
    };
  },
};
</script>
