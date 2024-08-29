## High Availability

**What is it?** *AKA **HA***. The ability of the system to stay online despite having failures at the infrastructure level. It is often expressed as a ***percentage***. You might often see this in *Service Level Agreements* (SLA) of cloud platforms.
<br>**Example:** A real world example is having back-up generators to ensure power in case of any power outages in hospital, air traffic control towers, etc.

**Why is it important?:**

- There are mission critical systems like aircrafts, spacecrafts, mining machines, hospital servers and stock market systems that cannot afford to go down. Lives pretty much depend on it.
- To meet high availability requirements, systems are designed to be ***fault tolerant*** and **redundant**.

### Reasons for System Failures

- Software crashes
    - Dealing with corrupt software files.
    - BSOD, OS crashing, memory hogging unresponsive processes.
    - Software running on cloud nodes crashes unpredictably and takes down the entire node.
- Hardware failures
    - Overloaded CPU and RAM, hard disk failures, nodes going down, and network outages.
- Human errors
    - Flawed configurations, Ex.Google made a tiny network configuration error and it took down almost half of the internet in Japan.
- Planned downtime
    - Routine maintenance operations, patching software, hardware upgrades.