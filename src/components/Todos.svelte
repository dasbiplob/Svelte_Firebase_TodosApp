<script>
    let todos = [{task:"Get Milk", isComplete: false, createdDate: "2024-02-17"}, 
    {task:"Get Fruits", isComplete: true, createdDate: "2024-02-8"},
    {task:"Get Eggs", isComplete: true, createdDate: "2024-02-10"}]


    let todo ={
        task: "",
        isComplete: "false",
        createdDate: new Date(),
    };

    //let count = $state(0);


    const addTask = () =>{
        todos = [...todos,todo];
    }

    $: console.table(todo);

</script>

<input type= "text" placeholder="Add a task" bind:value={todo.task}/>
<button on:click={addTask}>AddItem</button>
<ol>
    {#each todos as todo}
    <li class:complete={todo.isComplete}>
        {todo.task}
    </li>
    {/each}
</ol>


<style>
    .complete{
        text-decoration: line-through;
    }
</style>
