<script>
  export default {
    name: 'TodoItem',
    props: {
      todo: Object,
    },
    emits: ['update', 'remove'],
    data() {
      return {
        editing: false,
        newTitle: this.todo.title,
      }
    },
    methods: {
      toggle() {
        this.$emit('update', {
          ...this.todo,
          completed: !this.todo.completed,
        });
      },
      rename() {
        this.editing = false;

        if (!this.newTitle) {
          this.remove();

          return;
        }

        this.$emit('update', {
          ...this.todo,
          title: this.newTitle,
        });
      },
      edit() {
        this.newTitle = this.todo.title;
        this.editing = true;
      
        this.$nextTick(() => {
          this.$refs['title-field'].focus();
        });
      },
      remove() {
        this.$emit('remove');
      },
    },
  }
</script>

<template>
   <div
      class="todo"
      :class="{ completed: todo.completed }"
    >
    <label class="todo__status-label">
      <input
        type="checkbox"
        class="todo__status"
        :checked="todo.completed"
        @change="toggle"
      />
    </label>

    <form v-if="editing" @submit.prevent="rename">
      <input
        type="text"
        class="todo__title-field"
        v-model.trim="newTitle"
        ref="title-field"
        @keyup.esc="editing = false"
        @blur="editing = false"
      />
    </form>

    <template v-else>
      <span class="todo__title" @dblclick="edit">{{ todo.title }}</span>
      <button class="todo__remove" @click="remove">x</button>
    </template>

    <div class="modal overlay" :class="{ 'is-active': false }">
      <div class="modal-background has-background-white-ter"></div>
      <div class="loader"></div>
    </div>
  </div>
</template>