<template>
  <FilterButton
    :button-name="$t('metadata')"
    :is-button-active="isButtonActive"
  >
    <div class="metadata-button__badges" v-if="badges.length">
      <FilterBadge
        class="metadata-button__badge"
        :active-badge="activeBadge === badge && isActive"
        v-for="badge in visibleBadges"
        :key="badge.name"
        :text="badge.title"
        @on-click="onClickOnBadge(badge, $event)"
        @on-clear="onClickOnClear(badge, $event)"
      ></FilterBadge>
      <div
        class="metadata-button__badges__collapsed"
        v-if="badges.length > maxVisibleBadges"
        v-click-outside="{
          events: ['mousedown'],
          handler: onClickOutside,
        }"
      >
        <BaseBadge
          :text="`+ ${this.badges.length - this.maxVisibleBadges}`"
          @on-click="toggleTooltip"
        />
        <FilterTooltip v-if="visibleTooltip" class="metadata-button__tooltip">
          <FilterBadge
            class="badge"
            v-for="badge in collapsedBadges"
            :key="badge.name"
            :text="badge.title"
            @on-click="onClickOnBadge(badge, $event)"
            @on-clear="onClickOnClear(badge, $event)"
          ></FilterBadge>
          <BaseButton
            class="secondary full-width small clear metadata-button__tooltip__button"
            @on-click.stop="onClearAll"
            >{{ $t("reset") }}</BaseButton
          >
        </FilterTooltip>
      </div>
    </div>
  </FilterButton>
</template>

<script>
import "assets/icons/filter";
export default {
  props: {
    badges: {
      type: Array,
      default: () => [],
    },
    activeBadge: {
      type: Object,
    },
    maxVisibleBadges: {
      default: 2,
    },
    isActive: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      visibleTooltip: false,
    };
  },
  computed: {
    visibleBadges() {
      return this.badges.slice(0, this.maxVisibleBadges);
    },
    collapsedBadges() {
      return this.badges.slice(this.maxVisibleBadges, this.badges.length);
    },
    isButtonActive() {
      return this.isActive || !!this.badges.length;
    },
  },
  methods: {
    onClickOnBadge(badge, e) {
      e.stopPropagation();

      this.$emit("click-on-badge", badge, e);
      this.visibleTooltip = false;
    },
    onClickOnClear(badge, e) {
      e.stopPropagation();

      this.$emit("click-on-clear", badge, e);
    },
    onClearAll() {
      this.$emit("click-on-clear-all", this.badges);
    },
    toggleTooltip(e) {
      e.stopPropagation();
      this.visibleTooltip = !this.visibleTooltip;
    },
    onClickOutside() {
      this.visibleTooltip = false;
    },
  },
};
</script>
<style lang="scss" scoped>
.metadata-button {
  &__badges {
    display: flex;
    gap: calc($base-space / 2);
    &__collapsed {
      position: relative;
      max-height: 22px;
    }
  }
  &__badge :deep(.badge__text) {
    max-width: 140px;
  }
  &__tooltip {
    display: flex;
    flex-direction: column;
    gap: calc($base-space / 2);
    .badge {
      margin-right: auto;
    }
    &__button {
      margin: $base-space auto 0 auto;
    }
  }
}
</style>
