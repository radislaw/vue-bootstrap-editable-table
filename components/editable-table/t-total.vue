<template>
  <div class="tr table-total">
    <t-data v-for="field in fields" :key="field.name" :grow="field.grow">
      <b v-if="field.name === 'name'">Total</b>

      <div v-else-if="hasAggregate(field)">
        {{formatValue(value[field.name])}}

        <b-dropdown
          size="sm"
          variant="link"
          toggle-class="text-decoration-none"
          no-caret
        >
          <template v-slot:button-content>
            <i>{{'['+field.aggregate+']'}}</i>
          </template>

          <b-dropdown-item href="#" @click="onChange('sum', field.name)">sum</b-dropdown-item>
          <b-dropdown-item href="#" @click="onChange('mean', field.name)">mean</b-dropdown-item>
          <b-dropdown-item href="#" @click="onChange('median', field.name)">median</b-dropdown-item>
          <b-dropdown-item href="#" @click="onChange('min', field.name)">min</b-dropdown-item>
          <b-dropdown-item href="#" @click="onChange('max', field.name)">max</b-dropdown-item>
          <b-dropdown-item href="#" @click="onChange('count', field.name)">count</b-dropdown-item>
        </b-dropdown>
      </div>
    </t-data>

    <t-data empty></t-data>
  </div>
</template>

<script>
import tData from './t-data.vue'
import { isUndefinedOrNullOrEmpty, isObject, formatFloat } from '~/helpers'

export default {
  name: 't-total',
  components: {
    tData
  },
  props: {
    fields: { type: Array, required: true },
    value: { type: Object, required: true }
  },
  methods: {
    hasAggregate(field) {
      return (
        !isUndefinedOrNullOrEmpty(field.aggregate) &&
        !isUndefinedOrNullOrEmpty(this.value) &&
        isObject(this.value) &&
        !isUndefinedOrNullOrEmpty(this.value[field.name])
      );
    },
    formatValue(value) {
      return formatFloat(value);
    },
    onChange(func, fieldName) {
      this.$emit('change-aggregating', {
        aggregation: func,
        fieldName
      });
    }
  }
}
</script>

<style>
  .table-total {
    min-height: 2rem;
  }

  .table-total .dropdown {
    vertical-align: baseline;
  }

  .table-total .dropdown>button {
    padding: 1px;
  }
</style>
