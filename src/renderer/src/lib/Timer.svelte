<script>
    let cycle_on = false;
    let break_on = false;
    let started = false;

    export let events;
    export let handleChangeEvents;

    let timer_counter_value = new Date(3600000);
    let intervalId = 0

    $: timeNow = timer_counter_value.toISOString().substring(11,19);
    $: start_stop = "Start";
    $: work_break = break_on ? "Break" : "Working";

    export let numOfDoneEtc;
    let numOfDoneEventsBasedByTimeButInTimerLib = numOfDoneEtc;

    function reset(){
        stopInterval();
        if(!break_on){
            timer_counter_value = new Date(3600000); //new Date(10000)
        }
        else{
            timer_counter_value = new Date(1500000); //new Date(5000)
        }
    }

    function start_interval(){
        if(!started){
            start_stop = "Stop";
            intervalId = setInterval(function(){
                timer_counter_value.setTime(timer_counter_value.getTime() - 1000);
                timer_counter_value = timer_counter_value;

                if(timer_counter_value.getTime() == 0){
                    if(!break_on){
                        if(events.length > numOfDoneEventsBasedByTimeButInTimerLib){
                            numOfDoneEventsBasedByTimeButInTimerLib += 1;
                        }
                        handleChangeEvents(numOfDoneEventsBasedByTimeButInTimerLib);
                    }
                    stopInterval();
                    if(cycle_on){
                        break_on = !break_on;
                        reset();
                        start_interval();
                    }
                }
            }, 1000);
            started = true;
        }
        else{
            stopInterval();
        }
    }

    function stopInterval(){
        clearTimeout(intervalId);
        started = false;
        start_stop = "Start";
    }

</script>

<div class="container">
    <h2 class="header-info">{work_break} time!</h2>
    <p class="timer-counter">{timeNow}</p>
    <label><input type="checkbox" name="cycle" on:change={() => {
        cycle_on = !cycle_on;
    }}>When cycle ends go to another one?</label>
    <div>
        <button type="button" on:click={start_interval}>{start_stop}</button>
        <button type="button" on:click={reset}>Reset</button>
    </div>
</div>

<style>
    .container{
        background-color: rgb(0, 128, 187);
        color: white;
        padding: 10px;
        text-align: center;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        gap: 10px;
        margin-bottom: 20px;
        border-radius: 10px;
    }
    .timer-counter{
        font-size: 24px;
        font-weight: bold;
    }

    button {
		min-width: fit-content;
		max-width: 250px;
		width: 10vw;
		margin: 5px;
		margin-left: auto;
		background-color: lightcoral;
		border: 0;
		height: 80%;
		cursor: pointer;
        color: white;
        padding: 10px;
        border-radius: 10px;
	}

    button:hover{
        background-color: rgb(233, 79, 79);
    }
</style>