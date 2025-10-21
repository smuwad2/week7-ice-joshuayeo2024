<script>
import axios from 'axios';
    export default { 
        data(){
      return {
        emotions: ['Happy', 'Sad', 'Angry'],
        selEmotion: "",
        subject: "",
        entry: "",
        outputMessage: ""
      }
    },
       computed: {
        baseUrl() {
            if (window.location.hostname=='localhost')
                return 'http://localhost:3000' 
            else {
                const codespace_host = window.location.hostname.replace('5173', '3000')
                return `https://${codespace_host}`;
            }
        } 
    },
    methods: {
            addPost() {
                axios.get(`${this.baseUrl}/addPost`,{
                    params: { 'subject': this.subject, 'entry': this.entry, 'mood': this.selEmotion }
                })
                .then(response =>{
                    this.outputMessage = response.data.message;
                })
                .catch(error=>{
                    console.log(error)
                })
            }
        }

    }
</script>

<template>
    <div class="table m-2">
        <h3>Add a New Blog Post</h3>

        Subject: <input type='text' size='30' v-model='subject' required>
        <br>

        Entry: <br>
        <textarea name='entry' cols='80' rows='5' v-model='entry' required></textarea>
        <br>

        Mood:
        <!-- TODO: Build a dropdown list here for selecting the mood -->
         <select v-model="selEmotion">
            <option :value="emotion" v-for="emotion of emotions">{{ emotion }}</option>
         </select>

        <br>

        <br>
        <button @click="addPost">Submit New Post</button>
         <br></br>
         {{  outputMessage }}
        <hr> Click  <a><router-link to="/ViewPosts/">here</router-link></a>  to return to Main Page
       
    </div>
</template>

