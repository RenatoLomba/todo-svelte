<script lang="ts">
  import FaCheck from 'svelte-icons/fa/FaCheck.svelte';
  import FaTrashAlt from 'svelte-icons/fa/FaTrashAlt.svelte';
  import classNames from 'classnames';

  import { type ToDo } from '../models/todo';

  export let todosList: ToDo[];

  export let onCheckButtonClick: (id: string) => void;
  export let onDeleteButtonClick: (id: string) => void;
</script>

<div class="flex flex-col gap-4 mt-10">
  {#each todosList as todo}
    <div
      class="
        bg-slate-800 flex p-5 justify-between
        text-slate-200 rounded-md
      "
    >
      <div class="flex-1">
        <h2
          class={classNames('text-lg text-pink-400 font-medium leading-8', {
            'line-through': todo.isFinished,
          })}
        >
          {todo.title}
        </h2>
        <p
          class={classNames({
            'line-through': todo.isFinished,
          })}
        >
          {todo.description}
        </p>
      </div>

      <div class="flex items-center gap-2">
        {#if !todo.isFinished}
          <button
            on:click={() => onCheckButtonClick(todo.id)}
            type="button"
            class="
              p-2 rounded-md text-green-500 transition-colors
              hover:text-slate-50 hover:bg-green-500 group
            "
          >
            <div
              class="
                transition-colors group-hover:text-slate-50
                text-green-500 w-4 h-4
              "
            >
              <FaCheck />
            </div>
          </button>
        {/if}

        <button
          on:click={() => onDeleteButtonClick(todo.id)}
          type="button"
          class="bg-red-500 p-2 rounded-md"
        >
          <div class="text-slate-50 w-4 h-4">
            <FaTrashAlt />
          </div>
        </button>
      </div>
    </div>
  {:else}
    <div
      class="
        bg-slate-800 text-slate-50 p-5 text-xl
        text-center uppercase rounded-md
      "
    >
      No Todos available
    </div>
  {/each}
</div>
