<template>
<div id="noDisplay">
    <button @click="createGroup()">
        <h1><font-awesome-icon :icon="['fas', 'plus']" /></h1>
        <h2>Group List is empty</h2>
    </button>
</div>
</template>

<script>
export default {
    methods:{
        createGroup(){
            this.$root.$emit('createGroup')
        }
    }
}
</script>

<style lang="scss">

#noDisplay{
    button{
        width: 100%;
        height: 100%;

        cursor: pointer;

        background: rgba(230,230,250, .07);
        transition: background .4s;
        
        outline: 0;
        border: 2px dashed rgb(90,90,100);

        &:hover{
            background: rgba(230,230,250, .1);

            h1, h2{
                opacity: .6;
            }
        }

        h1{
            font-size: 80px;
        }

        h2{
            font-size: 32px;
        }

        h1, h2{
            opacity: .4;
            transition: opacity .4s;
        }
    }
}
</style>