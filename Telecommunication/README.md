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
## Workflow Diagram (Mermaid)
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
