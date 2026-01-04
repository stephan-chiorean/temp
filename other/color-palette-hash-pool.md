---
id: color-palette-hash-pool
alias: Color Palette Hash Pool System
type: kit
is_base: false
version: 1
tags:
  - ui
  - color-system
  - design-system
description: Deterministic color assignment system using hash-based pool mechanism with multiple styling variations for badges, tags, and color-coded components
---
# Color Palette Hash Pool System

## End State

After applying this kit, the application will have:

**Color assignment system:**
- Deterministic color mapping: same identifier always gets the same color
- Pool-based assignment: colors assigned from available pool, ensuring maximum diversity
- Colors only recycle after entire pool is exhausted (typically 20-30 colors)
- Hash-based selection: uses string hash to pick from available colors for determinism
- Normalization: identifiers normalized (lowercase, trimmed) for consistent assignment

**Color palette structure:**
- Comprehensive color palette covering full spectrum (reds, oranges, yellows, greens, blues, purples, pinks, etc.)
- Each color provides separate definitions for light and dark modes
- Each color definition includes: background, text, and border colors
- Light mode: subtle pastel backgrounds with darker text for readability
- Dark mode: semi-transparent colored backgrounds with lighter text for contrast

**Styling variations:**
- **Subtle outline**: Transparent/semi-transparent background with colored border and matching text
- **Solid fill**: Opaque background with light text (for dark backgrounds) or dark text (for light backgrounds)
- **Minimal border**: Very subtle background tint with prominent border
- **Gradient variants**: Optional gradient backgrounds using color palette
- All variations support interactive states (hover, focus, selected)

**Core functions available:**
- `getColor(identifier: string): ColorDefinition` - Get full color object for identifier
- `getColorForMode(identifier: string, mode: 'light' | 'dark'): ColorValues` - Get colors for specific mode
- `getColorIndex(identifier: string): number` - Get assigned color index from pool
- `registerIdentifiers(identifiers: string[]): void` - Pre-register identifiers for optimal distribution
- `resetColorPool(): void` - Reset pool state (useful for testing)
- `getPoolStats(): PoolStats` - Get current pool statistics (assigned count, used colors, generation)

**Component integration:**
- Reusable component/badge that accepts identifier string and styling variant
- Automatic color mode detection (light/dark)
- Support for interactive states (clickable, selected, hover)
- Configurable size variants (sm, md, lg)
- Optional prefix/suffix text (e.g., "#" for tags)

**Color pool management:**
- Module-level state tracks assigned identifiers to color indices
- Tracks which colors are currently in use
- Pool generation counter increments when pool is exhausted
- Consistent assignment order when pre-registering multiple identifiers

## Implementation Principles

- Use deterministic hash function (djb2 variant or similar) for consistent results across sessions
- Maintain color pool state at module level (not component level) to persist across renders
- Normalize identifiers (lowercase, trim) before hashing for consistency
- Design color palette with accessibility in mind: sufficient contrast ratios in both modes
- Support at least 20-30 colors in palette to minimize recycling
- Use hash modulo available colors for selection, not direct hash-to-index mapping
- When pool is exhausted, clear used colors and increment generation counter
- Pre-registration should process identifiers in sorted order for consistency
- Color definitions should use rgba for transparency control
- Support both CSS color formats (hex, rgba, named colors) based on framework needs

**Styling variation principles:**
- Subtle outline: `bg: transparent/semi-transparent`, `border: color`, `text: color`
- Solid fill: `bg: color (opaque)`, `text: contrasting color (light/dark based on mode)`
- Minimal border: `bg: very light tint`, `border: prominent color`, `text: color`
- Interactive states: opacity changes, scale transforms, or border weight changes
- Ensure hover/focus states maintain accessibility contrast

**Framework agnosticism:**
- Core color logic should be framework-independent (pure functions)
- Component wrapper adapts to framework (React, Vue, Svelte, etc.)
- Color values can be returned as objects, CSS strings, or theme tokens
- No assumptions about UI library (Chakra UI, Tailwind, Material, etc.)

## Verification Criteria

After generation, verify:
- ✓ Same identifier always returns the same color across multiple calls
- ✓ Different identifiers get different colors until pool is exhausted
- ✓ Colors recycle only after all colors in pool have been used
- ✓ Light mode colors provide sufficient contrast (WCAG AA minimum)
- ✓ Dark mode colors provide sufficient contrast (WCAG AA minimum)
- ✓ Normalized identifiers (case-insensitive, trimmed) map to same color
- ✓ Pre-registration assigns colors optimally (no duplicates in same batch)
- ✓ Styling variations render correctly in both light and dark modes
- ✓ Interactive states (hover, focus, selected) work correctly
- ✓ Component accepts identifier string and applies correct colors
- ✓ Pool statistics accurately reflect current state
- ✓ Reset function clears all assignments and resets generation counter

## Interface Contracts

**Provides:**
- Functions: `getColor(identifier)`, `getColorForMode(identifier, mode)`, `getColorIndex(identifier)`, `registerIdentifiers(identifiers)`, `resetColorPool()`, `getPoolStats()`
- Types: `ColorDefinition` (name, light, dark), `ColorValues` (bg, text, border), `PoolStats` (assignedCount, usedColors, generation, totalColors)
- Component: Reusable badge/tag component accepting identifier and variant props
- Color palette: Array of 20-30 color definitions with light/dark mode support

**Requires:**
- Runtime: JavaScript/TypeScript environment
- Framework: Any UI framework (React, Vue, Svelte, etc.) or vanilla JS
- Color mode detection: Mechanism to detect light/dark mode (context, hook, or prop)
- Styling system: CSS-in-JS, Tailwind, or standard CSS for applying colors

**Compatible With:**
- Design systems: Integrates with existing theme systems (Chakra UI, Material, Tailwind)
- Component libraries: Works with any badge/tag component library
- State management: Can be extended with persistent storage (localStorage, database) for cross-session consistency
- Multi-theme: Supports multiple theme variants beyond light/dark
- Accessibility: Can be extended with ARIA labels and keyboard navigation
