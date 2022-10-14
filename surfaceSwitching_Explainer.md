## State of the Art (in Chrome)
When an application calls getDisplayMedia(), and the user chooses to capture a tab, Chrome displays an tab-capture infobar right below the omnibar. When a tab other than the capturing/captured tab is focused, the only button in this infobar is [Stop sharing].

![image1](https://user-images.githubusercontent.com/22117736/185493207-dd7a5e46-d627-47fc-8e0b-8a7dfe568f3a.png)

If using the Extension API for screen-sharing, another button is exposed - [Share this tab instead].

![image2](https://user-images.githubusercontent.com/22117736/185493220-bae676bf-981d-416a-85ad-0fc31125d4f4.png)

Pressing this button changes the target of the tab-capture to the other tab.

![image3](https://user-images.githubusercontent.com/22117736/185493245-1476a378-89dd-417d-b16f-08ddc14b43b6.png)

## Innovation
We now expose this functionality to the Web at large. Web applications can control whether this button is shown by specifying a value to `DisplayMediaStreamConstraints.surfaceSwitching`.

* The relevant spec-change is [here](https://github.com/w3c/mediacapture-screen-share/pull/225/files).
* The resulting text in the spec is [here](https://w3c.github.io/mediacapture-screen-share/#dom-displaymediastreamconstraints-surfaceswitching).

## Motivation
The [Share this tab instead] button allows users to seamlessly switch which tab they’re sharing, without having to select the video-conferencing tab again (1), click a button to initiate getDisplayMedia() again (2), and selecting a new tab out of a long list of tabs again (3).

This behavior is exposed conditionally because not all Web applications are able to handle this behavior. Most notably, applications centered around self-capture and cropping might have trouble. For a more elaborate discussion, see [this GitHub thread](https://github.com/w3c/mediacapture-screen-share/issues/223).

## Activation
```js
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: true,
  surfaceSwitching: "include"  // Or exclude.
});
```

## Common Questions
### 1. What if my application does not specify an explicit value?
Chrome will try to guess what’s best for your application. This guess may change between milestones. Initially:
* If the user chooses the current tab, the [Share this tab instead] button will **not** be exposed.
* If the user chooses any other tab, the [Share this tab instead] button **will** be exposed.
### 2. What about other surfaces? Windows? Monitors?
At the moment, Chrome only supports fast-switching between shared tabs. In the future, hopefully more.
