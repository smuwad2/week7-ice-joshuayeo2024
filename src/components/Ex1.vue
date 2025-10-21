<script>
    export default { 
        // Add Code Here to complete the task
        // Note: DO NOT USE "eval()". In security, "eval" is considered "evil"!!!
        data(){
            return {
                x: 0,
                y: 0,
                selectedOp: "",
                operators: [
                    "+",
                    "-",
                    "*",
                    "/",
                    "%"
                ]
            }
        },
        computed: {
            result () {
                switch(this.selectedOp){
                    case "+": return this.x + this.y;
                    case "-": return this.x - this.y;
                    case "*": return this.x * this.y;
                    case "/": return this.x / this.y;
                    case "%": return this.x % this.y;
                }
                
            }
        }
    }
</script>

<template>
    <p>x <input v-model.number="x"></p>
    <select v-model="selectedOp">
        <option v-for="op in operators" >{{ op }}</option>
    </select>
    <p>y <input v-model.number="y"></p>
    
    <p>---------------------</p>

    <p>= {{result}}</p>
</template>

<style scoped>
    p, input { font-family: monospace; }
    p { white-space: pre; }

</style>
