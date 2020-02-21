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
></AudioControls>
```

Make sure to include `<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">` in your application for the icons.

Recommended width is 400px to 900px

Exposed Functions:
Function | Description
-- | --
`show()` | Display the controls
`hide()` | Hide the controls

Exposed Events:
Event | Description
-- | --
`on:play` | When the audio is played
`on:ended` | The audio has reached the end of it's buffer

Bindings:
Name | Description
-- | --
`bind:audio` | Internal audio element
`bind:paused` | The paused value of the audio
`bind:duration` | Duration of the audio element
`bind:muted` | Muted state of the audio element
`bind:volume` | Volume state of the audio

Optional settings:
Param | Description | Type
--- | --- | ---
`inlineTooltip`| Keeps the tooltip on the controls bar rather than hovering. | Boolean
`disableTooltip`| Disables the tooltip completely. | Boolean
`display` | Whether to show the controls initially | Boolean
`iconColor` `textColor` `barPrimaryColor` `barSecondaryColor` `backgroundColor` | Change the color of the elements in the controls | CSS colors
`preload` | Preload state for the audio component | Refer to the audio element for this, e.x: `"metadata"`


# Todo

- Better theming/styling access
- Sizing

# Links
[npm](https://www.npmjs.com/package/svelte-audio-controls)
[github](https://github.com/Linkcube/svelte-audio-controls)
