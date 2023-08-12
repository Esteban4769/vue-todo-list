<script>
export default {
  name: 'TodoFilter',
  props: {
    modelValue: String,
  },
  emits: ['update:modelValue'],
  methods: {
    updateFilter(status) {
      this.$emit('update:modelValue', status);
    },
  },
};
</script>

<template>
  <nav class="filter">
    <a
      href="#/"
      class="filter__link"
      :class="{ selected: modelValue === 'all' }"
      @click="updateFilter('all')"
    >
      All
    </a>

    <a
      href="#/active"
      class="filter__link"
      :class="{ selected: modelValue === 'active' }"
      @click="updateFilter('active')"
    >
      Active
    </a>

    <a
      href="#/completed"
      class="filter__link"
      :class="{ selected: modelValue === 'completed' }"
      @click="updateFilter('completed')"
    >
      Completed
    </a>
  </nav>
</template>

<style>
</style>

