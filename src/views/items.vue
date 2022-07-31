<template>
    <td>
        <div class="form-check form-check-inline" style="width: 100%">
            <input class="form-check-input" type="checkbox" v-model="item.completed" @change="updateList()">
            <label class="form-check-label" :class="[item.completed ? 'done' : '']" for="inlineCheckbox1">{{ item.name }}</label>
            <button class="btn btn-danger ml-auto" @click="removeItem()">Delete</button>
        </div>     
    </td>
</template>
<script>
import axios from "axios";
export default {
    props: ['item'],
    methods : {
        updateList() {
            axios.put( process.env.VUE_APP_API_URL + 'list/'+this.item.id, { 
                list : this.item
            })
            .then( res => {
                if (res.status == 200) {
                    this.$emit('itemchanged')
                }
            })
        },
        removeItem() {
            axios.delete( process.env.VUE_APP_API_URL + 'list/'+this.item.id, { 
                list : this.item
            })
            .then( res => {
                if (res.status == 200) {
                    this.$emit('itemchanged')
                }
            })
        }
    }
}
</script>

<style scoped>
    .done {
        text-decoration: line-through;
    }
</style>