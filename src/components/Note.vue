<template>
    <div class="note" :style=" `border-top: 8px solid ${dataPriority[note.priority]}` ">
        <div class="note-header">
            <p v-show="!isShow('title')" @click="editOn('title')">{{ note.title }}</p>
            <p v-show="isShow('title')">
                <span class="backdrop" @click="editFault('title')"></span>
                <textarea
                    ref="title"
                    v-model="title.value" 
                    @keydown.enter.prevent="editDone('title')" 
                    @keydown.esc="editFault('title')"
                    @input="auto_grow($event)"
                ></textarea>
            </p>
            <span @click="remove">&times;</span>
        </div>
        <div class="note-body">
            <p v-show="!isShow('descr')" @click="editOn('descr')">{{ note.descr }}</p>
            <p v-show="isShow('descr')">
                <span class="backdrop" @click="editFault('descr')"></span>
                <textarea
                    ref="descr"
                    v-model="descr.value" 
                    @keydown.enter.prevent="editDone('descr')" 
                    @keydown.esc="editFault('descr')"
                ></textarea>
            </p>
            <span>{{ note.date }}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Note",
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
        data() {
            return {
                title: {
                    value: this.note.title,
                    tmp: '',
                    isEdit: false
                },
                descr: {
                    value: this.note.descr,
                    tmp: '',
                    isEdit: false
                }
            }
        },
        methods: {
            remove() {
                this.$emit('remove')
            },
            isShow(type) {
                return this[type].isEdit
            },
            editOn(type) {
                this[type].isEdit = true
                this[type].tmp = this[type].value
                this.$nextTick(() => {
                    this.$refs[type].focus()
                    this.auto_grow({target: this.$refs[type]})
                })
                
            },
            editDone(type) {
                this.blurEdit(type)
                if (this.note[type] !== this[type].value) this.note.date = new Date(Date.now()).toLocaleString()
                this.note[type] = this[type].value
            },
            editFault(type) {
                this.blurEdit(type)
                this.note[type] = this[type].tmp
                this[type].value = this[type].tmp
            },
            blurEdit(type) {
                this[type].isEdit = false
            },
            auto_grow(e) {
                e.target.style.height = "5px";
                e.target.style.height = (e.target.scrollHeight)+"px";
            }
        }
    }
</script>

<style lang="scss">
    .note {
        width: 48%;
        padding: 18px 20px;
        margin-bottom: 20px;
        background-color: #fff;
        textarea {
            position: relative;
            border: none;
            background: transparent;
            padding: 0;
            font: inherit;
            color: inherit;
            margin-bottom: 0;
            z-index: 1;
            border-radius: 0;
            resize: none;
            overflow: hidden;
            min-height: 24px;
        }
        .backdrop {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0);
            z-index: 0;
            cursor: default;
        }
        &.full {
            width: 100%;
        }
        &-header {
            display: flex;
            align-items: flex-start;
            justify-content: space-between;
            h1 {
                font-size: 32px;
            }
            p {
                flex-grow: 1;
                font-size: 22px;
                color: #494ce8
            }
            span {
                font-size: 32px;
                color: #494ce8;
                cursor: pointer;
                line-height: 1;
            }
            svg {
                margin-right: 12px;
                color: #999;
                cursor: pointer;
                &:last-child {
                    margin-right: 0;
                }
                &.active {
                    color: #494ce8;
                }
            }
        }
        &-body {
            p {
                margin: 20px 0;
                min-height: 24px;
                min-width: 24px;
            }
            span {
                font-size: 14px;
                color: #999;
            }
        }
    }
</style>