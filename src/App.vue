<template>
  <div id='app'>
    <h4 class='bg-success text-white text-center p-2'>
      {{name}} To Do
    </h4>
    <div class='container-fluid p-4'>
      <div class='row' v-if='filteredTasks.length == 0'>
        <div class='col text-center'>
          <strong>Nothing to do</strong>
        </div>
      </div>
      <template v-else>
        <div class='row'>
          <div class='col font-weight-bold'>Task</div>
          <div class='col-2 font-weight-bold'>Done</div>
        </div>
        <div class='row' v-for='x in filteredTasks' v-bind:key='x.action'>
          <div class='col'>{{x.action}}</div>
          <div class='col-2 text-center'>
            <input type='checkbox' v-model='x.done' class='form-check-input' />
          </div>
        </div>
      </template>
      <div class='row py-2'>
        <div class='col'>
          <input v-model='newItemText' class='form-control' />
        </div>
        <div class='col-2'>
          <button class='btn btn-primary' v-on:click='addNewTodo'>Add</button>
        </div>
      </div>
      <div class='row bg-secondary py-2 mt-2 text-white'>
        <div class='col text-center'>
          <input type='checkbox' v-model='hideCompleted' class='form-check-input' />
          <label class='form-check-label font-weight-bold'>Hide completed</label>
        </div>
        <div class='col text-center'>
          <button class='btn btn-sm btn-warning' v-on:click='deleteCompleted'>Delete completed</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'app',
  data() {
    return {
      name: 'Fred',
      tasks: [],
      hideCompleted: true,
      newItemText: ''
    }
  },
  computed: {
    filteredTasks() {
      return this.hideCompleted ?
          this.tasks.filter(x => !x.done) : this.tasks
    }
  },
  methods: {
    addNewTodo() {
      this.tasks.push({
        action: this.newItemText,
        done: false
      });
    },
    storeData() {
      localStorage.setItem('todos', JSON.stringify(this.tasks));
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter(x => !x.done);
      this.storeData();
    }
  },
  created() {
    let data = localStorage.getItem('todos');
    if (data != null) {
      this.tasks = JSON.parse(data);
    }
  }
}
</script>
