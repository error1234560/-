# English Spelling Puzzle Game (英語拼圖遊戲)

### System Architecture (系統架構圖)

```mermaid
graph TD
    subgraph Client [Client Side / 客戶端]
        UI[UI: HTML/CSS/Flexbox Interface <br/> 遊戲介面]
        JS[JS Engine: Game Logic <br/> 邏輯引擎]
    end

    subgraph Logic [Core Logic / 核心邏輯]
        Parser[Word Parser: CN Hint & Length <br/> 題目解析與字數計算]
        Shuffle[Shuffle Algorithm <br/> 字母亂數打散]
        Validator[Sequence Validator <br/> 拼字順序判定]
    end

    subgraph Data [Data & Storage / 資料存儲]
        Bank[Word Bank: JSON <br/> 單字庫]
        Store[Session Storage <br/> 成績暫存]
        123
    end

    Bank --> Parser
    Parser --> UI
    Shuffle --> UI
    UI --> JS
    JS --> Validator
    Validator --> UI
    Validator --> Store