<template>
  <div class="description">
    <h2
      class="--heading5 --medium description__title"
      v-text="'Annotation guidelines'"
    />
    <RenderMarkdownBaseComponent
      class="--body1 description__text"
      :class="{ '--light': isColorLight }"
      :markdown="sanitizedDescription"
    />
  </div>
</template>

<script>
export default {
  props: {
    guidelines: {
      type: String,
      required: true,
    },
    isColorLight: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    sanitizedDescription() {
      return this.guidelines ?? "";
    },
  },
};
</script>

<style lang="scss" scoped>
.description {
  &__text {
    white-space: pre-wrap;
    color: $black-87;

    &.--light {
      color: $black-37;
    }
  }
}
</style>
