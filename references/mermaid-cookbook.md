# Mermaid Cookbook for Study Notes

Patterns for the visual content slide-text-extraction loses. When a chapter mentions a concept that the source slides clearly drew but the text dump lost, find the relevant pattern here.

---

## 1. Concept topology — `graph TB` / `graph LR`

For "what's in this chapter" (top-of-file 章导图) or "how do these concepts relate":

```mermaid
graph TB
    Idx[索引]
    Idx --> Ord[顺序索引]
    Idx --> Hash[哈希索引]
    Idx --> Bmp[位图索引]
    Ord --> B+[B+ 树]
    Hash --> Static[静态哈希]
    Hash --> Ext[可扩展哈希]
```

`TB` = top-to-bottom, `LR` = left-to-right. Use `LR` for course roadmaps (chapters left to right), `TB` for hierarchies.

To color-code:

```mermaid
graph TB
    A --> B
    style A fill:#fdb,stroke:#333
    style B fill:#bdf,stroke:#333
```

Common color palette:
- `#fdb` (peach) — "key/hot" topics
- `#bdf` (sky) — "data/storage"
- `#dfd` (mint) — "results/outputs"
- `#fed` (cream) — "memory/cache"
- `#ddd` (gray) — "background/old"

---

## 2. State machines — `stateDiagram-v2`

For transaction states, protocol states, lifecycle diagrams:

```mermaid
stateDiagram-v2
    [*] --> Active
    Active --> PartiallyCommitted: 最后语句执行完
    PartiallyCommitted --> Committed: 写入磁盘成功
    PartiallyCommitted --> Failed: 失败
    Active --> Failed: 失败
    Failed --> Aborted: 已回滚
    Committed --> [*]
    Aborted --> [*]
    Aborted --> Active: 重启 (无逻辑错误)
```

`[*]` is the start/end. Labels go after `:`. State names with spaces need camelCase or quotes.

---

## 3. ER diagrams — `erDiagram`

For database schemas, conceptual data models:

```mermaid
erDiagram
    INSTRUCTOR ||--o{ ADVISOR : has
    STUDENT ||--o{ ADVISOR : assigned_to
    INSTRUCTOR {
        int ID PK
        string name
        string dept_name
        decimal salary
    }
    STUDENT {
        int ID PK
        string name
        string dept_name
        int tot_cred
    }
```

Cardinality syntax:
- `||--||` one-to-one
- `||--o{` one-to-many
- `}o--o{` many-to-many
- `}o--||` many-to-one

`PK` = primary key, `FK` = foreign key, `UK` = unique key.

---

## 4. Algorithm flowcharts — `flowchart`

For algorithm decision trees, control flow:

```mermaid
flowchart TB
    Start[程序请求块] --> Q{在缓冲区?}
    Q -->|是| Return[返回内存地址]
    Q -->|否| Alloc[申请缓冲槽]
    Alloc --> Empty{有空槽?}
    Empty -->|是| Read[读入新块]
    Empty -->|否| Evict[挑一个块驱逐]
    Evict --> Dirty{脏?}
    Dirty -->|是| Writeback[先写回磁盘]
    Dirty -->|否| Read
    Writeback --> Read
    Read --> Return
```

`{}` = decision diamond, `[]` = process box, `()` = rounded.

---

## 5. Tree structures (B+-trees, ASTs, family trees)

Mermaid doesn't have a native tree diagram type, so use `graph TB` with manual layout:

```mermaid
graph TB
    Root["[ Mozart ]"]
    Root --> N1["[ El Said, Gold, Katz ]"]
    Root --> N2["[ Singh, Srinivasan ]"]
    N1 --> L1["[Adams, Brandt, Cali]"]
    N1 --> L2["[Crick, Einstein]"]
    N1 --> L3["[El Said, Gold]"]
    N1 --> L4["[Katz, Kim]"]
    N2 --> L5["[Mozart]"]
    N2 --> L6["[Singh]"]
    N2 --> L7["[Srinivasan, Wu]"]
```

For B+-trees specifically: use `[brackets]` for the key list inside each node. Quote the labels to keep brackets literal.

---

## 6. Timelines — `timeline`

For history of standards, list of contributors, evolution of a field:

```mermaid
timeline
    title 数据库领域的 4 位图灵奖得主
    1973 : Bachman 三层抽象
    1981 : E.F. Codd 关系理论
    1998 : Jim Gray 事务处理
    2014 : Stonebraker 早期RDBMS和新型数据库
```

Each line is `<event> : <description>`. No commas in descriptions (or quote them).

---

## 7. Sequence diagrams — `sequenceDiagram`

For concurrency schedules, network protocols, distributed systems messages:

```mermaid
sequenceDiagram
    participant T1 as Transaction T1
    participant DB as Database
    participant T2 as Transaction T2
    T1->>DB: read(A)
    T1->>DB: write(A=A-50)
    T2->>DB: read(A)
    T2->>DB: read(B)
    T2->>DB: display(A+B)
    T1->>DB: read(B)
    T1->>DB: write(B=B+50)
    Note over T1,T2: T2 saw inconsistent state
```

`->>` = solid arrow, `-->>` = dashed (response), `Note over X,Y` = annotation.

---

## 8. Subgraphs (clustering related nodes)

For grouping related concepts inside a larger diagram:

```mermaid
graph TB
    subgraph Memory[内存层]
        Cache[Cache]
        Main[Main Memory]
    end
    subgraph Disk[磁盘层]
        Flash[Flash]
        HDD[Hard Disk]
    end
    Cache --> Main --> Flash --> HDD
```

The `subgraph X[Label]` syntax gives the cluster a label. Use to make 3-tier architecture diagrams (presentation/business/data) and similar.

---

## When NOT to use Mermaid

- **Simple 2-3 item comparisons** — use a table.
- **Linear sequences without branching** — use a numbered list.
- **Lookup tables** — use a Markdown table.
- **Source code** — use a fenced code block.

A diagram should add information density, not replace text. If a sentence does the job, don't draw it.

---

## Common Mermaid mistakes

- **Special characters in node labels** — wrap labels with `[]` and use HTML entities or quotes for `<`, `>`, etc.
  Bad: `A[salary > 50000]` → "syntax error"
  Good: `A["salary > 50000"]`
- **Forgetting to quote strings with spaces** in `stateDiagram-v2`.
- **Too many nodes**. Past ~20 nodes Mermaid renders poorly. Split into multiple diagrams.
- **Cyclic graphs without `graph TD`/`graph LR`**. Use `graph` (not `flowchart`) for general graphs to avoid auto-layout fights.
- **Mixing Mermaid types in one block.** A `graph` block can't contain `stateDiagram` syntax.
