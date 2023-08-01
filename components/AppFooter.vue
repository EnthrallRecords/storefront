<template>
  <SfFooter
    :column="4"
    multiple
    class="footer"
  >
    <SfFooterColumn :title="$t('')">
      <SfList>
        <SfListItem v-for="link in orders" :key="link.name">
          <router-link v-if="link.link" :to="link.link" exact>
            <SfMenuItem class="sf-footer__menu-item" :label="$t(link.name)" />
          </router-link>
          <SfMenuItem
            v-else-if="link.clickHandler"
            class="sf-footer__menu-item"
            :label="$t(link.name)"
            @click="link.clickHandler"
          />
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('Info')">
      <SfList>
        <SfListItem>
          <a href="https://www.enthrallrecords.com/privacy-policy/">
            <SfMenuItem
              class="sf-footer__menu-item"
              :label="$t('privacy policy')"
            />
          </a>
        </SfListItem>
      </SfList>
    </SfFooterColumn>
    <SfFooterColumn :title="$t('')"></SfFooterColumn>
    <SfFooterColumn :title="$t('')"></SfFooterColumn>
    <SfFooterColumn>
      <div class="footer__socials">
        <a
          :key="item.name"
          :href="item.link"
          v-for="item in social"
          class="social-icon__link"
        >
          <SfImage
            :key="item.name"
            class="footer__social-image"
            :src="addBasePath('/icons/'+item.name+'.svg')"
            :alt="item.name"
            width="32"
            height="32"
          />
        </a>
      </div>
    </SfFooterColumn>
  </SfFooter>
</template>

<script lang="ts">
import {
  SfFooter, SfList, SfImage, SfMenuItem,
} from '@storefront-ui/vue';
import { defineComponent } from '@nuxtjs/composition-api';
import { addBasePath } from '~/helpers/addBasePath';

export default defineComponent({
  components: {
    SfFooter,
    SfList,
    SfImage,
    SfMenuItem,
  },
  setup() {
    return {
      addBasePath,
    };
  },
  data() {
    return {
      orders: [],
      social: [
        {name: 'facebook', link: 'https://www.facebook.com/enthrallrecords'},
        {name: 'instagram', link: 'https://www.instagram.com/enthrallrecords'},
        {name: 'youtube', link: 'https://www.youtube.com/c/Enthrallrecords/playlists'}
      ],
      isMobile: false,
      desktopMin: 1024,
    };
  },
});
</script>

<style lang="scss">

.footer {
  margin-bottom: 3.75rem;
  @include for-desktop {
    margin-bottom: 0;
  }
  &__socials {
    display: flex;
    justify-content: space-between;
    margin: 0 auto var(--spacer-lg);
    padding: var(--spacer-base) var(--spacer-xl);
    @include for-desktop {
      justify-content: flex-start;
      padding: var(--spacer-xs) 0;
      margin: 0 auto;
    }
  }
  &__social-image {
    margin: 0 var(--spacer-lg) 0 0;
    background-color: #fff;
    border-radius: 100%;
  }
}
.sf-footer {
  @include for-desktop {
    border-top: var(--spacer-xs) solid var(--c-primary);
    padding-bottom: 0;
    margin-top: var(--spacer-2xl);
  }
  &__container {
    margin: var(--spacer-sm);
    @include for-desktop {
      max-width: 69rem;
      margin: 0 auto;
    }
  }
}
</style>
