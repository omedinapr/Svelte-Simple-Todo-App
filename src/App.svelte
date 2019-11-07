<script>
  export let name;

  let taskDetails = "";
  let editingTodo = -1;
  let todoList = ["test", "lala", "lulu"];

  $: testArr = todoList.map(task => {
    return task;
  });

  const removeTask = task => {
    todoList = todoList.filter(t => {
      if (t != task) {
        return true;
      }
    });
  };

  const checkKey = event => {
    if (event.key === "Enter") {
      todoList = [...todoList, taskDetails];
      taskDetails = "";
    }
  };

  const editTask = (event, index) => {
    if (event.key === "Enter") {
      let todoListCopy = Array.from(todoList);
      todoListCopy.splice(index, 1, event.target.value);
      todoList = todoListCopy;
      editingTodo = -1;
    }
  };
</script>

<style>
  .taskList > li:nth-child(even) {
    background-color: #ccc;
    color: #fff;
  }

  .taskList > li:nth-child(odd) {
    background-color: #f5f5f5;
  }
</style>

<h1 class="w-full text-center text-4xl font-bold">TODO APP:</h1>

<div class="container mx-auto">
  <div class="flex items-center justify-around">
    <input
      type="text"
      bind:value={taskDetails}
      on:keyup={checkKey}
      class="rounded border border-gray-500 w-full p-1 text-center mt-8"
      placeholder="I need to..." />
  </div>

  <div class="mt-8">
    <ul class="taskList">
      {#each testArr as todo, index}
        <li
          on:dblclick={() => (editingTodo = index)}
          class="p-2 rounded items-center font-bold flex justify-between mb-1">
          {#if editingTodo != index}
            <div>{todo}</div>
          {:else}
            <input
              on:keyup={event => editTask(event, index)}
              class="w-full mr-4 text-gray-700 rounded"
              type="text"
              value={todo} />
          {/if}

          <button
            on:click={() => removeTask(todo)}
            class="rounded bg-red-400 text-xs px-2 py-1 text-white font-medium
            hover:bg-red-600">
            Remove
          </button>
        </li>
      {/each}
    </ul>
  </div>

</div>
