graph TD
    %% Style Definitions
    classDef user fill:#fff,stroke:#333,stroke-width:2px;
    classDef frontend fill:#e1f5fe,stroke:#01579b,stroke-width:2px;
    classDef security fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;
    classDef server fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef database fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    %% Client Side
    subgraph Client ["Client Side / 客戶端"]
        User_Action((User / 使用者)):::user
        Login_Page[Login Interface <br/> 登入/註冊介面]:::frontend
        Game_UI[Game & Puzzle UI <br/> 遊戲與拼圖介面]:::frontend
    end

    %% Security & API Layer
    subgraph Gateway ["Security Layer / 安全接口層"]
        Auth_Check{Auth Guard <br/> 權限檢查}:::security
        JWT[[JWT / Session Token <br/> 驗證令牌]]:::security
    end

    %% Backend Services
    subgraph Services ["Backend Services / 後端服務層"]
        User_Svc[Account Service <br/> 帳號與權限服務]:::server
        Word_Svc[Word API Service <br/> 單字庫服務]:::server
        Score_Svc[Ranking Service <br/> 成績儲存服務]:::server
    end

    %% Storage
    subgraph Storage ["Data Layer / 儲存層"]
        DB_User[(User Credentials <br/> 帳號密碼庫)]:::database
        DB_Word[(Lexicon DB <br/> 英語題庫)]:::database
        DB_Score[(Score Records <br/> 歷史成績紀錄)]:::database
    end

    %% Relationships
    User_Action -->|1. Credentials| Login_Page
    Login_Page -->|2. Verify| User_Svc
    User_Svc <--> DB_User
    User_Svc -->|3. Issue Token| JWT
    
    JWT -.->|4. Authorized Access| Auth_Check
    Auth_Check --> Game_UI
    
    Game_UI <-->|Fetch Words| Word_Svc
    Game_UI -->|Upload Score| Score_Svc
    
    Word_Svc <--> DB_Word
    Score_Svc <--> DB_Score