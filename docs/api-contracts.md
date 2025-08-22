# API Contracts

## POST /api/calc/weekly

**Request Example**
```json
{
  "weeklyIncome": 1250,
  "gstRegistered": true,
  "hecsEnabled": true,
  "superRate": 0.11,
  "state": "NSW",
  "businessType": "sole_trader"
}
```

**Response Example**
```json
{
  "components": {
    "incomeTax": 240.00,
    "gst": 125.00,
    "medicare": 25.00,
    "hecs": 15.00,
    "super": 137.50
  },
  "total": 542.50,
  "disclaimer": "General information only, not tax advice."
}
```

## POST /api/estimator

**Request Example**
```json
{
  "incomes": [/* user incomes */],
  "expenses": [/* user expenses */],
  "goals": [/* weekly goals */]
}
```

**Response Example**
```json
{
  "refundRange": [1000, 1600],
  "missingChecklist": ["add super contributions"]
}
```

## POST /api/reports/eofy

**Request Example**
```json
{
  "userId": "<uuid>",
  "fy": "FY2025"
}
```

**Response Example**
```json
{
  "pdfUrl": "https://example.com/your-report.pdf"
}
```
