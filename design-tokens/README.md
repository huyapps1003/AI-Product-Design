# MinV Design System — Design Tokens

Extracted from the [MinV Design System Figma file](https://www.figma.com/design/rWqZASye8TV6iz77FE5j7i/MinV-Design-System) on 2026-10-02.

## Structure

```
design-tokens/
├── primitives/
│   ├── colors.json       — [Core] Colors: grey, primary, secondary, info, success, warning, error, black, white, other
│   ├── spacing.json      — [Core] Spacing: 27 spacing steps (2px–160px, base unit 8px)
│   ├── radius.json       — [Core] Radius: 16 border-radius steps (4px–500px)
│   └── sizing.json       — [Core] Sizings: 220 component-specific dimension tokens
├── semantics/
│   └── themes.json       — [Themes]: Light/Dark mode semantic tokens resolving to primitives
├── styles/
│   ├── color-styles.json — Gradient paint styles (12 linear gradients, main + bold per color)
│   ├── text-styles.json  — Typography styles: desktop (h1–overline), mobile (h1–h6), components
│   ├── effect-styles.json — Shadow + blur styles (z1–z24, card, dialog, dropdown, per-color)
│   └── layout-styles.json — Grid/layout styles (vertical, horizontal, mini, desktop, mobile)
└── components/
    └── index.json        — Component catalog: 45+ components with Figma IDs, variant counts, properties
```

## Token Format

Files use the [W3C Design Token Community Group](https://design-tokens.github.io/community-group/format/) format:

```json
{
  "tokenName": {
    "$type": "color",
    "$value": "#00A76F"
  }
}
```

## Color Palette

| Token             | Value     |
|-------------------|-----------|
| primary/main      | `#00A76F` |
| primary/light     | `#5BE49B` |
| primary/lighter   | `#C8FAD6` |
| primary/dark      | `#007867` |
| primary/darker    | `#004B50` |
| secondary/main    | `#8E33FF` |
| info/main         | `#00B8D9` |
| success/main      | `#22C55E` |
| warning/main      | `#FFAB00` |
| error/main        | `#FF5630` |

## Typography

Font family: **Public Sans**

| Style          | Size | Weight | Line Height |
|----------------|------|--------|-------------|
| desktop/h1     | 64px | 800    | 80px        |
| desktop/h2     | 48px | 800    | 64px        |
| desktop/h3     | 32px | 700    | 48px        |
| desktop/h4     | 24px | 700    | 36px        |
| desktop/h5     | 20px | 700    | 30px        |
| desktop/h6     | 18px | 700    | 28px        |
| desktop/body1  | 16px | 400    | 24px        |
| desktop/body2  | 14px | 400    | 22px        |
| desktop/caption| 12px | 400    | 18px        |

## Themes (Light / Dark)

The `semantics/themes.json` file contains tokens that swap values between light and dark mode, referencing the primitive colors. Key tokens:

| Token                   | Light              | Dark               |
|-------------------------|--------------------|--------------------|
| text/primary            | grey/800 (#212B36) | white (#FFFFFF)    |
| background/default      | white (#FFFFFF)    | grey/900 (#161C24) |
| background/paper        | white (#FFFFFF)    | grey/800 (#212B36) |

## Components

45+ components organized by category:

- **Inputs**: Button, ButtonGroup, Checkbox, DatePicker, Fab, IconButton, Radio, Rating, Slider, Switch, TextField, Textarea, ToggleButton, Upload
- **Navigation**: Breadcrumb, Menu, MenuItem, Pagination, Stepper, Tabs
- **Data Display**: Avatar, Badge, Chip, Divider, Label, List, Table, Timeline, Tooltip
- **Feedback**: Alert, Backdrop, CircularProgress, LinearProgress, Snackbar, Dialog
- **Surfaces**: Accordion, Card, Paper
- **Charts**: Area, Bar, Column, Donut, Line, Pie, PolarArea, Radar, Radial, Sparkline

## Usage with Style Dictionary

```js
// style-dictionary.config.js
module.exports = {
  source: ["design-tokens/**/*.json"],
  platforms: {
    css: {
      transformGroup: "css",
      buildPath: "build/css/",
      files: [{ destination: "variables.css", format: "css/variables" }]
    }
  }
};
```

## Usage with Token Studio (Figma Tokens Plugin)

Import each JSON file as a token set in Token Studio. Map `semantics/themes.json` as a theme set with Light/Dark modes.

## Syncing to GitHub

```bash
cd /path/to/your/repo
cp -r design-tokens/ .
git add design-tokens/
git commit -m "chore: update MinV design tokens from Figma"
git push
```
