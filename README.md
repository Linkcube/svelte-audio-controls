# Svelte-audio-controls

Simple wrapper around the audio tag to provide the same gui controls across browsers, exposes some of the events/values so far as well as some styling options.

Demo: http://linkcube.github.io/

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
	display={true}
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

Make sure to include `<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">` in your application for the icons.

`controls.hide(); controls.show(); controls.audio.pause()`

Recommended width is 400px to 900px

# Todo

- Better theming/styling access
- Sizing

# Links
[npm](https://www.npmjs.com/package/svelte-audio-controls)
[github](https://github.com/Linkcube/svelte-audio-controls)
