<div align="center">

# 🎨 WotLK Atlas System

### *Bring Retail's Atlas API to WotLK 3.3. 5a*

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)](https://www.gnu.org/licenses/agpl-3.0)
[![WoW Version](https://img.shields.io/badge/WoW-3.3.5a%20(12340)-orange.svg)](https://wowpedia.fandom.com/wiki/Patch_3.3.5a)
[![Made with Lua](https://img.shields.io/badge/Made%20with-Lua-2C2D72.svg)](https://www.lua.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

*Stop hardcoding texture coordinates.  Start using atlases like it's 2026.*

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [API](#-api) • [Contributing](#-contributing)

</div>

---

## 🤔 The Problem

Ever written code like this? 

```lua
texture: SetTexture("Interface\\Glues\\CharacterCreate\\UI-CharacterCreate-Classes")
texture:SetTexCoord(0, 0.25, 0, 0.25)  -- warrior...  I think?  🤷
texture:SetSize(64, 64)
```
Yeah, we've all been there. It's 2026 and we're still manually calculating texture coordinates like it's 2004.

---

## ✨ The Solution

```lua
SetAtlas(texture, "class-warrior", true)
```
That's it. **One line.** Clean, readable, maintainable.

---

## 🚀 Features

### 🎯 Simple API
    
```lua
SetAtlas(texture, "class-mage", true)
```

### 🔌 XML Support
    
```xml
<Texture file="atlas: class-paladin" />
```

### 🌐 Universal
Works in FrameXML and GlueXML

---

## 📦 Installation
### Quick Start
- **Download** the latest release
- **Copy** `AtlasHelper.lua` and `AtlasInfo.lua` to `Interface/FrameXML/`
- **Add** to your `FrameXML.toc`, `AtlasHelper.lua` and `AtlasInfo.lua`
- **Reload** UI (`/reload`)

### Installation structure
```
World of Warcraft/
└── Data/
    └── Patch-{4-9/a-z}.MPQ/
        └── Interface/
            ├── FrameXML/
            │   ├── FrameXML.toc          ← Add atlas files here
            │   ├── AtlasHelper.lua       ← Copy this
            │   └── AtlasInfo.lua         ← Copy this
            └── GlueXML/
                ├── GlueXML. toc          ← (Optional) For login screen
                ├── AtlasHelper.lua       ← Copy if using Glue
                └── AtlasInfo.lua         ← Copy if using Glue
```

---

## 💡 Usage
### Lua Example
```lua
local frame = CreateFrame("Frame", nil, UIParent)
frame:SetSize(128, 128)
frame:SetPoint("CENTER")

local texture = frame:CreateTexture(nil, "ARTWORK")
texture:SetAllPoints()

-- ✨ Magic happens here
SetAtlas(texture, "groupfinder-icon-class-warrior", true)

frame:Show()
```

### XML Example
```xml
<Frame name="MyAwesomeFrame" parent="UIParent">
    <Size x="200" y="200"/>
    <Anchors>
        <Anchor point="CENTER"/>
    </Anchors>
    <Layers>
        <Layer level="ARTWORK">
            <!-- 🔥 New way -->
            <Texture file="atlas:class-warrior">
                <Size x="64" y="64"/>
            </Texture>
        </Layer>
    </Layers>
</Frame>
```

---

## 🛠️ API Reference

### `C_Texture.LoadAtlasData(atlasTable)`
Loads atlas coordinate data.

#### Parameters:
- `atlasTable` (table) - Atlas data structure

#### Returns:
- (number) - Number of atlases loaded

#### Example:
```lua
local myAtlases = {
    ["Interface/MyAddon/Icons"]={
        ["my-icon"]={64, 64, 0, 1, 0, 1, false, false, "1x"},
    },
}

C_Texture.LoadAtlasData(myAtlases)  -- Returns:  1
```

---

### `C_Texture.GetAtlasInfo(name)`
Gets atlas metadata.

#### Parameters:
- `name` (string) - Atlas name (case-insensitive)

#### Returns:
- (table|nil) - Atlas info or nil if not found

#### Example:
```lua
local info = C_Texture.GetAtlasInfo("class-mage")
--[[
{
    width = 256,
    height = 256,
    left = 0. 25,
    right = 0.5,
    top = 0,
    bottom = 0.25,
    file = "Interface/Glues/CharacterCreate/UI-CharacterCreate-Classes"
}
]]
```

---

### `C_Texture.SetAtlas(texture, name, useSize)`
Applies atlas to a texture.

#### Parameters:
- `texture` (Texture) - Texture object
- `name` (string) - Atlas name
- `useSize` (boolean) - Auto-resize to atlas dimensions

#### Returns:
- (boolean) - Success status

#### Example:
```lua
SetAtlas(myTexture, "class-warrior", true)  -- Returns: true
```

---

## 📚 Atlas Data Format
Atlas definitions follow Retail's format:
```lua
["path/to/texture/file"]={
    ["atlas-name"]={width, height, left, right, top, bottom, tilesH, tilesV, scale},
}
```

### Parameters
| Index | Name | Type | Description |
| -------- | ------- | ------- | ------- |
| 1 | width | number | Atlas width in pixels |
| 2 | height | number | Atlas height in pixels |
| 3 | left | number | Left UV coordinate (0-1) |
| 4 | right | number | Right UV coordinate (0-1) |
| 5 | top | number | Top UV coordinate (0-1) |
| 6 | bottom | number | Bottom UV coordinate (0-1) |
| 7 | tilesH | boolean | Tiles horizontally |
| 8 | tilesV | boolean | Tiles vertically |
| 9 | scale | string | Texture scale ("1x", "2x") |

### Example
```lua
["Interface/Glues/CharacterCreate/UI-CharacterCreate-Classes"]={
    ["class-warrior"]={256, 256, 0, 0.25, 0, 0.25, false, false, "1x"},
    ["class-mage"]={256, 256, 0.25, 0.5, 0, 0.25, false, false, "1x"},
}
```

---

## 🤝 Contributing
We love contributions!

### 🐛 Found a bug?
[Open an issue](https://github.com/iThorgrim/WotLK-Atlas-System/issues/new)

### 💡 Have an idea?
[Open an issue](https://github.com/iThorgrim/WotLK-Atlas-System/issues/new) (yes, again)

### 🔨 Want to code?
1. Fork the repo
2. Create a feature branch `(git checkout -b feature/amazing-feature)`
3. Commit your changes `(git commit -m 'Add amazing feature')`
4. Push to the branch `(git push origin feature/amazing-feature)`
5. Open a Pull Request

---

## 📜 License
### 🆓 Code (AGPL-3.0)

All original code is licensed under [AGPL-3.0](https://github.com/iThorgrim/WotLK-Atlas-System?tab=GPL-3.0-1-ov-file).

### 🎮 Data (Blizzard)
Atlas coordinate data is extracted from WoW and remains property of **Blizzard Entertainment**.
**Source:** [Townlong Yak](https://www.townlong-yak.com/framexml/beta/Helix/AtlasInfo.lua)

---

## 🙏 Credits
| Who | What |
| -------- | ------- |
| 🎮 Blizzard Entertainment | Original atlas system & data |
| [📚 Townlong Yak](https://www.townlong-yak.com/framexml/ptr) | FrameXML extraction & docs |
| [👨‍💻 iThorgrim](https://github.com/iThorgrim) | WotLK port & implementation |

---

## ⚠️ Disclaimer
This project is **not affiliated with, endorsed by, or supported** by Blizzard Entertainment.
World of Warcraft® and Blizzard Entertainment® are registered trademarks of Blizzard Entertainment, Inc.

**Use at your own risk.** This is a community project for educational purposes.

---

<div align="center">

💖 Made with love for the WotLK community

[⬆ Back to top](https://github.com/iThorgrim/WotLK-Atlas-System/blob/main/README.md#-wotlk-atlas-system)

*If this project helped you, consider giving it a ⭐! *

</div
