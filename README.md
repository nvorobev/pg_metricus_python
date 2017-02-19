# PG Metricus (pg_metricus)

### INFO

Sending metrics in the socket (Brubeck, Graphite, etc.) from pg_notify chanel.

### EXAMPLE

crontab
```
  * * * * *     /cron/pg_metricus.py -H 127.0.0.1 -P 5432 -D base -U user -W pass -A 10.9.5.164 -X 8124 -C metric
```

Format for Brubeck:
```plpgsql
select pg_notify(format('%s.%s:%s|%s', 
    metric_path, 
    metric_name, 
    metric_value, 
    metric_type
));
```

Format for Graphite:
```plpgsql
select pg_notify(format(E'%s.%s %s %s \n', 
    metric_path, 
    metric_name, 
    metric_value, 
    extract(epoch from now())::integer
));
```
