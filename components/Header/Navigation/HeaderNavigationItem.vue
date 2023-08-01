<template>
  <SfLink
    class="sf-header-navigation-item__link"
    :link="link"
  >
    {{ label }}
    <SfIcon
      v-if="hasChildren"
      icon="chevron_down"
      size="xxs"
      color="green-primary"
      viewBox="0 0 24 24"
      :coverage="1"
    />
  </SfLink>
</template>
<script lang="ts">
import { SfLink, SfIcon } from '@storefront-ui/vue';
import { defineComponent } from '@nuxtjs/composition-api';

export default defineComponent({
  name: 'HeaderNavigationItem',
  components: {
    SfLink,
    SfIcon,
  },
  props: {
    label: {
      type: String,
      default: '',
    },
    link: {
      type: [String, Object],
      default: '',
    },
    hasChildren: {
      type: Boolean,
      default: false,
    },
  },
});
</script>
<style lang="scss">
.sf-header-navigation-item__link {
  --header-navigation-item-color: var(--c-light-variant);
  display: flex;
  .sf-icon {
    --icon-color: var(--c-sunset);
    display: inline-flex;
    margin: 0 var(--spacer-xs);
  }
}
</style>
