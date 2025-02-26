Some data is persistent and doesn't change over time. For simplicity's sake, we declare it as enums.

### patient_living_condition
```sql
CREATE TYPE patient_living_condition AS ENUM (
  'Vivo',
  'Muerto');
```
#### patient_condition
```sql
CREATE TYPE patient_condition AS ENUM (
  'Crítico',
  'No crítico',
  'Estable',
  'Inestable');
```
### provinces
```sql
CREATE TYPE province AS ENUM (
  'Amazonas',
  'Antioquia',
  'Arauca',
  'Archipiélago de San Andrés, Providencia y Santa Catalina',
  'Atlántico',
  'Bogotá D.C.',
  'Bolívar',
  'Boyacá',
  'Caldas',
  'Caquetá',
  'Casanare',
  'Cauca',
  'Cesar',
  'Chocó',
  'Córdoba',
  'Cundinamarca',
  'Guainía',
  'Guaviare',
  'Huila',
  'La Guajira',
  'Magdalena',
  'Meta',
  'Nariño',
  'Norte de Santander',
  'Putumayo',
  'Quindío',
  'Risaralda',
  'Santander',
  'Sucre',
  'Tolima',
  'Valle del Cauca',
  'Vaupés',
  'Vichada');

```

### zone
```sql
CREATE TYPE zone AS ENUM (
  'Rural',
  'Urbana');
```
#### ips_staff_role
```sql
CREATE TYPE ips_staff_role as ENUM (
  'Médico general',
  'Auxiliar de enfermería',
  'Jefe de enfermería');
```

#### gender
```sql
CREATE TYPE gender as ENUM (
  'Masculino',
  'Femenino',
  'Otro');
```