<script>
  import TodoFilter from './components/TodoFilter.vue';
  import TodoItem from './components/TodoItem.vue';

  export default {
    components: {
    TodoFilter,
    TodoItem
  },

    data() {
      const todosJSON = localStorage.getItem('todos') || '[]';
      const todos = JSON.parse(todosJSON);

      return {
        todos,
        title: '',
        filter: 'all'
      };
    },
    computed: {
      activeTodos() {
        return this.todos
          .filter(todo => !todo.completed);
      },
      completedTodos() {
        return this.todos
          .filter(todo => todo.completed);
      },
      visibleTodos() {
        switch(this.filter){
          case 'all': 
            return this.todos;
          case 'active':
            return this.activeTodos;
          case 'completed': 
            return this.completedTodos;
          default: return null;
        }
      }
    },
    methods: {
      handleSubmit() {
        this.todos.push({
          id: Date.now(),
          title: this.title,
          completed: false,
        });

        this.title = '';
      },
      toggleAll() {
        if (this.todos.every(todo => todo.completed)) {
          this.todos.forEach(todo => todo.completed = false);

          return;
        }
        this.todos.forEach(todo => todo.completed = true);
      },
    },
    watch: {
      todos: {
        deep: true,
        handler() {
          localStorage.setItem('todos', JSON.stringify(this.todos))
        },
      }
    }
  }
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todo-list</h1>

    <div class="todoapp__content">
      <header 
        class="todoapp__header"
        :class="{active: activeTodos.length === 0 }"
      >
        <button 
          class="todoapp__toggle-all active"
          @click="toggleAll"          
        ></button>

        <form @submit.prevent="handleSubmit">
          <input
            type="text"
            class="todoapp__new-todo"
            placeholder="What needs to be done?"
            v-model="title"
          />
        </form>
      </header>

      <TransitionGroup
        name="list"
        tag="section"
        class="todoapp__main"
      >
        <TodoItem 
          v-for="todo of visibleTodos"
          :key="todo.id"
          :todo="todo"
          @update="Object.assign(todo, $event)"
          @remove="todos.splice(todos.indexOf(todo), 1)"
        />
      </TransitionGroup>

      <footer class="todoapp__footer">
        <span class="todo-count">
          {{ activeTodos.length }} items left
        </span>

        <TodoFilter v-model="filter"/>

        <button 
          class="todoapp__clear-completed"
          @click="todos = activeTodos"
        >
          Clear completed
        </button>
      </footer>
    </div>
  </div>
</template>

<style>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
  max-height: 60px;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  max-height: 0;
  transform: translateX(30px);
}
</style>