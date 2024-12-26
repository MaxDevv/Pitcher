<script>
	import { createEventDispatcher } from 'svelte';
	// let difficulty;
	let difficultyTimeMap = { "easy": 7, "medium": 4, "hard": 2 };
    
	let buttonText = 'Start';
	const dispatch = createEventDispatcher();
	function start() {
		dispatch('next');
		buttonText = 'Next';
		modifiable = false;
		setTimeout(() => {
			modifiable = true;
		}, difficultyTimeMap[difficulty]*1000);
	}
	let modifiable = true;
	export let difficulty;
</script>

<div class="mainContainer flickerGrowAnimation">
	<div class="settings">
		<button on:click={start}  disabled={!modifiable}>{buttonText} :D</button>
		<!-- <div class="subSetting"> -->
		<div>
			Difficulty
			<select name="dificulty" id="" bind:value={difficulty} disabled={!modifiable}>
				<option value="easy">easy</option>
				<option value="medium">medium</option>
				<option value="hard">hard</option>
			</select>
		</div>
		<a href="/faq">How pitcher works</a>
		<!-- </div> -->
	</div>
</div>

<style>
	.mainContainer {
		display: flex;
		/* flex-direction: column; */
		justify-content: center;
		width: 100%;
	}
	.settings {
		width: 50%;
		margin-left: 5%;
		margin-right: 5%;
		border-top-left-radius: 5vw;
		border-top-right-radius: 5vw;
		height: 100%;
		background-color: #7ae7c7;
		-webkit-box-shadow: 0px 0px 42px 10px rgba(0, 0, 0, 0.15);
		box-shadow: 0px 0px 42px 10px rgba(0, 0, 0, 0.15);
		display: flex;
		justify-content: space-evenly;
		flex-direction: column;
		align-items: center;
		padding: 15%;
		padding-top: 0px;
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
</style>
