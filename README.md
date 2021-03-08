# mysql_sys.x-host_summary_by_file_io_type

SELECT 
    IF((`performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST` IS NULL),
        'background',
        `performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST`) AS `host`,
    `performance_schema`.`events_waits_summary_by_host_by_event_name`.`EVENT_NAME` AS `event_name`,
    `performance_schema`.`events_waits_summary_by_host_by_event_name`.`COUNT_STAR` AS `total`,
    `performance_schema`.`events_waits_summary_by_host_by_event_name`.`SUM_TIMER_WAIT` AS `total_latency`,
    `performance_schema`.`events_waits_summary_by_host_by_event_name`.`MAX_TIMER_WAIT` AS `max_latency`
FROM
    `performance_schema`.`events_waits_summary_by_host_by_event_name`
WHERE
    ((`performance_schema`.`events_waits_summary_by_host_by_event_name`.`EVENT_NAME` LIKE 'wait/io/file%')
        AND (`performance_schema`.`events_waits_summary_by_host_by_event_name`.`COUNT_STAR` > 0))
ORDER BY IF((`performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST` IS NULL),
    'background',
    `performance_schema`.`events_waits_summary_by_host_by_event_name`.`HOST`) , `performance_schema`.`events_waits_summary_by_host_by_event_name`.`SUM_TIMER_WAIT` DESC
