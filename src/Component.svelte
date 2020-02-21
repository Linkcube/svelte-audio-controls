<svelte:options accessors/>

<script>
    import { createEventDispatcher } from 'svelte';
    import { fade } from 'svelte/transition';
    import { spring } from 'svelte/motion';
    
    export let src;
    export let audio = null;
    export let paused = true;
    export let duration = 0;
    export let muted = false;
    export let volume = 1;
    export let preload = "metadata";
    export let iconColor = "gray";
    export let textColor = "gray";
    export let barPrimaryColor = "lightblue";
    export let barSecondaryColor = "lightgray";
    export let backgroundColor = "white";
    export let display = false;
    export let inlineTooltip = false;
    export let disableTooltip = false;

    const dispatch = createEventDispatcher();
    let currentTime = 0;
    let tooltip;
    let tooltipX = 0;
    let tooltipY = 0;
    let showTooltip = false;
    let seekText = "";
    let seeking = false;
    let volumeSeeking = false;
    let songBar;
    let volumeBar;

    export function hide() {
        display = false;
    }

    export function show() {
        display = true;
    }

    function seek(event, bounds) {
        let x = event.pageX - bounds.left;
        return Math.min(Math.max(x / bounds.width, 0), 1);
    }

    function seekAudio(event) {
        if (!songBar) return;
        audio.currentTime = seek(event, songBar.getBoundingClientRect()) * duration;
    }

    function seekVolume(event) {
        if (!volumeBar) return;
        volume = seek(event, volumeBar.getBoundingClientRect());
        audio.volume = volume;
        muted = false;
    }

    function formatSeconds(seconds) {
		if (isNaN(seconds)) return "No Data";
		var sec_num = parseInt(seconds, 10)
		var hours   = Math.floor(sec_num / 3600)
		var minutes = Math.floor(sec_num / 60) % 60
		var seconds = sec_num % 60

		return [hours,minutes,seconds]
			.map(v => v < 10 ? "0" + v : v)
			.filter((v,i) => v !== "00" || i > 0)
			.join(":")
    }

    function seekTooltip(event) {
        if (!inlineTooltip) {
            let tooltipBounds = tooltip.getBoundingClientRect();
            tooltipX = event.pageX - tooltipBounds.width - 10;
            tooltipY = songBar.offsetTop + 10;
        }
        let bounds = songBar.getBoundingClientRect();
        let seekValue = (event.pageX - bounds.left) * duration / bounds.width;
        seekText = formatSeconds(seekValue);
    }

    function trackMouse(event) {
        if (seeking) seekAudio(event);
        if (showTooltip && !disableTooltip) seekTooltip(event);
        if (volumeSeeking) seekVolume(event);
    }
</script>

<svelte:window
    on:mouseup={() => seeking = volumeSeeking = false}
    on:mousemove={trackMouse}
/>

<style>
    .controls {
        display: flex;
        flex-flow: row;
        justify-content: space-around;
        color: var(--color);
        background-color: var(--background-color);
        padding-left: 10px;
        padding-right: 10px;
        -webkit-user-select: none; /* Safari */
        -ms-user-select: none; /* IE 10+ and Edge */
        user-select: none; /* Standard syntax */
        padding-top: 5px;
        padding-bottom: 5px;
    }

    .control-times {
        margin: auto;
        margin-right: 5px;
    }

    .tooltip {
        background-color: var(--background-color);
        padding: 1px;
        border-radius: 5px;
        border-width: 3px;
        box-shadow: 6px 6px var(--box-color);
        color: var(--text-color);
        pointer-events: none;
        min-width: 50px;
        text-align: center;
        margin-bottom: 5px;
    }

    .hover-tooltip {
        position: absolute;
        top: var(--top);
        left: var(--left);
    }

    .material-icons {
        font-size: 16px;
        margin-bottom: 0px;
        color: var(--icon-color);
        background-color: rgba(0,0,0,0);
        cursor: pointer;
        transition: 0.3s;
        border: none;
        border-radius:25px;
    }

    .material-icons:hover {
        box-shadow: 0px 6px  rgba(0,0,0,0.6);
    }

    .material-icons::-moz-focus-inner {
        border: 0;
    }

    progress {
		display: block;
        color: var(--primary-color);
        background: var(--secondary-color);
        border: none;
        height: 15px;
        margin: auto;
        margin-left: 5px;
        margin-right: 5px
    }
    
    progress::-webkit-progress-bar {background-color: var(--secondary-color); width: 100%}

    progress::-moz-progress-bar { background: var(--primary-color); }

    progress::-webkit-progress-value { background: var(--primary-color); }

    .song-progress {
        width: 100%;
    }

    .volume-progress {
        width: 10%;
        max-width: 100px;
        min-width: 50px;
    }
</style>

{#if display}
    <div class="controls" style="--color:{textColor}; --background-color:{backgroundColor}">
        <button
            class="material-icons"
            style="--icon-color:{iconColor}"
            on:click={() => audio.paused ? audio.play() : audio.pause()}>
            {#if paused}
                play_arrow
            {:else}
                pause
            {/if}
        </button>
        <progress
            bind:this={songBar}
            value={currentTime ? currentTime : 0}
            max={duration}
            on:mousedown={() => seeking = true}
            on:mouseenter={() => showTooltip = true}
            on:mouseleave={() => showTooltip = false}
            on:click={seekAudio}
            style="--primary-color:{barPrimaryColor}; --secondary-color:{barSecondaryColor}"
            class="song-progress"
        ></progress>
        <div class="control-times">{formatSeconds(currentTime)}/{formatSeconds(duration)}</div>
        <button
            style="--icon-color:{iconColor}"
            class="material-icons" on:click={() => muted = !muted}>
            {#if muted}
                volume_off
            {:else if volume < .01}
                volume_mute
            {:else if volume < .5}
                volume_down
            {:else}
                volume_up
            {/if}
        </button>
        <progress
            bind:this={volumeBar}
            value={volume}
            on:mousedown={() => volumeSeeking = true}
            on:click={seekVolume}
            style="--primary-color:{barPrimaryColor}; --secondary-color:{barSecondaryColor}"
            class="volume-progress"
        ></progress>
        {#if !disableTooltip && (inlineTooltip || showTooltip)}
            <div
                class:hover-tooltip={!inlineTooltip}
                transition:fade
                bind:this={tooltip}
                class="tooltip"
                style="--left:{tooltipX}px;
                --top:{tooltipY}px;
                --background-color:{backgroundColor};
                --box-color:{barSecondaryColor};
                --text-color:{textColor}">
                {#if showTooltip}
                    {seekText}
                {:else}
                    {#if duration > 3600}
                        --:--:--
                    {:else}
                        --:--
                    {/if}
                {/if}
            </div>
        {/if}
    </div>
{/if}

<audio
	bind:this={audio}
	bind:paused
	bind:duration
    bind:currentTime
    {muted}
    {volume}
	on:play
	on:ended
	{src}
	{preload}
></audio>