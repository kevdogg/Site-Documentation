```bash
[Unit]
Description=Refresh images and update containers
Requires=docker-compose@%i.service
After=docker-compose@%i.service

[Timer]
OnCalendar=hourly

[Install]
WantedBy=timers.target

```
