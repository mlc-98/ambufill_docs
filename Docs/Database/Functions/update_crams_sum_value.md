See associated [[trigger_update_crams_sum_value]]

```sql
CREATE OR REPLACE FUNCTION update_crams_sum_value()
RETURNS TRIGGER AS $$
BEGIN
    -- Calculate and update sum_value whenever a row is inserted or updated
    NEW.value := (SELECT value FROM public.crams_scale_base WHERE id = NEW.circulation_id)
                      + (SELECT value FROM public.crams_scale_base WHERE id = NEW.respiration_id)
                      + (SELECT value FROM public.crams_scale_base WHERE id = NEW.airway_id)
                      + (SELECT value FROM public.crams_scale_base WHERE id = NEW.motor_id)
                      + (SELECT value FROM public.crams_scale_base WHERE id = NEW.speech_id);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
