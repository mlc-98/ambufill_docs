## Tables
### auth
The `auth.users` table can hold metadata about the user in the column `app_metadata`. Since our implementation involves that table (primarily for authentication) and a second table `public.profiles`, the aforementioned column is a good place to put information that we want to share between the two tables, without modifying the predefined columns in `auth.users`.
### public
##### companies
Holds data for the companies that use the application

| column     | type      | default             | constraints         |
| ---------- | --------- | ------------------- | ------------------- |
| id         | uuid      | `gen_random_uuid()` |                     |
| name       | text      |                     | `not null` `unique` |
| nit        | number    |                     | `not null` `unique` |
| is_active  | bool      | `true`              | `not null`          |
| created_at | timestamp | `now()`             | `not null`          |
#### document_types
Represents the different types of identity documents. This table **shouldn't** change after initialization with a [[document_types|seed]] file.

| column       | type | default             | constraints         |
| ------------ | ---- | ------------------- | ------------------- |
| id           | uuid | `gen_random_uuid()` |                     |
| name         | text |                     | `not null` `unique` |
| abbreviation | text |                     | `not null` `unique` |
#### profiles
This table represents all the users in the app. Whenever a new user is created to `auth.users`, a function (TODO: create doc) is triggered, creating a new record in `public.profiles` with the same `id`. Information extracted from the `app_metadata` column is used to populate the appropriate fields.

| column           | type      | default                       | constraints         |
| ---------------- | --------- | ----------------------------- | ------------------- |
| id               | uuid      | from `auth.users`             |                     |
| fist_name        | text      | from`auth.users.app_metadata` | `not null`          |
| second_name      | text      | from`auth.users.app_metadata` |                     |
| first_surname    | text      | from`auth.users.app_metadata` | `not null`          |
| second_surname   | text      | from`auth.users.app_metadata` |                     |
| document_type_id | uuid      | from`auth.users.app_metadata` | `not null`          |
| document_number  | number    | from`auth.users.app_metadata` | `not null` `unique` |
| is_active        | bool      | `true`                        | `not null`          |
| created_at       | timestamp | `now()`                       | `not null`          |
#### cities
Holds the cities of the country. Since the app is designed to operate in one country only, provinces are not represented with a traditional table, but with an `enum` [[Enums#provinces|seed]] instead. [[cities|Dataset]]

| column   | type | default             | constraints         |
| -------- | ---- | ------------------- | ------------------- |
| id       | uuid | `gen_random_uuid()` |                     |
| name     | text |                     | `not null` `unique` |
| province | enum |                     | `not null`          |
#### eps
Holds all the EPS companies. [[eps|seed]]

| column | type | default             | constraints         |
| ------ | ---- | ------------------- | ------------------- |
| id     | uuid | `gen_random_uuid()` |                     |
| name   | text |                     | `not null` `unique` |

#### glasgow_coma_scale_base
Models the base layer of the Glasgow Coma Scale ([Glasgow](https://www.elsevier.com/es-es/connect/escala-de-coma-de-glasgow-tipos-de-respuesta-motora-y-su-puntuacion)), namely, it holds the ratings and their corresponding factor. [[glasgow_coma_scale|seed]]

| column   | type   | default             | constraints |
| -------- | ------ | ------------------- | ----------- |
| id       | uuid   | `gen_random_uuid()` |             |
| factor   | text   |                     | `not null`  |
| category | text   |                     | `not null`  |
| value    | number |                     | `not null`  |
#### glasgow_coma_scale
Represents the actual values of a GCS.

| column    | type   | default                  | constraints                          |
| --------- | ------ | ------------------------ | ------------------------------------ |
| id        | uuid   | `gen_random_uuid()`      |                                      |
| eye_id    | uuid   |                          | references `glasgow_coma_scale_base` |
| verbal_id | uuid   |                          | references `glasgow_coma_scale_base` |
| motor_id  | uuid   |                          | references `glasgow_coma_scale_base` |
| total     | number | [[update_gcs_sum_value]] |                                      |
#### crams_scale_base
Models the base layer of the [CRAMS](https://cetph.wordpress.com/2012/01/12/crams/) scale,  namely, it holds the ratings and their corresponding factor. [[crams_scale_base|seed]]

| column | type   | default             | constraints         |
| ------ | ------ | ------------------- | ------------------- |
| id     | uuid   | `gen_random_uuid()` |                     |
| factor | text   |                     | `not null` `unique` |
| value  | number |                     | `not null`          |
#### crams_scale
Represents the actual values of a GCS.

| column         | type   | default                    | constraints                   |
| -------------- | ------ | -------------------------- | ----------------------------- |
| id             | uuid   | `gen_random_uuid()`        |                               |
| circulation_id | uuid   |                            | references `crams_scale_base` |
| respiration_id | uuid   |                            | references `crams_scale_base` |
| airway_id      | uuid   |                            | references `crams_scale_base` |
| motor_id       | uuid   |                            | references `crams_scale_base` |
| speech_id      | uuid   |                            | references `crams_scale_base` |
| total          | number | [[update_crams_sum_value]] |                               |
#### pupil_dilation
Holds the possible values for the pupil dilation of a patient. [[pupil_dilation|seed]]

| column     | type | default             | constraints         |
| ---------- | ---- | ------------------- | ------------------- |
| id         | uuid | `gen_random_uuid()` |                     |
| conditiion | text |                     | `not null` `unique` |
#### triage
Represents the values of the triage scale. [[triage|seed]]

| column        | type   | default             | constraints         |
| ------------- | ------ | ------------------- | ------------------- |
| id            | uuid   | `gen_random_uuid()` |                     |
| urgency_level | number |                     | `not null` `unique` |
| color         | text   |                     | `not null` `unique` |
#### procedures
Holds the possible procedures performed while attending an emergency. [[procedures|seed]]

| column | type | default             | constraints         |
| ------ | ---- | ------------------- | ------------------- |
| id     | uuid | `gen_random_uuid()` |                     |
| name   | text |                     | `not null` `unique` |
#### health_status
Holds all the different factors about a patient's health status. For p

| column                    | type      | default             | constraints                           |
| ------------------------- | --------- | ------------------- | ------------------------------------- |
| id                        | uuid      | `gen_random_uuid()` |                                       |
| glasgow_scale_id          | uuid      |                     | fk -> `glasgow_coma_scale` `not null` |
| crams_scale_id            | uuid      |                     | fk -> `crams_scale` `not null`        |
| pupil_dilation_id         | uuid      |                     | fk -> `pupil_dilation` `not null`     |
| physical_exam             | text      |                     | `not null`                            |
| heart_rate                | number    |                     | `not null`                            |
| breathing_frequency       | number    |                     | `not null`                            |
| blood_pressure            | text      |                     | `not null`                            |
| patient_living_condidtion | enum      |                     | `not null`                            |
| triage_id                 | uuid      |                     | `not null`                            |
| created_at                | timestamp | `now()`             |                                       |
#### ips
Holds all IPS companies. To avoid duplicates, but leaving room for IPS with the same name in different cities, the unique index is composed of name + city_id

| column  | type | default             | constraints             |
| ------- | ---- | ------------------- | ----------------------- |
| id      | uuid | `gen_random_uuid()` |                         |
| name    | text |                     | `not null`              |
| city_id | uuid |                     | fk -> `cities` `not nul |
#### ips_staff
Employees at IPS. Their role is defined by this enum [[Enums#ips_staff_role]]. Their association with an IPS is defined by a junction table ([[#ips_staff_to_ips]])

| column           | type   | default             | constraints         |
| ---------------- | ------ | ------------------- | ------------------- |
| id               | uuid   | `gen_random_uuid()` |                     |
| first_name       | text   |                     | `not null`          |
| second_name      | text   |                     |                     |
| first_surname    | text   |                     | `not null`          |
| second_surname   | text   |                     |                     |
| document_type_id | uuid   |                     | `not null`          |
| document_number  | number |                     | `not null`          |
| role             | enum   |                     | `not null`          |
| signature_url    | text   |                     | `not null` `unique` |
#### soat_companies
Holds all the companies that issue SOAT insurances. [[soat_companies|seed]]

| column | type | default             | constraints         |
| ------ | ---- | ------------------- | ------------------- |
| id     | uuid | `gen_random_uuid()` |                     |
| name   | text |                     | `not null` `unique` |
#### vehicle_types
Holds the different types of vehicles.

| column   | type | default             | constraints         |
| -------- | ---- | ------------------- | ------------------- |
| id       | uuid | `gen_random_uuid()` |                     |
| category | text |                     | `not null` `unique` |
#### vehicle_brands
Holds vehicle brands.

| column | type | default             | constraints         |
| ------ | ---- | ------------------- | ------------------- |
| id     | uuid | `gen_random_uuid()` |                     |
| name   | text |                     | `not null` `unique` |
#### involved_vehicle
Vehicles involved in an incident.

| column                | type | default             | constraints                       |
| --------------------- | ---- | ------------------- | --------------------------------- |
| id                    | uuid | `gen_random_uuid()` |                                   |
| plate                 | text |                     | `not null`                        |
| brand_id              | uuid |                     | fk -> `vehicle_brands` `not null` |
| type_id               | uuid |                     | fk -> `vehicle_types` `not null`  |
| ownership_picture_url | text |                     | `not null` `unique`               |
#### ambulances
| column | type | default             | constraints |
| ------ | ---- | ------------------- | ----------- |
| id     | uuid | `gen_random_uuid()` |             |
| plate  | text |                     |             |
#### incidents
Models incidents attended by emergency crews. Since multiple vehicles can be involved in the same incident, that relationship is represented by a junction table.

| column           | type | default             | constraints                  |
| ---------------- | ---- | ------------------- | ---------------------------- |
| id               | uuid | `gen_random_uuid()` |                              |
| date             | date |                     | `not null`                   |
| incident_time    | time |                     | `not null`                   |
| contact_time     | time |                     | `not null`                   |
| departure_time   | time |                     | `not null`                   |
| ips_arrival_time | time |                     | `not null`                   |
| city_id          | uuid |                     | fk -> `cities` `not null`    |
| zone             | enum |                     | `not null`                   |
| description      | text |                     | `not null`                   |
| ips_id           | uuid |                     | fk -> `ips` `not null`       |
| ips_staff_id     | uuid |                     | fk -> `ips_staff` `not null` |
## Junction tables
#### procedures_to_health_status
This is structured as a junction table, since the same `health_status` record can have multiple procedures associated with it.

| column           | type | default             | constraints                          |
| ---------------- | ---- | ------------------- | ------------------------------------ |
| id               | uuid | `gen_random_uuid()` |                                      |
| procedure_id     | uuid |                     | fk -> `procedures` <br>`not null`    |
| health_status_id | uuid |                     | fk -> `health_status` <br>`not null` |
#### health_status_to_patient
The same patient could have multiple emergencies / services, so instead of creating a new patient every time, we use this pivot table.

| column           | type      | default             | constraints                          |
| ---------------- | --------- | ------------------- | ------------------------------------ |
| id               | uuid      | `gen_random_uuid()` |                                      |
| patient_id       | uuid      |                     | fk -> `patients` <br>`not null`      |
| health_status_id | uuid      |                     | fk -> `health_status` <br>`not null` |
| created_at       | timestamp | `now()`             |                                      |
#### ips_staff_to_ips
The same IPS worker can change their workplace over time. Thus, a junction table is more flexible.

| column       | type      | default             | constraints                      |
| ------------ | --------- | ------------------- | -------------------------------- |
| id           | uuid      | `gen_random_uuid()` |                                  |
| ips_staff_id | uuid      |                     | fk -> `ips_staff` <br>`not null` |
| ips_id       | uuid      |                     | fk -> `ips` <br>`not null`       |
| created_at   | timestamp | `now()`             |                                  |
#### soat_to_involved_vehicle
| column               | type      | default             | constraints                       |
| -------------------- | --------- | ------------------- | --------------------------------- |
| id                   | uuid      | `gen_random_uuid()` |                                   |
| soat_company_id      | uuid      |                     | fk -> `soat_companies` `not null` |
| soat_number          | text      |                     | `not null`                        |
| soat_expiration_date | date      |                     | `not null`                        |
| soat_picture_url     | text      |                     | `not null` `unique`               |
| created_at           | timestamp | `now()`             |                                   |
#### involved_vehicle_to_incident
| column              | type      | default             | constraints                              |
| ------------------- | --------- | ------------------- | ---------------------------------------- |
| id                  | uuid      | `gen_random_uuid()` |                                          |
| involved_vehicle_id | uuid      |                     | fk -> `involved_vehicles` <br>`not null` |
| incident_id         | uuid      |                     | fk -> `incidents` <br>`not null`         |
| created_at          | timestamp | `now()`             |                                          |
#### ambulance_to_company
| column       | type      | default             | constraints                       |
| ------------ | --------- | ------------------- | --------------------------------- |
| id           | uuid      | `gen_random_uuid()` |                                   |
| ambulance_id | uuid      |                     | fk -> `ambulances` <br>`not null` |
| company_id   | uuid      |                     | fk -> `companies` <br>`not null`  |
| created_at   | timestamp | `now()`             |                                   |
