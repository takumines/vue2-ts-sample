<template>
  <v-btn @click="onClick">Reset</v-btn>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator";

@Component
export default class ResetButton extends Vue {
  public initialValue!: string;

  @Prop()
  public value!: string;

  public created() {
    this.initialValue = this.value;
  }

  @Emit()
  // eslint-disable-next-line @typescript-eslint/no-empty-function
  public input(value: string) {
    this.value = value;
  }

  public onClick() {
    this.input(this.initialValue);
  }
}
</script>
