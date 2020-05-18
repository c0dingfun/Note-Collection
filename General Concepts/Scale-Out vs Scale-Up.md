Scale-Out vs Scale-Up
===

SCALE-OUT
---

- A type of capacity expansion concentrating on the addition of new hardware resources instead of increasing the capacity of already available hardware resources such as storage or processing silos. 

- This is often used in the context of storage because ideally, it is not just the storage capacity that needs to increase in such a system, but the controller and load balancing as well. 

- In large cloud storage systems where multitenancy and scalability are required, increasing the capacity alone in a SCALE-UP manner would not be sufficient to handle the increasing data traffic.

- Example: SQL Server SSIS's Scale-Out Master and Scale-Out Worker in SQL Server 2017

- [Ref](https://www.slideshare.net/SandyWinarko/running-ssis-2017-at-scale-everywhere)

SCALE-UP 
---

- An approach was an older method for growth since hardware resources were expensive, so it made sense to make the most out of existing hardware and just increase capacity. 

- But the decreasing hardware costs has made it easier to SCALE OUT, increasing all capacities in the process.