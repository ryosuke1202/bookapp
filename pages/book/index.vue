<template>
    <div>
      book/index
      {{ books }}
    </div>
</template>

<script>
export default {
    props: {
        books: {
            type: Array,
            default: () => {}
        }
    },
}
</script>

<style>

</style>