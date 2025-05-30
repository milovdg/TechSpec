# RFP-1657 Add P2C Deposits/Withdrawals

Integration with **[Payment Provider]** for **[Country]** for **[Broker]**.

---

## 🖼️ User Flow

![Big picture user flow](https://i.postimg.cc/zvvD0Ym0/UI-flow-for-deposits.jpg)

---

## 🔁 Sequence Diagram

```mermaid
sequenceDiagram
autonumber

actor Client

== CLient flow ==
activate Client
Client -> UPP ++: Select Local bank transfer method\nfill amount \npress press deposit
UPP -> PHUB ++: requestDeposit\nmethod = pay_growpayments_receiptless_*_in
PHUB -> PSA ++: /createIncomeTransfer\npaymentIntegrationCode = pay_growpayments_receiptless_*_in
PSA -> PSA: save transfer
PSA --> Kafka: psa-?--all: send transfer event
PSA -> GrowPayments ++: <b><font color=green>POST: /api/v5/invoice/get_noreceipt
GrowPayments --> PSA --: <b><font color=green>/api/v5/invoice/get response \n(redirect URL)
PSA --> PHUB --: RedirectInfo, redirect url, transferId
PHUB --> UPP --: RedirectInfo, redirect url, transferId
UPP -> Client --: redirect url (GrowPayments)

Client -> GrowPayments ++: load deposit page
GrowPayments --> Client --: get requisites for a bank transfer

par
    Client -> Bank ++: Make transfer to the given requisites
    Bank --> Client --: Done

    Client -> UPP ++: load result page
end par

UPP -> PHUB ++: updateIncomeTransfer\nwith transferId
PHUB -> PSA ++: updateIncomeTransfer\nwith transferId
PSA -> PSA: get transfer from DB
opt status not final
    PSA -> GrowPayments ++: <b><font color=green>GET: Get payment status /api/v5/invoice/status
    GrowPayments --> PSA --: <b><font color=green>status
    PSA -> PSA: update transfer
    PSA --> Kafka: psa-?--all: send transfer event
end opt
PSA -> PHUB --: Ok, transfer status
PHUB -> UPP --: Ok, transfer status
UPP -> Client --: result page
deactivate Client

== Statements ==
    activate PSA
    loop loop by async operation configuration
    PSA -> GrowPayments ++: <b><font color=green>GET: Get payment status /api/v5/invoice/status
    GrowPayments --> PSA --: <b><font color=green>status
    PSA -> PSA: update transfer
    PSA --> Kafka: transfer event
    deactivate PSA
end loop

== Webhook ==
activate GrowPayments
GrowPayments -> PSA ++: <b><font color=green>status
PSA -> PSA: update transfer
PSA --> Kafka: transfer event
PSA --> GrowPayments--: <b><font color=green>HTTP 200
deactivate GrowPayments

