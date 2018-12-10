<template>
  <div class="container-fluid"  @dragover.stop.prevent="dragover" @drop.stop.prevent="onDrop">
    <div class="row">
      <div class="col-md-5 text-center">
        <div id="dropzone">Glissez l'image ici</div>
        <material-picker v-model="colors" class="mt-4 mx-auto" />
      </div>
      <div class="col-md-7">
        <svg width="100%" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2241 1601">
          <defs>
            <pattern id="smallGrid" width="8" height="8" patternUnits="userSpaceOnUse">
              <path d="M 8 0 L 0 0 0 8" fill="none" stroke="gray" stroke-width="0.5"/>
            </pattern>
            <pattern id="grid" width="80" height="80" patternUnits="userSpaceOnUse">
              <rect width="80" height="80" fill="url(#smallGrid)"/>
              <path d="M 80 0 L 0 0 0 80" fill="none" stroke="gray" stroke-width="1"/>
            </pattern>

            <filter id="f1" x="0%" y="0%" width="100%" height="100%">
            <feColorMatrix id="greyscaler" type="matrix" :values="`${r/(3*x)} ${r/(3*x)} ${r/(3*x)} 0 0 
                                                            ${g/(3*x)} ${g/(3*x)} ${g/(3*x)} 0 0 
                                                            ${b/(3*x)} ${b/(3*x)} ${b/(3*x)} 0 0 
                                                             0    0   0 1 0`"/>
            </filter>
          </defs>

          <rect width="100%" height="100%" fill="url(#grid)" />
          <image :xlink:href="image" v-if="image" filter="url(#f1)" x=40 y=40 width=2160 height=1520 />
        </svg>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { Slider } from 'vue-color';

@Component({
  components: {
    'material-picker': Slider,
  },
})
export default class Colorizer extends Vue {
  colors = {"hsl": {"h":168.4729064039409,"s":0.5,"l":0.5,"a":1},"hex":"#40BFA7","hex8":"#40BFA7FF","rgba":{"r":64,"g":191,"b":167,"a":1},"hsv":{"h":168.4729064039409,"s":0.6666666666666666,"v":0.75,"a":1},"oldHue":168.4729064039409,"source":"hsl","a":1};
  image?: ArrayBuffer | string = "";

  private dragover(evt: DragEvent) {
    evt.dataTransfer.dropEffect = "copy";
  }

  public get x() {
    return 0.5;
  }

  public get r() {
    return this.colors.rgba.r / 255;
  }

  public get g() {
    return this.colors.rgba.g / 255;
  }

  public get b() {
    return this.colors.rgba.b / 255;
  }

  public onDrop(evt: DragEvent) {
    const file = evt.dataTransfer.files[0]; // FileList object.

    console.log("drop", file.name);

    if (!file.type.match("image.*")) {
      console.log("not an image");
      return;
    }

    const reader = new FileReader();

    reader.readAsDataURL(file);
    reader.onload = e => this.image = (e.target as FileReader).result;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

@import "~bootstrap/scss/bootstrap.scss";


#dropzone {
  border: 2px dashed #bbb;
  border-radius: 5px;
  padding: 25px;
  text-align: center;
  font: 20pt bold;
  color: #bbb;
  width: 100%;
}

</style>
