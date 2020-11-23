<template>
    <div class="input-group mb-3 create-container">
        <div class="input-group-prepend">
          <button
            @click="addItem(title)"
            type="button" class="btn btn-success"
            id="button-addon1"
          >
            <img src="../assets/add.svg" alt="trash" width="20"
          >
          </button>
        </div>
        <input type="text" class="form-control" placeholder="" v-model="title">
    </div>
</template>

<script>

export default {
    name: 'CreateItemInput',
    data() {
        return {
            title: '',
        }
    },
  methods: {
    addItem(title) {
        this.$emit('addItem', title);
    }
}
}
</script>

<style>
  .filter{
    width: 40%
  }
  .create-container{
    width: 40%;
    margin: 15px 0px 0px 40px
  }
</style>
