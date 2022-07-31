<template>
    <div>
        <table class="table table-bordered mt-2">
            <tr v-for="(list, index) in lists" :key="index">
                <items 
                    :item="list"
                    v-on:itemchanged="$emit('reloadlist')"
                />
            </tr>
        </table>        
    </div>
</template>
<script>
import items from './items';
// import Items from './items.vue';

export default {
    props : ['lists'],
    components: {
        items
    }
}
</script>