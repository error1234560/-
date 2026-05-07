# System Architecture (系統架構圖)

```mermaid
graph TD
    %% Style Definitions
    classDef frontend fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef gateway fill:#fff3e0,stroke:#ef6c00,stroke-width:2px;
    classDef server fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef database fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef payload fill:#ffffff,stroke:#333,stroke-dasharray: 5 5;

    %% 1. Frontend Layer
    subgraph Layer_Web ["Web Frontend / 前端呈現層"]
        Login[Auth Module <br/> 登入/註冊]:::frontend
        Mode[Mode Selector <br/> 模式選擇器]:::frontend
        
        subgraph Game_Systems ["Game & Test Engines"]
            M1[Listening Test <br/> 聽力測驗]:::frontend
            M2[Spelling Puzzle <br/> 中拼英測驗]:::frontend
            M3[Sentence Fill-in <br/> 句子填空]:::frontend
        end
        
        History[Score Inquiry <br/> 成績/歷史查詢]:::frontend
    end

    %% 2. Interface & Security
    subgraph Layer_Gateway ["API Gateway / Nginx 接口層"]
        Nginx{Nginx Reverse Proxy <br/> 路由分發}:::gateway
        JWT[[JWT Authentication <br/> 安全令牌驗證]]:::gateway
    end

    %% 3. API Data Structure (Your Specific Fields)
    subgraph Layer_Payload ["Data Transmission / 資料傳輸結構"]
        JSON_Score["<b>Score API Fields:</b><br/>schoolID, studentID<br/>time, used, grade<br/>question, wordID<br/>correctAnswer, yourAnswer"]:::payload
    end

    %% 4. Backend Services
    subgraph Layer_Service ["Backend Services / 後端服務層"]
        Auth_Svc[User Service <br/> 使用者管理]:::server
        Quiz_Svc[Content Service <br/> 題目派發]:::server
        Score_Svc[Analytics Service <br/> 成績與大數據處理]:::server
    end

    %% 5. Storage Layer
    subgraph Layer_Data ["Storage Layer / 資料儲存層"]
        DB_CN[(CN/EN Lexicon <br/> 中英雙語題庫)]:::database
        DB_User[(User Credentials <br/> 帳號密碼庫)]:::database
        DB_Record[(Performance DB <br/> 歷史成績紀錄)]:::database
    end

    %% Workflow Connections
    Login --> Nginx
    Mode --> Game_Systems
    Game_Systems -->|Pack Data| JSON_Score
    JSON_Score -->|POST| Nginx
    History -->|GET by Date/Time| Nginx
    
    Nginx --> JWT
    JWT --> Auth_Svc
    JWT --> Quiz_Svc
    JWT --> Score_Svc
    
    Auth_Svc <--> DB_User
    Quiz_Svc <--> DB_CN
    Score_Svc <--> DB_Record
    Score_Svc -.-|Indexed by Timestamp| DB_Record