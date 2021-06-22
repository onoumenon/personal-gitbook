# Tech Fundamentals

### YAML: human readable data serialisation language

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.38.38-pm.png)

### JSON: javascript object notation

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.43.47-pm.png)

JSON can be used for both Cloudformation and identity documents. YAML is used for Cloudformation.

### OSI model

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.47.40-pm.png)

Media layers: Layer 1 to 3

Host layers: 4 to 7

### Layer 1 - Physical

Physical connection \(ie: network cable\)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.51.55-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.49.49-pm.png)

#### Attributes:

* No device addressing, all data is processed by all devices \(it's like shouting to a room of strangers\)
* If multiple devices transmit at once, a collision occurs
* No media access control or collision detection

### Layer 2 - Data Link

Builds upon layer 1, Layer 2 example: Ethernet

![](../../../.gitbook/assets/screenshot-2021-06-21-at-8.57.34-pm.png)

Frames are a format for sending info over, is a container of sorts.

MAC addresses are uniquely tied to the hardware. 

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.06.24-pm.png)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.19.03-pm.png)

Layer 2 avoids collision as it checks carrier first before transmitting

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.21.22-pm.png)

CSMA/CD: [https://en.wikipedia.org/wiki/Carrier-sense\_multiple\_access\_with\_collision\_detection](https://en.wikipedia.org/wiki/Carrier-sense_multiple_access_with_collision_detection)

{% embed url="https://www.youtube.com/watch?v=iKn0GzF5-IU" %}

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.54.53-pm.png)

#### Attributes:

* Identifiable devices \(via MAC address\)
* Media access control \(sharing\)
* Collision detection
* Unicast \(1 to 1 instead of broadcasting to all\)
* Broadcast \(1 to all\)
* Switches \(similar to hubs, but able to direct messages to specific port\)

### Decimal &lt;=&gt; Binary

\(for IPv4 Addressing\)

![](../../../.gitbook/assets/screenshot-2021-06-21-at-9.59.29-pm.png)



