<template>
    <scroller>   
        <bui-button @click="changeStore" value="修改 store"></bui-button>
        <text>{{store}}</text>
        <text class="ff-icon ff-login">&#xe6ca;</text>
        <text class="ff-icon ff-login">&#xe6cb;</text>
        <image style="width:550px; height:550px; top:100px" src="bmlocal://assets/test.jpeg"></image>
        <input style="top:150px" placeholder="Input Text" /> 
    </scroller>
</template>

<script>
import 'Config'
import {buiButton} from 'Eros/bui'
export default {
    created() {
        this.bindEvent()
    },
    components: {
        buiButton
    },
    data(){
        return {
            store: null
        }
    },
    methods: {
        bindEvent() {
            this.$event.on('store.count.get', data => {
                this.store = data
            })
            this.$event.on('store.change', data => {
                this.store = data
            })
        },
        changeStore() {
            this.$event.emit('store.count.add')
        }
    }
}
</script>

<style lang="sass">
    @import "src/js/css/core/base";
</style>
