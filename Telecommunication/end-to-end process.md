```mermaid
stateDiagram-v2
    [*] --> DailyStart: Day Begins
    
    DailyStart --> InventorySnapshot: Create UCGSOH snapshot
    note right of InventorySnapshot: SnapshotDateID = Today<br/>Copy yesterday's QtyOnHand
    
    InventorySnapshot --> CustomerRequests: Process customer requests
    
    CustomerRequests --> CheckStock: Check UCGSOH for availability
    
    CheckStock --> SufficientStock: If QtyOnHand >= Required
    CheckStock --> InsufficientStock: If QtyOnHand < Required
    
    InsufficientStock --> OrderFromSupplier: Place order using LeadTime
    note right of OrderFromSupplier: Supplementary: LeadTime<br/>Schedule receipt
    
    OrderFromSupplier --> AwaitDelivery: Wait LeadTime days
    
    AwaitDelivery --> ReceiveGoods: Goods arrive
    
    ReceiveGoods --> UpdateStockIn: Update UCGSOH (+)
    note right of UpdateStockIn: QtyOnHand += OrderQty
    
    UpdateStockIn --> SufficientStock
    
    SufficientStock --> IssueToTech: Issue materials
    
    IssueToTech --> UpdateStockOut: Update UCGSOH (-)
    note right of UpdateStockOut: QtyOnHand -= TransactionQty
    
    UpdateStockOut --> RecordTransaction: Record in Transaction Data
    note right of RecordTransaction: TransactionCompleteDate<br/>ProcessingByWarehouseName<br/>TransactionUnitQuantity
    
    RecordTransaction --> Installation: Field installation
    
    Installation --> DailyReview: End of day review
    
    DailyReview --> Reconciliation: Reconcile UCGSOH vs Transactions
    
    Reconciliation --> ForecastUpdate: Update forecasts
    note right of ForecastUpdate: Use Transaction history<br/>Compare to AvgRequestsPerWeek
    
    ForecastUpdate --> ReorderCheck: Check reorder points
    
    ReorderCheck --> BelowROP: Products below ROP?
    
    BelowROP --> OrderFromSupplier: Yes - Place orders
    BelowROP --> DailyComplete: No - Day complete
    
    DailyComplete --> [*]
```
