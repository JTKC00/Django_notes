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
        -userId: Long
        -name: String
        -email: String
        -phone: String
        -registrationDate: Date
        +login(email: String, password: String) Boolean
        +logout() void
        +updateProfile(name: String, email: String) Boolean
        +resetPassword(newPassword: String) Boolean
    }
    class Order {
        -orderId: Long
        -amount: Float
        -orderDate: Date
        -status: OrderStatus
        -shippingAddress: String
        +calculateTotal() Float
        +updateStatus(status: OrderStatus) void
        +addItem(item: OrderItem) void
        +removeItem(itemId: Long) Boolean
    }
    class OrderItem {
        -itemId: Long
        -productName: String
        -quantity: Integer
        -unitPrice: Float
        +getSubtotal() Float
    }
    class Product {
        -productId: Long
        -name: String
        -price: Float
        -stock: Integer
        -category: String
        +updateStock(quantity: Integer) void
        +applyDiscount(percentage: Float) void
    }
    
    User "1" --> "*" Order : places
    Order "1" --> "*" OrderItem : contains
    OrderItem "*" --> "1" Product : references
    
    note for User "用戶可以有多個訂單\\n支援登錄驗證"
    note for Order "訂單狀態：PENDING, PAID, SHIPPED, DELIVERED"
```

### 2. 對象圖（Object Diagram）

- **定義和用途**：展示類的具體實例（對象）及其關係，是類圖的快照。
- **主要元素**：對象、鏈接。
- **範例**：表示某個用戶（`user1: User`）與其訂單（`order123: Order`）的具體關聯。

```mermaid
classDiagram
    class User_Instance["user1: User"] {
        name = "John Doe"
        email = "john@example.com"
        userId = 12345
        registrationDate = "2024-01-15"
    }
    class Order_Instance["order123: Order"] {
        orderId = 123
        amount = 299.99
        orderDate = "2024-06-15"
        status = "SHIPPED"
    }
    class OrderItem_Instance["item1: OrderItem"] {
        itemId = 1
        productName = "iPhone 15"
        quantity = 1
        unitPrice = 299.99
    }
    
    User_Instance --> Order_Instance : places
    Order_Instance --> OrderItem_Instance : contains
    
    note for User_Instance "具體用戶實例"
    note for Order_Instance "具體訂單實例"
```

### 3. 組件圖（Component Diagram）

- **定義和用途**：描述系統的組件（如庫、模塊、接口）及其依賴關係。
- **主要元素**：組件、接口、依賴。
- **範例**：展示前端的 `UI Component` 依賴於後端的 `REST API Component`。

```mermaid
flowchart TD
    subgraph "Frontend Layer"
        UI[UI Component]
        Router[Router Component]
        Store[State Store]
    end
    
    subgraph "Backend Layer"
        API[REST API Component]
        Auth[Authentication Service]
        Logger[Logging Component]
    end
    
    subgraph "Data Layer"
        DB[(Database)]
        Cache[(Redis Cache)]
        FileStore[File Storage]
    end
    
    UI --> Router
    UI --> Store
    Router --> API
    Store --> API
    API --> Auth
    API --> Logger
    API --> DB
    API --> Cache
    Auth --> DB
    Logger --> FileStore
    
    note "依賴關係：前端組件依賴後端 API"
```

### 4. 部署圖（Deployment Diagram）

- **定義和用途**：展示系統的物理部署架構，包括硬體節點（如伺服器）和軟體組件。
- **主要元素**：節點、組件、關聯。
- **範例**：展示網頁系統的部署，前端在 `Web Server`，後端 API 在 `Application Server`，資料庫在 `Database Server`。

```mermaid
flowchart TD
    subgraph "CDN Network"
        CDN[CDN Server<br/>靜態資源分發]
    end
    
    subgraph "Load Balancer"
        LB[Nginx Load Balancer<br/>負載均衡器]
    end
    
    subgraph "Web Servers"
        WS1[Web Server 1<br/>React Frontend]
        WS2[Web Server 2<br/>React Frontend]
    end
    
    subgraph "Application Servers"
        AS1[App Server 1<br/>Django Backend API]
        AS2[App Server 2<br/>Django Backend API]
    end
    
    subgraph "Database Cluster"
        DB_Master[(PostgreSQL Master<br/>主資料庫)]
        DB_Slave[(PostgreSQL Slave<br/>從資料庫)]
    end
    
    subgraph "Cache Layer"
        Redis[(Redis Cache<br/>快取服務)]
    end
    
    Client[用戶瀏覽器] --> CDN
    Client --> LB
    LB --> WS1
    LB --> WS2
    WS1 --> AS1
    WS2 --> AS2
    AS1 --> DB_Master
    AS2 --> DB_Master
    AS1 --> Redis
    AS2 --> Redis
    DB_Master --> DB_Slave
```

### 5. 組合結構圖（Composite Structure Diagram）

- **定義和用途**：描述組件的內部結構及其連接關係。
- **主要元素**：部分、連接器、端口。
- **範例**：表示 `Dashboard` 組件內含 `Chart` 和 `Table` 子組件及其交互接口。

```mermaid
flowchart TD
    subgraph Dashboard["Dashboard 組件"]
        subgraph Header["Header 部分"]
            Title[標題顯示]
            Controls[控制按鈕]
        end
        
        subgraph Content["內容區域"]
            Chart[圖表組件<br/>- 數據視覺化<br/>- 支援多種圖表類型]
            Table[表格組件<br/>- 數據列表<br/>- 支援排序和篩選]
            Filter[篩選器組件<br/>- 日期範圍<br/>- 分類篩選]
        end
        
        subgraph Footer["Footer 部分"]
            Export[匯出功能]
            Pagination[分頁控制]
        end
    end
    
    subgraph External["外部數據源"]
        API[REST API]
        Database[(資料庫)]
    end
    
    Filter --> Chart : 篩選數據
    Filter --> Table : 篩選數據
    Chart --> API : 請求圖表數據
    Table --> API : 請求表格數據
    API --> Database : 查詢數據
    Controls --> Chart : 控制顯示
    Controls --> Table : 控制顯示
    Export --> Table : 匯出數據
    Export --> Chart : 匯出圖表
```

---

## 行為圖（Behavioral Diagrams）

行為圖描述系統的動態行為，例如用戶與系統的交互、流程順序等。

### 1. 用例圖（Use Case Diagram）

- **定義和用途**：展示系統的功能需求以及參與者與系統的交互。
- **主要元素**：參與者、用例、關聯。
- **範例**：用戶（`Customer`）與電商網站交互，用例包括「瀏覽商品」、「加入購物車」、「結帳」。

```mermaid
flowchart TD
    Customer[👤 顧客<br/>Customer]
    Admin[👨‍💼 管理員<br/>Admin]
    PaymentSystem[💳 支付系統<br/>Payment System]
    
    subgraph "電商系統用例"
        UC1[瀏覽商品<br/>Browse Products]
        UC2[搜尋商品<br/>Search Products]
        UC3[查看商品詳情<br/>View Product Details]
        UC4[加入購物車<br/>Add to Cart]
        UC5[管理購物車<br/>Manage Cart]
        UC6[用戶註冊<br/>User Registration]
        UC7[用戶登錄<br/>User Login]
        UC8[結帳付款<br/>Checkout & Payment]
        UC9[查看訂單<br/>View Orders]
        UC10[商品管理<br/>Manage Products]
        UC11[訂單管理<br/>Manage Orders]
        UC12[用戶管理<br/>Manage Users]
    end
    
    Customer --> UC1
    Customer --> UC2
    Customer --> UC3
    Customer --> UC4
    Customer --> UC5
    Customer --> UC6
    Customer --> UC7
    Customer --> UC8
    Customer --> UC9
    
    Admin --> UC10
    Admin --> UC11
    Admin --> UC12
    Admin --> UC9
    
    UC8 --> PaymentSystem
    
    UC4 -.->|extends| UC7
    UC8 -.->|extends| UC7
    UC2 -.->|includes| UC1
```

### 2. 交互圖（Interaction Diagrams）

#### 2.1 序列圖（Sequence Diagram）

- **定義和用途**：展示對象之間按時間順序發送和接收消息的過程。
- **主要元素**：對象、生命線、消息。
- **範例**：展示用戶點擊「提交訂單」後，前端、API 和資料庫之間的消息傳遞。

```mermaid
sequenceDiagram
    participant User as 👤 用戶
    participant Frontend as 🖥️ 前端應用
    participant API as 🔧 後端 API
    participant Auth as 🔐 驗證服務
    participant Payment as 💳 支付服務
    participant Database as 🗄️ 資料庫
    participant Email as 📧 郵件服務
    
    User->>+Frontend: 點擊「提交訂單」
    Frontend->>+API: POST /api/orders
    
    API->>+Auth: 驗證用戶令牌
    Auth-->>-API: 令牌有效
    
    API->>+Database: 檢查商品庫存
    Database-->>-API: 庫存充足
    
    API->>+Payment: 處理支付
    Payment-->>-API: 支付成功
    
    API->>+Database: 保存訂單
    Database-->>-API: 訂單已保存
    
    API->>+Database: 更新庫存
    Database-->>-API: 庫存已更新
    
    par 並行處理
        API->>+Email: 發送訂單確認郵件
        Email-->>-API: 郵件已發送
    and
        API->>+Database: 記錄交易日誌
        Database-->>-API: 日誌已記錄
    end
    
    API-->>-Frontend: 訂單創建成功
    Frontend-->>-User: 顯示訂單確認頁面
    
    note over User, Email: 整個訂單流程包含驗證、支付、<br/>庫存管理和通知等多個步驟
```

#### 2.2 通信圖（Communication Diagram）

- **定義和用途**：強調對象之間的連接和消息流。
- **主要元素**：對象、鏈接、消息。
- **範例**：表示前端組件與後端服務之間的依賴和通訊。

```mermaid
flowchart LR
    subgraph "通信圖：訂單處理系統"
        Frontend[🖥️ 前端應用<br/>Frontend]
        API[🔧 API 服務<br/>API Service]
        OrderService[📦 訂單服務<br/>Order Service]
        PaymentService[💳 支付服務<br/>Payment Service]
        Database[🗄️ 資料庫<br/>Database]
        NotificationService[📧 通知服務<br/>Notification Service]
    end
    
    Frontend -->|1. 提交訂單<br/>submitOrder()| API
    API -->|2. 創建訂單<br/>createOrder()| OrderService
    OrderService -->|3. 處理支付<br/>processPayment()| PaymentService
    PaymentService -->|4. 支付確認<br/>paymentConfirmed| OrderService
    OrderService -->|5. 保存訂單<br/>saveOrder()| Database
    Database -->|6. 訂單已保存<br/>orderSaved| OrderService
    OrderService -->|7. 發送通知<br/>sendNotification()| NotificationService
    NotificationService -->|8. 通知已發送<br/>notificationSent| OrderService
    OrderService -->|9. 訂單確認<br/>orderConfirmed| API
    API -->|10. 響應成功<br/>success response| Frontend
    
    style Frontend fill:#e1f5fe
    style API fill:#f3e5f5
    style OrderService fill:#e8f5e8
    style PaymentService fill:#fff3e0
    style Database fill:#fce4ec
    style NotificationService fill:#f1f8e9
```

### 3. 狀態機圖（State Machine Diagram）

- **定義和用途**：描述對象的狀態及其轉換過程。
- **主要元素**：狀態、轉換、事件。
- **範例**：展示 `Order` 對象的狀態轉換：`Created` → `Paid` → `Shipped`。

```mermaid
stateDiagram-v2
    [*] --> Created : 用戶提交訂單
    
    Created --> Pending : 等待支付確認
    Created --> Cancelled : 用戶取消訂單
    
    Pending --> Paid : 支付成功
    Pending --> PaymentFailed : 支付失敗
    Pending --> Cancelled : 支付超時
    
    PaymentFailed --> Pending : 重新支付
    PaymentFailed --> Cancelled : 放棄支付
    
    Paid --> Processing : 開始處理訂單
    Processing --> Shipped : 商品已發貨
    Processing --> Cancelled : 處理失敗退款
    
    Shipped --> InTransit : 運輸中
    InTransit --> Delivered : 已送達
    InTransit --> ReturnRequested : 客戶要求退貨
    
    Delivered --> Completed : 確認收貨
    Delivered --> ReturnRequested : 申請退貨
    
    ReturnRequested --> Returned : 退貨完成
    ReturnRequested --> Delivered : 拒絕退貨
    
    Returned --> Refunded : 退款完成
    Cancelled --> [*] : 訂單結束
    Completed --> [*] : 訂單結束
    Refunded --> [*] : 訂單結束
    
    note right of Created : 初始狀態：訂單剛創建
    note right of Paid : 支付完成，準備處理
    note right of Delivered : 商品已送達客戶
    note right of Completed : 交易完成
```

### 4. 活動圖（Activity Diagram）

- **定義和用途**：描述業務流程、操作順序和控制流。
- **主要元素**：活動、轉換、決策、並行。
- **範例**：展示結帳流程：「選擇商品 → 輸入地址 → 支付 → 確認訂單」。

```mermaid
flowchart TD
    Start([開始購物]) --> Browse[瀏覽商品]
    Browse --> Select{選擇商品?}
    Select -->|是| AddCart[加入購物車]
    Select -->|否| Browse
    
    AddCart --> ContinueShopping{繼續購物?}
    ContinueShopping -->|是| Browse
    ContinueShopping -->|否| ViewCart[查看購物車]
    
    ViewCart --> CheckLogin{已登錄?}
    CheckLogin -->|否| Login[用戶登錄]
    Login --> EnterShipping
    CheckLogin -->|是| EnterShipping[輸入配送地址]
    
    EnterShipping --> ValidateAddress{地址有效?}
    ValidateAddress -->|否| EnterShipping
    ValidateAddress -->|是| SelectPayment[選擇支付方式]
    
    SelectPayment --> ProcessPayment[處理支付]
    ProcessPayment --> PaymentResult{支付成功?}
    PaymentResult -->|否| PaymentError[支付失敗]
    PaymentError --> SelectPayment
    
    PaymentResult -->|是| CreateOrder[創建訂單]
    CreateOrder --> UpdateInventory[更新庫存]
    
    UpdateInventory --> par1{{並行處理}}
    par1 --> SendConfirmEmail[發送確認郵件]
    par1 --> GenerateInvoice[生成發票]
    par1 --> LogTransaction[記錄交易]
    
    SendConfirmEmail --> par2{{匯合}}
    GenerateInvoice --> par2
    LogTransaction --> par2
    
    par2 --> DisplayConfirmation[顯示訂單確認]
    DisplayConfirmation --> End([結束])
    
    style Start fill:#e8f5e8
    style End fill:#ffebee
    style ProcessPayment fill:#fff3e0
    style CreateOrder fill:#e3f2fd
```

### 5. 時序圖（Timing Diagram）

- **定義和用途**：展示對象之間的時間關係和消息傳遞的時序。
- **主要元素**：生命線、狀態、時間約束。
- **範例**：展示用戶 A 發送消息後，用户 B 在特定時間內接收並回覆。

**時序圖說明表格**：

| 時間點 | 用戶 A 狀態 | 用戶 B 狀態 | 系統狀態 | 事件 |
|--------|-------------|-------------|----------|------|
| T0     | 空閒        | 空閒        | 待機     | 初始狀態 |
| T1     | 發送中      | 空閒        | 處理中   | A 開始發送消息 |
| T2     | 等待回覆    | 接收中      | 傳遞中   | B 開始接收消息 |
| T3     | 等待回覆    | 處理中      | 待機     | B 處理消息 |
| T4     | 接收中      | 回覆中      | 傳遞中   | B 發送回覆 |
| T5     | 完成        | 完成        | 完成     | 通信結束 |

```mermaid
sequenceDiagram
    participant UserA as 👤 用戶 A
    participant System as 🖥️ 系統
    participant UserB as 👤 用戶 B
    
    note over UserA, UserB: 時序圖：即時通訊系統
    
    UserA->>+System: T1: 發送消息 "Hello"
    System->>+UserB: T2: 轉發消息給用戶 B
    
    note right of UserB: T2-T3: 用戶 B 處理消息<br/>延遲 1 秒
    
    UserB-->>-System: T4: 回覆 "Hi there!"
    System-->>-UserA: T5: 轉發回覆給用戶 A
    
    rect rgb(200, 255, 200)
    note over UserA, UserB: 約束條件：<br/>響應時間 < 5 秒
    end
    
    UserA->>+System: T6: 發送消息 "How are you?"
    System->>+UserB: T7: 轉發消息
    
    alt 正常響應 (< 5秒)
        UserB-->>System: T8: 及時回覆
        System-->>UserA: T9: 轉發回覆
    else 超時 (> 5秒)
        System-->>UserA: T10: 超時通知
        note right of System: 系統檢測到超時
    end
```

### 6. 交互概覽圖（Interaction Overview Diagram）

- **定義和用途**：提供交互的高層次視圖，結合活動圖和序列圖的特點。
- **主要元素**：活動、交互、決策。
- **範例**：概述從首頁到商品頁再到結帳頁的交互流程。

```mermaid
flowchart TD
    Start([用戶訪問網站]) --> HomePage[首頁展示]
    HomePage --> UserAction{用戶操作}
    
    UserAction -->|瀏覽分類| CategoryPage[分類頁面]
    UserAction -->|搜尋商品| SearchResults[搜尋結果]
    UserAction -->|查看推薦| Recommendations[推薦商品]
    
    CategoryPage --> ProductDetails[商品詳情頁]
    SearchResults --> ProductDetails
    Recommendations --> ProductDetails
    
    ProductDetails --> ProductAction{商品頁操作}
    ProductAction -->|加入購物車| AddToCart[加入購物車]
    ProductAction -->|立即購買| DirectCheckout[直接結帳]
    ProductAction -->|返回瀏覽| UserAction
    
    AddToCart --> ContinueShopping{繼續購物?}
    ContinueShopping -->|是| UserAction
    ContinueShopping -->|否| CartPage[購物車頁面]
    
    CartPage --> CartAction{購物車操作}
    CartAction -->|修改數量| UpdateCart[更新購物車]
    CartAction -->|刪除商品| RemoveItem[移除商品]
    CartAction -->|結帳| CheckoutProcess[結帳流程]
    
    UpdateCart --> CartPage
    RemoveItem --> CartPage
    DirectCheckout --> CheckoutProcess
    
    CheckoutProcess --> AuthCheck{登錄檢查}
    AuthCheck -->|未登錄| LoginPage[登錄頁面]
    AuthCheck -->|已登錄| ShippingInfo[配送資訊]
    
    LoginPage --> LoginAction{登錄結果}
    LoginAction -->|成功| ShippingInfo
    LoginAction -->|失敗| LoginPage
    
    ShippingInfo --> PaymentPage[支付頁面]
    PaymentPage --> PaymentProcess[支付處理]
    PaymentProcess --> PaymentResult{支付結果}
    
    PaymentResult -->|成功| OrderConfirmation[訂單確認]
    PaymentResult -->|失敗| PaymentError[支付失敗]
    
    PaymentError --> PaymentPage
    OrderConfirmation --> End([流程結束])
    
    style Start fill:#e8f5e8
    style End fill:#ffebee
    style CheckoutProcess fill:#fff3e0
    style PaymentProcess fill:#e3f2fd
```

---

## 其他圖表

### 1. 包圖（Package Diagram）

- **定義和用途**：展示系統的包（如命名空間）及其依賴關係。
- **主要元素**：包、依賴。
- **範例**：表示前端和後端代碼的模塊分包，例如 `controllers`、`models`、`views`。

```mermaid
flowchart TD
    subgraph "前端架構包"
        subgraph "UI Package"
            Components[Components<br/>🔧 可重用組件]
            Pages[Pages<br/>📄 頁面組件]
            Layouts[Layouts<br/>🏗️ 佈局組件]
        end
        
        subgraph "Business Package"
            Services[Services<br/>⚙️ 業務服務]
            Store[Store<br/>🗄️ 狀態管理]
            Utils[Utils<br/>🛠️ 工具函數]
        end
        
        subgraph "Infrastructure Package"
            API[API<br/>🌐 API 呼叫]
            Config[Config<br/>⚙️ 配置管理]
            Auth[Auth<br/>🔐 認證模組]
        end
    end
    
    subgraph "後端架構包"
        subgraph "Controllers Package"
            UserController[UserController<br/>👤 用戶控制器]
            OrderController[OrderController<br/>📦 訂單控制器]
            ProductController[ProductController<br/>🛍️ 商品控制器]
        end
        
        subgraph "Services Package"
            UserService[UserService<br/>👤 用戶服務]
            OrderService[OrderService<br/>📦 訂單服務]
            PaymentService[PaymentService<br/>💳 支付服務]
        end
        
        subgraph "Models Package"
            UserModel[User Model<br/>👤 用戶模型]
            OrderModel[Order Model<br/>📦 訂單模型]
            ProductModel[Product Model<br/>🛍️ 商品模型]
        end
        
        subgraph "Infrastructure Package"
            Database[Database<br/>🗄️ 資料庫]
            Cache[Cache<br/>⚡ 快取]
            Logger[Logger<br/>📝 日誌]
        end
    end
    
    %% 前端內部依賴
    Pages --> Components
    Pages --> Services
    Services --> API
    Services --> Store
    API --> Auth
    API --> Config
    
    %% 後端內部依賴
    UserController --> UserService
    OrderController --> OrderService
    ProductController --> OrderService
    
    UserService --> UserModel
    OrderService --> OrderModel
    OrderService --> PaymentService
    
    UserModel --> Database
    OrderModel --> Database
    ProductModel --> Database
    
    UserService --> Cache
    OrderService --> Logger
    
    %% 前後端依賴
    API -.->|HTTP/HTTPS| UserController
    API -.->|HTTP/HTTPS| OrderController
    API -.->|HTTP/HTTPS| ProductController
```

### 2. 配置文件圖（Profile Diagram）

- **定義和用途**：定義 UML 的擴展，定制特定領域的符號。
- **主要元素**：原型、標籤。
- **範例**：為網頁專案定義表示 REST API 的自定義符號。

**配置文件圖說明**：

由於 Mermaid 不直接支援 Profile Diagram，我們用文字和表格來說明：

#### Web 開發專用的 UML 擴展配置

| 原型 (Stereotype) | 適用元素 | 描述 | 圖標 |
|------------------|----------|------|------|
| `<<RestAPI>>` | Class | REST API 服務類 | 🌐 |
| `<<Controller>>` | Class | 控制器類 | 🎮 |
| `<<Service>>` | Class | 業務服務類 | ⚙️ |
| `<<Repository>>` | Class | 資料存取類 | 🗄️ |
| `<<Model>>` | Class | 資料模型類 | 📊 |
| `<<Component>>` | Class | 前端組件 | 🔧 |
| `<<Middleware>>` | Class | 中介軟體 | 🔗 |
| `<<Singleton>>` | Class | 單例模式 | 1️⃣ |
| `<<Factory>>` | Class | 工廠模式 | 🏭 |
| `<<DTO>>` | Class | 資料傳輸對象 | 📦 |

#### 範例：使用配置文件的類圖

```mermaid
classDiagram
    class UserController {
        <<Controller>>
        +getUsers() List~User~
        +createUser(userData) User
        +updateUser(id, userData) User
        +deleteUser(id) Boolean
    }
    
    class UserService {
        <<Service>>
        +findAllUsers() List~User~
        +createUser(user) User
        +validateUser(user) Boolean
    }
    
    class UserRepository {
        <<Repository>>
        +findAll() List~User~
        +findById(id) User
        +save(user) User
        +delete(id) Boolean
    }
    
    class User {
        <<Model>>
        -id: Long
        -name: String
        -email: String
        +validate() Boolean
    }
    
    class UserDTO {
        <<DTO>>
        +name: String
        +email: String
        +toUser() User
    }
    
    class AuthMiddleware {
        <<Middleware>>
        +authenticate(request) Boolean
        +authorize(user, resource) Boolean
    }
    
    UserController --> UserService : uses
    UserController --> UserDTO : uses
    UserService --> UserRepository : uses
    UserService --> User : manages
    UserRepository --> User : persists
    UserController ..> AuthMiddleware : protected by
    
    note for UserController "RESTful API 控制器\n處理 HTTP 請求"
    note for UserService "業務邏輯服務\n處理核心業務規則"
    note for UserRepository "資料存取層\n封裝資料庫操作"
```

#### 配置文件的優點：

1. **領域特定**：為特定領域（如 Web 開發）定義專用符號
2. **標準化**：團隊內部統一使用相同的符號約定
3. **可讀性**：通過原型標籤快速識別組件類型
4. **文檔化**：將設計模式和架構概念視覺化

---

## 總結與補充

### 總結

UML 提供多種圖表類型，聚焦系統的不同視角。通過選擇合適的圖表，網頁開發人員可以清晰設計系統、與團隊溝通並記錄細節，提升開發效率和品質。

### 補充

- **UML 工具**：常用工具包括 Enterprise Architect、Visual Paradigm、StarUML 等，支援生成和編輯 UML 圖表。
- **UML 版本**：目前主流為 UML 2.5，增加了更多圖表類型並改進語義。
- **實踐建議**：根據專案需求選擇圖表類型。小型網頁專案可能只需用例圖和類圖，大型系統可能需要部署圖和序列圖。