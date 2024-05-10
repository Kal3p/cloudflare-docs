---
_build:
  publishResources: false
  render: never
  list: never
inputParameters: productName
---

Magic $1 customers can run traceroutes to understand the hop by hop Internet path and latencies from Cloudflare’s network to their own network.

To run a traceroute from a specific Cloudflare data center to your network:

1. Log in to the [Cloudflare dashboard](https://dash.cloudflare.com/) and select your account.
2. Go to **Magic $1** > **Tunnel Health**.
3. Expand a Magic Tunnel, and find the Cloudflare data center where the traceroute will be run from.
4. Select **Traceroute for details**.

You can access detailed data from the traceroute, including:

- TTL Host
- AS Number
- Packets sent in the traceroute
- Average, minimum, and maximum latency
- Standard deviation of latency