<template>
  <div class="vert-search">
    <div class="">
      <span name="search-box-fade">
        <div>
          <div class="row">
            <!-- <div xs12>
              <div class="text-xs-left py-2 headline">
                <span class>Search</span>
              </div>
            </div> -->
            <div v-for="fieldDetails in orderedSearchFields" :key="fieldDetails.fieldName"
              class="col-sm-12 col-xs-12 q-py-md">
              <SelectField @selectChanged="triggerSearchUpdate" :fieldDetails="fieldDetails"
                :currentFieldValue="routeParams[fieldDetails.queryStringName]"
                :currentMinPriceValue="currentMinPriceValue"></SelectField>
            </div>
          </div>
        </div>
      </span>
    </div>
  </div>
</template>
<script>
import SelectField from 'components/fields/SelectField.vue'
import useSearchFields from 'src/compose/useSearchFields.js'
import { useRoute } from 'vue-router'
export default {
  components: {
    SelectField,
  },
  setup() {
    const { getSearchFields } = useSearchFields()
    const route = useRoute()
    let saleOrRental = 'rental'
    if (route.name === 'rForSaleSearch') {
      saleOrRental = 'sale'
    }
    return {
      searchFields: getSearchFields(saleOrRental),
    }
  },

  props: {
    currentSearchFieldsParams: {
      type: Object,
      default() {
        return {}
      },
    },
    routeParams: {
      type: Object,
      default() {
        return {}
      },
    },
    isLoading: {
      type: Boolean,
    },
    isMobileModal: {
      type: Boolean,
    },
  },
  methods: {
    triggerSearchUpdate(fieldDetails, newValue) {
      if (fieldDetails.fieldName === 'minPrice') {
        this.newCurrentMinPriceValue = newValue
      }
      // up the chain
      fieldDetails.newValue = newValue
      this.$emit('triggerSearchUpdate', fieldDetails)
    },
  },
  computed: {
    currentMinPriceValue() {
      return (
        this.newCurrentMinPriceValue ||
        this.currentSearchFieldsParams.price_min
      )
    },
    orderedSearchFields() {
      const searchFields = this.searchFields || []
      const sortedFields = searchFields.sort(
        (a, b) => a.sort_order - b.sort_order
      )
      return sortedFields
    },
  },
  data: () => ({
    newCurrentMinPriceValue: null,
  }),
  mounted: function () { },
}
</script>
<style scoped>
</style>
