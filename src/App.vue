<template>
<div id="app">
  <body>
    <div class="row">
      <img class="col-4" id="logo" alt="Ubiqum logo" src="./assets/ubiqumBookstoreLogo.png" />
      <div class="search-wrapper col=4">
        <input
          id="searchbar"
          v-on:keyup="searchBooks()"
          type="text"
          name="search"
          placeholder="Search books.."
        />
      </div>
    </div>

    <div id="mainDiv" class="mainDiv">
      <div v-for="book in books" :key="book.index">
        <div class="container">
          <div class="wholeCard">
            <div class="frontCard">
              <img :src="book.cover" />
            </div>
            <div class="backCard">
              <h2>{{book.title}}</h2>
              <p>{{book.description}}</p>

              <button>
                More Info
                <a
                  :href="book.detail"
                  data-fancybox
                  data-caption="&lt;b&gt;Single photo&lt;/b&gt;&lt;br /&gt;Caption can contain &lt;em&gt;any&lt;/em&gt; HTML elements"
                ></a>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </body>
</div>
</template>


<script>
// use computed hook

import Books from "@/components/Books.vue";

export default {
  name: "app",
  components: {
    Books
  },
  data() {
    return {
      books: []
    };
  },
  methods: {
    fetchData: function() {
      fetch("https://api.myjson.com/bins/zyv02")
        .then(function(response) {
          console.log(response);
          return response.json();
        })
        .then(data => {
          this.books = data.books;
          console.log(this.books);
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    searchBooks: function() {
      console.log("here");
      let input = document.getElementById("searchbar").value;
      input = input.toLowerCase();
      let searchTitles = document.getElementsByTagName("h2");

      for (let i = 0; i < searchTitles.length; i++) {
        if (!searchTitles[i].innerHTML.toLowerCase().includes(input)) {
          searchTitles[i].style.display = "none";
        } else {
          searchTitles[i].style.display = "block";
        }
      }
    }
  },

  created: function() {
    this.fetchData();
  }
};
</script>


<style>
body {
  padding: 10px;
}

#logo {
  height: 50px;
  width: 240px;
}

.search-wrapper {
  position: relative;
}
label {
  position: absolute;
  font-size: 12px;
  color: rgba(0, 0, 0, 0.5);
  top: 8px;
  left: 12px;
  z-index: -1;
  transition: 0.15s all ease-in-out;
}
input {
  padding: 4px 12px;
  color: rgba(0, 0, 0, 0.7);
  border: 1px solid rgba(0, 0, 0, 0.12);
  transition: 0.15s all ease-in-out;
  background: white;
}

.mainDiv {
  position: relative;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding-bottom: 50px;
}

.container {
  margin-top: 20px;
  background-color: transparent;
  width: 300px;
  height: 480px;
  perspective: 1000px;
}

img {
  width: 300px;
  height: 480px;
  object-fit: cover;
}

.wholeCard {
  width: 100%;
  height: 100%;
  text-align: center;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}

.container:hover .wholeCard {
  transform: rotateY(180deg);
}

.frontCard,
.backCard {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
}

.frontCard {
  background-color: #bbb;
  color: black;
}

.backCard {
  background-color: #2980b9;
  color: white;
  transform: rotateY(180deg);
}
</style>
