
```sql
CREATE TRIGGER trigger_update_gcs_sum_value
BEFORE INSERT OR UPDATE ON public.glasgow_coma_scale
FOR EACH ROW
EXECUTE FUNCTION update_gcs_sum_value();
```
