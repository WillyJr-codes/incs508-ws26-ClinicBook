<!--
  Headings follow the Week 7 IEEE SRS template.
  Each part has one owner, listed in M1/OWNERS.md. Edit only the parts you own.
  Members may draft their parts with an AI agent using the fill-srs skill; every such use is logged in AI-USE.md.
  Delete this comment before submission.
-->

# Software Requirements Specification: ClinicBook

**Version:** 
**Date:** 
**Team:** 

## Revision History

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-26 | WillyJr-codes | Drafted F1 (4.1) and R01 to R12 |

## 1. Introduction
### 1.1 Purpose
### 1.2 Document Conventions
### 1.3 Intended Audience
### 1.4 Product Scope
### 1.5 References

## 2. Overall Description
### 2.1 Product Perspective
### 2.2 Product Functions
### 2.3 User Classes and Characteristics
### 2.4 Operating Environment
### 2.5 Design and Implementation Constraints
### 2.6 User Documentation
### 2.7 Assumptions and Dependencies

## 3. External Interface Requirements
### 3.1 User Interfaces
### 3.2 Hardware Interfaces
### 3.3 Software Interfaces
### 3.4 Communication Interfaces

## 4. System Features

### 4.1 F1: View Available Slots and Book an Appointment
#### 4.1.1 Description and Priority
Students and university employees log in with their university account, look up free appointment slots and book one without calling the UHC. Reception staff book for patients who phone or walk in, under the same slot rules. ClinicBook offers four appointment types: general consultation (15 minutes, physician), extended consultation (30 minutes, physician), nurse visit for vaccinations or blood pressure checks (15 minutes, nurse) and counseling session (50 minutes, psychologist). Counseling is a confidential appointment type. Slots are offered only during opening hours (Monday to Thursday 08:00 to 16:00, Friday 08:00 to 13:00, closed on weekends), from 60 minutes after the current time up to 14 days ahead. A patient may hold any number of active bookings.

Priority: High. Patients who book online no longer need the phone line, which serves BG3 directly and reduces the reception team's phone time (BG1).

#### 4.1.2 Stimulus/Response Sequences
1. The patient opens the booking screen. The system shows the free slots for the first appointment type on the first open day that has free slots.
2. The patient picks an appointment type, a date and, optionally, a clinician. The system shows the matching free slots, grouped by clinician.
3. The patient selects a slot. The system books it, shows the date, time and clinician on screen, and sends a confirmation message on the patient's chosen reminder channel.
4. The patient selects a slot that another user booked a moment earlier. The system rejects the booking and asks the patient to pick another slot.
5. A receptionist selects a patient who phoned or walked in, then picks a slot. The system books it for that patient under the same rules as an online booking.

#### 4.1.3 Requirements
- **R01:** The system shall let only students and university employees who are logged in with their university account book appointments online. (High)
- **R02:** The system shall offer the appointment types general consultation (15 minutes, physician), extended consultation (30 minutes, physician), nurse visit (15 minutes, nurse) and counseling session (50 minutes, psychologist). (High)
- **R03:** The system shall let patients book counseling sessions online under the same rules as the other appointment types. (High)
- **R04:** The system shall show patients the free slots for a selected appointment type and date. (High)
- **R05:** The system shall let patients filter the free slots by clinician. (Low)
- **R06:** The system shall offer slots only from 08:00 to 16:00 Monday to Thursday and from 08:00 to 13:00 on Friday, and no slots on Saturday or Sunday. (High)
- **R07:** The system shall not offer slots that start more than 14 days after the current date. (High)
- **R08:** The system shall not offer slots that start less than 60 minutes after the current time. (High)
- **R09:** The system shall reject a booking for a slot that is already booked and ask the patient to choose another slot. (High)
- **R10:** The system shall show the date, time and clinician of a new booking on screen as soon as it is booked. (High)
- **R11:** The system shall send the patient a booking confirmation on their chosen reminder channel that does not name the appointment type when the type is confidential. (Medium)
- **R12:** The system shall let reception staff book an appointment for a patient who phones or walks in, under the same slot rules as online booking. (Medium)

### 4.2 F2: 
#### 4.2.1 Description and Priority
#### 4.2.2 Stimulus/Response Sequences
#### 4.2.3 Requirements

### 4.3 F3: 
#### 4.3.1 Description and Priority
#### 4.3.2 Stimulus/Response Sequences
#### 4.3.3 Requirements

### 4.4 F4: 
#### 4.4.1 Description and Priority
#### 4.4.2 Stimulus/Response Sequences
#### 4.4.3 Requirements

### 4.5 F5: 
#### 4.5.1 Description and Priority
#### 4.5.2 Stimulus/Response Sequences
#### 4.5.3 Requirements

## 5. Other Nonfunctional Requirements
### 5.1 Performance Requirements
### 5.2 Safety Requirements
### 5.3 Security Requirements
### 5.4 Software Quality Attributes
### 5.5 Business Rules

## 6. Other Requirements

## Requirements Overview

| ID | Requirement | Class (FR/NFR/C) | Feature | Case trace | Prototype trace |
|---|---|---|---|---|---|
| R01 | Only logged-in students and employees book online | C | F1 | Constraint: Patients must log in using their existing university account | Login page |
| R02 | Four appointment types with fixed durations | FR | F1 | BG3 | Patient > Book |
| R03 | Counseling bookable online like other types | FR | F1 | BG3 | Patient > Book |
| R04 | Show free slots by type and date | FR | F1 | Scope: Viewing available appointment slots | Patient > Book |
| R05 | Filter free slots by clinician | FR | F1 | BG3 | Patient > Book |
| R06 | Slots only within opening hours | FR | F1 | BG3 | Patient > Book |
| R07 | Booking horizon of 14 days | FR | F1 | BG3 | Patient > Book |
| R08 | Minimum lead time of 60 minutes | FR | F1 | BG3 | Patient > Book |
| R09 | Prevent double booking of a slot | FR | F1 | BG3 | Patient > Book > Book this slot |
| R10 | On-screen booking confirmation | FR | F1 | BG3 | Patient > Book |
| R11 | Confirmation message hides confidential type | FR | F1 | BG4 | Reminder outbox |
| R12 | Reception books for phone and walk-in patients | FR | F1 | BG1 | Reception > Book for patient |
| R13 | | | | | |
| R14 | | | | | |
| R15 | | | | | |

## Appendix A: Glossary

| Term | Definition | Added by |
|---|---|---|
| Slot | A free time window with one clinician, as long as the chosen appointment type, that a patient can book. | WillyJr-codes |
| Appointment type | A kind of visit with a fixed length and clinician role, such as general consultation or counseling session. | WillyJr-codes |
| Confidential appointment type | An appointment type whose name ClinicBook keeps out of messages and hides from roles without clinical access. Counseling session is the only one. | WillyJr-codes |
| Booking horizon | How far ahead a patient can book: 14 days. | WillyJr-codes |
| Lead time | The minimum time between now and the start of a bookable slot: 60 minutes. | WillyJr-codes |

## Appendix B: Analysis Models

## Appendix C: To Be Determined List

| # | Open item | Section | Added by | Status |
|---|---|---|---|---|
