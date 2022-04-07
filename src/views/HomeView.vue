<template>
  <div class="home">
    <p>{{ greetText }}</p>
    <p>
      <MyButton :greet="greetText" @click="onMyButtonClicked"></MyButton>
    </p>
    <p>
      <ResetButton initialValue="Hello" v-model="greetText"></ResetButton>
    </p>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import MyButton from "@/components/MyButton.vue";
import ResetButton from "@/components/ResetButton.vue";

@Component({
  components: {
    MyButton,
    ResetButton,
  },
})
export default class HomeView extends Vue {
  public greetText = "soccer";

  public onMyButtonClicked() {
    this.greetText = "こんにちは";
  }
}
</script>
