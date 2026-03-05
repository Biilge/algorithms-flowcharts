# Task 3 – Hospital Appointment and Test Results System

## System Description

This task models a hospital information system that allows patients to make appointments and access their laboratory test results.

The system verifies patient identity, allows appointment scheduling with doctors, and enables patients to view or download their test results.

## Functional Requirements

The system includes the following processes:

- Patient identity verification using ID number
- Action selection (Make Appointment or View Test Results)
- Appointment module:
  - Clinic selection
  - Doctor list display
  - Available time slots (loop)
  - Appointment confirmation
  - SMS notification
- Test results module:
  - Check if tests exist
  - Check if results are ready
  - Display test results
  - Option to download results as PDF
- Option to perform another action (loop)

## Algorithm Design

The algorithm was written using structured pseudocode including:

- START and END statements
- READ and PRINT operations
- IF–THEN decision structures
- LOOP constructs for appointment scheduling and repeated actions

## Flowcharts

Two diagram formats were created to represent the system logic:

- Graphviz DOT flowchart
- Mermaid flowchart

Both diagrams follow standard flowchart conventions:

- Oval for start and end
- Parallelogram for input and output
- Rectangle for processes
- Diamond for decision points
