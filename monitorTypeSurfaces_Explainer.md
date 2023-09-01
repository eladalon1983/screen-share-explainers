![image](https://github.com/eladalon1983/screen-share-explainers/assets/22117736/32fc8c10-71f5-433b-9ebc-2a8a65d20c39)## Summary
When `getDisplayMedia()` is called, the browser offers the user a choice of display surfaces: tabs, windows, or monitors. Using the `monitorTypeSurfaces` option, the web application may now hint to the browser if it prefers to include display surfaces whose type is monitor among the choices offered to the user. The browser may still ignore this hint. Web applications are therefore encouraged to still check the displaySurface setting of the tracks they receive.

* The relevant spec-change is [here](https://github.com/w3c/mediacapture-screen-share/pull/274/files).
* The resulting text in the spec is [here](https://w3c.github.io/mediacapture-screen-share/#dom-displaymediastreamoptions-monitortypesurfaces).

## Motivation
* Protect the user from the pitfall of choosing a source which the app will reject eventually.
* Protect companies from leakage of private information through employee-error.

## Activation
```js
// Hint the browser to exclude monitors from the choices offered to the user.
const stream = await navigator.mediaDevices.getDisplayMedia({
  monitorTypeSurfaces: "exclude",
});

// We still need to check though...
const { displaySurface } = stream.getTracks()[0].getSettings();
if (displaySurface == "monitor") {
  throw new Error('User has picked a monitor.');
}
```

## Result (Chrome)
### monitorTypeSurfaces: "include"
![image](https://github.com/eladalon1983/screen-share-explainers/assets/22117736/667a7bdb-025e-4060-86cf-cbd0743c9099)

### monitorTypeSurfaces: "exclude"
![image](https://github.com/eladalon1983/screen-share-explainers/assets/22117736/0c45a0df-49d7-4d4f-8e2d-729778686253)

