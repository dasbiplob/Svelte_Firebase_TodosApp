<script>
    import { initializeApp, getApps, getApp } from "firebase/app";
    import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc, addDoc} from "firebase/firestore";
    import { firebaseConfig } from "$lib/firebaseConfig";
  
    let firebaseApp;
    let db;
  
    if (getApps().length === 0) {
      firebaseApp = initializeApp(firebaseConfig);
    } else {
      getApp();
    }
  
    // Initialize Cloud Firestore and get a reference to the service
    db = getFirestore(firebaseApp);
  
    const colRef = collection(db, "todos");
    let todos = [];
    const unsubscribe = onSnapshot(colRef, (querySnapshot) => {
      let fbTodos = [];
      querySnapshot.forEach((doc) => {
        let todo = { ...doc.data(), id: doc.id };
        fbTodos = [todo, ...fbTodos];
      });
      console.table(fbTodos);
      todos = fbTodos;
    });
  
    console.log(firebaseApp);
  
    // Array to store todo items
    // Variable to store the task input
    let task = "";
    let error = "";
  
    // Function to add a new task to the todos array
    const addTask = async () => {
      // Create a new todo object
      if (task.trim() !== "") {
        //todos = [todo, ...todos];

        const docRef = await addDoc(collection(db, "todos"), {
            task: task,
            isComplete: false,
            createdDate: new Date(),
});

        error = "";
      } else {
        error = "Task is Empty";
      }
      // Reset the task input
      task = "";
    };
  
    // Function to toggle the completion status of a todo
    const markTodoAsComplete = async(todo) => {
      // Toggle the isComplete property of the todo at the specified index
      //todos[index].isComplete = !todos[index].isComplete;
        await updateDoc(doc(db, "todos", todo.id), {
        isComplete: !todo.isComplete
    });
};
  
    // Function to delete a todo from the todos array
    const deleteTodo = async (id) => {
      // Get the todo to be deleted
     // let deleteItem = todos[index];
      // Filter out the todo to be deleted from the todos array
     //todos = todos.filter((item) => item !== deleteItem);
     await deleteDoc(doc(db, "todos", id));
    };
  
    const keyIsPressed = (event) => {
      if (event.key === "Enter") {
        addTask();
      }
    };
  </script>
  

  <!-- Input field to add a new task -->
  <input type="text" placeholder="Add a task" bind:value={task} on:keypress={keyIsPressed} />
  <!-- Button to trigger the addTask function -->
  <button on:click={addTask}>Add Item</button>
  <!-- Display the list of todos -->
  <ol>
    {#each todos as todo}
    <li class:complete={todo.isComplete}>
      <!-- Display the task text -->
      <span>{todo.task}</span>
      <!-- Buttons to mark the todo as complete or delete it -->
      <span>
        <button on:click={() => markTodoAsComplete(todo)}>✔</button>
        <button on:click={() => deleteTodo(todo.id)}>✘</button>
      </span>
    </li>
    {:else}
    <!-- Display a message if there are no todos -->
    <p>No Todos Found</p>
    {/each}
    <p class="error">{error}</p>
  </ol>
  
  <style>
    /* Style to apply line-through text decoration for completed todos */
    .complete {
      text-decoration: line-through;
    }
    .error {
      color: red;
    }
  </style>
  