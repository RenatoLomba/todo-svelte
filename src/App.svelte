<script lang="ts">
  import ToDosList from './components/ToDosList.svelte';
  import { type ToDo } from './models/todo';

  let title: string = '';
  let description: string = '';

  let todosList: ToDo[] = [];

  $: formIsValid = title.length > 0 && description.length > 0;

  function clearForm(): void {
    title = '';
    description = '';
  }

  function handleFormSubmit(): void {
    const newTodo: ToDo = {
      id: new Date().getTime().toString(),
      title,
      description,
      isFinished: false,
    };

    todosList.push(newTodo);
    todosList = todosList;

    clearForm();
  }

  function finishTodo(todo: ToDo): void {
    const todoIndex = todosList.findIndex(t => t.id === todo.id);

    if (todoIndex !== -1) {
      todosList[todoIndex].isFinished = true;
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

    <form
      on:submit|preventDefault={handleFormSubmit}
      class="flex flex-col gap-8 w-full bg-slate-800 p-5 mt-20 rounded-md"
    >
      <label>
        <span>Title</span>
        <input
          class="
            bg-slate-700 rounded-sm p-2 text-slate-200
            focus:outline focus:outline-2 focus:outline-pink-500
          "
          bind:value={title}
        />
      </label>

      <label>
        <span>Description</span>
        <textarea
          class="
            resize-none h-24 bg-slate-700 rounded-sm p-2
            text-slate-200 focus:outline focus:outline-2
            focus:outline-pink-500
          "
          bind:value={description}
        />
      </label>

      <div class="flex gap-4">
        <button
          disabled={!formIsValid}
          type="submit"
          class="
            bg-blue-500 disabled:cursor-not-allowed
            enabled:hover:bg-blue-400 enabled:active:bg-blue-600
          ">Create</button
        >
        <button
          on:click={clearForm}
          type="button"
          class="
            bg-red-500 hover:bg-red-400 active:bg-red-600
          ">Cancel</button
        >
      </div>
    </form>

    <ToDosList {todosList} onCheckButtonClick={finishTodo} />
  </div>
</main>

<style lang="postcss">
  button {
    @apply focus:outline-1 focus:outline focus:outline-slate-100 focus:outline-offset-2 transition-colors py-2 px-4 rounded-md text-slate-50 font-medium;
  }

  label {
    @apply flex flex-col gap-4;
  }

  span {
    @apply text-lg text-slate-100 font-medium;
  }
</style>
