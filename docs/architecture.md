## Product Choice

Product name: Yandex Go
Link to the product's website: <https://go.yandex/ru_ru/>
Short description of the product: It is a service combining several useful services in itself, like taxi, food delivery etc.

## Main components

![Yandex Go Component Diagram](diagrams/out/yandex-go/architecture-component/Component%20Diagram.svg)

I think that user service interacts with users having problems
I think that pricing service corrects the pricing formula whenever it is needed
I think that payment services handle all payment related interactions
I think that notification service handles notifying users about deals
I think that Maps & Routing service handles the route finding and showing the product of interest on the map (taxi, delivery)

## Data flow

![Yandex Go Sequence Diagram](diagrams/out/yandex-go/architecture-sequence/Sequence%20Diagram.svg)

In the Price estimation block (2) first user enters the destination. Then the mobile app estimates the ride cost, which happens by calculating options with Maps & Pricing, then fetching route and traffic from external maps API, it returns the route data to the Maps & Pricing service. It then fetches tariff rules from the operational DB and checks demand surge in state cache. After that it returns options to the API gateway, so the mobile app can show the route and price.

## Deployment

![Yandex Go Deployment Diagram](diagrams/out/yandex-go/architecture-deployment/Deployment%20Diagram.svg)

The mobile app and web browser app are deployed on the user's device. The API gateway is deployed on a Load Balancer. Every service is deployed in a Kubernetes cluster (Pod). Redis cache has its own separate Redis cluster. Kafka is deployed on a message broker cluster. The analytics and operational database are deployed on a data storage cluster, and finally Yandex pay API and Yandex maps API are on an external Kubernetes cluster

## Assumptions

I assume that the pricing service pulls the info about tariffs from the operational DB
I assume that the API gateway is deployed on a Load Balancer to redistribute large internet traffic to several servers

## Open questions

What does the load balancer actually do and how it decided which server to put the user on
What are the components of pricing while calculating the price
