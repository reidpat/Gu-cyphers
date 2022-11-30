<script>
	import { onMount } from "svelte";
	import { Confetti } from "svelte-confetti"





    let input = "";

    let commands = [];
    let showConfetti = false;
    let problemIndex = 0;
    let problemStatements = [];
    let problemSolutions = [];
    let maxChar = 100;
    let problemSolIndex = 0;
    //added variable to track what part of the multi step problem we are at

    function submitCommand(e){
        if (Array.isArray(problemSolutions[problemIndex])){
            //added functionality where there may be multiple options that the user has to enter correctly in a row otherwise they have to start over
            //if the problem solution is an array, it means it will be multi step problem
            if(input.length > 0){
            commands.push({text:input});
            commands = commands;

            if(input.toLowerCase() == problemSolutions[problemIndex][problemSolIndex]){
                // added to LowerCase to catch accidental uppercases
                if(problemSolIndex==problemSolutions[problemIndex].length-1){
                    //only when we reach the end of the multi step problem do we move on
                    addCommand("text-success", "Correct!!");
                    problemIndex++;
                }
                else{
                    problemSolIndex++;
                    addCommand("text-successs", "Correct!");
                    addCommand("text-warning", problemStatements[problemIndex][problemSolIndex]);
                    //will display next part of question
                    
                }
            }
            else{
                // commands.push({class: "text-error", text: "Incorrect"})
                addCommand("text-error", "Incorrect, start from beginning of problem");
                problemSolIndex=0;
                //if the answer is wrong, they have to restart
            }

        input = "";
        }
            
        focusText();

        }
        else{
        //if it is not a multi step problem then the code is that same as it was
            if(input.length > 0){
            commands.push({text:input});
            commands = commands;


            if(input.toLowerCase() == problemSolutions[problemIndex]){
                //added to catch accidental uppercases
                // commands.push({class: "text-success", text: "Correct!"})
                addCommand("text-success", "Correct!");
                problemIndex++;

            }
            else{
                // commands.push({class: "text-error", text: "Incorrect"})
                addCommand("text-error", "Incorrect");
            }

            input = "";
        }
        focusText();
        }
        if(problemIndex < problemStatements.length){
            // commands.push({class: "text-warning", text: problemStatements[problemIndex]})
            if (Array.isArray(problemStatements[problemIndex])){
                if (problemSolIndex ==0){
                    //will display beginning of multistep question only if it is the beginning of the question
                    addCommand("text-warning", problemStatements[problemIndex][0]);
                }
                // Check if there will be multiple problemStatements in one problem. If problemStatements is an array with multiple statements then will only display the first
            }
            else{
                addCommand("text-warning", problemStatements[problemIndex]);
            }
            
            }
        else {
            // commands.push({class: "text-success", text: "You have solved all the problems!"})
            addCommand("text-success", "You have solved all the problems!");
            showConfetti = true;
            }
    }

    //given a class and a text problem, split it up across multiple commands as to preserve line spacing
    //Then add the command to the console
    function addCommand(c, t){
        let textArray = [];
        if(t.length > maxChar){
            let wordArray = t.split(" ");
            let reducedCommand = "";
            wordArray.forEach(word => {
                if(reducedCommand.length + word.length < maxChar){
                    reducedCommand += word + " ";
                }
                else{
                    textArray.push(reducedCommand);
                    reducedCommand = word + " ";
                }
            });
            textArray.push(reducedCommand);
        }
        else{
            textArray.push(t);
        }
        
        textArray.forEach(text => {
            commands.push({class: c, text});
        });
        commands = commands;
    }

    function determineWidth(){
        let clientWidth = document.getElementById("terminal").clientWidth;

        maxChar = 0;
    }


    let textElement;
    onMount(()=>{
        textElement = document.getElementById("codeInput");
        focusText();

        
        
        //Format for adding problems:
        //problemStatements.push("Problem Hint/Statement")
        //problemSolutions.push("Problem Answer") <- case sensitive
        //repeat above two lines for each problem

        //Example problems:
        problemStatements.push("Find the hidden password on the page and enter it below")
        problemSolutions.push("you can't find me")
        problemStatements.push("OY SGEUTTGOYK GT OTYZXASKTZ (C5)");
        problemSolutions.push("is mayonnaise an instrument");
        problemStatements.push("What color is hidden in this sentence:  \n'Temper or anger are signs of weakness'");
        problemSolutions.push("orange");
        problemStatements.push("A red house is made from red bricks. A blue house is made from blue bricks. A yellow house is made from yellow bricks. What is a green house made from?")
        problemSolutions.push("glass")
        problemStatements.push("These next problems all have to be answered in a row. You can look at the handout for more information on the question. If you mess up, you have to restart from here. Type yes to begin")
        problemSolutions.push("yes")
        problemStatements.push(["Question 1. Decode the message","Question 2. How many hexagons can you find?","Question 3. Which is these figures is wrong","Question 4. What time was earl in the room", "What room was Tess in(just the number)","What room was Grad in(just the number)","Question 5. Who came in first place?", "Who came in second place", "Who came in last place"])
        problemSolutions.push(["hello","21","b","11:20", "215", "202", "johannes", "rene", "louis"])

        //Begin program by showing the first problem statement
        addCommand("text-warning", problemStatements[0])

        
        
    })


    function focusText(){
        textElement.focus();
    }

            

</script>

<div class="flex flex-col justify-center items-center min-h-screen">
<h1 class="text-3xl font-bold text-center">
    Geering Up Hacker Database
</h1>

<p class="password">Password is "you can't find me"</p>

<div class="mockup-code mt-10 h-96" id="terminal" on:click={focusText} on:keydown={focusText}>
    <pre data-prefix="$"><code>GU Hacker Terminal</code></pre> 

    {#each commands as command}
        <pre data-prefix=">" class={command.class}><code>{command.text}</code></pre> 
    {/each}
    

    {#if !showConfetti}    
    <form on:submit={()=> {event.preventDefault(); submitCommand(event)}} action="" preventDefault>
    <!-- svelte-ignore a11y-autofocus -->
    <pre data-prefix=">"><code><input id="codeInput" type="text" name="input" bind:value={input} autofocus autocomplete="off"></code></pre>
        
    </form>
    {/if}
    
    
  </div>
  <p class="text-xs" id="signature">Made by Reid and Marcus</p>
</div>


{#if showConfetti}
    <div id="confetti">
            <Confetti infinite />
        </div>
        {/if}




<style>
    #codeInput{
        background-color: hsl(var(--n));
    }
    #codeInput:focus{
        outline: none;
    }
    .mockup-code{
        overflow-y: scroll;
        overflow-x: auto;
        max-width: 1200px;
        width: 80%;
        min-width: 500px;
        
    }
    .mockup-code::-webkit-scrollbar {
        width: 1em;
}
 
.mockup-code::-webkit-scrollbar-track {
   background-color: hsl(var(--nf));
   border-radius: 10px;
}
 
.mockup-code::-webkit-scrollbar-thumb {
    background-color: hsl(var(--b3));
    border-radius: 10px;
}
.password{
    color: hsl(var(--b1));
}
#confetti{
    position: absolute;
    top: 50%;
    left: 50%;
}
</style>
