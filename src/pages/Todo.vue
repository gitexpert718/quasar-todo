<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn
            @click="addTask"
            round
            dense
            flat
            icon="add"
          />
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered
    >
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.status = !task.status"
        :class="{'status bg-blue-1': task.status}"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.status"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.status"
          side
        >
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-if="!tasks.length"
      class="no-tasks absolute-center"
    >
      <q-icon
        name="check"
        size="100px"
        color="primary"
      ></q-icon>
      <div class="text-h5 text-primary text-color">
        No Tasks
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      newTask: '',
      tasks: []
    }
  },
  methods: {
    deleteTask (index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really delete it?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        const deleteTask = this.tasks[index].title
        this.tasks.splice(index, 1)
        this.$q.notify({
          message: '"' + deleteTask + '"' + ' was deleted',
          color: 'red'
        })
      })
    },
    addTask () {
      if (!this.newTask) {
        return
      }
      this.tasks.push({
        title: this.newTask,
        status: false
      })
      this.newTask = ''
    }
  }
}
</script>

<style lang="scss">
  .status {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-tasks {
    opacity: 0.5;
  }
</style>
