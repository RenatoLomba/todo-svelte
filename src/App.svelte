<script lang="ts">
  import ToDosList from './components/ToDosList.svelte';
  import ToDoForm from './components/ToDoForm.svelte';
  import { type ToDo } from './models/todo';
  import { type ToDoFormFields } from './models/todo-form-fields';
  import { todosListLocalStorageName } from './models/enums/todos-list-localstorage-name';

  function getTodosListFromLocalStorage() {
    const todosListString = localStorage.getItem(todosListLocalStorageName);

    if (todosListString) {
      return JSON.parse(todosListString);
    }
    return [];
  }

  let todosList: ToDo[] = getTodosListFromLocalStorage();

  $: {
    localStorage.setItem(todosListLocalStorageName, JSON.stringify(todosList));
  }

  function handleFormSubmit({ title, description }: ToDoFormFields): void {
    const newTodo: ToDo = {
      id: new Date().getTime().toString(),
      title,
      description,
      isFinished: false,
    };

    todosList.unshift(newTodo);
    todosList = todosList;
  }

  function finishTodo(id: string): void {
    const todoIndex = todosList.findIndex(t => t.id === id);

    if (todoIndex !== -1) {
      todosList[todoIndex].isFinished = true;
      todosList = todosList;
    }
  }

  function deleteTodo(id: string): void {
    const todoIndex = todosList.findIndex(t => t.id === id);

    if (todoIndex !== -1) {
      todosList.splice(todoIndex, 1);
      todosList = todosList;
    }
  }
</script>

<main class="min-h-screen bg-slate-900">
  <div class="max-w-4xl mx-auto py-5">
    <h1
      class="
        text-pink-500 text-4xl
        text-center font-bold
      "
    >
      ToDo Svelte
    </h1>

    <ToDoForm onFormSubmit={handleFormSubmit} clearFormOnSubmit />

    <ToDosList
      {todosList}
      onCheckButtonClick={finishTodo}
      onDeleteButtonClick={deleteTodo}
    />
  </div>
</main>
