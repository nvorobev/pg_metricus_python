# PG Metricus (pg_metricus)

### INFO

Sending metrics in the socket (Brubeck, Graphite, etc.) from pg_notify chanel.

### INSTALLATION

crontab
```
  * * * * *     /cron/pg_metricus.py -H 127.0.0.1 -P 5432 -D base -U user -W pass -A 10.9.5.164 -X 8124 -C metric
```

### FORMAT

For Brubeck:
```plpgsql
select pg_notify('chanel_test', format(E'%s.%s:%s|%s\n', 
    metric_path, 
    metric_name, 
    metric_value, 
    metric_type
));
```

For Graphite:
```plpgsql
select pg_notify('chanel_test', format(E'%s.%s %s %s \n', 
    metric_path, 
    metric_name, 
    metric_value, 
    extract(epoch from now())::integer
));
```

### EXAMPLE

```plpgsql
do language plpgsql $$
declare
	x1 timestamp;
	x2 timestamp;
	v_val_hstore text;
begin

	x1 = clock_timestamp();

	v_val_hstore = get_val_hstore();

	x2 = clock_timestamp();

	perform pg_notify('chanel_test', format(E'%s.%s:%s|%s\n', 
        'db.sql.metric', 
        'get_val_hstore_duration', 
        extract(millisecond from (x2 - x1))::bigint::text, 
        'ms'
    ));

end
$$;
```
