<template>
  <div padding>
    <div class="row flex flex-center">
      <q-input @keyup.enter="saveTodo" v-model="desc" label="Description" />
    </div>
    <div class="row flex flex-center">
      <q-btn @click="saveTodo" icon="save" color="primary" label="Save" />
    </div>
    <div class="flex flex-center">
      <q-list bordered padding class="rounded-borders" style="max-width: 350px">
        <q-item-label header>Todos {{ ind }}</q-item-label>

        <q-item v-for="(todo, i) in todos" :key="i" clickable v-ripple>
          <q-item-section avatar top>
            <q-checkbox v-model="todo.isDone" />
          </q-item-section>

          <q-item-section>
            <q-item-label>{{ i+1 }}</q-item-label>
            <q-item-label lines="1">
              <span v-if="!todo.editing">{{ todo.desc }}</span>
              <q-input v-else v-model="editDesc"/>
            </q-item-label>
            <q-item-label caption>{{ todo.dateCreated }}</q-item-label>
          </q-item-section>

          <q-item-section side>
            <div class="row q-gutter-sm">
              <q-btn v-if="todo.editing" size="sm" round color="info" icon="save" @click="update(i)" />
              <q-btn v-else-if="ind == null" size="sm" round color="info" icon="mdi-pencil" @click="edit(i)" />
              <q-btn v-if="ind == null" @click="removeTodo(i)" size="sm" round color="negative" icon="delete" />
            </div>
          </q-item-section>
        </q-item>
      </q-list>
    </div>
  </div>
</template>
<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      ind: null,
      desc: '',
      editDesc: 'gg',
      todos: [
        { editing: false, desc: 'create a front project', isDone: false, dateCreated: new Date() },
        { editing: false, desc: 'create a list', isDone: true, dateCreated: new Date() },
        { editing: false, desc: 'input box', isDone: false, dateCreated: new Date() }
      ]
    }
  },
  methods: {
    update () {
      // update todo.desc
      this.todos[this.ind].desc = this.editDesc
      this.todos[this.ind].editing = false
      this.editDesc = ''
      this.ind = null
    },
    edit (ind) {
      this.todos[ind].editing = true
      this.ind = ind
      this.editDesc = this.todos[ind].desc
      // get the desc of todo then copy to editDesc
    },
    saveTodo () {
      if (this.desc !== '') {
        let todo = {
          desc: this.desc,
          isDone: false,
          dateCreated: new Date()
        }
        this.todos.push(todo)
        this.desc = ''
      } else {
        this.$q.notify({
          message: 'Please enter description',
          color: 'negative'
        })
      }
    },
    removeTodo (index) {
      this.todos.splice(index, 1)
      this.$q.notify({
        message: 'Todo has been removed',
        icon: 'info',
        color: 'info'
      })
    }
  }
}
</script>
