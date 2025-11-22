# Telecom Company Inventory and Installation Efficiency Optimization: In-Depth Data Science Analysis
**One-line summary:** what you did and the business outcome.


## 1. Business Context & Objective
- Background: This project is based on hundreds of thousands of inventory transaction records from a telecommunications equipment installation company. The company faces challenges such as project delivery delays and inventory cost control. 
- Objective: provide data-driven decision support for the company's operational efficiency and cost control through in-depth data analysis and machine learning modeling.
- Challenges:
  1.Delivery Delays: What factors cause installation work to not be completed before the customer's expected date?
  2.Cost Control: How to predict and control inventory transaction costs?
  3.Demand Forecasting: How to more accurately predict product demand and optimize inventory management?


## 2. Key findings & impact
- Finding 1 (one sentence + KPI)
- Finding 2 (one sentence + KPI)
- Recommended action & expected impact


## 3. Skills & Tools
- Data Processing: Pandas, NumPy
- Visualization: Matplotlib, Seaborn, Plotly
- Machine Learning: Scikit-learn, XGBoost
- Statistical Analysis: SciPy
- Special Techniques: Association Rule Mining (Apriori), SHAP Explainable AI


## 4. Repo structure
├── telecom_inventory_analysis.ipynb  # Main analysis Notebook
├── data/
│   └── telecom_data.xlsx             # Raw data
├── images/                           # Images for README
└── README.md                         # Project documentation


## 5. How to run (reproducibility)
1. Clone repo
2. Create venv: `python -m venv venv && source venv/bin/activate`
3. `pip install -r requirements.txt`
4. Run `python src/etl.py` then open `notebooks/`


## 6. Data
- Scale: ~250,000 inventory transaction records
- Key Table: telecom_inventory_transactions.csv
- Data Dictionary: Included in project files


## 7. Analysis summary
High-level summary + short list of important code cells and SQL queries (link to files)


## 8. Dashboard
Small embedded images of dashboards + link to interactive version (if hosted)


## 9. Business recommendations
Actionable list: who should do what, expected lift, risks, further work


## 10. About me & contact
Short bio + link to resume + link to GitHub Pages personal site

## 11. workflow
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
