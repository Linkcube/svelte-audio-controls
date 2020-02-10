# Svelte-audio-controls

Simple wrapper around the audio tag to provide the same gui controls across browsers, exposes some of the events/values so far as well as some styling options.

Until I get a static site up with component demos: https://twitter.com/Linkcube2/status/1226616117428281344

## Features

Takes in an audio source, will expose the internal audio element as well as some of the bind-able events (i.e play/ended).
Includes:
- Play/pause button
- Song progress bar with click to seek
- Volume mute button
- Volume progress bar with click to seek
- current time / total duration text
- Tooltip on song bar hover with the hovered time

## Styling

This part is going to be fairly rough until I figure out a standard for themeing across my components, but for now it exposes iconColor/textColor/barPrimaryColor/barSecondaryColor/backgroundColor on creation.

## Usage

```
let src = "my_file.mp3";
<AudioControls
	{src}
	bind:this={controls}
	bind:audio
	bind:paused
	bind:duration
	on:play={stopOthers}
	on:ended={playNext}
	iconColor={$sub_text_color}
	textColor={$sub_text_color}
	barPrimaryColor={$highlight}
	barSecondaryColor={$hover_color}
	backgroundColor={$background}
></AudioControls>
```

`controls.hide(); controls.show(); controls.audio.pause()`

# Todo

- Better theming/styling access
- Sizing

# Links
[npm](https://www.npmjs.com/package/svelte-audio-controls)
[github](https://github.com/Linkcube/svelte-audio-controls)
