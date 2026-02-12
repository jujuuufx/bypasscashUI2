# xezios UI Library Documentation

A clean, modern, mobile-friendly Roblox UI library with theme support, configs, notifications, resizable/draggable windows, and full touch/mouse compatibility.

## Table of Contents

- [Quick Start](#quick-start)
- [Window Creation](#window-creation)
- [Tabs](#tabs)
- [Sections](#sections)
- [Elements Overview](#elements-overview)
  - Toggle
  - Slider
  - Dropdown
  - Button
  - Label
  - Textbox
  - Keybind
  - Colorpicker
- [Configs System](#configs-system)
- [Notifications](#notifications)
- [Theme Customization](#theme-customization)
- [Mobile Support Features](#mobile-support-features)
- [Important Notes & Tips](#important-notes--tips)

## Quick Start

```lua
local Library = loadstring(game:HttpGet("your-script-url-here"))()

local Window = Library:Window({
    Prefix = "MyCheat",
    Suffix = ".v1",
    Size = UDim2.fromOffset(580, 420)
})

local Tab = Window:Tab({
    Name = "Main",
    Icon = "rbxassetid://1234567890"   -- any valid Roblox asset id
})

local Section = Tab:Section({
    Name = "Aimbot",
    Side = "Left"   -- or "Right"
})

Section:Toggle({
    Name = "Silent Aim",
    Flag = "SilentAim",
    Default = false,
    Callback = function(enabled)
        print("Silent Aim:", enabled)
    end
})

Section:Slider({
    Name = "FOV",
    Flag = "AimbotFOV",
    Min = 10,
    Max = 300,
    Default = 120,
    Decimal = 1,
    Suffix = "°",
    Callback = function(value)
        -- update aimbot fov
    end
})

-- etc...
```

## Window Creation

```lua
Library:Window(properties: table)
```

| Property   | Type          | Default              | Description |
|------------|---------------|----------------------|-------------|
| `Prefix`   | string        | "inferno."           | Left part of title |
| `Suffix`   | string        | "wtf"                | Right part of title |
| `Size`     | UDim2         | 620×471              | Initial window size |

**Example with custom size & title:**
```lua
Window = Library:Window({
    Prefix = "Aura",
    Suffix = "Hub",
    Size = UDim2.fromOffset(640, 520)
})
```

## Tabs

```lua
Window:Tab(properties: table)
```

| Property | Type   | Default | Description |
|----------|--------|---------|-------------|
| `Name`   | string | —       | Tab name |
| `Icon`   | string | —       | rbxassetid://... |

Tabs automatically create two columns (`Left` & `Right`).

```lua
local Visuals = Window:Tab({
    Name = "Visuals",
    Icon = "rbxassetid://112730572155522"
})
```

## Sections

```lua
Tab:Section(properties: table)
```

| Property | Type   | Default | Description |
|----------|--------|---------|-------------|
| `Name`   | string | —       | Section title |
| `Side`   | string | "Left"  | "Left" or "Right" |

```lua
local LeftSection = Tab:Section({ Name = "ESP", Side = "Left" })
local RightSection = Tab:Section({ Name = "Chams", Side = "Right" })
```

## Elements Overview

### Toggle

```lua
Section:Toggle({
    Name = string,
    Flag = string,           -- required for saving
    Default = bool,
    Callback = function(bool)
    end
})
```

### Slider

```lua
Section:Slider({
    Name = string,
    Flag = string,
    Min = number,
    Max = number,
    Default = number,
    Decimal = number,        -- step size (0.1 = 1 decimal, 1 = whole numbers)
    Suffix = string,         -- optional "°", "%", etc.
    Callback = function(value)
    end
})
```

### Dropdown

```lua
Section:Dropdown({
    Name = string,
    Flag = string,
    Options = {string, ...},
    Multi = bool,            -- multi-select mode
    Default = string or {string, ...},
    Callback = function(value or {values})
    end
})
```

Refresh options later:
```lua
dropdown:RefreshOptions({"New", "List", "Of", "Options"})
```

### Button

```lua
Section:Button({
    Name = string,
    Callback = function()
        -- one-time action
    end
})
```

### Label

```lua
Section:Label({
    Name = string
})
```

Update later:
```lua
label:Set("New text here")
```

### Textbox

```lua
Section:Textbox({
    Name = string,
    Flag = string,
    PlaceHolder = string,
    Default = string,
    Callback = function(text)
    end
})
```

### Keybind

```lua
Section:Keybind({
    Name = string,           -- optional
    Flag = string,
    Key = Enum.KeyCode or Enum.UserInputType,
    Mode = "Toggle" | "Hold" | "Always",
    Default = bool,          -- active by default?
    Callback = function(active: bool)
    end
})
```

Right-click keybind button → change mode (Hold/Toggle/Always)

### Colorpicker

```lua
Section:Label("Color"):Colorpicker({
    Name = string,           -- optional
    Flag = string,
    Color = Color3,
    Alpha = number,          -- 0..1 (optional)
    Callback = function(color: Color3, alpha: number)
    end
})
```

or shorter (most common use):
```lua
Section:Colorpicker({
    Name = "Accent",
    Flag = "ThemeAccent",
    Color = Color3.fromRGB(255,80,80),
    Callback = function(c) Library:RefreshTheme("accent", c) end
})
```

## Configs System

Built-in save/load/delete system (Settings tab auto-created when you call):

```lua
Library:Configs(Window)   -- call this once after creating window
```

It adds:

- Configs dropdown
- Config name textbox
- Save / Load / Delete buttons
- Theme color pickers
- Menu bind keybind

Files saved in: `xezios/configs/YourConfig.cfg`

## Notifications

```lua
Library.Notifications:Create({
    Name = "Hello world!",
    LifeTime = 4           -- optional, default 3 seconds
})
```

## Theme Customization

Change any theme color live:

```lua
Library:RefreshTheme("accent", Color3.fromRGB(0, 170, 255))
Library:RefreshTheme("text_color", Color3.fromRGB(240, 240, 240))
-- etc.
```

Available theme keys:

- `accent`
- `window_outline`
- `inline`
- `background`
- `visible_backgrounds`
- `text_color`
- `glow`
- `deselected`

## Mobile Support Features

- Resizable window with red triangle indicator (bottom-right)
- Draggable window (drag title bar only)
- Mobile toggle button (floating image button)
- Touch-friendly sliders, colorpickers, dropdowns, keybinds
- No clamping when dragging window (can go off-screen)
- Larger hitboxes on mobile for resizer & toggle

## Important Notes & Tips

- **Flags are required** for anything you want to save/load (toggles, sliders, dropdowns, colorpickers, etc.)
- Call `Library:Configs(Window)` at the end to enable config system & theme picker
- Use `Flag` names consistently — they are used as keys in config files
- Mobile toggle uses your custom asset `rbxassetid://122122220490038`
- To unload everything cleanly:
  ```lua
  Library:Unload()
  ```
