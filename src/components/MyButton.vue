<template>
  <v-btn @click="onClick">MyButton</v-btn>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

@Component
export default class MyButton extends Vue {
  private count = 0;

  @Prop()
  public greet?: string;

  @Emit()
  // eslint-disable-next-line @typescript-eslint/no-empty-function
  public click(count: number) {
    this.count = count;
  }

  public onClick() {
    alert(this.greet);
    this.count++;
    this.click(this.count);
  }
}
</script>
