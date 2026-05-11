---
created: 2026-05-12 02:21
updated:
tags:
  - interview
  - "#interview-backend-canva"
status: active
company: Canva
level: Senior
related:
---

As per the conversation with Ex-Canva Employee, Here are key takeaways for preparing for Backend Engineer interview.

### System Design
 - Design Canva feature
 - How would you code file management
 - Designs related to canva

	AWS Usage -> Canva uses DynamoDB ( used Postgres before), Elastic Cache, EC2, ELB

### DS & Algos
- HashMap
- Tree
- Quad Tree

### Coding Round (Pair programming)
- Configure IDE (vs code/intellij) with Claude
- Ask AI to plan before asking to write code, spend time reviewing the plan (10-15 mins)
- Ask the interviewer, do we need TDD (not followed in Canva but good to ask rather assume)
- 

### Behaviour Round
- How you handle conflict?

---
**

# Notes by Prepfully expert

  

Hao Wooi Lim (11 May 2026, 8:16 PM)

### System Design

 - Design a word cloud generator (e.g. https://www.wordclouds.com/)

- given an input of words -> out come a PNG of pretty word cloud

- describe the algorithm, where would you put the word and ensuring they don't overlap

- No need to write actual code, psedudocode is fine

### Coding round

 - Given an actual Java interface, and asked to implement them

- Will be good to have some familiarity with Java

- No way to execute, so refreshing your Java syntax will be useful

### Algo round

### Behavior round

## Sample Questions

#### 1. Design a real-time collaborative drawing/document editing system

Question:

Design the backend for a canvas editor where multiple users can edit the same design at the same time, similar to Google Docs/Figma/Canva.

They may ask about:

WebSocket architecture

Conflict resolution

Operational Transform vs CRDT

Presence indicators: cursor, selection, “Simon is editing”

Offline edits and reconnection

Document versioning

Undo/redo

Scaling rooms with many users

Testing:

Whether you understand distributed state, low-latency messaging, consistency, and collaboration.

  

#### 2.Design the data model and storage system for a canvas document

Question:

How would you store a complex design containing text boxes, shapes, images, layers, groups, pages, fonts, animations, and metadata?

They may ask:

Do you store the canvas as JSON?

How do you version the document?

How do you support undo/redo?

How do you avoid saving the entire document on every change?

How do you handle very large designs?

How do you migrate old document schemas?

  
**

---


## Question

## Short Answer

## Deep Explanation

## Internal Mechanics

## Trade-Offs

## Real-World Example

## Common Mistakes

## Follow-Up Questions

## Related Notes

- [[ ]]

## Revision Notes
