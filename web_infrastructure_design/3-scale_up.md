# Scale Up Infrastructure

```mermaid
graph TD
    User((User)) -->|HTTPS| DNS[DNS: foobar.com]
    DNS -->|www record| CLB[Load Balancer Cluster: HAproxy]

    CLB -->|Round Robin| WS1[Web Server 1: Nginx]
    CLB -->|Round Robin| WS2[Web Server 2: Nginx]

    WS1 & WS2 -->|Requests| AS1[Application Server 1]
    WS1 & WS2 -->|Requests| AS2[Application Server 2]

    AS1 & AS2 -->|Queries| DB[(Database Server: MySQL)]

    %% Explanations
    classDef server fill:#f9f,stroke:#333,stroke-width:2px
    classDef lb fill:#f96,stroke:#333,stroke-width:2px
    class WS1,WS2,AS1,AS2,DB server
    class CLB lb
```

## Description

### Component Split
- **Web Servers**: Dedicated Nginx servers for static content
- **Application Servers**: Separate servers for business logic
- **Database Server**: Standalone server for data management
- **Load Balancer Cluster**: HAproxy in high availability

### Benefits
- **Specialized Optimization**: Each component can be tuned for its specific role
- **Independent Scaling**: Components can scale based on their specific needs
- **Resource Isolation**: No competition between different services
- **Maintenance Flexibility**: Components can be maintained independently
- **Better Monitoring**: Clearer metrics per component

### Additional Elements
- **Load Balancer Cluster**: Eliminates load balancer as single point of failure
- **Separated Components**: Each major function has dedicated resources
