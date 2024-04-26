<script>
    import '../style.css';
    let taskItem = '';
    let taskList = [
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
    ];
    let completedList = [
        {
            text: "bow down in worship.",
            priority: 4
        }
    ];
    let priorityLevel = 1;

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
        taskList = [...taskList, {
            text: taskItem,
            priority: priorityStyle()
        }];
        taskItem = '';
        console.log("plevel: " + priorityLevel)
    }

    function taskCompleted(index) {
        completedList = [taskList[index], ...completedList];
        completedList = completedList;
        taskList.splice(index, 1)
        taskList = taskList;
    }

    function priorityEdit(index) {
        taskList[index].priority = priorityStyle();
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
        {#each taskList as item, index}
            <li>
                <button on:click={() => priorityEdit(index)}>Change Priority</button>
                <span class="taskText {item.priority}">{item.text}</span>
                <button class="rightButton" on:click={() => taskCompleted(index)}>Task Complete</button>
            </li>
        {/each}
    </ul>

    <ul class="ordersComplete">
        {#each completedList as item}
            <li>
                <span class="done taskText">{item.text}</span>
            </li>
        {/each}
    </ul>
</div>