<script>
    let todos = [];
    let task = "";
 

    //let count = $state(0);


    const addTask = () =>{
        let todo ={
        task: task,
        isComplete: false,
        createdDate: new Date(),
    };
        todos = [todo, ...todos];
        task ="";
    }


    const markTodoAsComplete = (index) =>{
       // console.log({index});
        todos[index].isComplete = !todos[index].isComplete;

    }

    const deleteTodo = (index) =>{
        let deleteItem = todos[index];
        console.log(deleteItem);
        todos= todos.filter((item)=>item != deleteItem);
    }

    $: console.table(todos);

</script>



<input type= "text" placeholder="Add a task" bind:value={task}/>
<button on:click={addTask}>AddItem</button>
<ol>
    {#each todos as todo, index}
    <li class:complete={todo.isComplete}>
        <span>
            {todo.task}
        </span>
        <span>
            <button on:click={()=> markTodoAsComplete(index)}>✔</button>
            <button on:click={()=> deleteTodo(index)}>✘</button>
        </span>
    </li>
    {/each}
</ol>


<style>
    .complete{
        text-decoration: line-through;
    }
</style>
