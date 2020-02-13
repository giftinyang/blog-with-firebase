<template>
  <div id="add">
    <div v-if="show">
        <h1>Add a New Blog Post</h1>
        <form>
            <label> Blog Title:</label>
            <input type="text" v-model.lazy="blogs.title" required />
            <label> Blog Content:</label>
            <textarea v-model.lazy="blogs.content"></textarea>
            <label>Author:</label>
            <select v-model="blogs.author">
                <option v-for="author in authors" :key="author">{{author}}</option>
                
            </select>
            <div>
            <button @click.prevent="submitPost">submit</button>
            </div>
        </form>
    </div>
    <div v-if="hide">
        <button @click="togglePost">Add a New Blog Post</button>
        <div id="preview" v-for="(blog, index) in blogPost" :key="index">
            <h3>Preview Blog</h3>
            <p>Blog title: {{ blog.title }}</p>
            <p>Blog content:</p>
            <p>{{ blog.content }}</p>
            <p>{{blog.author}}</p>
            <button @click="deletePost(blog.id)">Delete</button>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import sweetalert from 'sweetalert'
import swal from 'sweetalert'

export default {
    data() {
        return{
            blogs:{
            title:"",
            content:"",
            author: ""
            },
            blogPost: [],

            authors:['The Net Ninja', 'The Angular Avenger', 'The Vue Vindicator'],
            show: false,
            hide: true,
        }
    },
    methods:{
        submitPost(){
            this.blogPost.push(this.blogs);
            axios.post('https://blog-firebase-34205.firebaseio.com/data.json', this.blogs)
            swal({
                title: "Good job!",
                text: "You clicked the button!",
                icon: "success",
                button: "Aww yiss!",
            })
            .then(function(res){
                console.log(res)
            }, function(error){
                console.log(error)
            })
            this. blogs = {
                title:"",
                content:"",
                author: ""
            }
            this.show = false
            this.hide = true
        },
        togglePost(){
            this.show = true
            this.hide = false
        },
        deletePost(i){
            console.log(i)
            axios.delete(`https://blog-firebase-34205.firebaseio.com/data/${i}.json`)
            .then(function(res){
                console.log(res)
            }, function(error){
                console.log(error)
            })
        }
    },
    created(){
        axios.get('https://blog-firebase-34205.firebaseio.com/data.json')
        .then(res => {
            const data = res.data;
            for (let key in data){
                const post = data[key]
                post.id = key
                this.blogPost.unshift(post)
            }
        }).catch(error => {
            console.log(error);
        })
    }
  
}
</script>

<style>
#add *{
    box-sizing: border-box;

}
#add{
    margin: 20px auto;
    max-width: 500px;
}
label{
    display: block;
    margin: 20px 0 10px;
}
input[type="text"], textarea{
    display: block;
    width: 100%;
    padding: 8px;
}
#preview{
    padding: 10px 20px;
    border: 1px dotted #ccc;
    margin: 30px 0;
}
h3{
    margin-top: 10px;
}
#checkboxes input{
    display: inline-block;
    margin-right: 10px;
}
#checkboxes label{
    display: inline-block
}
 
</style>
