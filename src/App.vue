<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>VueJs + Firebase</h1>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add a book</h3>

      </div>
        <div class="panel-body">
          <form id="form" class="form-inline" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
            </div>
            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
            </div>
            <div class="form-group">
              <label for="bookUrl">URL:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newBook.url">
            </div>
            <br/> <br/>
            <input id="butn" type="submit" class="btn btn-primary" value="Add Book">
          </form>
        </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Books List</h3>


      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>
                Title
              </th>
              <th>
                Author 
                {{logd()}}
              </th>
              <th>
                Delete
              </th>
            </tr>
            
          </thead>
          <tbody>
            <tr v-for="book in books" :key="book.id">
              <td>
                <a v-bind:href="book.url">{{book.title}}</a>
              </td>
              <td>
                {{book.author}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeBook(book)"></span>
              </td>

            </tr>
          </tbody>
        </table>

      </div>

    </div>
  </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
    apiKey: "AIzaSyCUDqFwjVacIiMd_FL38gGclAWn0dq4LVk",
    authDomain: "vuejs-firebase-02-27975.firebaseapp.com",
    databaseURL: "https://vuejs-firebase-02-27975.firebaseio.com",
    projectId: "vuejs-firebase-02-27975",
    storageBucket: "vuejs-firebase-02-27975.appspot.com",
    messagingSenderId: "845954562794"
}

let app = Firebase.initializeApp(config);
let db = app.database();
let bookRef = db.ref('books');

export default {
  name: 'app',
  firebase: {
    books: bookRef
  },
  data(e){
    return {
      newBook: {
        title: null,
        author: null,
        url: null
      }
    }
    e.preventDefault();
  },
  methods: {
    logd(){
      console.log(bookRef);
    },
    addBook(){
      if (!(this.newBook.url.includes("https://"))) 
      {
        if (!(this.newBook.url.includes("http://"))) {
          this.newBook.url = "https://" + this.newBook.url;
        }
      }

      bookRef.push(this.newBook);
      this.newBook = {
        title: null,
        author: null,
        url: null
      }
    },
    removeBook(b){
      bookRef.child(b['.key']).remove();
      toastr.success("Book removed");
    }
  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}
tbody span:hover {
  color:darkred;
}


</style>
