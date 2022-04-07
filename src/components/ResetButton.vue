<template>
  <button @click="onClick">Reset</button>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

@Component
export default class ResetButton extends Vue {
  @Prop()
  public initialValue!: string;

  @Prop()
  public value!: string;

  @Emit()
  // eslint-disable-next-line @typescript-eslint/no-empty-function
  public input(value: string) {}

  public onClick() {
    this.input(this.initialValue);
  }
}
</script>
