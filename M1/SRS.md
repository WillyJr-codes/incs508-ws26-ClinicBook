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
| 0.2 | 2026-11-27 | Xart3mis | Drafted F2 (4.2), R13 to R23 and 1.1 to 1.5 |

## 1. Introduction
### 1.1 Purpose
This SRS specifies the requirements for ClinicBook, an online appointment booking system for the University Health Center (UHC) of a Berlin university. It covers booking, changing and canceling appointments, viewing free slots, reminders, and the reception and clinician views of the schedule. It is the basis for design, prototyping and testing in the INCS508 project (Winter 2026).

### 1.2 Document Conventions
- Each requirement has an ID (R01, R02, …), reads "The system shall …", describes one testable behavior, and ends with its priority in brackets: High, Medium or Low.
- Features are numbered F1 to F5. Business goals BG1 to BG4 are those of the case (see 1.5).
- The Requirements Overview classifies each requirement as FR (functional), NFR (nonfunctional) or C (constraint).
- Each rule or number is stated once, in one requirement or in 5.5 Business Rules. Other requirements refer to it by ID.

### 1.3 Intended Audience
- The course instructor and teaching assistants, who assess the document.
- The project team: developers and testers.
- The client stakeholders named in the case: Dr. Miriam Kessler, Ms. Jana Novak, Mr. Tobias Brandt and Mr. Felix Wagner.

### 1.4 Product Scope
ClinicBook lets students and university employees find and book UHC appointments themselves. It aims to cut the reception team's phone time for booking (BG1), reduce unattended appointments (BG2), let patients book without calling (BG3) and protect patient confidentiality at least as well as today (BG4).
- In scope: booking, changing and canceling appointments; viewing available slots; sending appointment reminders; reception and clinician views of the schedule.
- Out of scope: medical records, prescriptions, billing, video consultations.

### 1.5 References
1. ClinicBook case, INCS508, Winter 2026 (`docs/case-clinicbook.md`).
2. IEEE SRS template (IEEE 830), Week 7 lecture.
3. Lecture 8: Principle-Driven Requirements Engineering (PDRE), Dr. John Zaki.

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

### 4.2 F2: Change or Cancel an Appointment
#### 4.2.1 Description and Priority
Patients can cancel a booking, or move it to another free slot, without calling the UHC. A freed slot is available to other patients at once. A change keeps the appointment type and follows the slot rules of F1 (R06 to R08). The number of changes per booking is not limited. Online changes and cancellations end at the change cutoff (R14). After that, the patient phones reception, and reception staff can still cancel or change any booking.

Priority: High. Canceling early frees slots for other patients, which serves BG2, and removes a reason to phone the UHC (BG1).

#### 4.2.2 Stimulus/Response Sequences
1. The patient opens their bookings. The system lists their upcoming bookings and offers cancel and change on each booking that is before the change cutoff.
2. The patient cancels a booking. The system cancels it, frees the slot and sends a confirmation on the patient's chosen reminder channel.
3. The patient changes a booking and picks a new free slot of the same appointment type. The system books the new slot, frees the old one and sends a confirmation.
4. The patient picks a new slot that another user booked a moment earlier. The system rejects it (R09) and keeps the original booking.
5. The patient opens a booking that is past the change cutoff. The system offers neither cancel nor change and tells the patient to contact reception.
6. A receptionist selects a patient's booking and cancels or changes it. The system applies the action at any time, including after the change cutoff, and sends the patient a confirmation.

#### 4.2.3 Requirements
- **R13:** The system shall show a logged-in patient a list of their upcoming bookings. (High)
- **R14:** The system shall let a patient cancel their own booking online until 24 hours before the slot starts (the change cutoff). (High)
- **R15:** The system shall let a patient change their own booking online until the change cutoff. (High)
- **R16:** The system shall let a patient change a booking only to a free slot of the same appointment type that meets the slot rules R06 to R08. (High)
- **R17:** The system shall keep the original booking when the new slot of a change cannot be booked. (High)
- **R18:** The system shall make a slot bookable again as soon as its booking is canceled or changed. (High)
- **R19:** The system shall not let a patient cancel or change a booking online after the change cutoff and shall tell the patient to contact reception. (High)
- **R20:** The system shall let a patient cancel or change only their own bookings. (High)
- **R21:** The system shall let reception staff cancel any booking at any time, including after the change cutoff. (Medium)
- **R22:** The system shall let reception staff change any booking at any time, including after the change cutoff. (Medium)
- **R23:** The system shall send the patient a confirmation of each cancellation or change on their chosen reminder channel, without naming the appointment type when the type is confidential. (Medium)

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
| R13 | List of upcoming bookings | FR | F2 | Scope: Booking, changing, and canceling appointments | — |
| R14 | Patient cancels until 24 h before slot | FR | F2 | BG2 | — |
| R15 | Patient changes until the change cutoff | FR | F2 | BG2 | — |
| R16 | Change only to same type under slot rules | FR | F2 | Scope: Booking, changing, and canceling appointments | — |
| R17 | Keep original booking if change fails | FR | F2 | BG3 | — |
| R18 | Freed slot bookable again at once | FR | F2 | BG2 | — |
| R19 | No online change or cancel after cutoff | FR | F2 | BG2 | — |
| R20 | Patients touch only their own bookings | FR | F2 | BG4 | — |
| R21 | Reception cancels any booking | FR | F2 | BG1 | — |
| R22 | Reception changes any booking | FR | F2 | BG1 | — |
| R23 | Cancel/change confirmation hides confidential type | FR | F2 | BG4 | — |

## Appendix A: Glossary

| Term | Definition | Added by |
|---|---|---|
| Slot | A free time window with one clinician, as long as the chosen appointment type, that a patient can book. | WillyJr-codes |
| Appointment type | A kind of visit with a fixed length and clinician role, such as general consultation or counseling session. | WillyJr-codes |
| Confidential appointment type | An appointment type whose name ClinicBook keeps out of messages and hides from roles without clinical access. Counseling session is the only one. | WillyJr-codes |
| Booking horizon | How far ahead a patient can book: 14 days. | WillyJr-codes |
| Lead time | The minimum time between now and the start of a bookable slot: 60 minutes. | WillyJr-codes |
| Change cutoff | The latest time a patient can cancel or change a booking online, set in R14. | Xart3mis |
| Change (of a booking) | Moving a booking to another free slot of the same appointment type. The old slot becomes free. | Xart3mis |

## Appendix B: Analysis Models

## Appendix C: To Be Determined List

| # | Open item | Section | Added by | Status |
|---|---|---|---|---|
| 1 | Reception contact details shown to patients after the change cutoff (phone number, phone hours) | 4.2 | Xart3mis | Open |
| 2 | Move the 24-hour change cutoff into a business rule once 5.5 has an owner | 4.2 | Xart3mis | Open |
