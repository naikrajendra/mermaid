# Cisco IT Order-to-Cash Mermaid Preview

## 1) Consolidated order-to-cash flow

```mermaid
flowchart TD
    A[Customer Request / Quote] --> B[Sales / Partner creates order]
    B --> C[Validate order and pricing]
    C --> D{Order line type}

    D -->|Subscription| S1[Create Subscription line]
    D -->|License| L1[Create License line]
    D -->|Services| SV1[Create Services line]
    D -->|Hardware| H1[Create Hardware line]

    S1 --> S2[Check contract / renewal / entitlements]
    L1 --> L2[Validate SKU, edition, compliance, seats]
    SV1 --> SV3[Validate SOW, service scope, resources]
    H1 --> H4[Check stock, lead time, shipping]

    S2 --> O1[Approve order]
    L2 --> O1
    SV3 --> O1
    H4 --> O1

    O1 --> P1[Create order in CRM / ERP]
    P1 --> P2[Route to fulfillment]

    P2 --> P3{Line type}
    P3 -->|Subscription| P4[Activate subscription / entitlement]
    P3 -->|License| P5[Generate license key / token]
    P3 -->|Services| P6[Assign service resources / project]
    P3 -->|Hardware| P7[Pick, pack, ship hardware]

    P4 --> D1[Deliver service]
    P5 --> D1
    P6 --> D1
    P7 --> D1

    D1 --> I1[Generate invoice]
    I1 --> I2[Send invoice and payment terms]
    I2 --> R1[Receive payment / cash application]
    R1 --> A1[Reconcile accounting / revenue recognition]
    A1 --> F1[Close order / renew or continue service]

    F1 --> END[Order-to-cash complete]
```

## 2) Container diagram (C4)

```mermaid
flowchart LR
    subgraph CustomerExperience[Customer Experience]
        Customer[Customer Portal]
        Partner[Partner Portal]
    end

    subgraph SalesCommerce[Sales & Commerce Containers]
        Quote[Quote Service]
        SalesApp[Sales Experience App]
        CRM[CRM Container]
        CPQ[CPQ Container]
    end

    subgraph OrderFulfillment[Order & Fulfillment Containers]
        OMS[Order Management Service]
        Entitlement[Entitlement Service]
        Provisioning[Provisioning Service]
        ServiceOps[Service Delivery Container]
    end

    subgraph FinanceRevenue[Finance & Revenue Containers]
        ERP[Billing & AR Container]
        Payment[Payment Gateway Container]
        Accounting[Accounting Container]
    end

    subgraph SupportOperations[Support & Operations]
        Support[Case Management Container]
        Warehouse[Warehouse / Logistics]
    end

    Customer -->|Browse / request| Partner
    Partner -->|Quote & order intake| SalesApp
    SalesApp -->|Opportunity data| Quote
    Quote -->|Pricing request| CPQ
    CPQ -->|Approved quote| CRM
    CRM -->|Order details| OMS

    OMS -->|Validation and order state| ERP
    OMS -->|Check eligibility| Entitlement
    OMS -->|Dispatch fulfillment| Provisioning

    Provisioning -->|Activate subscription| Entitlement
    Provisioning -->|Schedule service work| ServiceOps
    Provisioning -->|Ship hardware| Warehouse

    ERP -->|Invoice to customer| Payment
    Payment -->|Payment receipt| Accounting
    Accounting -->|Revenue posting| ERP

    ServiceOps -->|Service updates| Support
    Support -->|Case and usage| CRM
    Support -->|Escalation notes| OMS
```

## 3) Enterprise architecture view

```mermaid
flowchart LR
    subgraph CustomerLayer[Customer & Sales]
        C[Customer]
        P[Partner / Sales Rep]
        Q[Quote / Opportunity]
    end

    subgraph CorePlatform[Core Order Platform]
        CRM[CRM]
        CPQ[CPQ / Pricing]
        OM[Order Management]
        E[Entitlement / Contracts]
    end

    subgraph Fulfillment[Fulfillment & Delivery]
        PR[Provisioning Engine]
        SUB[Subscription Activation]
        LIC[License Key Generator]
        SVC[Service Delivery / PMO]
        WH[Warehouse / Logistics]
    end

    subgraph Finance[Finance & Billing]
        ERP[ERP / Billing]
        INV[Invoice Engine]
        AR[AR / Cash Application]
        GL[GL / Revenue Recognition]
    end

    subgraph External[External Systems]
        PAY[Payment Gateway]
        BANK[Bank / Treasury]
        TAX[Tax / Compliance]
    end

    C -->|Request| P
    P -->|Quote and order| Q
    Q -->|Opportunity data| CRM
    CRM -->|Price & validate| CPQ
    CPQ -->|Approved order| OM
    OM -->|Contract checks| E
    OM -->|Dispatch line fulfillment| PR

    PR -->|Subscription line| SUB
    PR -->|License line| LIC
    PR -->|Services line| SVC
    PR -->|Hardware line| WH

    SUB -->|Activation status| ERP
    LIC -->|License activation| ERP
    SVC -->|Service milestones| ERP
    WH -->|Shipment / receipt| ERP

    ERP -->|Creates invoice| INV
    INV -->|Send invoice| PAY
    PAY -->|Payment receipt| AR
    AR -->|Cash application| GL
    GL -->|Reporting / compliance| ERP

    ERP -->|Tax / statutory| TAX
    AR -->|Settlement| BANK
```

## Open preview in VS Code

1. Open this file in VS Code.
2. Press Ctrl+Shift+P.
3. Run: `Markdown: Open Preview` or `Markdown: Open Preview to the Side`.

The Mermaid diagrams will render in the preview panel.
