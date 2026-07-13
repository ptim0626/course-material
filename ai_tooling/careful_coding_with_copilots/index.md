---
name: Careful coding with copilots
id: careful_coding_with_copilots
dependsOn: [introductory_courses.python]
files: [
   00_introduction.md,
   01_fluorescence_extraction.md,
   02_ode_simulation_and_inference.md,
   03_calibration.md]
learningOutcomes:
  - Decompose a complex programming problem into sub-problems
  - Apply systematic debugging strategies to LLM-generated code
summary: |
  An LLM has been asked to create a Python package to analyse fluorescence microscopy images of cells expressing a three-member negative feedback loop (the so-called "repressilator") and to recover the governing parameters of the network for each cell. It has done a poor job: you are tasked with fixing its implementation.
---

An LLM has been asked to create a Python package to analyse fluorescence microscopy images of cells expressing a three-member negative feedback loop (the so-called "repressilator") and to recover the governing parameters of the network for each cell.
It has done a poor job: you are tasked with fixing its implementation.
