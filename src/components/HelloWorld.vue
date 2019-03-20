<template>
  <div>
    <h1>Pagination in vuejs</h1>
   <div class="album py-5 bg-light">
        <div class="container" v-cloak>
          <div class="row">
            <div class="col-md-4" v-for="post in displayData" v-bind:key="post.name">
              <div class="card mb-4 box-shadow post-cards">
                <div class="card-body">
                    <h5>{{post.name}}</h5>
                  <p class="card-text">{{post.name}}</p>
                </div>
              </div>
            </div>
          </div>
          
          <div class="col-md-12">
            <button type="button" class="btn btn-sm btn-outline-secondary" v-if="page != 1" @click="page--"><<</button>
            <button type="button" class="btn btn-sm btn-outline-secondary" v-for="pageNumber in pages.slice(page-1, page+5)" @click="page = pageNumber" > {{pageNumber}} </button>
            <button type="button" @click="page++" v-if="page < pages.length" class="btn btn-sm btn-outline-secondary"> >> </button>
          </div>
        </div>
   </div>
   <div>Developed by <a href="http://allenshaji.me">Allen Shaji Pulthakidiyel</a></div>
  </div>
</template>
<script lang="js">
import axios from 'axios'
export default {
  name: 'news',
  data () {
    return {
       posts: [],
        baseUrl: 'http://18.191.40.18/',
        page: 1,
        perPage: 3,
        pages: [],
    }
  },
   mounted () {
        this.getPosts();
    },
     computed: {
        displayData () {
            return this.pagination(this.posts);
        }
    },
    watch: {
        posts () {
            this.PageSet();
        }
    },
    methods: {
        getPosts () {
            axios.get(this.baseUrl+'snake/all')
            .then(response => {
                this.posts = response.data.data;
            })
            .catch(response => {
                console.log(response);
            });
        },
        PageSet () {
            var numberOfPages = Math.ceil(this.posts.length / this.perPage);
            for (let index = 1; index <= numberOfPages; index++) {
                this.pages.push(index);
            }
        },
        pagination (posts) {
            var page = this.page;
            var perPage = this.perPage;
            var from = (page * perPage) - perPage;
            var to = (page * perPage);
            return  posts.slice(from, to);
        },
    },
}
</script>
