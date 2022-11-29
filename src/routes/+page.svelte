<script>
	import { onMount } from "svelte";
	import { Confetti } from "svelte-confetti"





    let input = "";

    let commands = [];
    let showConfetti = false;
    let problemIndex = 0;
    let problemStatements = [];
    let problemSolutions = [];

    let maxChar = 60;

    function updateScroll(){
        textElement.scrollIntoView({behavior: "smooth"});
    }

    function submitCommand(e){
        if(input.length > 0){
            commands.push({text:input});
            commands = commands;


            if(input == problemSolutions[problemIndex]){
                // commands.push({class: "text-success", text: "Correct!"})
                addCommand("text-success", "Correct!");
                problemIndex++;
                if(problemIndex < problemStatements.length){
                    // commands.push({class: "text-warning", text: problemStatements[problemIndex]})
                    addCommand("text-warning", problemStatements[problemIndex]);
                }
                else {
                    // commands.push({class: "text-success", text: "You have solved all the problems!"})
                    addCommand("text-success", "You have solved all the problems!");
                    showConfetti = true;
                }
            }
            else{
                // commands.push({class: "text-error", text: "Incorrect"})
                addCommand("text-error", "Incorrect");
            }

            input = "";
            setInterval(updateScroll, 500);
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
        
        terminalElement.scrollTop = terminalElement.scrollHeight;
        focusText();
    }

    function determineWidth(){
        let clientWidth = document.getElementById("terminal").clientWidth;

        maxChar = 0;
    }


    let textElement;
    let terminalElement;
    onMount(()=>{
        textElement = document.getElementById("codeInput");
        terminalElement = document.getElementById("terminal");
        focusText();

        
        
        //Format for adding problems:
        //problemStatements.push("Problem Hint/Statement")
        //problemSolutions.push("Problem Answer") <- case sensitive
        //repeat above two lines for each problem

        //Example problems:
        problemStatements.push("Find the hidden password on the page and enter it below")
        problemSolutions.push("Reid is awesome")
        problemStatements.push("What is the best summer camp in the world?");
        problemSolutions.push("Geering Up");


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

<p class="password">Password is "Reid is awesome"</p>

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
  <p class="text-xs" id="signature">Made by Reid</p>
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
