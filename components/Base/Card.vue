<template>
  <div class="card">
    <h3 v-if="title">
      {{ title }}
    </h3>

    <slot></slot>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: null
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~assets/variables.scss";

.card {
  background: $dark-800;
  border: 1px solid rgba( $dark-600, 0.2 );
  box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  padding: 20px;
  margin: 40px 0;
  overflow: hidden;

  &.disable-padding {
    padding: 0;
  }
}
</style>