## Summary
When `getDisplayMedia()` is called, the browser offers the user a choice of display surfaces - tabs, windows and monitors. Using the displaySurface constraint, the Web application may now hint to the browser if it prefers that a certain surface type be more prominently offered to the user.

* The relevant spec-change is [here](https://github.com/w3c/mediacapture-screen-share/pull/186/files).
* The resulting text in the spec is [here](https://w3c.github.io/mediacapture-screen-share/#dfn-displaysurface). (The part that reads “As a constraint, the value signals the application's preference...”)

## Motivation
* Less friction for user journeys that are tied to specific pairings between the capturing Web application and the captured Web application.
* The default Chrome implementation as of the time of this writing presents the current monitor as the most prominent option. Using this API, an application can nudge the user in another direction; any other direction is an improvement privacy-wise. This is a boon to reputable video-conferencing applications, who do not wish to see their users accidentally overshare their private information with remote participants. (Plans to change this default in Chrome are progressing independently.)

## Activation
```js
// Offer the user tabs most prominently:
// Note that “browser” in place of “tab” is an artifact
// of the values of this enum.
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: {displaySurface: “browser”}});

// Offer the user windows most prominently:
const stream = await navigator.mediaDevices.getDisplayMedia({
  video: {displaySurface: “window”}});
```

## Common Questions
### 1. Would this behave differently once the default order of the media-picker changes in Chrome?
* **Prefer-tab** and **prefer-window** are intended to always be supported.
* **Prefer-monitor** is debatable and is undergoing internal discussions. Presently it is supported in Chrome, as it represents no change from the status quo there.
