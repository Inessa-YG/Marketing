graph TB
    Start([Customer Request<br/>Fiber Installation])
    
    Start --> Step1[Job Classification<br/>━━━━━━━━━━━━<br/>SiteID created<br/>SiteName recorded<br/>JobDueDate set<br/>ProgrammeName assigned<br/>Workstream allocated]
    
    Step1 --> Step2[Material Planning<br/>━━━━━━━━━━━━<br/>ProductCode identified<br/>ProductDescription<br/>Calculate Quantity<br/>Set LowCheck/HighCheck]
    
    Step2 --> Step3[Assign Technician<br/>━━━━━━━━━━━━<br/>Schedule based on JobDueDate<br/>Calculate DaysToCompleteVsDue]
    
    Step3 --> Step4[Create Sales Order<br/>━━━━━━━━━━━━<br/>RelatedTransactionReference = SO####<br/>TranTypeGroup = Consume<br/>TranTypeName = Sales Order]
    
    Step4 --> Decision1{Material Source?}
    
    Decision1 -->|Van Stock| Path1[Use Van Inventory<br/>━━━━━━━━━━━━<br/>ProcessingByWarehouseName:<br/>AMP COMMUNICATIONS LIMITED_1<br/>Quick access]
    
    Decision1 -->|Warehouse| Path2[Warehouse Fulfillment<br/>━━━━━━━━━━━━<br/>ProcessingByWarehouseName:<br/>Warehouse Location<br/>Complex/Urgent/Fault jobs]
    
    Path1 --> Step5[Sales Shipment<br/>━━━━━━━━━━━━<br/>TransactionReference = SS####<br/>TransactionCompleteDate<br/>TranStatusName = Completed]
    
    Path2 --> Step5
    
    Step5 --> Step6[Quantity Validation<br/>━━━━━━━━━━━━<br/>Check: LowCheck ≤ Qty ≤ HighCheck<br/>Set IsWithinCheck flag]
    
    Step6 --> Decision2{CheckOutcome?}
    
    Decision2 -->|Fail| Exception[Exception Flagged<br/>━━━━━━━━━━━━<br/>Management Review<br/>Daily Report Generated]
    
    Decision2 -->|Pass| Step7[Travel to Site<br/>━━━━━━━━━━━━<br/>Navigate to SiteName<br/>Materials in hand]
    
    Step7 --> Step8[Perform Installation<br/>━━━━━━━━━━━━<br/>Connect fiber<br/>Materials consumed<br/>Job completed]
    
    Step8 --> Decision3{Need More<br/>Materials?}
    
    Decision3 -->|Yes| Step4
    Decision3 -->|No| End([Job Complete])
    
    Exception -.->|Monitor| Reports[Daily Performance<br/>Monitoring]
    
    classDef requestStyle fill:#dbeafe,stroke:#3b82f6,stroke-width:2px
    classDef planningStyle fill:#e9d5ff,stroke:#a855f7,stroke-width:2px
    classDef inventoryStyle fill:#bbf7d0,stroke:#22c55e,stroke-width:2px
    classDef validationStyle fill:#fef08a,stroke:#eab308,stroke-width:2px
    classDef installStyle fill:#fed7aa,stroke:#f97316,stroke-width:2px
    classDef exceptionStyle fill:#fecaca,stroke:#ef4444,stroke-width:2px
    classDef decisionStyle fill:#fef08a,stroke:#eab308,stroke-width:3px
    
    class Start,Step1 requestStyle
    class Step2,Step3 planningStyle
    class Step4,Path1,Path2,Step5 inventoryStyle
    class Step6,Decision2 validationStyle
    class Step7,Step8 installStyle
    class Exception,Reports exceptionStyle
    class Decision1,Decision3 decisionStyle
