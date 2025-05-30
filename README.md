# RFP-1657: Add P2C Deposits/Withdrawals

## Overview

This document describes integration with **[Payment Provider]** for **[Country]** for **[Broker]**. The goal is to implement P2C deposits and withdrawals using a new payment provider.

---

## 🔗 Swagger Definition

Open this link in [Swagger Editor](https://editor.swagger.io/) to see and try the API definition:

👉 [Swagger Editor link](https://editor.swagger.io/#)

<details>
<summary>Click to expand YAML</summary>

```yaml
openapi: 3.0.0
info:
  title: RFP-1657 Add P2C Deposits/Withdrawals
  version: 1.0.0
  description: >
    Integration with **[Payment Provider]** for **[Country]** for **[Broker]**.
    <br><br>
    ![Big picture user flow](https://i.postimg.cc/zvvD0Ym0/UI-flow-for-deposits.jpg)
servers:
  - url: https://sandbox.[domain1]
paths:
  /api/v5/invoice/get_noreceipt:
    post:
      summary: Create deposit (no receipt)
      operationId: createDeposit
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                amount:
                  type: string
                  example: "20000"
                currency:
                  type: string
                  example: "ARS"
                currency2currency:
                  type: integer
                  example: 1
                filter_payment_system_types:
                  type: array
                  items:
                    type: string
                  example: ["netting_ars2"]
                order_id:
                  type: string
                  example: "bff2fae8-7a88-11ec-90d6-021802250822"
                order_desc:
                  type: string
                  example: "Order for order #1234"
                return_url:
                  type: string
                  example: "https://[domain2]"
                response_url:
                  type: string
                  example: "https://[domain3]"
                cancel_url:
                  type: string
                  example: "https://[domain4]/"
                server_url:
                  type: string
                  example: "https://webhook.site/d5c235c1-7801-4516-a0c8-2f87722f6839"
                customer_id:
                  type: string
                  example: "123456"
                payment_attributes:
                  type: object
                  properties:
                    client_name:
                      type: string
                      example: "Dima Dima"
                    client_email:
                      type: string
                      example: "test-lic@test.tst"
                locate:
                  type: string
                  example: "en"
      responses:
        '200':
          description: Success response
          content:
            application/json:
              schema:
                type: object
                properties:
                  redirect_url:
                    type: string
                    example: "https://[provider]/redirect"
