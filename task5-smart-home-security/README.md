# Task 5 – Smart Home Security System

## System Description
This task models an IoT-based smart home security system that continuously monitors sensors, triggers alarms based on threat conditions, and sends notifications to the homeowner.

## Functional Requirements
- Check whether the system is active
- Continuous sensor-reading loop
- Motion sensor check
- Door/window sensor check
- Camera activation when a trigger is detected
- False alarm check (homeowner at home?)
- Determine alarm level (1-Low, 2-Medium, 3-High)
- Send notifications (SMS + App + Email)
- Wait and re-check in a loop
- Reset alarm or continue monitoring

## Algorithm Design
The algorithm uses an infinite monitoring loop. If the system is inactive, it waits and re-checks. If any sensor triggers, the camera is activated and the system checks whether the homeowner is at home to reduce false alarms. Alarm level is determined based on the combination of sensor triggers. Notifications are sent immediately, and a re-check loop continues until the alarm is reset or the threat disappears.

## Flowcharts
Two flowchart representations were created:
- Graphviz DOT format
- Mermaid flowchart format

Both diagrams use standard symbols for Start/End, Input/Output, Process, and Decision.
