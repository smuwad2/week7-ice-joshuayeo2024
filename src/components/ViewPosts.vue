<script setup>
    import axios from 'axios';
</script>

<script>
export default {
    data() {
        return {
            moods: ["Happy", "Sad", "Angry"],
            posts: [], // array of post objects
            entry: "",
            mood: "",
            showEditPost: false,
            editPostId: null,
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
    created() { 
        this.fetchPosts()
    },
    methods: {
        fetchPosts() {
            axios.get(`${this.baseUrl}/posts`)
                .then(response => {
                    // Sort the posts by ID descending to match the test's row selection
                    this.posts = response.data.sort((a, b) => b.id - a.id)
                })
                .catch(error => {
                    this.posts = [{ entry: 'There was an error loading posts: ' + error.message, id: 0, mood: 'N/A' }]
                })
        },
        editPost(id) {
            const postToEdit = this.posts.find(post => post.id === id);

            if (postToEdit) {
                this.editPostId = id;
                this.entry = postToEdit.entry;
                this.mood = postToEdit.mood;
                this.showEditPost = true;
            }
        },
        updatePost(event) {
            event.preventDefault();

            if (!this.editPostId) return;

            const updatedData = {
                id: this.editPostId,
                entry: this.entry,
                mood: this.mood,
            };

            axios.post(`${this.baseUrl}/updatePost`, updatedData)
                .then(response => {
                    const index = this.posts.findIndex(post => post.id === this.editPostId);
                    if (index !== -1) {
                        
                        // ⭐ MODIFIED: Use splice for explicit array mutation and fastest possible re-render
                        const currentPost = this.posts[index];
                        const updatedPost = { 
                            ...currentPost, 
                            entry: this.entry, 
                            mood: this.mood 
                        };
                        this.posts.splice(index, 1, updatedPost);

                        // Clear and hide the form
                        this.entry = "";
                        this.mood = "";
                        this.editPostId = null;
                        this.showEditPost = false;
                    }
                })
                .catch(error => {
                    console.error("Error updating post:", error);
                    alert(`Failed to update post: ${error.message}.`);
                });
        }
    }
}
</script>

<template>
    <div id="demo">
        <h2> View Blog Posts </h2>
        <table class="table m-2">
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Entry</th>
                    <th>Mood</th>
                    <th></th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="post in posts" :key="post.id">
                    <td>{{ post.id }}</td>
                    <td>{{ post.entry }}</td>
                    <td>{{ post.mood }}</td>
                    <td><button @click="editPost(post.id)" class="btn btn-secondary btn-sm">Edit</button></td>
                </tr>
            </tbody>
        </table>

        <div id="editPost" v-if="showEditPost">
            <h3>Edit Post</h3>
            <div id="postContent" class="mx-3">
                <form @submit.prevent="updatePost">
                    <div class="mb-3">
                        <label for="entry" class="form-label">Entry</label>
                        <textarea id="entry" class="form-control" v-model="entry" required></textarea>
                    </div>
                    <div class="mb-3">
                        <label for="mood" class="form-label">Mood</label>
                        <select id="mood" class="form-select" v-model="mood" required>
                            <option value="" disabled>Select Mood</option>
                            <option v-for="mood in moods" :value="mood" :key="mood">{{ mood }}</option>
                        </select>
                    </div>
                    <button type="submit" class="btn btn-primary">Update Post</button>
                </form>
            </div>
        </div>
    </div>
</template>

<style scoped></style>