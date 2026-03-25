# Modern-old-attacks

## GraphQL + SQL Injection Modern Attack Flow Mindmap
```
                    ┌──────────────────────────┐
                    │   Attacker (Client)      │
                    └────────────┬─────────────┘
                                 │
                                 ▼
                    ┌──────────────────────────┐
                    │   GraphQL API Endpoint   │
                    │   (Entry Point)          │
                    └────────────┬─────────────┘
                                 │
               ┌─────────────────┼─────────────────┐
               │                 │                 │
               ▼                 ▼                 ▼

     ┌────────────────┐  ┌────────────────┐  ┌────────────────┐
     │ Introspection  │  │ Input Fields   │  │ Nested Queries │
     │ (Recon)        │  │ (Attack Points)│  │ (Amplification)│
     └──────┬─────────┘  └──────┬─────────┘  └──────┬─────────┘
            │                   │                   │
            ▼                   ▼                   ▼

   Discover schema       Find injection       Plan large data
   - queries             points               extraction
   - fields              - id                 - relations
   - arguments           - search             - deep queries
                         - filters

                                 │
                                 ▼

                    ┌──────────────────────────┐
                    │   GraphQL Resolver      │
                    │   (Critical Layer ⚠️)   │
                    └────────────┬─────────────┘
                                 │
                     (Developer mistake here)
                                 │
                                 ▼

                    ┌──────────────────────────┐
                    │ Unsafe Query Building    │
                    │ (String Concatenation)   │
                    └────────────┬─────────────┘
                                 │
                                 ▼

                    ┌──────────────────────────┐
                    │   SQL Query Execution    │
                    │   (Database Layer)       │
                    │   :contentReference[oaicite:0]{index=0} │
                    └────────────┬─────────────┘
                                 │
                                 ▼

                    💣 SQL Injection Happens
                                 │
                                 ▼

     ┌─────────────────────────────────────────────────────┐
     │ Data Extraction via GraphQL                         │
     │                                                     │
     │ - users                                             │
     │ - emails                                            │
     │ - posts                                             │
     │ - payments                                          │
     │ - linked/nested data                                │
     └─────────────────────────────────────────────────────┘
                                 │
                                 ▼

                    ┌──────────────────────────┐
                    │ Structured JSON Output   │
                    │ (Clean, Organized Data)  │
                    └──────────────────────────┘
```

**🔥 Same thing in “chain” form** (super important)
```
Recon → Mapping → Injection → Expansion → Extraction
```

**⚠️ Where things go wrong**:
```
Safe:
db.query("SELECT * FROM users WHERE id = ?", [id])

Unsafe:
db.query("SELECT * FROM users WHERE id = " + id)
```
👉 Vulnerability is NOT GraphQL itself
👉 It’s in the resolver → SQL connection

**🧩 Mental Model**:
```
GraphQL = Smart Router
Resolvers = Decision Makers
SQL = Data Vault

If router passes bad input → vault gets compromised
```

👉 “GraphQL finds everything, SQL leaks everything — if the resolver is weak.”
