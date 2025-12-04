```mermaid
%%{init: {'theme': 'dark', 'themeVariables': { 'background': '#000000', 'primaryTextColor': '#ffffff', 'fontFamily': 'Arial, sans-serif', 'fontWeight': 'bold'}}}%%
flowchart TD
    %% ===== 定义所有节点 =====
    Start([<b>Start</b>]):::start

    A[<b>Customer Request</b>]:::phase1
    B{<b>Triage & Classify</b>}:::phase1
    C[<b>Create Project</b><br>SiteID, ExtRef]:::phase1
    D{<b>Scheduling Engine</b>}:::phase1

    E{<b>Material Source?</b>}:::phase2
    F[<b>Allocate from AMP Stock</b>]:::phase2
    G{<b>Check Warehouse QtyOnHand</b>}:::phase2
    H[<b>Allocate from Warehouse</b>]:::phase2
    I{<b>Replenish by ProductGroup</b>}:::phase2
    J[<b>Order from Supplier<br>Long Lead Time</b>]:::phase2
    K[<b>Request from Chorus Warehouse</b>]:::phase2
    L[<b>Order from UCG Supplier</b>]:::phase2
    M[<b>Generate SO/SS Transactions</b>]:::phase2

    N[<b>Technician to Site</b>]:::phase3
    O[<b>Perform Installation</b>]:::phase3
    P{<b>Field Qty Check<br>Low/HighCheck</b>}:::phase3
    Q([<b>Complete Installation</b>]):::phase3
    R[<b>Emergency Request</b>]:::phase3

    %% ===== 定义主流程走向 =====
    Start --> A
    A --> B
    B --> C
    C --> D
    D --> E

    %% 物料来源决策分支
    E -->|Path 1: Mobile Stock| F
    E -->|Path 2: Warehouse| G
    G -->|Sufficient| H
    G -->|Shortage| I

    %% 补货决策分支
    I -->|Chorus| J
    I -->|Chorus Inv.| K
    I -->|UCG| L

    %% 汇集到生成交易
    F --> M
    H --> M
    J --> M
    K --> M
    L --> M

    %% 进入现场执行阶段
    M --> N
    N --> O
    O --> P
    P -->|Pass| Q

    %% 现场紧急请求的反馈回路
    P -->|Fail - Shortage| R
    R -.-> E

    %% ===== 定义样式类 =====
    classDef start fill:#555,stroke:#fff,stroke-width:2px,color:#fff;
    classDef phase1 fill:#4a6fa5,stroke:#fff,stroke-width:2px,color:#fff;
    classDef phase2 fill:#8a6baf,stroke:#fff,stroke-width:2px,color:#fff;
    classDef phase3 fill:#4e8c7a,stroke:#fff,stroke-width:2px,color:#fff;
```
