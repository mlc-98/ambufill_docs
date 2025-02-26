```sql
-- Seed data for the three possible values in the Glasgow scale
INSERT INTO public.crams_scale_base (factor, category, value) VALUES
('Espontánea', 'ocular', 4),
('Orden verbal', 'ocular', 3),
('Dolor', 'ocular', 2),
('No responden', 'ocular', 1),
('Orientado y conversando', 'verbal', 5),
('Desorientado y hablando', 'verbal', 4),
('Palabras inapropiadas', 'verbal', 3),
('Sonidos incomprensibles', 'verbal', 2),
('Ninguna respuesta', 'verbal', 1),
('Orden verbal obedece', 'motor', 6),
('Localiza el dolor', 'motor', 5),
('Retirada y flexión', 'motor', 4),
('Flexión anormal', 'motor', 3),
('Extensión', 'motor', 2),
('Ninguna respuesta', 'motor', 1),
ON CONFLICT (value) DO NOTHING;
```