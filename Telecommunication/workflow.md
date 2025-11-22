```mermaid
flowchart TD
    Start([Customer Request]) --> A[Job Classification<br/>SiteID, SiteName, JobDueDate<br/>ProgrammeName, Workstream]
    
    A --> B[Material Planning<br/>ProductCode, ProductDescription<br/>UnitofMeasure, Quantity<br/>LowCheck, HighCheck]
    
    B --> C[Assign Technician<br/>DaysToCompleteVsDue]
    
    C --> D[Create Sales Order<br/>RelatedTransactionReference = SO####<br/>TranTypeGroup = Consume]
    
    D --> E{Material<br/>Source?}
    
    E -->|Van Stock| F[Technician Van Inventory<br/>ProcessingByWarehouseName:<br/>AMP COMMUNICATIONS LIMITED_1]
    
    E -->|Warehouse| G[Warehouse Fulfillment<br/>ProcessingByWarehouseName:<br/>Warehouse Location<br/>Complex/Urgent/Fault Jobs]
    
    F --> H[Sales Shipment Created<br/>TransactionReference = SS####<br/>TransactionCompleteDate<br/>TranStatusName = Completed]
    
    G --> H
    
    H --> I[Quantity Validation<br/>IsWithinCheck<br/>LowCheck ≤ Quantity ≤ HighCheck]
    
    I --> J{CheckOutcome?}
    
    J -->|Fail| K[Exception Flagged<br/>Management Review]
    K -.->|Daily Report| L[Performance Monitoring]
    
    J -->|Pass| M[Travel to Site<br/>Navigate to SiteName]
    
    M --> N[Perform Installation<br/>Materials Consumed<br/>Job Completed]
    
    N -.->|Need More Materials| D
    
    N --> O([Job Complete])

    style A fill:#e9d5ff
    style B fill:#bbf7d0
    style C fill:#fed7aa
    style D fill:#bfdbfe
    style E fill:#fef08a
    style F fill:#bbf7d0
    style G fill:#fef08a
    style H fill:#86efac
    style I fill:#fef08a
    style J fill:#fef08a
    style K fill:#fecaca
    style L fill:#e0e7ff
    style M fill:#fed7aa
    style N fill:#86efac
    style Start fill:#dbeafe
    style O fill:#86efac

sequenceDiagram
    participant C as Customer
    participant S as System
    participant P as Planning
    participant I as Inventory
    participant T as Technician
    participant Site as Customer Site

    C->>S: Request Installation
    S->>S: Create SiteID & JobDueDate
    S->>P: Classify Job (ProgrammeName)
    P->>P: Calculate Materials Needed
    P->>I: Create Sales Order (SO####)
    
    alt Van Stock Available
        I->>T: Issue from Van (AMP_1)
    else Warehouse Required
        I->>I: Pick from Warehouse
        I->>T: Fulfill Request
    end
    
    I->>I: Create Sales Shipment (SS####)
    I->>I: Validate Quantity (LowCheck/HighCheck)
    
    alt Check Pass
        T->>Site: Travel & Install
        Site->>S: Job Complete
    else Check Fail
        I->>P: Flag Exception
        P->>P: Review & Adjust
    end
    
    opt Additional Materials Needed
        T->>I: Request More Materials
        I->>T: Issue Additional Items
    end
```
