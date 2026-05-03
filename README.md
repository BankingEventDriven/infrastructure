# infrastructure

Local services exposed by Docker Compose:
- Apache Pulsar broker: `pulsar://localhost:6650`
- Apache Pulsar admin API: `http://localhost:8085`
- Apache Pulsar Manager UI: `http://localhost:9527`
- Apache Pulsar Manager backend API: `http://localhost:7750`

Pulsar Manager login:
- Username: `admin`
- Password: `apachepulsar`

The Compose stack auto-initializes this default Pulsar Manager user on startup.
