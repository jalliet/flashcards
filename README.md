# Generating STEM Flashcards

A Claude skill for generating atomic, cognitively-principled flashcards from technical source material.

## What It Does

Upload lecture notes, textbooks, or technical documentation. Get interactive flashcards organised into three cognitive layers:

| Layer | Tests | Example |
|-------|-------|---------|
| **L1: Recall** | Facts, definitions, formulas | "Define the Jacobian matrix" |
| **L2: Understanding** | Why/how, intuitions | "Why is the Jacobian useful for coordinate transforms?" |
| **L3: Boundaries** | Limitations, edge cases | "When does the Jacobian become singular?" |

The skill enforces atomicity (one concept per card), refuses to card inappropriate content (proofs, worked examples), and renders mathematical notation via KaTeX.

---

## Installation

1. Clone this repo or download as ZIP: `git clone https://github.com/jalliet/flashcards.git`
2. Create a skill package: `zip -r flashcards.zip SKILL.md references/ assets/`
3. Upload to Claude.ai: **Settings > Capabilities > Skills > Upload Skill**

---

## Usage

### Basic

Upload a PDF, paste lecture notes, or describe a topic:

> "Generate flashcards spanning topicas from all the project files."

> "Create flashcards covering gradient descent, including failure modes"

> "Make revision cards for chapters 3-5 of the uploaded PDF"

### With Layer Filtering

> "Generate only L3 (boundary) cards for this material"

> "Focus on L1 recall cards for definitions and formulas"

### Iterative Refinement

> "Card 12's formula is missing the inverse; fix it"

> "Split card 7 into two separate cards"

> "Add more L2 cards for the eigenvalue section"

---

## Recommended Setup

### Projects

Create a Claude Project for your course/subject. Add:

1. This skill (`.skill` file)
2. Your lecture notes and readings
3. Project instructions like: "Always use the flashcard skill when I ask for revision materials"

### Complementary Skills

For best results with complex documents, also install from [anthropics/skills](https://github.com/anthropics/skills):

| Skill | Why |
|-------|-----|
| `pdf` | Better extraction from scanned/complex PDFs |
| `docx` | Preserves formatting from Word documents |
| `frontend-design` | Improves React artifact rendering and styling for academic content|

The flashcard skill works without these, but they improve source parsing.

---

## Output

The skill produces an interactive React artifact with:

- Layer filtering (L1/L2/L3 tabs)
- Topic filtering
- Star/favourite cards
- Shuffle mode
- Responsive design

### Anki Import

The exported `.txt` file can be imported directly into Anki. You have two options:

#### Quick Import (Basic note type)

Import the file as-is using Anki's built-in **Basic** note type. You get Front/Back cards with layer and topic info in the Tags column. No setup required — just **File → Import** and go.

#### Full Experience (Custom note type)

For styled cards with a layer badge on the front and topic metadata on the back, create a custom note type first:

1. Open Anki → **Tools → Manage Note Types**
2. Click **Add** → choose **Add: Basic** → name it **"3-Layer Card"**
3. Click **Fields...** and add two new fields: **Layer** and **Topic** (Front and Back already exist)
4. Click **Cards...** and paste the templates below into the corresponding editors
5. Click **Save**

Then import: **File → Import** → select the `.txt` file → choose **"STEM Flashcards (3-Layer)"** as the note type → verify the 5-column field mapping → **Import**.

<details>
<summary><strong>Front Template</strong> (paste into front template editor)</summary>

```html
<div class="layer-badge">{{Layer}}</div>
<div class="question">{{Front}}</div>
```

</details>

<details>
<summary><strong>Back Template</strong> (paste into back template editor)</summary>

```html
{{FrontSide}}
<hr id="answer">
<div class="answer">{{Back}}</div>
<div class="metadata">
  Topic: {{Topic}}
</div>
```

</details>

<details>
<summary><strong>Styling</strong> (paste into styling editor)</summary>

```css
.card {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  font-size: 20px;
  text-align: center;
  color: #1a1a1b;
  background-color: #ffffff;
  padding: 20px;
}

.layer-badge {
  display: inline-block;
  padding: 6px 14px;
  border-radius: 4px;
  font-size: 0.8em;
  font-weight: 600;
  margin-bottom: 20px;
  background: #f0f0f0;
  color: #555;
}

.question {
  font-size: 1.1em;
  margin: 30px 0;
  line-height: 1.5;
}

.answer {
  margin: 30px 0;
  line-height: 1.6;
  text-align: left;
}

.metadata {
  margin-top: 25px;
  padding-top: 15px;
  border-top: 1px solid #e0e0e0;
  font-size: 0.8em;
  color: #888;
}

hr#answer {
  border: none;
  border-top: 2px solid #e0e0e0;
  margin: 20px 0;
}
```

</details>

---

## File Structure

```
generating-stem-flashcards/
├── README.md                 # This file
├── THEORY.md                 # Scientific foundations (Bloom, CLT, etc.)
├── SKILL.md                  # Main skill instructions
├── references/
│   ├── COGNITIVE_LAYERS.md   # L1/L2/L3 definitions
│   ├── ATOMICITY.md          # Quality rules, refusal policy
│   └── LATEX_SYNTAX.md       # KaTeX reference
└── assets/
    └── flashcard_template.jsx
```

**Note**: Only the skill files (SKILL.md, references/, assets/) are packaged in the `.skill` file. README.md and THEORY.md are GitHub documentation only.

---

## Why This Skill?

Most AI flashcard generators produce low-quality cards: compound questions, no cognitive framework, no quality control.

This skill encodes learning science directly into generation:

- **Bloom's Taxonomy** → Three-layer structure
- **Cognitive Load Theory** → Atomicity rules
- **Minimum Information Principle** → Refusal policy

See [THEORY.md](THEORY.md) for the full scientific foundation with citations.

---

## License

MIT