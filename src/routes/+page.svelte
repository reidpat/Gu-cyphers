<script>
	import { onMount } from "svelte";
	import { Confetti } from "svelte-confetti"





    let input = "";

    let commands = [];
    let showConfetti = false;
    let problemIndex = 0;
    let problemStatements = [];
    let problemSolutions = [];

    function submitCommand(e){
        if(input.length > 0){
            commands.push({text:input});
            commands = commands;


            if(input == problemSolutions[problemIndex]){
                commands.push({class: "text-success", text: "Correct!"})
                problemIndex++;
                if(problemIndex < problemStatements.length){
                    commands.push({class: "text-warning", text: problemStatements[problemIndex]})
                }
                else {
                    commands.push({class: "text-success", text: "You have solved all the problems!"})
                    showConfetti = true;
                }
            }
            else{
                commands.push({class: "text-error", text: "Incorrect"})
            }

            input = "";
        }
        focusText();
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
        problemSolutions.push("Reid is awesome")
        problemStatements.push("What is the best summer camp in the world?");
        problemSolutions.push("Geering Up");


        //Begin program by showing the first problem statement
        commands.push({class: "text-warning", text: problemStatements[0]});
        commands = commands;

        
        
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

<div class="mockup-code w-1/2 mt-10 h-96" on:click={focusText} on:keydown={focusText}>
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
#id {
    position: fixed;
    bottom: -20px;
    left: 50%;
}
</style>
