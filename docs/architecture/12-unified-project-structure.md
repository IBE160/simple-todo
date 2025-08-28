# 12. Unified Project Structure

```
simple-todo/
├── .github/
│   └── workflows/
│       └── ci.yaml
├── apps/
│   ├── web/
│   │   ├── src/
│   │   │   ├── components/
│   │   │   ├── app/
│   │   │   ├── hooks/
│   │   │   ├── services/
│   │   │   ├── stores/
│   │   │   └── lib/
│   │   ├── public/
│   │   └── package.json
│   └── api/
│       ├── main.py
│       ├── routers/
│       ├── models.py
│       ├── schemas.py
│       └── database.py
├── packages/
│   └── shared/
│       └── src/
│           └── types/
├── docs/
│   ├── prd.md
│   ├── front-end-spec.md
│   └── architecture.md
├── .env.example
├── package.json
└── README.md
```

---
