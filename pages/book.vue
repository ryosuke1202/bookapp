<template>
  <div>
    <NuxtChild 
    :books="books"
    @add-book-list="addBook"
    @update-book-info="updateBookInfo"
    />
  </div>
</template>

<script>
const STORAGE_KEY = 'books'
export default {
    data() {
        return{
            books: [],
            newBook: null
        }
    },
    created() {
        if (localStorage.getItem(STORAGE_KEY)) {
        try {
            this.books = JSON.parse(localStorage.getItem(STORAGE_KEY));
        } catch(e) {
            // localStorage.removeItem(STORAGE_KEY);
        }
        }
    },
    methods: {
        addBook(e) { // 引数eで子供コンポーネントからの値を受け取れる
            this.books.push({
                id: this.books.length,
                title: e.title,
                image: e.image,
                description: e.description,
                readDate: '',
                memo: ''
            });
            this.saveBooks();
            this.goToEditPage(this.books.slice(-1)[0].id); // 引数には最後に追加したデータのid
        },
        removeBook(x) {
            this.books.splice(x, 1);
            this.saveBooks();
        },
        saveBooks() {
            const parsed = JSON.stringify(this.books);
            localStorage.setItem(STORAGE_KEY, parsed);
        },
        goToEditPage(id) {
            this.$router.push(`/book/edit/${id}`)// vue-routerの機能でページ遷移
        },
        updateBookInfo(e) {
            const updateInfo = {
                id: e.id,
                readDate: e.readDate,
                memo: e.memo,
                title: this.books[e.id].title,
                image: this.books[e.id].image,
                description: this.books[e.id].description,
            }
            this.books.splice(e.id, 1, updateInfo);
            this.saveBooks();
            this.$router.push('/book');
        }
    }
}
</script>

<style>

</style>