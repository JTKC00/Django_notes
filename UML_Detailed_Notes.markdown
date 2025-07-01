# UML 詳細筆記

## 簡介

UML（Unified Modeling Language，統一建模語言）是一種標準化的視覺化建模語言，用於描述、設計和記錄軟體系統。它通過圖形符號和規範化的表示方法，幫助開發人員、設計師和利益相關者更好地理解系統的結構和行為。在網頁開發中，UML 可用於規劃系統架構、梳理需求並與團隊成員溝通。

---

## 結構圖（Structural Diagrams）

結構圖展示系統的靜態結構，例如類、組件和它們之間的關係。

### 1. 類圖（Class Diagram）

- **定義和用途**：描述系統中的類、屬性、方法以及類之間的關係（如繼承、關聯、聚合等）。
- **主要元素**：類、屬性、方法、關聯、繼承、聚合、組合。
- **範例**：在電商網站中，表示 `User` 類（屬性：姓名、電子郵件；方法：登錄、登出）與 `Order` 類（屬性：訂單號、金額）的關聯。

```mermaid
classDiagram
    class User {
        name: String
        email: String
    }
    class Order {
        orderId: String
        amount: Float
    }
    User "John Doe" -- "order123" Order : places
```

### 2. 對象圖（Object Diagram）

- **定義和用途**：展示類的具體實例（對象）及其關係，是類圖的快照。
- **主要元素**：對象、鏈接。
- **範例**：表示某個用戶（`user1: User`）與其訂單（`order123: Order`）的具體關聯。

```mermaid
objectDiagram
    object user1 : User {
        name = "John Doe"
        email = "john@example.com"
    }
    object order123 : Order {
        orderId = "123"
        amount = 100.0
    }
    user1 --> order123 : places
```

### 3. 組件圖（Component Diagram）

- **定義和用途**：描述系統的組件（如庫、模塊、接口）及其依賴關係。
- **主要元素**：組件、接口、依賴。
- **範例**：展示前端的 `UI Component` 依賴於後端的 `REST API Component`。

```mermaid
componentDiagram
    component "UI Component"
    component "REST API Component"
    "UI Component" --> "REST API Component" : depends on
```

### 4. 部署圖（Deployment Diagram）

- **定義和用途**：展示系統的物理部署架構，包括硬體節點（如伺服器）和軟體組件。
- **主要元素**：節點、組件、關聯。
- **範例**：展示網頁系統的部署，前端在 `Web Server`，後端 API 在 `Application Server`，資料庫在 `Database Server`。

```mermaid
deploymentDiagram
    node "Web Server" {
        artifact "Frontend"
    }
    node "Application Server" {
        artifact "Backend API"
    }
    node "Database Server" {
        artifact "Database"
    }
    "Frontend" --> "Backend API" : calls
    "Backend API" --> "Database" : accesses
```

### 5. 組合結構圖（Composite Structure Diagram）

- **定義和用途**：描述組件的內部結構及其連接關係。
- **主要元素**：部分、連接器、端口。
- **範例**：表示 `Dashboard` 組件內含 `Chart` 和 `Table` 子組件及其交互接口。

```mermaid
compositeStructureDiagram
    component Dashboard {
        part Chart
        part Table
        connector Chart -> Table : dataflow
    }
```

---

## 行為圖（Behavioral Diagrams）

行為圖描述系統的動態行為，例如用戶與系統的交互、流程順序等。

### 1. 用例圖（Use Case Diagram）

- **定義和用途**：展示系統的功能需求以及參與者與系統的交互。
- **主要元素**：參與者、用例、關聯。
- **範例**：用戶（`Customer`）與電商網站交互，用例包括「瀏覽商品」、「加入購物車」、「結帳」。

```mermaid
useCase
    actor Customer
    usecase "Browse Products"
    usecase "Add to Cart"
    usecase "Checkout"
    Customer --> "Browse Products"
    Customer --> "Add to Cart"
    Customer --> "Checkout"
```

### 2. 交互圖（Interaction Diagrams）

#### 2.1 序列圖（Sequence Diagram）

- **定義和用途**：展示對象之間按時間順序發送和接收消息的過程。
- **主要元素**：對象、生命線、消息。
- **範例**：展示用戶點擊「提交訂單」後，前端、API 和資料庫之間的消息傳遞。

```mermaid
sequenceDiagram
    participant Frontend
    participant API
    participant Database
    Frontend->>API: Submit Order
    API->>Database: Save Order
    Database-->>API: Order Saved
    API-->>Frontend: Order Confirmed
```

#### 2.2 通信圖（Communication Diagram）

- **定義和用途**：強調對象之間的連接和消息流。
- **主要元素**：對象、鏈接、消息。
- **範例**：表示前端組件與後端服務之間的依賴和通訊。

```mermaid
communicationDiagram
    object Frontend
    object API
    Frontend -> API : 1. Submit Order
    API -> Frontend : 2. Order Confirmed
```

### 3. 狀態機圖（State Machine Diagram）

- **定義和用途**：描述對象的狀態及其轉換過程。
- **主要元素**：狀態、轉換、事件。
- **範例**：展示 `Order` 對象的狀態轉換：`Created` → `Paid` → `Shipped`。

```mermaid
stateDiagram
    [*] --> Created
    Created --> Paid : pay
    Paid --> Shipped : ship
    Shipped --> [*]
```

### 4. 活動圖（Activity Diagram）

- **定義和用途**：描述業務流程、操作順序和控制流。
- **主要元素**：活動、轉換、決策、並行。
- **範例**：展示結帳流程：「選擇商品 → 輸入地址 → 支付 → 確認訂單」。

```mermaid
activityDiagram
    start
    :Select Products;
    :Enter Address;
    :Make Payment;
    :Confirm Order;
    stop
```

### 5. 時序圖（Timing Diagram）

- **定義和用途**：展示對象之間的時間關係和消息傳遞的時序。
- **主要元素**：生命線、狀態、時間約束。
- **範例**：展示用戶 A 發送消息後，用户 B 在特定時間內接收並回覆。

```mermaid
timingDiagram
    @startuml
    robust "User A" as A
    robust "User B" as B
    A is Sending
    B is Waiting
    @A
    0 is Sending
    +10 is Sent
    @B
    0 is Waiting
    +5 is Receiving
    +10 is Replying
    @enduml
```

### 6. 交互概覽圖（Interaction Overview Diagram）

- **定義和用途**：提供交互的高層次視圖，結合活動圖和序列圖的特點。
- **主要元素**：活動、交互、決策。
- **範例**：概述從首頁到商品頁再到結帳頁的交互流程。

```mermaid
interactionOverviewDiagram
    start
    :Home Page;
    :Product Page;
    :Checkout Page;
    stop
```

---

## 其他圖表

### 1. 包圖（Package Diagram）

- **定義和用途**：展示系統的包（如命名空間）及其依賴關係。
- **主要元素**：包、依賴。
- **範例**：表示前端和後端代碼的模塊分包，例如 `controllers`、`models`、`views`。

```mermaid
packageDiagram
    package "Frontend" {
        package "Views"
        package "Controllers"
    }
    package "Backend" {
        package "Models"
        package "Controllers"
    }
    "Frontend" --> "Backend" : depends on
```

### 2. 配置文件圖（Profile Diagram）

- **定義和用途**：定義 UML 的擴展，定制特定領域的符號。
- **主要元素**：原型、標籤。
- **範例**：為網頁專案定義表示 REST API 的自定義符號。

```mermaid
profileDiagram
    stereotype <<REST API>>
```

---

## 總結與補充

### 總結

UML 提供多種圖表類型，聚焦系統的不同視角。通過選擇合適的圖表，網頁開發人員可以清晰設計系統、與團隊溝通並記錄細節，提升開發效率和品質。

### 補充

- **UML 工具**：常用工具包括 Enterprise Architect、Visual Paradigm、StarUML 等，支援生成和編輯 UML 圖表。
- **UML 版本**：目前主流為 UML 2.5，增加了更多圖表類型並改進語義。
- **實踐建議**：根據專案需求選擇圖表類型。小型網頁專案可能只需用例圖和類圖，大型系統可能需要部署圖和序列圖。