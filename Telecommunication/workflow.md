
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
