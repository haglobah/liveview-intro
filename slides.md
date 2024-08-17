---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: Phoenix LiveView
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# Getting productive with [Phoenix LiveView](https://github.com/phoenixframework/phoenix_live_view), fast.

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/haglobah/learn-liveview" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---

# Creating a LiveView

````md magic-move {lines: true}
```elixir
# step 1: Create the new route in `lib/demo_web/router.ex`
defmodule DemoWeb.Router do
  use DemoWeb, :router

  # ...

  scope "/", DemoWeb do
    pipe_trough :browser

    get "/", PageController, :home
  end
end
```

```elixir
# step 1: Create the new route in `lib/demo_web/router.ex`
defmodule DemoWeb.Router do
  use DemoWeb, :router

  # ...

  scope "/", DemoWeb do
    pipe_trough :browser

    get "/", PageController, :home
    live "/new", NewLive
  end
end
```

```elixir
# step 2: Create the LiveView: In lib/demo_web/live, create new_live.ex
defmodule HelloWeb.NewLive do
  use HelloWeb, :live_view

end
```
```elixir
# step 2: Create the LiveView: In lib/demo_web/live, create new_live.ex
defmodule HelloWeb.NewLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello World!</h1>
    """
  end
end
```
```elixir
# step 2: Create the LiveView: In lib/demo_web/live, create new_live.ex
defmodule HelloWeb.NewLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello World!</h1>
    """
  end

  def mount(_params, _session, socket) do
    {:ok, socket}
  end
end
```
````

<!--
Here is another comment.
-->

---

Now, create a new LiveView named CounterLive (at `/counter`) on your own.

--- 

````md magic-move {lines: true}
```elixir
# step 1: Your LiveView should look something like this
defmodule HelloWeb.CounterLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello Human!</h1>
    """
  end

  def mount(_params, _session, socket) do
    {:ok, socket}
  end
end
```
```elixir
# step 2: Add the `:counter` assign
defmodule HelloWeb.CounterLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello Human! </h1>
    """
  end

  def mount(_params, _session, socket) do
    socket =
      socket
      |> assign(:counter, 0)

    {:ok, socket}
  end
end
```
```elixir
# step 3: Show the `:counter` assign
defmodule HelloWeb.CounterLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello Human! </h1>
    <p> The value of the counter is <span class="font-mono"><%= @counter %></span></p>
    """
  end

  def mount(_params, _session, socket) do
    socket =
      socket
      |> assign(:counter, 0)

    {:ok, socket}
  end
end
```
```elixir
# step 4: Add the button
defmodule HelloWeb.CounterLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello Human! </h1>
    <p> The value of the counter is <span class="font-mono"><%= @counter %></span></p>
    <button phx-click="increment-counter">+</button>
    """
  end

  def mount(_params, _session, socket) do
    socket =
      socket
      |> assign(:counter, 0)

    {:ok, socket}
  end
end
```
```elixir
# step 5: Add the callback
defmodule HelloWeb.CounterLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello Human! </h1>
    <p> The value of the counter is <span class="font-mono"><%= @counter %></span></p>
    <button phx-click="increment-counter">+</button>
    """
  end

  def mount(_params, _session, socket) do
    socket =
      socket
      |> assign(:counter, 0)

    {:ok, socket}
  end

  def handle_event("increment-counter", _unsigned_params, socket) do
    socket = update(socket, :counter, fn count -> count + 1 end)

    {:noreply, socket}
  end
end
```
```elixir
# step 6: Use the button component
defmodule HelloWeb.CounterLive do
  use HelloWeb, :live_view

  def render(assigns) do
    ~H"""
    <h1>Hello Human! </h1>
    <p> The value of the counter is <span class="font-mono"><%= @counter %></span></p>
    <.button phx-click="increment-counter">+</.button>
    """
  end

  def mount(_params, _session, socket) do
    socket =
      socket
      |> assign(:counter, 0)

    {:ok, socket}
  end

  def handle_event("increment-counter", _unsigned_params, socket) do
    socket = update(socket, :counter, fn count -> count + 1 end)

    {:noreply, socket}
  end
end
```

<style>
.slidev-code-wrapper {
  overflow: visible !important;
}
</style>

````
