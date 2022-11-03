<script>
	import { onMount } from "svelte";
	import { Confetti } from "svelte-confetti"





    let input = "";

    let commands = [];

    let password = "Reid is awesome"

    let showConfetti = false;

    function submitCommand(e){
        if(input.length > 0){
            commands.push({text:input});
            commands = commands;


            if(input == password){
                commands.push({class: "text-success", text: "Correct!"})
                showConfetti = true;
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
    <pre data-prefix="$"><code>Please enter the password</code></pre> 

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
        overflow-x: hidden;
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
