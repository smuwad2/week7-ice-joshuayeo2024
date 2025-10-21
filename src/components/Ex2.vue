<script>
    export default { 
        // Add code in appropriate places
        data() {
            return {
                items: ['keyboard', 'mouse', 'iPhone', 'macbook', 'adapter'],
                enteredItem: ""
            }
            
        },
        methods: {
            AddItem() {
                this.items.push(this.enteredItem);
            }
        }
    }
</script>

<template>

    <h2>Shopping Cart</h2>
    <!-- TODO: Add Code Here -->
     <ul>
        <li v-for="(item,idx) in items" :value="item">{{ item }}<button @click="items.splice(idx, 1)">Delete!</button></li>
     </ul>
     <input type="text" v-model="enteredItem"><button @click="AddItem">Add</button>
   
</template>
