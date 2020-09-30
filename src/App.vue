<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">
          
          <Message v-if="message" :message="message" />

          <NewNote :note="note" :dataPriority="dataPriority" @addNote="addNote" />

          <div class="note-header" style="margin: 36px 0; align-items: center">
            <!-- title -->
            <h1>{{ title }}</h1>

            <!-- search -->
            <Search :value="search" placeholder="Find you note" @search="search = $event" />

            <!-- icons controls -->
            <div class="icons">
              <svg :class="{active: grid}" @click="grid = true" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" ><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
              <svg :class="{active: !grid}" @click="grid = false" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="8" y1="6" x2="21" y2="6"></line><line x1="8" y1="12" x2="21" y2="12"></line><line x1="8" y1="18" x2="21" y2="18"></line><line x1="3" y1="6" x2="3" y2="6"></line><line x1="3" y1="12" x2="3" y2="12"></line><line x1="3" y1="18" x2="3" y2="18"></line></svg>
            </div>
          </div>

          <Notes :notes="notesFilter" :dataPriority="dataPriority" :grid="grid" @removeNote="removeNote" />

        </div>
      </section>

    </div>
  </div>
</template>

<script>
import Message from '@/components/Message.vue'
import NewNote from '@/components/NewNote.vue'
import Notes from '@/components/Notes.vue'
import Search from '@/components/Search.vue'

import { uuid } from '@/utils/index'

export default {
  components: {
    Message,
    NewNote,
    Notes,
    Search
  },
  data () {
    return {
      title: 'Notes App',
      message: null,
      grid: true,
      search: '',
      dataPriority: {
        'normal': '#eee',
        'very': 'rgba(255, 165, 0, 0.5)',
        'must': 'rgba(255, 0, 0, 0.5)'
      },
      note: {
          title: '',
          descr: '',
          priority: 'normal'
      },
      notes: [
          {
            id: uuid(),
            title: 'First Note',
            descr: 'Description for first note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'normal'
          },
          {
            id: uuid(),
            title: 'Second Note',
            descr: 'Description for second note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'very'
          },
          {
            id: uuid(),
            title: 'Third Note',
            descr: 'Description for third note',
            date: new Date(Date.now()).toLocaleString(),
            priority: 'must'
          }
      ]
  }
  },
  computed: {
    notesFilter() {
      let arr = this.notes,
          search = this.search
      if (!search) return arr
      // small
      search = search.trim().toLowerCase()
      // filter
      arr = arr.filter(item => item.title.toLowerCase().indexOf(search) > -1)
      // Error
      return arr
    }
  },
  methods: {
    addNote() {
        // console.log(this.note)
        if (!this.note.title) {
            this.message = 'Title can\'t be blank!'
            return;
        }

        this.notes.push({
            ...this.note,
            date: new Date(Date.now()).toLocaleString(),
            id: uuid()
        })

        this.message = ''
        this.note = {
            title: '',
            descr: '',
            priority: 'normal'
        }
    },
    removeNote(index) {
      console.log(`Note ID ${index} - remove`)
      this.notes.splice(index, 1)
    }
  }
}
</script>

<style scoped>
.container {
  max-width: 800px;
}
</style>
