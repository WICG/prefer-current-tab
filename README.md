Sites often wish to self-capture. For example, a slides deck application might wish to let the user stream the presentation to a virtual conference.

Calling getDisplayMedia offers the user a wide selection of possible capture-sources. What if the application really just wants the current tab? It could be hard for the user to hunt down the specific tab out of all the tabs they have open.

Ideally, the application would be able to present a confirmation-only dialog to the user - share the current tab, yes/no? Standardization efforts for this feature as [getViewportMedia](https://github.com/w3c/mediacapture-screen-share/pull/148) are underway.

However, [getViewportMedia](https://github.com/w3c/mediacapture-screen-share/pull/148) will be gated by (1) cross-origin isolation and (2) an opt-in header. That will limit adoption, at least initially.

We therefore extend [getDisplayMedia](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getDisplayMedia) in a way that allows the application to inform the browser that it prefers the current tab. [getDisplayMedia](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getDisplayMedia) currently accepts a single parameter of type [MediaStreamConstraints](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamConstraints) (a dictionary). We extend that dictionary with a new member called preferCurrentTab. This new member is a boolean defaulting to false. When set to true, the browser presents the current tab as the most prominent option.

Sample code:

```js
navigator.mediaDevices.getDisplayMedia({
  video: true,
  audio: true,
  preferCurrentTab: true,
});
```

And the resulting media picker is:
![Screen Shot 2021-06-09 at 15 29 40](https://user-images.githubusercontent.com/22117736/121363947-a6937d00-c937-11eb-8594-ce35d3252e50.png)

This is an imperfect solution; a compromise between two needs:

- Applications need a way to signal preference for the current tab. Possibly even exclusive need of the current tab.
- [getViewportMedia](https://github.com/w3c/mediacapture-screen-share/pull/148) is a long way off, and the security requirements gating it will need a long time to gain widespread adoption.

Read the spec [here](https://wicg.github.io/prefer-current-tab/).
