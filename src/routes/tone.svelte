<script>
    import { onMount } from "svelte";
    import * as Tone from "tone";
    let props = $props();
    let toneColorMap = {
        "C" : "Red",
        "C#": "Blue",
        "D": "Purple",
        "D#": "Magenta",
        "E": "Yelow",
        "F": "greenyellow",
        "F#": "Hotpink",
        "G": "Orangered",
        "G#": "Khaki",
        "A": "Azure",
        "A#": "cadetBlue",
        "B": "skyblue",
        "none": ""
    }    
    let started = $state(false);
    let color = $derived(toneColorMap[props["tone"]]);
    let container;
    let audioElement;
    let colorOpacity = $state(1);
	let delay= $state(0.0);
    let difficultyTimeMap = { "easy": 7, "medium": 4, "hard": 2 };
    let opacity = $derived((difficultyTimeMap[props["difficulty"]] - 1)/ 30);
    let synth;
    
    onMount(() => {
        console.log("mounted");
        synth = new Tone.Synth().toDestination();
    });
	setInterval(() => {
		if (delay > 0){
             delay -= 0.1;
			delay = delay.toFixed(1);
        } else {

        }
		}, 100);
    function triggerAnimation() {
        started = true;
        // alert("animationTriggered");
        delay = difficultyTimeMap[props["difficulty"]];
        playTone();
        colorOpacity = 0;
        if (container.classList.contains("flickerGrowAnimation")) {container.classList.remove("flickerGrowAnimation");}

        setTimeout(() => {
            container.classList.add("flickerGrowAnimation");
            colorOpacity = 1;
        }, (delay-1)*1000);
        // alert("Triggered Animation");
        // containerStyle = `
        // animation: flickerGrow 10s cubic-bezier(.96,0,.98,.56);
        // animation-fill-mode: forwards;`;
    }

    function playTone() {
        let tone = props["tone"];
        let duration = delay;
        audioElement.play();
        synth.triggerAttackRelease(tone+((Math.random()*3)+2), duration+0.5);
    }
    // triggerAnimation();
    export { triggerAnimation };
</script>


<div class="container flickerGrowAnimation" bind:this={container} style="opacity: {opacity};">
    <div class="color" style="background-color: {color}; opacity: {colorOpacity};"></div>
</div>
<audio src="/silent.mp3" bind:this={audioElement}></audio>
{#if started}
{#if (delay > 0)}

<span>Reveal in {delay}s...</span>
    {:else}
    <span>{props["tone"]}, {toneColorMap[props["tone"]]}</span>
    {/if}
{/if}
<style>
    :root {
        --blur: 20px;
        --diameter: 100px;
    }
    /* .hidden{
        display: none;
    } */
    .container {
        border-radius: 50%;
        background-color: rgba(0, 0, 0, 0.25);
        width: fit-content;
        -webkit-box-shadow: 0px 0px 42px 10px rgba(0,0,0,0.35); 
box-shadow: 0px 0px 42px 10px rgba(0,0,0,0.35);
        padding: 3em;
        opacity: 0;
        z-index: -1;
        /* opacity: 1; */
    }
    .flickerGrowAnimation {
        animation: flickerGrow 2s cubic-bezier(.96,0,.98,.56);
        animation-fill-mode: forwards;
    }

    @keyframes flickerGrow {
        0% {
            opacity: 0;
        }
        10% {
            opacity: 0.1;
        }
        12% {
            opacity: 0.06;
        }
        15% {
            opacity: 0.15;
        }
        20% {
            opacity: 0.2;
        }
        25% {
            opacity: 0.18;
        }
        30% {
            opacity: 0.3;
        }

        100% {
            opacity: 1;
        }
    }
    .color {
  -webkit-filter: blur(var(--blur));
  -moz-filter: blur(var(--blur));
  -o-filter: blur(var(--blur));
  -ms-filter: blur(var(--blur));
  filter: blur(var(--blur));
  width: var(--diameter);
    height: var(--diameter);
    border-radius: 50%;
}
</style>
