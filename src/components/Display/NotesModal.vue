<template>
<div id="note"  @click.self="closeNote" >
    <div>
        <input type="text" v-model="title">

        <textarea v-model="content"></textarea> 

        <button @click="closeNote"><font-awesome-icon :icon="['fas', 'times']" /></button>           
    </div>
</div>
</template>

<script>
export default {
    props:{
        title: String,
        content: String
    },
    methods:{
        closeNote(){
            this.$emit('closeNote')
            this.$root.$emit('saveNotes')
        }
    }
}
</script>

<style lang="scss">
#note{
    background-color: rgba(0,0,0,0.5);
    z-index: 1000;
    height: 100%;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;

    > div{
        height: 70%;
        width: 92%;
        max-width: 1024px;

        background-color: var(--depth-4);
        border-radius: 32px;
        padding: 8px 16px;
        position: relative;

        input, textarea{
            width: 100%;
            background-color: var(--depth-3);
            outline: 0;
            border: 2px solid var(--depth-2);
            border-radius: 16px;
            padding: 4px 8px;

            &:focus{
                border: 2px solid rgb(160,160,180);
            }
        }

        input{
            font-size: 28px;
            font-weight: 600;
        }

        textarea{
            margin-top: 16px;
            font-size: 20px;
            resize: none;
            height: 70%;
            font-weight: 500;
        }

        button{
            background: rgb(80,80,90);
            border: 0;
            outline: 0;
            font-weight: 700;
            font-size: 36px;
            padding: 4px 16px;
            border-radius: 16px;
            margin-top: 8px;
            cursor: pointer;
            position: absolute;
            top: -44px;
            right: -40px;

            &:hover{
                background: var(--blue);
            }

            &:active{
                background: var(--blue-dark);
            }
        }
        
    }
}
</style>