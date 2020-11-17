<template>
    <nav>
      <ul class="pagination">
        <li class="page-item">
          <button
            class="page-link"
            :disabled="pageNumber === 0" 
            @click="changePage(null, 'prev')"
          >
            Previous
          </button>
        </li>
        <div class="page-link page-number">{{pageNumber + 1}}</div>
        <li class="page-item">
          <button 
            class="page-link" 
            :disabled="pageNumber >= pageCount -1" 
            @click="changePage(null, 'next')"
          >
            Next
          </button>
        </li>
      </ul>
    </nav>
</template>

<script>
export default {
  name: "pagination",
  props: {
    pageNumber: Number,
    pageCount: Number,
  },
  methods: {
    changePage(e, mode){
      this.$emit('changePage', { mode } );
    },
  }
}
</script>

<style scoped>
  .pagination{
    margin: 45px 0px 0px 40px;
  }
  .page-number{
    margin: 0px 10px;
    color:rgb(30, 30, 102);
    font-weight: 900;
  }
</style>
