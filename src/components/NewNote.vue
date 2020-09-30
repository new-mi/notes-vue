<template>
    <!-- new note -->
    <div class="new-note">
        <label>Title</label>
        <input v-model="note.title" type="text">
        
        <label>Description</label>
        <textarea v-model="note.descr"></textarea>

        <label>Priority</label>
        <div class="new-note__radio">
            <label v-for="(val, key) in dataPriority" :key="key">
            <input type="radio" :value="key" v-model="note.priority">
            <span :style=" `border-bottom: 4px solid ${val}` ">{{ key }}</span>
        </label>
        </div>

        <button class="btn btnPrimary" @click="addNote">New note</button>
    </div>
</template>

<script>
    export default {
        name: 'NewNote',
        props: {
            note: {
                type: Object,
                required: true
            },
            dataPriority: {
                type: Object,
                required: true
            }
        },
        methods: {
            addNote() {
                this.$emit('addNote', this.note)
            }
        }
    }
</script>

<style lang="scss">
    .new-note {
        display: flex;
        flex-direction: column;
        padding: 0 0 40px;
        &__radio {
            display: flex;
            margin-bottom: 30px;
            label {
                display: inline-flex;
                align-items: center;
                margin-right: 20px;
                cursor: pointer;
            }
            input {
                width: auto;
                flex-shrink: 0;
                margin-right: 8px;
                margin-bottom: 0;
            }
            span {
                display: inline-block;
            }
        }
        button {
            margin: 0 auto;
        }
    }
</style>