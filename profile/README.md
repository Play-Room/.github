<img width="2560" height="1440" alt="playroom_2560x1440_no_crop" src="https://github.com/user-attachments/assets/aed03c79-7b20-4fee-b71e-3df5be3c22e6" />

# PlayRoom

**PlayRoom** is a lightweight, framework-agnostic game hub. It does not own game logic or data services — each game is an independent module that PlayRoom can register, list, and launch. Games can run standalone or be embedded inside any host application through a growing set of framework adapters.

---

## Core Concepts

- **Game Hub** — PlayRoom acts as a registry and launcher. It doesn't know what games do; it just loads, lists, and launches them.
- **Independent Games** — Every game is a self-contained package with its own logic, UI, localization, and persistence.
- **Adapter Ecosystem** — First-class adapters for Alpine.js, React, Vue 3, Laravel Livewire, and Filament let you drop PlayRoom into any stack with minimal setup.
- **Locale & Theme API** — Locale and theme are owned by PlayRoom core and propagated to all registered games in real time.
- **Inline or Modal** — Games can be launched inline inside a container or inside a resizable, draggable modal overlay.

---

## Packages

### 🎮 Core

| Package | Description |
|---|---|
| [`@play-room/core`](https://github.com/Play-Room/core) | The game hub — register, list, and launch game modules. Manages locale, theme, and the game picker UI. |
| [`@play-room/quizz`](https://github.com/Play-Room/quizz) | A standalone multi-category quiz game with credits, leaderboard, localization, and theme support. |
| [`data`](https://github.com/Play-Room/data) | Public JSON data repository — quiz questions and answers consumed by `@play-room/quizz` by default. |

### 🔌 Framework Adapters

| Package | Stack | Description |
|---|---|---|
| [`@play-room/alpinejs-adapter`](https://github.com/Play-Room/alpinejs-adapter) | Alpine.js | Exposes PlayRoom as `$playroom()` magic helper and `$playRoomDefaults` in Alpine components. |
| [`@play-room/reactjs-adapter`](https://github.com/Play-Room/reactjs-adapter) | React | Provides a `createPlayRoom` factory and a `usePlayRoom` hook for React apps. |
| [`@play-room/vuejs-adapter`](https://github.com/Play-Room/vuejs-adapter) | Vue 3 | Vue plugin with `$playroom()` instance factory and a `usePlayRoom` composable. |
| [`playroom/livewire-adapter`](https://github.com/Play-Room/livewire-adapter) | Laravel / Livewire 4 | Exposes PlayRoom as a Livewire 4 component with full locale and theme sync. |
| [`playroom/filament-adapter`](https://github.com/Play-Room/filament-adapter) | Filament v5 | Filament plugin and dashboard widget for embedding PlayRoom in admin panels. |

---

## Quick Start

### Vanilla / CDN

```ts
import { PlayRoom } from "@play-room/core";

const playRoom = new PlayRoom({ locale: "en", theme: "light" });
playRoom.registerDefaultGames();
playRoom.renderGamePicker(document.getElementById("app"));
```

### React

```tsx
import { usePlayRoom } from "@play-room/reactjs-adapter";

export function App() {
  const room = usePlayRoom({ browserStartMode: "inline" });
  // mount and register games in a useEffect
  return <div id="playroom-browser" />;
}
```

### Vue 3

```ts
import { usePlayRoom } from "@play-room/vuejs-adapter";

const room = usePlayRoom({ browserStartMode: "inline" });
room.registerDefaultGames();
```

### Alpine.js

```html
<div x-data="{ room: $playroom() }" x-init="room.registerDefaultGames()">
  <div x-init="room.renderGamePicker($el)"></div>
</div>
```

### Laravel / Livewire

```blade
<livewire:play-room />
```

### Filament

```php
PlayRoomPlugin::make()->floating();
```

---

## Game Authoring

Any package can become a PlayRoom game by exporting a `GameDefinition`:

```ts
export function createMyGameDefinition() {
  return {
    id: "my-game",
    name: "My Game",
    locales: {
      en: { name: "My Game", description: "...", tags: ["puzzle"] }
    },
    async createInstance(context) {
      return {
        mount(container) { /* render into container */ },
        unmount()       { /* cleanup */                }
      };
    }
  };
}
```

Then register it:

```ts
playRoom.registerGame({ game: createMyGameDefinition() });
```

See the [Game Authoring Guide in `@play-room/core`](https://github.com/Play-Room/core#game-authoring-guide) for the full contract.

---

## Built-in Games

| Game | Package | Categories | Languages |
|---|---|---|---|
| Smart Quizz | [`@play-room/quizz`](https://github.com/Play-Room/quizz) | Technology, Science, Sports | English, Srpski |

