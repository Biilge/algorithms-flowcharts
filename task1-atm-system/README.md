# Task 1 – ATM Cash Withdrawal System

## System Description

This project models the complete process of withdrawing cash from an ATM machine.

The system includes PIN verification with a maximum of three attempts, account balance validation, withdrawal amount rules, daily withdrawal limit control, and the option to repeat the transaction.

---

## Functional Requirements

- Card insertion and PIN request
- Maximum 3 incorrect PIN attempts (card blocking)
- Account balance query
- Withdrawal amount input
- Insufficient balance validation
- Multiple of 20 TL validation
- Daily withdrawal limit validation
- Cash dispensing and receipt printing
- Option to perform another transaction (loop)

---

## Algorithm Structure

The algorithm was designed using structured pseudocode including:

- START / END constructs
- READ and PRINT operations
- IF–THEN decision structures
- LOOP structures for repetition
- Nested conditional validations

The withdrawal process includes three sequential checks:

1. Amount must be a multiple of 20 TL  
2. Amount must not exceed account balance  
3. Amount must not exceed daily withdrawal limit  

If all conditions are satisfied, the system dispenses cash and prints a receipt.

---

## Flowchart Representation

Two flowchart representations were created:

- Graphviz DOT format
- Mermaid flowchart format

Flowchart symbols used:

- Oval → Start / End
- Parallelogram → Input / Output
- Rectangle → Process
- Diamond → Decision

---

## Personal Reflection

This task helped strengthen my understanding of structured algorithm modeling, nested condition logic, and system validation processes.

Modeling repeated PIN attempts and layered withdrawal validations was the most critical part of the design.
