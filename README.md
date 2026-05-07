# Notch Encoding System

A visual encoding algorithm that represents text as **nested squares with edge notches**. Each letter is a square layer — the innermost square is the first letter, the outermost is the last. Words are separate nested clusters read left to right.

Inspired by iterative art — the idea that repeating a shape inside itself can carry meaning.

---

## How It Works

### The Base Unit

Every letter is represented by a **square** with a notch pattern cut into one of its vertical sides. The top and bottom sides are never used.

```
┌──────────────┐
│              ├──┐   ← notch segment (active)
│              │  │
│              ├──┘   ← notch segment (active)
│              │
│              │      ← flat segment (inactive)
└──────────────┘
```

### Case Encoding — Which Side

The **side** the notch appears on encodes whether the letter is capital or lowercase:

| Side | Case |
|------|------|
| **Left** | Capital (A–Z) |
| **Right** | Lowercase (a–z) |

### Letter Encoding — Notch Pattern

Each side is divided into **5 equal segments**. Each segment is either **notched** (1) or **flat** (0), forming a 5-bit binary number.

```
Segment positions (bottom → top):  [ 1 ][ 2 ][ 3 ][ 4 ][ 5 ]
```

Letters are mapped sequentially:

| Letter | Binary | Letter | Binary |
|--------|--------|--------|--------|
| A / a  | 00001  | N / n  | 01110  |
| B / b  | 00010  | O / o  | 01111  |
| C / c  | 00011  | P / p  | 10000  |
| D / d  | 00100  | Q / q  | 10001  |
| E / e  | 00101  | R / r  | 10010  |
| F / f  | 00110  | S / s  | 10011  |
| G / g  | 00111  | T / t  | 10100  |
| H / h  | 01000  | U / u  | 10101  |
| I / i  | 01001  | V / v  | 10110  |
| J / j  | 01010  | W / w  | 10111  |
| K / k  | 01011  | X / x  | 11000  |
| L / l  | 01100  | Y / y  | 11001  |
| M / m  | 01101  | Z / z  | 11010  |

> `00000` (no notches) is reserved and never assigned to a letter.

### Word Encoding — Nesting

Letters within a **single word** are stacked as nested squares:

- **Innermost** square = **first** letter
- **Outermost** square = **last** letter
- Each layer has a consistent gap from the one inside it


## Example: "abc"

<img width="496" height="480" alt="abc" src="https://github.com/user-attachments/assets/306c712c-e5d0-49ab-9050-60105d23fd32" />

-  a: innermost, 1st letter, 1 notch, binary= 00001
-  b: middle, 2nd letter, 1 notch, binary= 00010
-  c: outermost, 3rd letter, 2 notches, binary= 00011


### Sentence Encoding — Word Clusters

Each **word** is its own independent nested cluster. Words are placed **left to right** with a gap between them, just like normal writing.

```
"hello world"  →  [nested cluster for hello]   [nested cluster for world]
```

---

## Visual Reference

Below is a conceptual rendering of the letter `a` (lowercase, right side, code `00001`):

```
┌──────────┐
│          │
│          │
│          │
│          │
│          ├──┐   ← segment 5 active (the '1' in 00001)
└──────────┴──┘
```

And `A` (capital, left side, same code `00001`):

```
      ┌──────────┐
      │          │
      │          │
      │          │
      │          │
   ┬──┤          │   ← segment 5 active
   ┴──┴──────────┘
```

---

## Running the Generator

Will be added later

---

## Design Principles

**Topology over grid** — Unlike most encoding systems (Braille, QR codes) which use a fixed grid of binary dots, this system uses *containment* and *edge features*. The meaning lives in the relationship between shapes, not in a flat matrix.

**Reading order is spatial** — You read from the inside out, which mirrors how the shapes are drawn: start at the core, expand outward.

**Case is positional** — There is no separate symbol for uppercase vs lowercase. The same notch pattern on the left means capital; on the right means lowercase. One rule, zero ambiguity.

**Expandable** — The 5-bit system supports 31 unique codes (excluding `00000`). With 26 letters assigned, 5 codes remain free for future use such as punctuation, digits, or other scripts.

---

## Future Work

- Punctuation and digit support using the 5 remaining codes
- Multi-language extension using alternative base shapes (circle, triangle)
- A formal font/typeface specification
- Decoder implementation

---

## Origin

This encoding system was conceived as an extension of iterative geometric art — the observation that shapes nested inside one another have an inherent visual rhythm, and that rhythm could carry information.
