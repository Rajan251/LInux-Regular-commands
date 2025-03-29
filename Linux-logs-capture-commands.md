# System Logs and Monitoring Commands

## MongoDB Slow Query Logs
```sh
sudo grep -i "slow query" /var/log/mongodb/mongod.log | tail -n 50
```

## System Error Logs
```sh
sudo grep -i "error" /var/log/syslog | tail -n 500
```

## View Nginx Logs
```sh
journalctl -u nginx --no-pager -n 100
```
- `-u nginx` → Filters logs for the Nginx service.
- `--no-pager` → Prints all logs without pagination.
- `-n 100` → Shows the last 100 lines (adjust as needed).

## View MongoDB Logs
```sh
journalctl -u mongod --no-pager -n 100
```

## View Docker Logs
```sh
journalctl -u docker --no-pager -n 100
```

## View Kernel/System Logs
```sh
journalctl -k --no-pager -n 100
```
- `-k` → Filters only kernel logs.

## View Errors and Warnings Only
```sh
journalctl -p 3 --no-pager  # Only critical errors
journalctl -p 4 --no-pager  # Warnings and errors
journalctl -p 0..3 --no-pager  # Emergency to Error level
```
- `-p 0..3` → Filters logs with priority from Emergency (0) to Error (3).

## Monitor Logs in Real-time
```sh
journalctl -f -u nginx -u mongod -u docker
