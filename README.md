# Clinic Management System – ER Diagram

## excalidraw - https://excalidraw.com/#json=SVPzPnBFpx4O4GLNgwRIS,a68xc06icSyVOvvROqj5dg

This project contains the **Entity Relationship Diagram (ERD)** for a **Clinic Management System**.
The goal of the system is to digitally organize clinic operations such as **patient management, doctor scheduling, consultations, diagnostic testing, reporting, and payments**.

The database design models the real workflow of a clinic where:

1. Patients register in the system.
2. Patients book appointments with doctors.
3. When they arrive, staff performs a **check-in**.
4. Doctors conduct **consultations**.
5. Doctors may prescribe **diagnostic tests**.
6. Labs generate **diagnostic reports**.
7. Payments are recorded for consultations and tests.

This ERD focuses on creating a **clean, scalable, and realistic clinic database** while keeping the scope manageable for a clinic-level system.

---

# System Workflow

The database models the following clinic workflow:

```
PATIENT
   │
   │ books
   ▼
APPOINTMENT
   │
   │ patient arrives
   ▼
CHECKIN
   │
   │ doctor consultation
   ▼
CONSULTATION
   │
   │ doctor prescribes tests
   ▼
TEST_ORDER
   │
   │ lab generates report
   ▼
DIAGNOSTIC_REPORT
   │
   │ payment recorded
   ▼
PAYMENT
```

This structure separates **appointment scheduling** from **actual consultation**, which reflects real-world clinic behavior.

---

# Entities in the System

The ERD includes the following main entities.

## 1. Patient

Stores patient personal and contact information.

Key attributes:

* patient_id (PK)
* first_name
* last_name
* date_of_birth
* gender
* phone
* email
* address_line1
* address_line2
* city
* state
* pincode
* blood_group
* emergency_contact_name
* emergency_contact_phone
* created_at
* updated_at

A **patient can have multiple appointments and consultations**.

---

## 2. Doctor

Stores doctor information.

Key attributes:

* doctor_id (PK)
* first_name
* last_name
* license_number
* phone
* email
* years_of_experience
* consultation_fee
* is_active

A **doctor can attend many patients and consultations**.

---

## 3. Specialty

Represents medical specialties.

Examples:

* Cardiology
* Dermatology
* Orthopedics
* Pediatrics

Attributes:

* specialty_id (PK)
* name
* description

---

## 4. Doctor_Specialty

Junction table connecting doctors and specialties.

This allows a doctor to have **multiple specialties**.

Attributes:

* doctor_id (FK)
* specialty_id (FK)

Relationship:

```
DOCTOR (M) ----- (M) SPECIALTY
```

---

## 5. Appointment

Represents a scheduled meeting between a patient and doctor.

Attributes:

* appointment_id (PK)
* patient_id (FK)
* doctor_id (FK)
* appointment_date
* start_time
* end_time
* appointment_status
* appointment_type
* booking_source
* notes
* created_at

Possible appointment statuses:

* scheduled
* cancelled
* completed
* no_show

Important design decision:

**An appointment does not always lead to a consultation.**

---

## 6. Staff

Represents clinic staff members.

Examples:

* Receptionist
* Nurse
* Lab Technician

Attributes:

* staff_id (PK)
* first_name
* last_name
* role
* phone
* email
* is_active
* created_at

Staff help manage **patient check-ins and operations**.

---

## 7. Checkin

Represents the moment a patient arrives at the clinic.

Attributes:

* checkin_id (PK)
* appointment_id (FK)
* staff_id (FK)
* checkin_time
* queue_number
* status
* notes
* created_at

Possible statuses:

* waiting
* in_consultation
* completed

This enables **queue management in the clinic**.

---

## 8. Consultation

Represents the actual doctor visit.

Attributes:

* consultation_id (PK)
* appointment_id (FK)
* doctor_id (FK)
* patient_id (FK)
* consultation_start
* consultation_end
* diagnosis
* doctor_notes
* followup_required
* followup_date
* created_at

Relationship:

```
APPOINTMENT (1) → (0..1) CONSULTATION
```

This ensures:

* cancelled appointments have **no consultation**
* completed visits create **one consultation**

---

## 9. Diagnostic Test

Master catalog of all available tests.

Examples:

* Blood Test
* X-Ray
* MRI
* ECG

Attributes:

* test_id (PK)
* test_name
* test_description
* test_category
* price
* is_active
* created_at

---

## 10. Test Order

Represents tests prescribed by a doctor during consultation.

Attributes:

* test_order_id (PK)
* consultation_id (FK)
* test_id (FK)
* patient_id (FK)
* ordered_by_doctor_id (FK)
* order_status
* priority
* order_date
* notes

Order statuses:

* ordered
* sample_collected
* processing
* completed
* cancelled

A **consultation can have multiple test orders**.

---

## 11. Diagnostic Report

Generated after the lab processes the test.

Attributes:

* report_id (PK)
* test_order_id (FK)
* patient_id (FK)
* consultation_id (FK)
* report_file_url
* report_summary
* result_status
* generated_at
* verified_by_doctor_id (FK)
* created_at

Result statuses:

* normal
* abnormal
* critical

---

## 12. Payment

Stores payment transactions.

Attributes:

* payment_id (PK)
* patient_id (FK)
* appointment_id (FK)
* consultation_id (FK)
* test_order_id (FK)
* amount
* payment_method
* payment_status
* transaction_reference
* payment_date
* created_at

Payment methods may include:

* cash
* card
* UPI
* insurance

---

# Relationship Summary

Main relationships in the system:

```
PATIENT 1 ──── M APPOINTMENT

DOCTOR 1 ──── M APPOINTMENT

APPOINTMENT 1 ──── 0..1 CONSULTATION

CONSULTATION 1 ──── M TEST_ORDER

TEST_ORDER 1 ──── 0..1 DIAGNOSTIC_REPORT

PATIENT 1 ──── M PAYMENT

DOCTOR M ──── M SPECIALTY
```

---

# Key Design Decisions

### Appointment vs Consultation

An **appointment is scheduling**, while a **consultation is the actual visit**.

This allows the system to track:

* cancellations
* no-shows
* rescheduling

---

### Test Orders Linked to Consultation

Tests are linked to **consultations**, not appointments, because tests are only prescribed **after doctor evaluation**.

---

### Reports Linked to Test Orders

Each report belongs to a **specific test order**, ensuring traceability.

---

### Check-in System

Adding a **check-in entity** supports real-world clinic operations:

* queue management
* waiting room tracking
* staff workflow

---

# Technologies Used

The ER diagram was designed using diagram tools such as:

* Excalidraw
* Draw.io
* FigJam
* Eraser

The database design is compatible with relational databases such as:

* PostgreSQL
* MySQL
* MongoDB (with schema modeling)
* SQLite

---

# Future Improvements

Possible enhancements include:

* Prescription management
* Medicine inventory
* Insurance claim handling
* Online appointment booking
* Telemedicine consultations
* Patient medical history tracking

---

# Conclusion

This ER diagram models a **complete clinic workflow**, including:

* patient management
* appointment scheduling
* doctor consultations
* diagnostic testing
* report generation
* payment handling

The design ensures **clear relationships, normalized data structure, and scalability** while remaining suitable for a **clinic-scale healthcare system**.
