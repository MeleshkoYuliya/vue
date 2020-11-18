<template>
    <div class="input-group flex-nowrap filter">
        <div class="input-group-prepend">
            <button class="btn btn-dark" id="addon-wrapping">
            <img src="../assets/filter.svg" alt="filter" width="20">
            </button>
        </div>
        <input
            type="text"
            class="form-control"
            placeholder="Find todo"
            v-model="internalValue"
            @input="$emit('input', internalValue)"
        >
    </div>
</template>

<script>

export default {
    name: 'FilterInput',
    data() {
        return {
            internalValue: '',
        }
    },
    watch: {
        value() {
            this.internalValue = this.value;
        }
  }
}
</script>

<style>
  .filter{
    width: 40%
  }
</style>
