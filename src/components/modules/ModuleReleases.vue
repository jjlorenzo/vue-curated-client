<template>
  <div class="module-releases">
    <release v-for="release of releases" :key="release.id" :data="release"></release>

    <VueLoadingIndicator
      v-if="loading"
      class="overlay primary big"
    />
  </div>
</template>

<script>
import RELEASES_QUERY from 'graphql/ModuleReleases.gql'

import Release from './ModuleRelease.vue'

export default {
  components: {
    Release,
  },

  props: {
    moduleId: {
      type: String,
      required: true,
    },
  },

  data () {
    return {
      loading: 0,
      releases: [],
    }
  },

  apollo: {
    releases: {
      query: RELEASES_QUERY,
      variables () {
        return {
          id: this.moduleId,
        }
      },
      update: ({ module }) => {
        return module &&
          module.releases
      },
      loadingKey: 'loading',
    },
  },
}
</script>

<style lang="stylus" scoped>
@import "~style/imports";

.module-releases {
  position: relative;
  min-height: 400px;
}
</style>
