# Task 4 – University Course Registration System

## System Description
This task models the rules and checks of a university course registration system. The student logs in, browses courses, adds/drops courses with multiple validation checks, and finalizes registration with possible advisor approval.

## Functional Requirements
- Student login (student ID + password)
- View course list (loop)
- Add course with checks:
  - Capacity / quota check
  - Prerequisite check
  - Schedule conflict check
  - Credit limit check (total credits ≤ 35)
- Drop course (loop)
- Advisor approval requirement if GPA < 2.5
- Display registration summary and confirm registration

## Algorithm Design
The algorithm was written using structured pseudocode with:
- START / END
- READ / PRINT for input-output
- IF–THEN decisions for validation steps
- LOOP for menu-based actions and repeated add/drop operations

The add-course process uses sequential validation (quota → prerequisite → schedule conflict → credit limit). Only if all checks pass, the course is added and total credits are updated.

## Flowcharts
Two representations were created:
- Graphviz DOT format
- Mermaid flowchart format

Both follow standard flowchart symbols:
- Oval: Start/End
- Parallelogram: Input/Output
- Rectangle: Process
- Diamond: Decision
