
```sql
CREATE TRIGGER trigger_update_crams_sum_value
BEFORE INSERT OR UPDATE ON public.crams_scale
FOR EACH ROW
EXECUTE FUNCTION update_crams_sum_value();
```
