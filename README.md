# RFP-1657 Add P2C Deposits/Withdrawals

Integration with **[Payment Provider]** for **[Country]** for **[Broker]**.

---

## 🖼️ User Flow

![Big picture user flow](https://i.postimg.cc/zvvD0Ym0/UI-flow-for-deposits.jpg)

---

## 🔁 Sequence Diagram

```mermaid
sequenceDiagram
  participant Client
  participant Broker
  participant Provider

  Client->>Broker: Sends deposit request
  Broker->>Provider: Creates payment session
  Provider-->>Broker: Redirect URL
  Broker-->>Client: Redirect to Provider
  Client->>Provider: Completes payment
  Provider-->>Broker: Notify via webhook
  Broker-->>Client: Confirm deposit
