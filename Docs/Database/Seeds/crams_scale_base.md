```sql
-- Seed data for the three possible values in the CRAMS scale
INSERT INTO public.crams_scale_base (factor, value) VALUES
('Normal', 2),
('Ligeramente afectada', 1),
('Severamente afectada', 0),
ON CONFLICT (value) DO NOTHING;
```