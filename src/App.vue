<template>
  <div class="min-h-screen bg-gray-100 py-8">
    <div class="max-w-md mx-auto bg-white rounded-lg shadow p-6">
      <h1 class="text-2xl font-bold mb-6 text-gray-800">Todo List</h1>
      
      <!-- Add Todo Form -->
      <div class="flex gap-2 mb-6">
        <input 
          v-model="newTodo" 
          @keyup.enter="addTodo"
          type="text" 
          placeholder="Add a new task"
          class="flex-1 px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
        >
        <button 
          @click="addTodo"
          class="px-4 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition-colors"
        >
          Add
        </button>
      </div>

      <!-- Todo List -->
      <div class="space-y-2">
        <!-- Todo Items -->
        <div 
          v-for="todo in filteredTodos" 
          :key="todo.id"
          class="flex items-center justify-between p-4 bg-gray-50 rounded-lg hover:bg-gray-100 transition-colors"
        >
          <div class="flex items-center gap-3">
            <input 
              type="checkbox" 
              v-model="todo.completed"
              class="w-4 h-4 text-blue-500 rounded focus:ring-blue-500"
            >
            <span 
              :class="{'line-through text-gray-400': todo.completed}"
              class="text-gray-700"
            >
              {{ todo.text }}
            </span>
          </div>
          <button 
            @click="deleteTodo(todo.id)"
            class="text-red-500 hover:text-red-600"
          >
            ✕
          </button>
        </div>

        <!-- Filter Buttons -->
        <div class="flex gap-2 mt-4">
          <button
            v-for="filter in filters"
            :key="filter"
            @click="currentFilter = filter"
            :class="{'bg-blue-500 text-white': currentFilter === filter}"
            class="px-3 py-1 rounded-full text-sm transition-colors"
          >
            {{ filter }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      currentFilter: 'all',
      filters: ['all', 'active', 'completed']
    }
  },
  computed: {
    filteredTodos() {
      switch(this.currentFilter) {
        case 'active': return this.todos.filter(todo => !todo.completed);
        case 'completed': return this.todos.filter(todo => todo.completed);
        default: return this.todos;
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.todos.push({
          id: Date.now(),
          text: this.newTodo.trim(),
          completed: false
        });
        this.newTodo = '';
      }
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    }
  }
}
</script>