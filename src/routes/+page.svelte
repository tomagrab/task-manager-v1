<script lang="ts">
  type List = {
    name: string;
    tasks: Task[];
    taskName: string;
    taskContent: string;
  };

  type Task = {
    name: string;
    content: string;
    status: boolean;
  };

  let listName = "";
  let lists: List[] = [];

  function createList() {
    lists = [
      ...lists,
      { name: listName, tasks: [], taskName: "", taskContent: "" },
    ];
    listName = "";
  }

  function createTask(list: List) {
    const newTask = {
      name: list.taskName,
      content: list.taskContent,
    };
    const index = lists.indexOf(list);
    lists[index].tasks = [...lists[index].tasks, newTask];
    lists[index].taskName = "";
    lists[index].taskContent = "";
    lists = [...lists];
  }

  function deleteList(index: number) {
    lists = lists.filter((_, i) => i !== index);
  }
</script>

<!-- Form for creating list with tasks -->
<div class="border rounded my-4 p-4">
  <h2>Create a list</h2>
  <form on:submit|preventDefault={createList} class="flex flex-col">
    <input
      type="text"
      bind:value={listName}
      placeholder="List name"
      class="p-2 my-4 border"
    />
    <button class="btn btn-primary p-2" type="submit">Create list</button>
  </form>
</div>

<!-- List of lists -->
{#if lists.length === 0}
  <p>No lists yet</p>
{:else}
  <div class="border rounded p-4">
    <h2 class="text-4xl text-center underline">Your lists</h2>
    <div class="grid grid-cols-3">
      {#each lists as list}
        <div class="mb-4 relative">
          <h2 class="text-2xl">{list.name}</h2>
          <button
            class="btn btn-danger p-2 absolute top-0 right-0"
            on:click={() => deleteList(lists.indexOf(list))}>&#128465;</button
          >
          <!-- Form to create tasks -->
          <form
            on:submit|preventDefault={() => createTask(list)}
            class="flex flex-col mt-8"
          >
            <input
              type="text"
              bind:value={list.taskName}
              placeholder="Task name"
              class="p-2 border"
            />
            <textarea
              bind:value={list.taskContent}
              placeholder="Task content"
              class="p-2 border mt-2"
            />
            <button class="btn btn-primary mt-4 p-2" type="submit">
              Create task
            </button>
          </form>

          <!-- Task List -->
          <ul>
            {#each list.tasks as task}
              <div class="border rounded mt-4 p-4">
                <li>{task.name}</li>
                <li>{task.content}</li>
                <input
                  type="checkbox"
                  name="status"
                  id="status"
                  bind:value={task.status}
                />
                <label for="status">Completed</label>
              </div>
            {/each}
          </ul>
        </div>
      {/each}
    </div>
  </div>
{/if}
