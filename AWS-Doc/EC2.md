# [EC2](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html)

## [스팟 인스턴스 요청](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/spot-requests.html)

Request type: one-time | persistent

요청이 영구적인 경우 스팟 인스턴스가 중단된 후 요청이 다시 열림, 요청이 영구적이고, 사용자가 인스턴스를 중단하는 경우 스팟 인스턴스를 시작한 후에만 요청이 열림

- open – 요청이 이행될 때까지 대기 중입니다.
- active – 요청이 이행되며 연결된 스팟 인스턴스가 있습니다.
- failed – 요청에 하나 이상의 잘못된 파라미터가 있습니다.
- closed – 스팟 인스턴스가 중단되거나 종료되었습니다.
- disabled – 사용자가 스팟 인스턴스를 중지했습니다.
- cancelled – 사용자가 요청을 취소했거나 요청이 만료되었습니다.

## [스팟 집합](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/spot-fleet.html)

### Configuring Spot Fleet for cost optimization and diversification
To create a fleet of Spot Instances that is both cheap and diversified, use the lowestPrice allocation strategy in combination with InstancePoolsToUseCount. Spot Fleet automatically deploys the cheapest combination of instance types and Availability Zones based on the current Spot price across the number of Spot pools that you specify. This combination can be used to avoid the most expensive Spot Instances.

### Configuring Spot Fleet for capacity optimization
- Pricing changes slowly, but capacity fluctuates in real time
- The capacityOptimized strategy automatically launches Spot Instances into the most available pools by looking at real-time capacity data and predicting which are the most available.
- This works well for high performance computing that may have a higher cost of interruption associated with restarting work and checkpointing.

### Choosing an appropriate allocation strategy
- small or rus for a short time - lowestPrice
- To create a cheap and diversified fleet, use the lowestPrice strategy in combination with InstancePoolsToUseCount. 
- If your fleet runs workloads that may have a higher cost of interruption associated with restarting work and checkpointing, then use the capacityOptimized strategy. This strategy offers the possibility of fewer interruptions, which can lower the overall cost of your workload.

### Spot price overrides
Global maximum price vs specific price

## Control spending
- Spot Fleet stops launching instances when it has either reached the target capacity or the maximum amount you’re willing to pay. 

## Spot Fleet instance weighting
- Spot Instances to launch by dividing the target capacity by the instance weight.

## Fulfillment
diversified - calculate per unit

## Burstable performance instances
- T2, T3, T3a
- credits for CPU usage
- Ensure the minimum memory requirements

# [AMI](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/AMIs.html)
Component
- One or more EBS snapshots, or 

# [Placement groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/placement-groups.html)
When you launch a new EC2 instance, the EC2 service attempts to place the instance in such a way that all of your instances are spread out across underlying hardware to minimize correlated failures. You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. Depending on the type of workload, you can create a placement group using one of the following placement strategies:
- Cluster - 인스턴스를 AZ 안에 서로 근접하게 패킹, 낮은 지연 시간의 네트워크 성능
- Partition - 인스턴스를 논리적 파티션에 분산, 서로 다른 파티션의 인스턴스 그룹이 기본 하드웨어를 공유하지 않게
- Spread - Reduce correlated failures

