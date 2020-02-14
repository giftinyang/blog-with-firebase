<template>
  <div id="add">
    <div v-if="show">
        <h1>Add a New Blog Post</h1>
        <form>
            <label> Blog Title:</label>
            <input type="text" v-model.lazy="blogs.title" required />
            <label> Blog Content:</label>
            <textarea v-model.lazy="blogs.content"></textarea>
            <label>Category:</label>
            <select v-model="blogs.author">
                <option v-for="category in categories" :key="category">{{category}}</option>
                
            </select>
            <div>
            <button @click.prevent="submitPost">submit</button>
            </div>
        </form>
    </div>
    <div v-if="hide">
        <button @click="togglePost">Add a New Blog Post</button>
        <div id="preview" v-for="(blog, index) in blogPost" :key="index">
            <div v-if="display">
                <h3>Preview Blog</h3>
                <p>Blog title: {{ blog.title }}</p>
                <p>Blog content:</p>
                <p>{{ blog.content }}</p>
                <p>{{blog.category}}</p>
                <button @click="deletePost(blog.id)">Delete</button>
                <button @click="editPost(blog)">Edit</button>
            </div>
             <div v-else>
                 <form @submit.prevent>
                    <label> Blog Title:</label>
                    <input type="text" v-model.lazy="blogs.title" required />
                    <label> Blog Content:</label>
                    <textarea v-model.lazy="blogs.content"></textarea>
                    <label>Category:</label>
                    <select v-model="blogs.author">
                        <option v-for="category in categories" :key="category">{{category}}</option>
                        
                    </select>
                    <div>
                    <button @click.prevent="updatePost(blog.id)">Update</button>
                    </div>
                </form>
             </div>
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

            categories:['Personal', 'Work', 'Education', 'Social', 'Sport',],
            show: false,
            hide: true,
            display: true
        }
    },
    methods:{
        submitPost(){
            if(this.blogs.title === ''|| this.blogs.content === ''|| this.blogs.author === '' ){
               swal("Error!", "Please fill in all fields!", "error");
            }else{
                this.blogPost.push(this.blogs);
                            axios.post('https://blog-firebase-34205.firebaseio.com/data.json', this.blogs)
                            swal({
                                title: "Good job!",
                                text: "Your story has been successfully added!",
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
            }
        },
        editPost(id){
            this.display = false
            this.blogs = id
        },
        togglePost(){
            this.show = true
            this.hide = false
        },
        deletePost(i){
            console.log(i)
            axios.delete(`https://blog-firebase-34205.firebaseio.com/data/${i}.json`)
            swal({
                    title: "Are you sure?",
                    text: "Once deleted, you will not be able to recover this imaginary file!",
                    icon: "warning",
                    buttons: true,
                    dangerMode: true,
                    })
                    .then((willDelete) => {
                    if (willDelete) {
                        swal("Poof! Your imaginary file has been deleted!", {
                        icon: "success",
                        });
                    } else {
                        swal("Your imaginary file is safe!");
                    }
                    })
            .then(function(res){
                console.log(res)
            }, function(error){
                console.log(error)
            })
        },
        updatePost(i){
            axios.put(`https://blog-firebase-34205.firebaseio.com/data/${i}.json` , this.blogs)
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
