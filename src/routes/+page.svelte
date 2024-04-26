<script>
    import '../style.css';
    import {writable} from 'svelte/store';
    let taskItem = '';
    let storedTaskList;
    let taskList = writable([
        {
            text: "clean the latrines.",
            priority: 2
        },
        {
            text: "sharpen the axes.",
            priority: 3
        },
        {
            text: "take a bath.",
            priority: 1
        }
    ]);
    let storedCompletedList;
    let completedList = writable([
        {
            text: "bow down in worship.",
            priority: 4
        }
    ]);
    let priorityLevel = 1;
    let tempItem;

    if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
        storedTaskList = localStorage.getItem('storedTaskList');
        if(storedTaskList) {
            $taskList = (JSON.parse(storedTaskList));
        }
        storedCompletedList = localStorage.getItem('storedCompletedList');
        if(storedCompletedList) {
            $completedList = (JSON.parse(storedCompletedList));
        }
    }

    function updateLists() {
        storedCompletedList = localStorage.setItem('storedCompletedList', JSON.stringify($completedList));
        storedTaskList = localStorage.setItem('storedTaskList', JSON.stringify($taskList));
    }

    function priorityStyle() {
        switch(priorityLevel) {
            case 1:
                return "p1";
            case 2:
                return "p2";
            case 3:
                return "p3";
            case 4:
                return "p4";
            case 5:
                return "p5";
        }
    }

    function addTask() {
        if (taskItem == '') {
            return;
        }
        $taskList = [...$taskList, {
            text: taskItem,
            priority: priorityStyle()
        }];
        taskItem = '';
        updateLists();
    }

    function taskCompleted(index) {
        $completedList = [$taskList[index], ...$completedList];
        $completedList = $completedList;
        $taskList.splice(index, 1);
        $taskList = $taskList;
        updateLists();
    }

    function priorityEdit(index) {
        $taskList[index].priority = priorityStyle();
        updateLists();
    }

    function deleteTask(index) {
        $completedList.splice(index, 1);
        $completedList = $completedList;
        updateLists();
    }

    function moveTask(direction, index) {
        if (direction == "up" && index !== 0) {
            tempItem = $taskList[index-1];
            $taskList[index-1] = $taskList[index];
            $taskList[index] = tempItem;
            updateLists();
        }
        else if (direction == "down" && index !== $taskList.length) {
            tempItem = $taskList[index+1];
            $taskList[index+1] = $taskList[index];
            $taskList[index] = tempItem;
            updateLists();
        }
    }

    function clearCompleted() {
        $completedList = [];
        updateLists();
    }
</script>

<div class="mainDiv">
    <h1>ORDERS</h1>
    <h2>Your overlord has dictated that
        <br><u>YOU MUST COMPLETE THESE TASKS.</u> 
    </h2>

    <hr>

    <form on:submit|preventDefault={addTask}>
        <h2>Priority Level:</h2>
        <label class="customRadio">
            <input type="radio" class="priorityRadio" name="taskPriority" value={1} bind:group={priorityLevel}>
            <div class="customButton"></div>
        </label>
        <label class="customRadio">
            <input type="radio" class="priorityRadio" name="taskPriority" value={2} bind:group={priorityLevel}>
            <div class="customButton"></div>
        </label>
        <label class="customRadio">
            <input type="radio" class="priorityRadio" name="taskPriority" value={3} bind:group={priorityLevel}>
            <div class="customButton"></div>
        </label>
        <label class="customRadio">
            <input type="radio" class="priorityRadio" name="taskPriority" value={4} bind:group={priorityLevel}>
            <div class="customButton"></div>
        </label>
        <label class="customRadio">
            <input type="radio" class="priorityRadio" name="taskPriority" value={5} bind:group={priorityLevel}>
            <div class="customButton"></div>
        </label>
        <input class="orderText" type="text" bind:value={taskItem} placeholder="Enter orders here.">
        <button class="addOrder rightButton" type="submit">Add Order</button>
    </form>

    <hr>

    <ul class="ordersWaiting">
        {#each $taskList as item, index}
            <li>
                <button on:click={() => priorityEdit(index)}>Change Priority</button>
                <span class="taskText {item.priority}">{item.text}</span>
                <button class="rightButton" on:click={() => taskCompleted(index)}>Task Complete</button>
                <button class="rightButton arrowButton" on:click={() => moveTask("up", index)}>↑</button>
                <button class="rightButton arrowButton" on:click={() => moveTask("down", index)}>↓</button>
            </li>
        {/each}
    </ul>

    <ul class="ordersComplete">
        {#each $completedList as item, index}
            <li>
                <button class="deleteButton" on:click={() => deleteTask(index)}>X</button>
                <span class="done taskText">{item.text}</span>
            </li>
        {/each}
        <li class="clearButton">
            <button on:click={() => clearCompleted()}>Clear All Completed Tasks</button>
        </li>
    </ul>
</div>