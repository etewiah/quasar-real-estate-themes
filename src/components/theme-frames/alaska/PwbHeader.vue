<template>
  <q-scroll-observer @scroll="onScroll" />
  <q-toolbar
    :class="
      ((scrollInfo && scrollInfo.position && scrollInfo.position.top) || 0) > 130
        ? 'fixed-top delayed_toolbar'
        : 'mega-header-hidden'
    "
    class="text-white q-pl-xl"
    style="height: 90px; z-index: 2"
  >
    <q-toolbar-title :class="$q.platform.is.mobile ? 'text-center' : ''">
      PWB
      <!-- <q-img class="q-ma-lg q-ml-xl" style="width: 140px" :src="subHeaderLogoUrl" /> -->
    </q-toolbar-title>
    <div>
      <q-btn
        flat
        dense
        round
        icon="menu"
        aria-label="Menu"
        class="q-mr-lg"
        @click="menu = true"
      />
      <q-btn class="q-mr-lg" color="white" flat :label="selected_lang">
        <q-menu>
          <q-list>
            <q-item clickable v-close-popup @click="selected_lang = 'EN'">
              <q-item-section>EN</q-item-section>
            </q-item>
            <q-item clickable v-close-popup @click="selected_lang = 'ES'">
              <q-item-section>ES</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
      </q-btn>
    </div>
  </q-toolbar>
  <!-- -->
</template>
<script>
import { defineComponent, ref } from "vue"
// import subHeaderLogoUrl from "assets/images/llm/logo-180x180-bg-black.jpeg"
export default defineComponent({
  name: "PwbHeader",
  // inject: ["sitedetailsProvider"],
  setup() {
    const scrollInfo = ref({})
    return {
      menu: ref(false),
      slide: ref(1),
      featured_slide: ref(1),
      slide_exclusive_dev: ref(1),
      type: ref(null),
      accept: ref(false),
      selected_lang: ref("EN"),
      scrollInfo,
      onScroll(info) {
        scrollInfo.value = info
      },
    }
  },
  created() {
    // // this.subHeaderLogoUrl = subHeaderLogoUrl
  },
  data() {
    return {}
  },
  props: {
    thm: {
      type: Object,
      default() {
        return {}
      },
    },
  },
  computed: {
    langNavs() {
      const supportedLocales = this.sitedetailsProvider.state.supportedLocales || []
      // ["en", "es"]
      const langNavs = []
      supportedLocales.forEach((supportedLocale) => {
        const shortLocale = supportedLocale.split("-")[0]
        langNavs.push({
          shortLocale,
          route: {
            params: {
              publicLocale: shortLocale,
            },
          },
        })
      })
      return langNavs
    },
    // topNavLinks() {
    //   return this.sitedetailsProvider.state.topNavLinkItems
    // },
  },
})
</script>
<style scoped>
.display-none {
  display: none;
}
.q-page-sticky--expand {
  z-index: 7000 !important;
}

.telephone {
  font-weight: 600;
  font-size: 1.25rem;
  line-height: 1.2;
}

.height_330 {
  height: 330px;
}

.height_180 {
  height: 180px;
}

.height_140 {
  height: 140px;
}

.custom_margin {
  margin: 50px 0 50px 0;
}

.custom_border {
  border: 1px solid black;
}

.circle {
  background-color: #fff;
  border: 2px solid #505050;
  height: 120px;
  border-radius: 50%;
  -moz-border-radius: 50%;
  -webkit-border-radius: 50%;
  width: 120px;
}

.top_triangle {
  width: 100%;
  border-bottom: 40px solid #2a2a2a;
  border-left: 97vw solid transparent;
  margin-bottom: -1px;
}

.bottom_triangle {
  width: 100%;
  border-top: 40px solid #2a2a2a;
  border-left: 97vw solid transparent;
  margin-top: -1px;
}

* {
  margin: 0;
  padding: 0;
}

.delayed_toolbar {
  background-color: rgba(1, 1, 1, 0.75);
  height: 90px;
}

.pic-wrapper {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

/* .main-pwb-header {
  background-color: var(--q-pwb-primary);
  color: var(--q-pwb-primary-contrast);
}

.q-item.q-router-link--active,
.q-item--active {
  color: black
}

.pwb-header-tabs {
  margin-bottom: -1px;
} */
</style>
