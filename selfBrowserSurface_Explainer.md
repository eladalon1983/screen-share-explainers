## Summary
When an application calls getDisplayMedia(), a list of browser surfaces is offered to the user. This list contains screens, windows and tabs. Normally, the current tab is among those tabs.

We add the ability for Web applications to provide a hint, indicating to the user agent whether the application wishes the current tab to be offered to the user.

* The relevant spec-change is [here](https://github.com/w3c/mediacapture-screen-share/pull/216/files).
* The resulting text in the spec is [here](https://w3c.github.io/mediacapture-screen-share/#dom-displaymediastreamconstraints-selfbrowsersurface).

## Motivation
Accidental self-capture is a common problem for video conferencing software. When users accidentally choose the tab in which the VC app is running, a Hall-of-Mirrors effect is produced, confusing users and derailing discussions with remote users.

![image1](https://user-images.githubusercontent.com/22117736/185492428-0c00d65a-86c7-4d73-beb1-12969e95f705.png)


Eliminating this would improve the lives of users and Web-developers alike.

![image2](https://user-images.githubusercontent.com/22117736/185492438-115e1547-9f4a-46ae-8920-84153b5e598c.png)

## Activation
```js
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: true,
  selfBrowserSurface: "exclude"  // Or include.
});
```

## Security Considerations
The current tab is the surface most under an attacker’s control. Nudging the user away from this risky surface is a good thing. Of course, malicious applications can avoid using this new control, or use it to retain the old behavior (current tab still offered). This is not a problem - it simply means that this new surface offers no degradation in security, although not a security feature in its own right.
Common Questions
### 1. What if video is not requested?
That’s not really possible. It has been previously specified that calls to getDisplayMedia() return a rejected Promise if video is not requested.
### 2. What about other forms of self-capture?
Out of scope for the time being. But the option has been raised, and may be discussed again in the future
### 3. What if I don’t specify an explicit value?
It’s implementation-dependent. To minimize disruption to existing applications, Chrome will initially default to the pre-existing behavior - including the current tab among the options offered to the user.

