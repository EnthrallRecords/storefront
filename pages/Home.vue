<template>
  <div id="home">
    <LazyHydrate when-visible>
      <SfBannerGrid
        :banner-grid="1"
        class="banner-grid"
      >
        <template
          v-for="item in banners"
          #[item.slot]
        >
          <router-link :key="item.slot" :to="item.link" class="sf-link">
            <SfBanner
              :key="item.slot"
              :title="item.title"
              image-tag="nuxt-img"
              :image="item.image"
              :nuxt-img-config="item.imageConfig"
              :class="item.class"
            />
          </router-link>
        </template>
      </SfBannerGrid>
    </LazyHydrate>
  </div>
</template>
<script lang="ts" type="module">
import {
  defineComponent,
  ref,
  useContext,
  onMounted,
  useFetch,
} from '@nuxtjs/composition-api';
import LazyHydrate from 'vue-lazy-hydration';
import { useCache, CacheTagPrefix } from '@vue-storefront/cache';
import { SfBanner, SfBannerGrid } from '@storefront-ui/vue';
import { CmsPage } from '~/modules/GraphQL/types';
import { getMetaInfo } from '~/helpers/getMetaInfo';
import { useContent } from '~/composables';
import LoadWhenVisible from '~/components/utils/LoadWhenVisible.vue';

export default defineComponent({
  name: 'HomePage',
  components: {
    LazyHydrate,
    LoadWhenVisible,
    SfBanner,
    SfBannerGrid,
  },
  // eslint-disable-next-line @typescript-eslint/explicit-module-boundary-types
  setup() {
    const { addTags } = useCache();
    const { loadPage } = useContent();
    const { app } = useContext();
    const year = new Date().getFullYear();
    const { isDesktop } = app.$device;

    const page = ref<CmsPage | null>(null);
    const banners = ref([
      {
        slot: 'banner-A',
        title: 'TAPES',
        description: 'Small form factor, analog audio, something worth listening to.',
        image: '/homepage/bannerCassette.webp',
        imageConfig: {
          fit: 'cover',
          width: isDesktop ? 332 : 328,
          height: isDesktop ? 400 : 343,
          format: 'webp',
        },
        class: 'sf-banner--slim',
        link: 'music%2Ftape'
      },
      {
        slot: 'banner-B',
        title: 'MUSIC',
        description: 'All the stuff worth injecting in your ear holes.',
        image: '/homepage/bannerMusic.webp',
        imageConfig: {
          fit: 'cover',
          width: isDesktop ? 332 : 328,
          height: isDesktop ? 400 : 343,
          format: 'webp',
        },
        class: 'sf-banner--slim',
        link: '/music'
      },
      {
        slot: 'banner-C',
        title: 'MERCH',
        description: 'A bunch of stuff that can be put on shit.',
        image: '/homepage/bannerMerch.webp',
        imageConfig: {
          fit: 'cover',
          width: isDesktop ? 332 : 328,
          height: isDesktop ? 400 : 343,
          format: 'webp',
        },
        class: 'sf-banner--slim',
        link: '/merch'
      }
    ]);

    useFetch(async () => {
      page.value = await loadPage({ identifier: 'home' });
    });

    onMounted(() => {
      addTags([{ prefix: CacheTagPrefix.View, value: 'home' }]);
    });

    // @ts-ignore
    return {
      banners,
      page,
    };
  },
  head() {
    return getMetaInfo(this.page);
  },
});
</script>

<style lang="scss" scoped>

#home {
  box-sizing: border-box;
  padding: 0 var(--spacer-sm);
  @include for-desktop {
    max-width: 1240px;
    padding: 0;
    margin: 0 auto;
  }
}

.banner-grid {
  --banner-container-width: 50%;
  margin: var(--spacer-xl) 0;
  --banner-title-color: var(--c-primary);

  ::v-deep .sf-link {
    text-decoration: none;
  }

  ::v-deep .sf-link:hover {
    color: var(--c-white);
  }

  @include for-desktop {
    margin: var(--spacer-2xl) 0;
    ::v-deep .sf-link {
      --button-width: auto;
    }
  }
}

.banner {
  &__tshirt {
    background-position: left;
  }

  &-central {
    @include for-desktop {
      --banner-container-flex: 0 0 70%;
    }
  }
}

</style>
