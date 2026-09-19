# Prototype · Juno

> Module 1 · Prompting. The clickable Claude prototype that deployed on Replit.

## Prototype link

_https://juno-kori.replit.app_

_____

## What it demonstrates

_A clickable three column workflow for Juno PM. Raw transcripts on the left, structured insight cards in the middle, a draft PRD in markdown on the right, showing where an AI associate PM fits into the synthesis loop without taking the pen away from the human._

_____

## Debrief

- **What worked:** The information architecture. Three columns — raw, structured, artifact — with the input never leaving the screen, so the PM can check a conclusion against its source without switching context. It's held up through every rebuild since. The insight card also proved to be the right unit of synthesis: priority, sentiment, source timestamp, verbatim quote, theme and confidence in one object you can accept or argue with on its own terms.
- **What broke / felt like a toy:** Because nothing was actually computed, nothing could fail: no error state, no empty result, no low-confidence path, no escalation. A product whose whole premise is judgement under uncertainty had no representation of uncertainty anywhere in it, and the confidence labels were decoration printed from a literal.
- **What I'd change next pass:** Make the input actually matter, parse the pasted sources so the card count, IDs and timestamps are derived rather than authored by me. Then show the work: a visible tool trace with declared side effects and a human approval gate on anything that writes, so the PM can supervise the agent instead of just reading its output.
