See associated [[trigger_update_gcs_sum_value]]

```sql
CREATE OR REPLACE FUNCTION update_gcs_sum_value()
RETURNS TRIGGER AS $$
BEGIN
    -- Calculate and update sum_value whenever a row is inserted or updated
    NEW.value := (SELECT value FROM public.glasgow_coma_scale_base WHERE id = NEW.eye_id)
                      + (SELECT value FROM public.glasgow_coma_scale_base WHERE id = NEW.verbal_id)
                      + (SELECT value FROM public.glasgow_coma_scale_base WHERE id = NEW.motor_id);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```
