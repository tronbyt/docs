---
icon: lucide/smile
---
# Emoji

Pixlet renders **1753+ Unicode emojis** directly in `render.Text()` widgets. Just include emoji characters in your strings and the runtime will pick the right sprite from the bundled sheet.

!!! tip

    Emojis are tiny on Tronbyt displays. Highly detailed glyphs can look cleaner if you scale them up first.

## Quick Examples

```python
# Basic usage
render.Text("Hello World!", font="6x10")

# Multiple emojis
render.Text("🎉🎊🎈🎁", font="5x8")

# Mixed content
render.Text("Weather: ☀️ 75°F", font="tb-8")
```

### Examples
- ![😀](https://raw.githubusercontent.com/SerenityOS/serenity/refs/heads/master/Base/res/emoji/U+1F600.png) Grinning Face
- ![🎉](https://raw.githubusercontent.com/SerenityOS/serenity/refs/heads/master/Base/res/emoji/U+1F389.png) Party Popper
- ![🎊](https://raw.githubusercontent.com/SerenityOS/serenity/refs/heads/master/Base/res/emoji/U+1F38A.png) Confetti Ball
- ![🎈](https://raw.githubusercontent.com/SerenityOS/serenity/refs/heads/master/Base/res/emoji/U+1F388.png) Balloon
- ![🎁](https://raw.githubusercontent.com/SerenityOS/serenity/refs/heads/master/Base/res/emoji/U+1F381.png) Wrapped Gift
- ![☀️](https://raw.githubusercontent.com/SerenityOS/serenity/refs/heads/master/Base/res/emoji/U+2600.png) Sun

## Previewing The Set

For a full visual catalog of the SerenityOS artwork Pixlet uses, visit [Emojipedia](https://emojipedia.org/serenityos). Each entry shows the exact glyph provided by the upstream pack.

## Example Apps

### Text Widget with Emojis
```python
load("render.star", "render")

def main():
    return render.Root(
        child = render.Column(
            children = [
                render.Text("Weather: ☀ 75°F", font="6x10"),
                render.Text("Status: ✅ Online", font="5x8"),
                render.Text("🎉 Happy Birthday! 🎂", font="tb-8"),
                render.Text("🇺🇸🇬🇧🇫🇷🇩🇪🇯🇵", font="6x10"),
            ]
        )
    )
```

## The Emoji Widget

For creating **large, scalable emojis**, Pixlet provides a dedicated `render.Emoji` widget:

```python
# Create emojis at any size
render.Emoji(emoji="🚀", height=32)   # Large rocket
render.Emoji(emoji="⚡", height=64)   # Huge lightning
render.Emoji(emoji="🎉", height=16)   # Medium party

# Use in layouts
render.Row(
    children = [
        render.Text("Status:", font="6x10"),
        render.Emoji(emoji="✅", height=12),
    ]
)
```

## Attribution

The emoji artwork used in Pixlet is provided by the [SerenityOS project](https://emoji.serenityos.org/). SerenityOS has created a beautiful, open-source emoji set that perfectly fits Pixlet's aesthetic for small displays.

- **Emoji Source**: [emoji.serenityos.org](https://emoji.serenityos.org/)
- **SerenityOS Project**: [serenityos.org](https://serenityos.org/)
- **License**: The emoji artwork is released under permissive licensing terms
