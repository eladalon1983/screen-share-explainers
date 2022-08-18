## Clarification
`suppressLocalAudioPlayback` is a new member of the [extended MediaTrackSupportedConstraints dictionary](https://w3c.github.io/mediacapture-screen-share/#extensions-to-mediatracksupportedconstraints).

## Summary
Consider a Web application APP which is display-capturing a tab TAB. We add a mechanism by which APP may control whether the audio playing in TAB would be played out of the user’s local speakers.

* The relevant spec-change is [here](https://github.com/w3c/mediacapture-screen-share/pull/164/files).
* The resulting text in the spec is [here](https://w3c.github.io/mediacapture-screen-share/#dom-mediatracksupportedconstraints-suppresslocalaudioplayback).

## Motivation
A common scenario in corporate settings is for several users to gather in a shared physical room, and for one of them to use their laptop and VC software to share a tab to a large monitor in that room. Often, audio is also played out over the room’s speakers. (Because these speakers are typically louder than the laptop’s speakers, and because this way, audio is in-sync with the video played out over the monitor.)

## Activation
```js
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: true, audio: true});
const [track] = stream.getAudioTracks();
track.applyConstraints({suppressLocalAudioPlayback: true});
```

## Common Questions
### 1. How are multiple concurrent captures handled?
This constraint can only be used to suppress, not to “unsuppress.” In the case of multiple concurrent captures of the same tab, the tab’s local audio playback will be suppressed if there is at least one capture session which is asking to suppress.
### 2. Is it possible to observe the state of the tab’s local audio playback?
No, and that’s by design, because:
* In the case of multiple concurrent captures, we don’t want to leak information between sessions.
* The local audio playback state of a tab may be affected by the user via browser-level controls, and we don’t want to expose this information.
### 3. So what does track.getConsraints() give me?
It tells you whether you have previously applied this constraint to this track.
It does NOT tell you whether this has had an effect.
