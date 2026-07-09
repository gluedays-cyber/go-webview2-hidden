[![Go](https://github.com/jchv/go-webview2/actions/workflows/go.yml/badge.svg)](https://github.com/jchv/go-webview2/actions/workflows/go.yml) [![Go Report Card](https://goreportcard.com/badge/github.com/jchv/go-webview2)](https://goreportcard.com/report/github.com/jchv/go-webview2) [![Go Reference](https://pkg.go.dev/badge/github.com/jchv/go-webview2.svg)](https://pkg.go.dev/github.com/jchv/go-webview2)

# go-webview2
This package provides an interface for using the Microsoft Edge WebView2 component with Go. It is based on [webview/webview](https://github.com/webview/webview) and provides a compatible API.

Please note that this package only supports Windows, since it provides functionality specific to WebView2. If you wish to use this library for Windows, but use webview/webview for all other operating systems, you could use the [go-webview-selector](https://github.com/jchv/go-webview-selector) package instead. However, you will not be able to use WebView2-specific functionality.

If you wish to build desktop applications in Go using web technologies, please consider [Wails](https://wails.io/). It uses go-webview2 internally on Windows.

## Demo
If you are using Windows 10+, the WebView2 runtime should already be installed. If you don't have it installed, you can download and install a copy from Microsoft's website:

[WebView2 runtime](https://developer.microsoft.com/en-us/microsoft-edge/webview2/)

After that, you should be able to run go-webview2 directly:

```
go run ./cmd/demo
```

This will use go-winloader to load an embedded copy of WebView2Loader.dll. If you want, you can also provide a newer version of WebView2Loader.dll in the DLL search path and it should be picked up instead. It can be acquired from the WebView2 SDK (which is permissively licensed.)

---

## Fork Specifications: Hidden Window Initialization Support
This repository is a fork of jchv/go-webview2 modified to support starting the webview window in a hidden state.

**Why: The Reason for Modification**
By default, go-webview2 displays the window immediately upon initialization. This causes a noticeable window flickering or layout stuttering effect on Windows startup while the window size, positioning, and high-DPI scaling are being computed and applied.

To prevent this layout stutter, the window initialization logic has been modified to start in a hidden state, allowing developers to set the size and position first, and then programmatically show the window once rendering preparations are complete.

**What: The Changes Made in webview.go**
Window Style Modification: Modified the Win32 CreateWindowEx parameters or style flags (removing WS_VISIBLE or adjusting startup show commands) inside webview.go to ensure the window is initialized as hidden.
Control of Initial Visibility: Allows layout calculations and sizing modifications to be executed seamlessly in the background before the window is rendered visible to the end user.
