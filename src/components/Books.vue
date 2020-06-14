<template>
  <div class="container">

      <form @submit.prevent="addBook">
        <input type="text" autocomplete="off" placeholder="Book title..." v-model="title" name="title">

        <input type="text" autocomplete="off" placeholder="Author..." v-model="author" name="author">

        <label for="read">Have you read this book already?
        <input type="checkbox" id="read-checkbox" name="read" v-model="read">
        </label>
        <button type="submit">Add Book</button>

        <div class="book-cover" v-bind:class="{ faded: (!title && !author) }">
          <div class="content">
            <h1 class="title"> {{ title }} </h1>
            <h2 class="author"> {{ author }} </h2>
          </div>
        </div>
      </form>

<!-- need to use different value for the key value... -->

      <transition-group name="list" tag='div' class="book-shelf" enter-active-class ="animate__animated animate__flipInY" leave-active-class ="animate__animated animate__flipOutY">
        <div v-for="(data, index) in books" :key="index" v-bind:class="{ read: data.read }" class="book-cover">
          <div class="content">
            <h1 class="title"> {{ data.title }} </h1>
            <h2 class="author"> {{ data.author }} </h2>
            <i class="fa fa-minus-circle delete" v-on:click="remove(index)"></i>
          </div>
        </div>
      </transition-group>

  </div>
</template>

<script>
export default {
  name: 'Books',
  data() {
    return {
      title: null,
      author: null,
      read: false,
      books: [
        {'title': 'Adios a las Armas', 'author': 'Ernest Hemingway', 'read': true},
        {'title': 'Don Quixote', 'author': 'Miguel Cervantes', 'read': false},
        {'title': 'End of Faith','author': 'Sam Harris', 'read': true},
        {'title': 'Catch-22','author': 'J.D. Salinger', 'read': false},
      ],
    }
  },
  methods: {
    addBook() {
      this.books.push({title: this.title, author: this.author, read: this.read});
      this.title = null;
      this.author = null;
      this.read = false;
    },
    remove(book) {
      this.books.splice(book,1);
    }
  }
}
</script>

<style scoped>
@import "https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.0.0/animate.min.css";
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

h1 {
  font-size: 24px;
}

h2 {
  font-size: 14px;
}

.container {
  background-color: white;
}

.container form {
  justify-content: center;
  border: 1px solid black;
  height: 270px;
  width: 460px;
  display: grid;
  grid-template-rows: 1fr 1fr 1fr 1fr;
  grid-template-columns: 1fr 1fr;
}

.container form input {
  grid-column-start: 1;
  grid-column-end: 2;
  height: 60%;
}

.container form .book-cover {
  grid-row-start: 1;
  grid-row-end: 5;
  grid-column-start: 2;
  grid-column-end: 3;
}

.faded {
  filter: grayscale(75%);
}

.book-shelf {
  display: flex;
  flex-wrap: wrap;
}

.book-cover {
  display: flex;
  margin: 10px 10px;
  height: 250px;
  width: 200px;
  background-color: #C45457;
  box-shadow: 0px 0px 10px 3px rgba(0,0,0,0.75);
}

.content {
  text-align: center;
  color: #FAF322;
  width: 180px;
  height: 230px;
  border: 2px solid #FAF322;
  margin: auto;
  display: grid;
  grid-template-rows: 3fr 1fr 1fr;
}

.book-cover:hover {
  transform: scale(1.05);
}

.title {
  grid-row-start: 1;
  grid-row-end: 2;
}

.author {
  grid-row-start: 2;
  grid-row-end: 3;
}

i {
  cursor: pointer;
}

i:hover {
  transform: scale(1.05);
}

.delete{
  color: red;
  grid-row-start: 3;
  grid-row-end: 4;
  margin: auto;
}
</style>
