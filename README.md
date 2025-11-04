# -Delhi-Air-Quality-Pollution-Trends
flowchart TD
A[Load raw CSV: /mnt/data/final_dataset.csv] --> B[Inspect & Summary]
B --> C[Parse Date/Time]
C --> D[Missing Value Analysis]
D --> E{Drop or Impute?}
E -->|Drop cols with >50% missing| F[Drop Columns]
E -->|Impute numeric| G[Interpolate/Median]
E -->|Impute categorical| H[Mode / Backfill]
G --> I[Handle Duplicates]
F --> I
H --> I
I --> J[Outlier Detection]
J --> K{IQR or Z-score?}
K -->|IQR cap| L[Cap Outliers]
K -->|Z-score remove| M[Flag/Remove Rows]
L --> N[Standardize Station Names]
M --> N
N --> O[Feature Engineering]
O --> P[Scaling / Normalization]
P --> Q[Save cleaned CSV: /mnt/data/cleaned_final_dataset.csv]
Q --> R[Push to GitHub & Document Pipeline]
