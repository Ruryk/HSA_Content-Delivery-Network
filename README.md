# HSA12  14. Content Delivery Network

## Handmade CDN

Your goal is to create your own cdn for delivering millions of images across the globe.

Set up 7 containers - bind server, load balancer 1, load balancer 2, node1, node2, node3, node 4. Try to implement
different balancing approaches.

Implement efficient caching. Write down pros and cons of each approach.

## Load Balancing Approaches

### Round Robin (Load Balancer 1):

Requests are distributed evenly across Node1 and Node2.
Each request is sent to the next available node in a cyclical manner.

### Weighted Round Robin (Load Balancer 2):

Node3 has a higher weight (3), meaning it receives more requests than Node4 (weight 1).

## Caching Mechanism

Nginx is configured to cache images for 1 hour, reducing the load on the backend nodes.
Cache status headers (X-Cache-Status) are included to track cache hits and misses.
Cache is bypassed for certain requests based on URI rules.

## Bind DNS Configuration

The Bind server resolves the following domains:

````bash
load-balancer-1.cdn.test
load-balancer-2.cdn.test
node1.cdn.test
node2.cdn.test
node3.cdn.test
node4.cdn.test
````

Ensure that your hosts file includes the appropriate DNS mappings:

````bash
127.0.0.1 load-balancer-1.cdn.test
127.0.0.1 load-balancer-2.cdn.test
127.0.0.1 node1.cdn.test
127.0.0.1 node2.cdn.test
127.0.0.1 node3.cdn.test
127.0.0.1 node4.cdn.test
````

## Run the Project

````bash
docker-compose up --build
````

## Test DNS resolution with nslookup or dig:

````bash
nslookup load-balancer-1.cdn.test
````

## Test access to load balancers and nodes:

````bash
curl http://load-balancer-1.cdn.test/images/1.jpg
curl http://load-balancer-2.cdn.test/images/5.jpg
````

## Results and Analysis
### Load Balancing Approaches:
#### Round Robin (Load Balancer 1)
Pros: Simple and effective distribution of traffic, ensuring equal load on nodes.

Cons: Does not account for node capacity or performance, which may lead to uneven performance if nodes have different
capacities.

#### Weighted Round Robin (Load Balancer 2)
Pros: Allows more powerful nodes to handle more requests, improving overall performance and efficiency.

Cons: Requires manual adjustment of weights based on node capacity, and incorrect weights may cause imbalance.

#### Caching:
Pros:
Reduces load on backend nodes by serving cached images.
Improves response times for frequently accessed images.

Cons:
Cache configuration may need fine-tuning to avoid caching stale content.
Cache expiration management can be complex.


## Performance Metrics
### Round Robin
Equal distribution across Node1 and Node2.

### Weighted Round Robin
Node3 handle approximately 3x more requests than Node4.
