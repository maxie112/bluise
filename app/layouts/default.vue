<template>
  <main :class="[$route.name]" class="main px-4 md:px-0">
    <div class="container mx-auto">
      <site-header />
      <nuxt class="nuxt" />
    </div>

    <svg class="divider -mx-4 sm:mx-0" width="100%" viewBox="0 0 10 1">
      <polygon stroke="none" stroke-width="0" fill="#0b3765" points="0,0.6 0,1 10,1 10,0" />
      <text x="10" y="0.8" font-size="0.8" fill="#fff" dominant-baseline="middle" text-anchor="end">GOREACH</text>
      <text x="10" y="1.6" font-size="0.8" fill="#fff" dominant-baseline="middle" text-anchor="end">VAN NELLEWEG 1</text>
      <text x="10" y="2.4" font-size="0.8" fill="#fff" dominant-baseline="middle" text-anchor="end">3044BC ROTTERDAM</text>
      <text x="10" y="3.2" font-size="0.8" fill="#fff" dominant-baseline="middle" text-anchor="end">NEDERLAND</text>
    </svg>

    <site-footer />
  </main>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import SiteHeader from '@/components/partials/header.vue';
import SiteFooter from '@/components/partials/footer.vue';

@Component({
  components: {
    SiteHeader,
    SiteFooter,
  },
})
export default class DefaultLayout extends Vue {}
</script>

<style lang="scss">
.divider {
  position: relative;
  min-width: 100vw;
  margin-top: auto;
}

.nuxt {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}
</style>
