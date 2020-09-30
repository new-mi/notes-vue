<template>
    <!-- note list -->
    <div class="notes">
        <Note :class="{full: !grid}" :dataPriority="dataPriority" v-for="(note, index) in notes" :key="note.id" :note="note" @remove="removeNote(index)"/>
    </div>
</template>

<script>
    import Note from '@/components/Note.vue'

    export default {
        name: "Notes",
        components: {
            Note
        },
        props: {
            notes: {
                type: Array,
                required: true
            },
            grid: {
                type: Boolean,
                required: true
            },
            dataPriority: {
                type: Object,
                required: true
            }
        },
        methods: {
            removeNote(index) {
                this.$emit('removeNote', index)
            }
        }
    }
</script>

<style lang="scss">
    .notes {
        display: flex;
        align-items: flex-start;
        justify-content: space-between;
        flex-wrap: wrap;
        padding: 40px 0;
    }
</style>