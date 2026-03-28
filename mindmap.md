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
