# Distributed Web Infrastructure

```mermaid
graph TD
    User((User)) -->|HTTP/TCP| DNS[DNS: foobar.com]
    DNS -->|www record| LB[Load Balancer: HAproxy]

    LB -->|Round Robin| Server1[Server 1]
    LB -->|Round Robin| Server2[Server 2]

    subgraph Server1
        Nginx1[Web Server: Nginx] --> App1[Application Server]
        App1 --> Code1[Application Files]
        App1 --> DB1[(Database: MySQL Primary)]
    end

    subgraph Server2
        Nginx2[Web Server: Nginx] --> App2[Application Server]
        App2 --> Code2[Application Files]
        App2 --> DB2[(Database: MySQL Replica)]
    end

    DB2 -.->|Replication| DB1

    %% Explanations
    classDef server fill:#f9f,stroke:#333,stroke-width:2px
    class Server1,Server2 server
```

## Description

### Added Components
- **Load Balancer (HAproxy)**: Distributes incoming traffic
- **Server 2**: Additional server for redundancy
- **Database Primary-Replica**: MySQL replication setup

### Load Balancer Configuration
- **Algorithm**: Round Robin
- **Setup**: Active-Active
  - Both servers actively handle requests
  - Traffic distributed evenly
  - Better resource utilization than Active-Passive

### Database Primary-Replica
- **Primary Node**: Handles writes
- **Replica Node**: Handles reads
- **Replication**: Async from Primary to Replica
- **Application Impact**: Read queries can be distributed

### Infrastructure Issues

#### SPOF
- Load balancer is single point of failure
- Primary MySQL database

#### Security Issues
- No firewall protection
- No HTTPS encryption
- Unsecured traffic

#### Monitoring
- No monitoring system
- Unable to track:
  - Server health
  - Application performance
  - Database status
  - Traffic patterns
