# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is an Obsidian vault containing academic notes, primarily focused on Computer Science coursework but including Math, Statistics, Spanish, and Literature notes. The vault uses several Obsidian-specific features including Excalidraw diagrams, frontmatter metadata, and wiki-style links.

## Structure

The vault is organized into subject-based directories:

- `Csci/` - Computer Science courses organized by course number and name
  - `2011 - Discrete Math/`
  - `2021 - Machine Architecture/`
  - `2033 - Computational Linear Algebra/`
  - `3081W - Program Design & Development/`
  - `3921W - Issues in Computing/`
  - `3923 - Ethics in Computing/`
  - `4041 - Algorithms & Data Structures/`
  - `4061 - Intro to OS/` - Contains detailed OS concepts, C programming notes
  - `4521 - Applied Machine Learning/`
  - `4551 - Intro to Robotics/`
- `Math/` - Mathematics courses (e.g., Calculus)
- `Stat/` - Statistics courses (primarily 3021 - Intro to Probability & Statistics)
- `Span/` - Spanish language learning notes
- `Literature Notes/` - Notes on philosophical and literary works
- `Excalidraw/` - Visual diagrams created with Excalidraw plugin
- `.obsidian/` - Obsidian configuration and plugins (do not modify)

## Obsidian-Specific Features

### Frontmatter
Literature notes use YAML frontmatter for book metadata:
```yaml
---
title: Book Title
author: Author Name
publisher: Publisher
publishDate: YYYY-MM-DD
totalPage: 162
description: Description text
---
```

### Internal Links
- Wiki-style links: `[[filename]]` or `[[filename|display text]]`
- Image embeds: `![[image.png]]`
- Notes extensively use internal linking between related concepts

### Excalidraw Integration
- Files with `.excalidraw.md` extension contain embedded Excalidraw diagrams
- Library files (`.excalidrawlib`) contain reusable diagram components
- These are JSON-based but render as diagrams in Obsidian

## Content Conventions

### Code Examples
CS notes contain extensive code examples, particularly C programming:
- Code blocks use triple backticks with language identifier
- Include comments explaining key concepts
- Often accompanied by memory diagrams and tables

### Mathematical Content
- LaTeX math is supported inline with `$...$` and display mode with `$$...$$`
- Tables are used extensively for data type specifications and comparisons

### File Naming
- Course directories: `[Course Number] - [Course Name]/`
- Note files: Descriptive names like "Processes & Programs.md", "Lecture 7 — Linear Transformations.md"
- Use em dashes (—) in lecture titles, regular hyphens elsewhere

## Obsidian Configuration

The vault uses these plugins (found in `.obsidian/plugins/`):
- dataview - For creating dynamic note queries
- templater-obsidian - Template system for new notes
- obsidian-excalidraw-plugin - Diagram creation
- table-editor-obsidian - Enhanced table editing
- obsidian-linter - Markdown formatting
- highlightr-plugin - Text highlighting
- Various UI customization plugins

Configuration files in `.obsidian/`:
- `app.json` - Core Obsidian settings (links auto-update, preview mode default)
- `workspace.json` - Current workspace layout
- `graph.json` - Graph view configuration

## Working with This Vault

When creating or modifying notes:
1. Respect the existing directory structure and naming conventions
2. Use appropriate frontmatter for literature notes
3. Maintain consistent formatting (tables, code blocks, headings)
4. Use internal links to connect related concepts
5. For technical CS content, include code examples and memory diagrams where applicable
6. Do not modify `.obsidian/` configuration unless explicitly requested

This is a personal knowledge management system, so preserve the organizational 
structure and academic context when making changes.

## Lecture Material Processing

### Overview

The user may paste raw lecture content (typically from slides) directly into lecture note files when they miss a class. These notes need to be transformed into comprehensive, structured study materials.

### Detection of Raw Lecture Content

Identify raw lecture notes by these characteristics:

- Predominantly bullet points or fragmented text
- Slide-style formatting (terse phrases, minimal explanation)
- Lack of proper section organization
- Missing explanations, context, or examples
- No summary or review questions
- Minimal or no internal links
- Absence of visual aids for complex concepts

**When detected, proactively offer to restructure the content.**

### Processing Protocol

#### 1. Content Restructuring Requirements

Transform raw content into this structure:

**Overview** (2-3 sentences)

- Context and learning objectives
- Connection to previous lectures

**Key Concepts** (### headings)

- Define all major terms (**bold** on first use)
- Hierarchical organization with #### subheadings

**Detailed Explanations**

- Expand bullet points into complete explanations
- Add "why it matters" context
- Step-by-step processes for algorithms
- Intuition before formalism

**Important Formulas/Algorithms**

- LaTeX for math: `$inline$` or `$$display$$`
- Explain all variables and components
- Note assumptions and constraints

**Examples and Applications**

- Concrete examples for abstract concepts
- Real-world applications
- Worked problems with solutions
- Properly formatted code blocks

**Visual Aids**

- Create Excalidraw diagrams for complex concepts

**Summary**

- 3-5 key takeaways in bullet form

**Exercises**

- 1-2 guided practice problems with solutions

- 3-5 additional practice problems without solutions

**Review Questions**

- 3-5 questions testing understanding
- Mix recall and application levels

#### 2. Excalidraw Diagram Generation

**Create diagrams for:**

- System architecture or component relationships
- Process flows and algorithm steps
- Memory layouts and data structures
- State machines and automata
- Geometric/spatial concepts
- Network topologies
- Kinematic chains (robotics)
- Control system diagrams

**File specifications:**

- Location: `Excalidraw/` directory (vault root)
- Naming: `[Course]-Lec[N]-[Topic].excalidraw.md`
    - Example: `Csci40411-Lec3-FileSystems.excalidraw.md`

**Diagram principles:**

- Use rectangles for components/entities
- Use arrows for relationships/data flow
- Use text elements for all labels
- Maintain organized layout with consistent spacing (50-100px)
- Position elements logically (left-to-right, top-to-bottom)
- Generate unique IDs for each element

#### 3. Internal Linking
Create connections throughout the note:

- Related lectures: `[[Lecture N — Topic]]`
- Prerequisite concepts from earlier courses
- Related topics within the course
- Use descriptive link text: `[[filename|display text]]`
- Verify linked files exist in vault structure

#### 4. Subject-Specific Guidelines

**Computer Science:**

- Heavily commented code examples with language identifiers
- Memory layout diagrams
- Algorithm comparison tables
- Time/space complexity analysis
- Edge cases and common errors

**Mathematics:**

- Extensive LaTeX usage
- Step-by-step derivations
- Geometric interpretations
- Theorem statements and proofs

**Statistics:**

- Distribution and test statistic tables
- Worked examples with data
- Result interpretation guidance
- Assumption and condition notes

**Robotics (Csci 4551):**

- Kinematic configuration diagrams
- Transformation matrices with detailed notation
- Coordinate frame illustrations
- Control system block diagrams
- Planning/control algorithm flowcharts

**Spanish:**

- Conjugation tables
- Example sentences with translations
- Grammar explanations
- Cultural context

#### 5. Quality Checklist

Before completing restructuring, verify:

- [ ] All technical terms clearly defined
- [ ] Code blocks have language identifiers
- [ ] LaTeX formatting correct
- [ ] Internal links use valid filenames
- [ ] Diagrams created and embedded properly
- [ ] Summary captures all key points
- [ ] Review questions test understanding
- [ ] Proper heading hierarchy (##, ###, ####)
- [ ] Consistent with vault conventions
- [ ] No orphaned concepts

### File Operations

**Main lecture note:**

- Modify existing file in place
- Preserve original filename and location
- Maintain frontmatter if present

**Excalidraw diagrams:**

- Create new files in `Excalidraw/` directory
- One file per diagram
- Use descriptive, lecture-tied names

### Completion Summary Template

After processing, provide:

```
Lecture note restructured successfully.

Changes made:
- Organized into [N] main sections
- Added detailed explanations for [concepts]
- Created [N] Excalidraw diagrams
- Added [N] internal links to related topics
- Included summary with [N] key takeaways
- Generated [N] review questions

Diagram files created:
- Excalidraw/[filename1].excalidraw.md
- Excalidraw/[filename2].excalidraw.md
```

### Edge Case Handling

**Unclear/ambiguous content:**

- Make reasonable assumptions based on course context
- Mark uncertainties: `<!-- TODO: Verify [point] -->`
- Ask for clarification on critical ambiguities

**Very short lectures (<500 words):**

- Scale sections appropriately
- May combine sections (e.g., examples with explanations)
- Minimum: overview, key concepts, summary

**Very long lectures (>3000 words):**

- Confirm full processing or specific sections
- Consider suggesting split into multiple notes
- Prioritize comprehensive diagrams for complex sections

**Complex diagrams:**

- Create multiple simple diagrams vs. one complex diagram
- Provide clear captions
- Use progressive complexity (overview → details)

### Important Principles

- Maintain technical accuracy and academic integrity
- Balance detail with readability
- Match existing vault note style
- Don't skip important original content
- Add insights and connections beyond raw slides
- Use em dashes (—) in lecture titles
- Generate meaningful, non-trivial review questions