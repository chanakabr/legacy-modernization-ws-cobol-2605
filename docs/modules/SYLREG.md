# SYLREG — Syllabus Registration Module

## Purpose
SYLREG captures syllabus master data and weekly plans from screen input, validates the course ID via a shared routine, and writes records to the syllabus indexed file. It loops to allow consecutive registrations until the operator chooses to exit.

## Per-paragraph documentation

### MAIN-PROCESS SECTION
- **Purpose**: Orchestrate the registration workflow in a loop and terminate after closing the file.
- **Reads**: WS-EXIT, OPEN-FILE, INITIALIZE-SYLLABUS-RECORD, INPUT-SYLLABUS-DATA, INPUT-WEEK-PLAN-DATA, WRITE-SYLLABUS-RECORD, CHECK-CONTINUE, CLOSE-FILE
- **Writes**: (none)
- **Calls**: OPEN-FILE, INITIALIZE-SYLLABUS-RECORD, INPUT-SYLLABUS-DATA, INPUT-WEEK-PLAN-DATA, WRITE-SYLLABUS-RECORD, CHECK-CONTINUE, CLOSE-FILE
- **Notes**: Uses PERFORM UNTIL WS-EXIT; exit depends on accepted continue flag values.

### OPEN-FILE SECTION
- **Purpose**: Open the syllabus file for I/O and create it when it does not yet exist.
- **Reads**: WS-FILE-NOT-FOUND, SYLLABUS-FILE
- **Writes**: WS-FILE-STATUS, SYLLABUS-FILE
- **Calls**: (none)
- **Notes**: Retries with OPEN OUTPUT then reopens I-O when initial open reports not found.

### CLOSE-FILE SECTION
- **Purpose**: Close the syllabus file handle before program return.
- **Reads**: SYLLABUS-FILE
- **Writes**: SYLLABUS-FILE
- **Calls**: (none)
- **Notes**: None.

### INITIALIZE-SYLLABUS-RECORD SECTION
- **Purpose**: Reset the file record buffer before new input is captured.
- **Reads**: SYLLABUS-FILE-REC
- **Writes**: SYLLABUS-FILE-REC
- **Calls**: (none)
- **Notes**: None.

### INPUT-SYLLABUS-DATA SECTION
- **Purpose**: Collect core syllabus fields, validate course ID through SYLCOM, and retry on validation error.
- **Reads**: SYLLABUS-INPUT-SCREEN, SYL-COURSE-ID, WS-RETURN-CODE, WS-RESULT
- **Writes**: SYL-COURSE-ID, SYL-COURSE-NAME, SYL-DEPARTMENT-ID, SYL-TEACHER-ID, SYL-SEMESTER, SYL-CREDITS, SYL-DESCRIPTION, SYL-OBJECTIVES, WS-FUNCTION-CODE, WS-PARAM-1, WS-PARAM-2, WS-RESULT, WS-RETURN-CODE
- **Calls**: SYLCOM, INPUT-SYLLABUS-DATA
- **Notes**: Recursively PERFORMs itself when validation fails, which is an early retry branch.

### INPUT-WEEK-PLAN-DATA SECTION
- **Purpose**: Capture the 15-week plan fields from the week-plan screen.
- **Reads**: WEEK-PLAN-SCREEN
- **Writes**: SYL-WEEK-PLAN(1), SYL-WEEK-PLAN(2), SYL-WEEK-PLAN(3), SYL-WEEK-PLAN(4), SYL-WEEK-PLAN(5), SYL-WEEK-PLAN(6), SYL-WEEK-PLAN(7), SYL-WEEK-PLAN(8), SYL-WEEK-PLAN(9), SYL-WEEK-PLAN(10), SYL-WEEK-PLAN(11), SYL-WEEK-PLAN(12), SYL-WEEK-PLAN(13), SYL-WEEK-PLAN(14), SYL-WEEK-PLAN(15)
- **Calls**: (none)
- **Notes**: None.

### WRITE-SYLLABUS-RECORD SECTION
- **Purpose**: Persist the current syllabus record and report duplicate-key write failures.
- **Reads**: SYLLABUS-FILE-REC, SYL-COURSE-ID
- **Writes**: SYLLABUS-FILE, WS-ERROR-MSG
- **Calls**: (none)
- **Notes**: INVALID KEY branch composes an error message with the conflicting course ID.

### CHECK-CONTINUE SECTION
- **Purpose**: Ask whether to continue and update the continuation flag.
- **Reads**: WS-MSG-CONTINUE
- **Writes**: WS-CONTINUE-FLAG
- **Calls**: (none)
- **Notes**: Any non-Y/y input effectively drives loop exit because WS-EXIT includes N/n only.

## Module dependencies
```mermaid
flowchart LR
    SYLREG --> SYLCOM
```

## Open questions
- (none — every paragraph fully understood)
