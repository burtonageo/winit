# winit - Cross-platform window creation and management in Rust

[![](http://meritbadge.herokuapp.com/winit)](https://crates.io/crates/winit)
[![Docs.rs](https://docs.rs/winit/badge.svg)](https://docs.rs/winit)
[![Build Status](https://travis-ci.org/tomaka/winit.svg?branch=master)](https://travis-ci.org/tomaka/winit)
[![Build status](https://ci.appveyor.com/api/projects/status/5h87hj0g4q2xe3j9/branch/master?svg=true)](https://ci.appveyor.com/project/tomaka/winit/branch/master)

```toml
[dependencies]
winit = "0.18"
```

## [Documentation](https://docs.rs/winit)

## Usage

Winit is a window creation and management library. It can create windows and lets you handle
events (for example: the window being resized, a key being pressed, a mouse movement, etc.)
produced by window.

Winit is designed to be a low-level brick in a hierarchy of libraries. Consequently, in order to
show something on the window you need to use the platform-specific getters provided by winit, or
another library.

```rust
extern crate winit;

fn main() {
    let mut events_loop = winit::EventsLoop::new();
    let window = winit::Window::new(&events_loop).unwrap();

    events_loop.run_forever(|event| {
        match event {
            winit::Event::WindowEvent {
              event: winit::WindowEvent::CloseRequested,
              ..
            } => winit::ControlFlow::Break,
            _ => winit::ControlFlow::Continue,
        }
    });
}
```

### Cargo Features

Winit provides the following features, which can be enabled in your `Cargo.toml` file:
* `icon_loading`: Enables loading window icons directly from files. Depends on the [`image` crate](https://crates.io/crates/image).
* `serde`: Enables serialization/deserialization of certain types with [Serde](https://crates.io/crates/serde).

### Platform-specific usage

#### Emscripten and WebAssembly

Building a binary will yield a `.js` file. In order to use it in an HTML file, you need to:

- Put a `<canvas id="my_id"></canvas>` element somewhere. A canvas corresponds to a winit "window".
- Write a Javascript code that creates a global variable named `Module`. Set `Module.canvas` to
  the element of the `<canvas>` element (in the example you would retrieve it via `document.getElementById("my_id")`).
  More information [here](https://kripken.github.io/emscripten-site/docs/api_reference/module.html).
- Make sure that you insert the `.js` file generated by Rust after the `Module` variable is created.
