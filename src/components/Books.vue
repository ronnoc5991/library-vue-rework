<template>
  <div class="hello">
    <div class="holder">

      <form @submit.prevent="addBook">
        <input type="text" autocomplete="off" placeholder="Enter a new Book.." v-model='book' v-validate="'min:5'" name="book">
        
        <transition enter-active-class="animate__animated animate__flipInX" leave-active-class="animate__animated animate__flipOutX">
          <p class="alert" v-if="errors.has('book')">{{ errors.first('book') }}</p>
        </transition>

        <input type="checkbox" id="read-checkbox" v-model="read">
      </form>

      <ul>
        <transition-group name="list" enter-active-class ="animate__animated animate__backInLeft">
          <li v-for="(data, index) in books" :key='index' v-bind:class="{ read: data.read }">
            {{ data.book }} 
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>
        </transition-group>
      </ul>

    </div>
  </div>
</template>

<script>
export default {
  name: 'Books',
  data() {
    return {
      read: false,
      book: '',
      books: [
        {'book': 'Adios a las Armas', 'read': true},
        {'book': 'Don Quixote', 'read': false},
        {'book': 'End of Faith', 'read': true},
        {'book': 'Catch-22', 'read': false},
      ],
    }
  },
  methods: {
    addBook() {
      this.$validator.validateAll().then((result) => {
        if (result) {
          this.books.push({book: this.book, read: this.read});
          this.book = '';
          this.checked = false;
        } else {
          console.log('Not valid');
        }
      })
    },
    remove(id) {
      this.books.splice(id,1);
    }
  }
}
</script>

<style scoped>
@import "https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.0.0/animate.min.css";
@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

.alert {
  background-color: yellow;
  width: 100%;
  height: 30px;
}

ul {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}

li {
  text-align: center;
  list-style-type: none;
  margin: 10px 10px;
  height: 150px;
  width: 120px;
  background-color: orange;
}

.read {
  background-color: red;
}
</style>
