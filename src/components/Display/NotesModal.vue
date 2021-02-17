<template>
<div id="note"  @click.self="closeNote" >
    <div>
        <input type="text" v-model="note[0]" placeholder="Note title">

        <textarea v-model="note[1]" placeholder="Note content"></textarea> 

        <button @click="closeNote">Close</button>           
    </div>
</div>
</template>

<script>
export default {
    props:{
        note: Array
    },
    methods:{
        closeNote(){
            this.$root.$emit('saveNotes')
            this.$emit('closeNote')
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
        width: 90%;
        max-width: 1024px;

        background-color: var(--depth-4);
        border-radius: 24px;
        padding: 16px;
        position: relative;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-between;

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
            font-size: 18px;
            resize: none;
            height: 70%;
            font-weight: 500;
            padding-top: 8px;
        }

        button{
            background: rgb(80,80,90);
            border: 0;
            outline: 0;
            font-weight: 700;
            font-size: 28px;
            padding: 4px 16px;
            border-radius: 16px;
            margin-top: 8px;
            cursor: pointer;

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