## Summary
We add the ability for Web application to provide a hint, indicating to the user agent whether the application, upon calling `getDisplayMedia({audio: true})` or similar input, wishes *system audio* to be offered to the user. (If not - the application only wishes for tab-audio to be offered, or no audio at all.)

* The relevant spec-change is [here](https://github.com/w3c/mediacapture-screen-share/pull/222/files).
* The resulting text in the spec is [here](https://w3c.github.io/mediacapture-screen-share/#dom-displaymediastreamconstraints-systemaudio).

## Motivation
User agents may offer audio to be captured alongside video, if the application specifies {audio: true}, or maps audio to anything else that's different from false. But not all audio is created alike. Consider video-conferencing applications. Tab-audio is often useful, and can be shared with remote participants. But system-audio includes participants' own audio, and it is NOT desirable to share it back.

State of the art? VC applications can ask for "audio", use it if it's tab-audio, and discard it otherwise. This works, but it's sub-optimal. The user is left confused. The user wanted to share system-audio, the user was offered to share-system, the user explicitly approved sharing system-audio - and now remote participants are telling the user that they can't, in fact, hear the system-audio. Now how confusing is that?!

It’s better to allow applications to ask for less - allow the application to ask for non-system audio.

## Activation
```js
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: true,
  audio: true,
  systemAudio: "exclude"  // Or include.
});
```

## Result (Chrome)
![image1](https://user-images.githubusercontent.com/22117736/185494241-11988b5c-806b-4f7b-a057-ed4004fd5c6b.png)
![image2](https://user-images.githubusercontent.com/22117736/185494939-51c12aba-19dc-4c04-a015-0c608edb5744.png)


## Common Questions
### 1. I don’t see system-audio anyway.
In Chrome, system-audio is only supported on some platforms. At the time of this writing, only on Windows.
### 2. What if audio is not requested?
Then this flag would have no effect.
### 3. Why not allow specifying which audio is desired?
Originally, a more general mechanism was suggested, where the application would say which types of audio it is interested in potentially receiving from the user. However, the WebRTC Working Group was concerned that allowing an application to only ask for system-audio, might let applications nudge their users towards sharing more than they otherwise would. This more limited mechanism was therefore chosen.
### 4. What if I don’t specify an explicit value?
It’s implementation-dependent. To minimize disruption to existing applications, Chrome currently defaults to the existing behavior - system-audio is still offered (on [supported platforms](https://docs.google.com/document/d/1q3oGy7hLJmdQA4ZK7QG7DnwgtcpL6oB2pqLQJ6MP1tY/edit#heading=h.1tj3sdscfzgt)).
