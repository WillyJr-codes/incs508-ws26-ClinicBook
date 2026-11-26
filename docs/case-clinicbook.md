<!--
  Verbatim transcription of the course case PDF "Case - ClinicBook.pdf" (INCS508, Winter 2026).
  Do not edit the wording. Use it as the source for the "Case trace" column in M1/SRS.md.
-->

# ClinicBook: Online Appointment Booking for a University Health Center

German International University. Faculty: Informatics and Computer Science. Course: Requirements Engineering. Semester: Winter 2026.

## Context

The University Health Center (UHC) of a mid-size university in Berlin offers free basic medical care to enrolled students and university employees.

The UHC has:

- General physicians.
- A nurse.
- A psychologist who offers short-term counseling.
- A reception team.

The UHC also runs an existing practice management system, **MediDesk**, in which clinicians maintain medical records and manage their calendars.

## Problem

Currently, appointments can only be made by phone during a short morning window or by walking in.

This creates several problems:

- The phone line is overloaded at the start of the booking window, and many callers give up.
- The reception team spends much of its day on the phone instead of attending to patients.
- A noticeable share of booked appointments are not attended, resulting in lost appointment slots.

The UHC wants an online booking system, **ClinicBook**, that students and university employees can use themselves.

## Known Stakeholders

| Name | Role |
|---|---|
| Dr. Miriam Kessler | Head of the UHC, physician, project client and budget owner |
| Dr. Samir Haddad | General physician (full-time) |
| Ms. Jana Novak | Lead receptionist |
| Ms. Eva Lindgren | Psychologist, student counseling |
| Mr. Tobias Brandt | University data protection officer (DPO) |
| Ms. Aylin Demir | Student union representative for health and well-being |
| Mr. Felix Wagner | University IT, identity and integration services |
| Students and university employees | Patients who will book appointments |

## Business Goals

- **BG1:** Reduce the reception team's time spent on the phone for booking.
- **BG2:** Reduce the share of booked appointments that are not attended.
- **BG3:** Let patients find and book a suitable appointment without calling.
- **BG4:** Protect patients' confidentiality at least as well as today.

## Scope Hints

**In Scope:**

- Booking, changing, and canceling appointments.
- Viewing available appointment slots.
- Sending appointment reminders.
- Reception and clinician views of the schedule.

**Probably Out of Scope:**

- Medical records.
- Prescriptions.
- Billing.
- Video consultations.

The UHC expects ClinicBook to work with MediDesk in some way. How exactly this integration will work is not yet clear.

## Known Constraints

- Patients must log in using their existing university account.
- The system will process health-related data about identifiable people.
- The budget is limited and fixed by the university for this financial year.

## Deliberately Left Open

The following information has not yet been specified and must be unique to your project:

- Opening hours.
- Appointment types and durations.
- Booking and cancellation rules.
- How no-shows are handled.
- Which data each user group may see.
- How ClinicBook and MediDesk exchange data.
- Reminder channels.
- Supported languages.
- Expected volumes and peak periods.
- Deadlines and budget figures.
- Any additional information about users or stakeholders not listed above.

## First Questions to Think About

1. How do patients book appointments today, and what goes wrong with the current process?
2. Which appointment types exist, and do they follow different rules?
3. What exactly happens today when a patient does not show up or cancels late?
4. Which information is confidential, and from whom?
5. Which stakeholders might disagree with each other, and about what?
6. What does "success" look like for Dr. Kessler one year after go-live?
