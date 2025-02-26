```sql
-- Seed data for common Colombian identity document types
INSERT INTO public.document_types (name, abbreviation) VALUES
('Cédula de Ciudadanía', 'CC'),
('Tarjeta de Identidad', 'TI'),
('Cédula de Extranjería', 'CE'),
('Pasaporte', 'PA'),
('Registro Civil', 'RC'),
('Documento Nacional de Identidad', 'DNI')
ON CONFLICT (abbreviation) DO NOTHING;
```