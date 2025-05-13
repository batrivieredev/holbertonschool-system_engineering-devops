# Secured and Monitored Web Infrastructure

```mermaid
graph TD
    User((User)) -->|HTTPS| DNS[DNS: foobar.com]
    DNS -->|www record| LB[Load Balancer: HAproxy + SSL Termination]

    subgraph Security
        FW_LB[Firewall] --> LB
        LB --> FW1[Firewall 1]
        LB --> FW2[Firewall 2]
    end

    FW1 --> Server1[Server 1]
    FW2 --> Server2[Server 2]

    subgraph Server1
        M1[Monitoring Client] --> Nginx1[Web Server: Nginx]
        Nginx1 --> App1[Application Server]
        App1 --> Code1[Application Files]
        App1 --> DB1[(Database: MySQL Primary)]
    end

    subgraph Server2
        M2[Monitoring Client] --> Nginx2[Web Server: Nginx]
        Nginx2 --> App2[Application Server]
        App2 --> Code2[Application Files]
        App2 --> DB2[(Database: MySQL Replica)]
    end

    DB2 -.->|Replication| DB1

    Monitor[Monitoring Service: Sumologic] --> M1 & M2

    %% Explanations
    classDef server fill:#f9f,stroke:#333,stroke-width:2px
    classDef security fill:#f96,stroke:#333,stroke-width:2px
    class Server1,Server2 server
    class FW_LB,FW1,FW2 security
```

## Description

### Added Security Components
- **3 Firewalls**: Protection layers for load balancer and servers
- **SSL Certificate**: HTTPS encryption for www.foobar.com
- **3 Monitoring Clients**: Data collectors for infrastructure monitoring

### Security Measures
- **Firewalls**: Filter and control traffic
- **HTTPS**: Encrypted data transmission
- **SSL Termination**: At load balancer level

### Monitoring Setup
- **Service**: Sumologic (or similar)
- **Data Collection**:
  - System metrics (CPU, memory, disk)
  - Application logs
  - Network traffic
  - Security events
- **QPS Monitoring**: Track queries per second via web server logs

### Infrastructure Issues

#### SSL Termination at Load Balancer
- Decrypted traffic between load balancer and servers
- Internal network vulnerability

#### Single Write Database
- No write redundancy
- Potential data loss on primary failure

#### Uniformity Risk
- All servers have identical components
- No component-specific optimization
- Resource competition between services
