Provide your solution here:
1. System Architecture Overview
Below is the high-level system architecture diagram for the trading platform:

Core Components
Load Balancer (AWS ALB) – Distributes traffic to API servers based on demand.
API Gateway + Lambda – Handles authentication, rate-limiting, and routing to microservices.
Compute Layer (EKS + EC2 + Fargate) – Runs trading engines, order book, and core microservices.
Database Layer (Amazon RDS Aurora + ElastiCache) – Ensures low-latency data retrieval.
Streaming (Kafka on MSK + S3 for Logs) – Handles real-time order matching.
Monitoring & Security (CloudWatch, GuardDuty, WAF) – Provides visibility and security.
2. Explanation of AWS Services and Alternatives Considered
Component	AWS Service Used	                Why Chosen?	                        	Alternative Considered
Load Balancing	AWS ALB         	                Layer 7 routing, supports WebSockets    	Nginx on EC2, CloudFront
API Gateway	AWS API Gateway + Lambda                Handles authentication, rate-limiting		Nginx reverse proxy
Compute	        AWS EKS (Kubernetes)	                Scalable microservices architecture		EC2 ASG, AWS Lambda
Database	Amazon Aurora (PostgreSQL)              HA, auto-scaling read replicas	                DynamoDB, RDS MySQL
Cache	        Amazon ElastiCache (Redis)              Low-latency order book storage	                Memcached, Local disk
Streaming	Amazon MSK (Kafka)	                Ensures real-time event-driven processing	Kinesis, RabbitMQ
Storage	        Amazon S3	                        Stores logs, order history	                EFS, FSx
Security	AWS WAF + GuardDuty	                Blocks malicious attacks, DDoS protection	Cloudflare, Nginx ModSecurity
Monitoring	AWS CloudWatch + Prometheus/Grafana	Observability	                                ELK stack
3. Scaling Strategy
Load Balancer Scaling:

AWS ALB auto-scales with incoming requests, redirecting to available API nodes.
API Gateway can throttle traffic to prevent overloading backend services.
Compute Layer Scaling:

Kubernetes Horizontal Pod Autoscaler (HPA) auto-scales trading services based on CPU/memory load.
EC2 Auto Scaling Group (ASG) launches new nodes when demand spikes.
Database Scaling:

Aurora Auto-Scaling Read Replicas handle increased read traffic.
Sharding strategy with Aurora Partitioning for high transaction throughput.
Order Matching Engine Optimization:

Redis (ElastiCache) caches active orders for fast retrieval.
Kafka distributes trades asynchronously to avoid blocking.
Disaster Recovery & Resilience:

Multi-AZ Aurora ensures data redundancy.
AWS Global Accelerator minimizes latency by routing users to the closest region.
Active-Active deployment with AWS Route 53 DNS Failover.
4. Performance Considerations
Throughput: API Gateway + Lambda scales automatically to handle 500 RPS.
Latency: Redis caching + Aurora reduces DB query time, ensuring p99 <100ms.
Fault Tolerance: Multi-AZ RDS, EKS self-healing, and Kafka message replay prevent data loss.
