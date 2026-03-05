# Class Diagram

**Type:** Class Diagram
**Exported:** 2026-03-05T06:05:33.760Z
**Source:** PlanVersion

## Linked Requirements

- 21223f04-60ce-4f5f-8794-597065ef8dbc
- 6becae11-4e6e-4aff-9a73-0801e62c2fb0
- 79040c72-4468-4308-a44e-215e21d83d87
- bec630ec-c4dd-4348-a159-bb59c1be29f0
- f5c02e4a-eb53-4f9e-8d09-7590a4a9a3e0
- 4b989d63-1847-4314-9548-29845ecb9da8
- 897a82d5-a28d-41b7-8566-6967c89b03df
- e156fcd5-019c-40cb-8a6d-acd3048fe396

## Diagram

```mermaid
classDiagram

%% Traceability: FR-001 FR-002 FR-004 FR-006 UC-001 UC-002
class User {
  +userId: string
  +name: string
  +email: string
  #role: string
  +authenticate(password: string): boolean
  +viewTimeline(): Entry[]
}

%% Traceability: FR-003 FR-004 FR-005 FR-006 UC-001
class Patient {
  +patientId: string
  +dateOfBirth: Date
  +bookAppointment(date: Date): Appointment
  +rescheduleAppointment(appointmentId: string, newTime: Date): boolean
  +submitPreVisitData(data: map): boolean
}

%% Traceability: FR-001 FR-002 FR-006 UC-003 UC-004
class Clinician {
  +clinicianId: string
  +specialty: string
  +manageTask(taskId: string): boolean
  +conductTelehealth(appointmentId: string): boolean
}

%% Traceability: FR-003 FR-004 FR-005 FR-006
class Entry {
  -entryId: string
  -timestamp: Date
  +type: string
  +content: string
  +attachFile(url: string): void
  +getSummary(): string
}

%% Traceability: FR-001 FR-006 FR-007
class Task {
  -taskId: string
  +title: string
  +description: string
  +status: string
  +assignTo(userId: string): void
  +complete(): void
}

%% Traceability: FR-007 FR-008
class Appointment {
  +appointmentId: string
  +scheduledAt: Date
  +status: string
  +reschedule(newTime: Date): boolean
  +cancel(): boolean
}

%% Traceability: FR-003 FR-004 FR-005
class Timeline {
  +timelineId: string
  +getEntries(): Entry[]
  +addEntry(e: Entry): void
}

%% Inheritance: Users specialize into Patient and Clinician
User <|-- Patient
User <|-- Clinician

%% Composition: Patient owns a Timeline (lifecycle bound)
Patient "1" *-- "1" Timeline : owns

%% Composition: Timeline contains Entries (entries are part of timeline)
Timeline "1" *-- "0..*" Entry : contains

%% Aggregation: Clinician aggregates Tasks (tasks can exist independently)
Clinician "1" o-- "0..*" Task : manages

%% Associations
User "1" --> "0..*" Entry : creates
Patient "1" --> "0..*" Appointment : appointments
Appointment "0..*" --> "0..1" Clinician : scheduledWith
Task "0..*" --> "0..*" Entry : relatesTo

%% Traceability summary (global)
%% Traceability: FR-001 FR-002 FR-003 FR-004 FR-005 FR-006 FR-007 FR-008
```
