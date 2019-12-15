---
layout: post
title: Working with WebViews in React Native
tags: react react-native javascript ios
---

{% raw %}
Sometimes it is too resource-intensive to recreate the interface and business logic of a complex web application in React Native. For those instances, the framework allows us to simply embed existing web pages in our apps with the WebView component. Let's look at some of the functionality exposed through the WebView API.

## Getting started

The React Native team is planning the [imminent removal of WebViews](https://facebook.github.io/react-native/docs/webview) from the core library as part of its efforts to reduce the framework's size. As a result, we'll need to import the WebView component from a separate package, namely `react-native-webview`. So, let's `npm install` it into our existing React Native project to get started.

```bash
npm install react-native-webview
```

Then, import it into the component where we'd like to render a WebView.

```js
import { WebView } from "react-native-webview";
```

Using the WebView component couldn't be easier. Just pass it a `source` prop, which will be an object with key of "uri" and a value of the web address to render in the component.

```js
<WebView source={{ uri: "https://example.com" }} />
```

## Useful WebView Properties and APIs

Once we have the WebView component set up, maybe we want to show users a placeholder while it is rendering. For this, we can hook into the `onLoadProgress` prop and use the progress attribute of the `nativeEvent` object to determine how long to display our placeholder, which in this case is an `ActivityIndicator` component.

```jsx
import { useState } from "react";
import { ActivityIndicator, View } from "react-native";

const WebViewComponent = () => {
  const [progress, setProgress] = useState(0.0);

  return (
    <>
    <WebView
        source={{ uri: "https://example.com" }}
        onLoadProgress={event => {
        setProgress(event.nativeEvent.progress);
        }}
    />;
    {
        progress < 0.99 ? (
        <View style={{ justifyContent: "center", alignItems: "center" }}>
            <ActivityIndicator size="large" color="khaki" />
        </View>
        ) : null;
    }
    </>
  )
};
```

Another useful prop when working with WebViews is `onNavigationStateChange`. With this prop, we can expose an object representing the navigation state of the component, which will let us do things like create custom forward and back navigation buttons and listen for url changes. To use this prop, you'll also need to define a `ref` to uniquely identify the instance of the WebView.

```jsx
import { Button } from "react-native-elements";
import { useRef, useState } from "react";
import { View, Dimensions } from "react-native";
import { WebView } from "react-native-webview";

const WebViewComponent = () => {
  const [canGoBack, setCanGoBack] = useState(false);
  const [canGoForward, setCanGoForward] = useState(false);
  const [currentUrl, setCurrentUrl] = useState("");

  const { width } = Dimensions.get("window");

  const wvRef = useRef();

  function backHandler() {
    if (wvRef.current) wvRef.current.goBack();
  }

  function forwardHandler() {
    if (wvRef.current) wvRef.current.goForward();
  }

  // listen for url changes and trigger an action on certain endpoint
  useEffect(() => {
    if (currentUrl.match(/\/my-target-endpoint\//)) {
      console.log(currentUrl);
    }
  }, [currentUrl]);

  return (
    <>
      <View style={{ flexDirection: "row", height: 50, width: width }}>
        <Button
          title="< Back"
          disabled={!canGoBack}
          onPress={backHandler}
          accessibilityLabel="Go back"
        />
        <Button
          title="> Forward"
          disabled={!canGoForward}
          onPress={forwardHandler}
          accessibilityLabel="Go forward"
        />
      </View>
      <WebView
        ref={wvRef}
        source={{ uri: "https://example.com" }}
        onNavigationStateChange={navState => {
          setCanGoBack(navState.canGoBack);
          setCanGoForward(navState.canGoForward);
          setCurrentUrl(navState.url);
        }}
      />
    </>
  );
};
```

One of the most powerful features of WebViews is the ability to inject JavaScript to do things like remove page elements, disable buttons, and attach event handlers to the JavaScript code within the webpage. For this functionality there are two props that the WebView can take, `injectJavascript` and `injectedJavascript`.

`InjectedJavascript` only gets evaluated once on load of the WebView, so for most use-cases you'll probably want to use the `injectJavascript` API, which runs for the full lifetime of the component. I encountered errors when trying to inject the JavaScript directly, until I started experimenting with wrapping the code in a `setTimeout()` function to add it to the event loop.

Used in conjuction with the `onMessage` and `postMessage` APIs, one can create two-way communication channels between the React Native code and the WebView quite easily. In the example below, the WebView component communicates to React Native when it detects that a user has submitted a form element on the webpage.

```jsx
import { useRef, useState } from "react";
import { WebView } from "react-native-webview";

const WebViewComponent = () => {
  const [formSubmitted, setFormSubmitted] = useState(false);

  const wvRef = useRef();

  const jsToInject = `
    setTimeout(() => {
        document.getElementById("submit-form").addEventListener("submit", function(){
            window.ReactNativeWebView.postMessage("form submitted")
        });
        true;
    }, 0);
  `;

  if (wvRef.current !== undefined) {
    wvRef.current.injectJavaScript(jsToInject);
  }

  return (
    <>
      <WebView
        ref={wvRef}
        source={{ uri: "https://a-page-with-a-form.com" }}
        onMessage={event => {
          console.log(event.nativeEvent.data); // "form submitted"
          setFormSubmitted(true);
        }}
      />
    </>
  );
};
```

## Conclusion

WebViews are not the most performant way to write a React Native application, but they provide a useful bridge between web interfaces and native code when it would be impractical to fully rebuild the interface natively. Luckily, the WebView component exposes many useful APIs that let us make this bridge happen more seamlessly.

Hopefully this post has given you some ideas for how to integrate a WebView into your next React Native application. Check out the [React Native WebView API reference docs](https://github.com/react-native-community/react-native-webview/blob/master/docs/Reference.md) for more ideas and examples.

{% endraw %}
