# 📊 Data Cleaning Report — Google Play Store (Updated)

**Dataset:** 2,312,944 apps → Cleaned to 2,312,939 apps  
**Final status:** ✅ ZERO NULL VALUES (using Mean / Median / Mode)

---

## 📌 Column-by-Column Report

### ❌ Columns that HAD problems → ✅ How we FIXED them

---

### 1. App Name — 5 Nulls
| | |
|---|---|
| **Problem** | 5 apps had no name |
| **Fix** | Dropped rows |
| **Method** | `dropna(subset=['App Name'])` |
| **Result** | 0 nulls |

---

### 2. Rating — 22,883 Nulls
| | |
|---|---|
| **Problem** | Missing ratings |
| **Fix** | Filled with **Mean = 2.20** |
| **Why Mean?** | Continuous feature, preserves overall average |
| **Method** | `fillna(mean)` |
| **Result** | 0 nulls |

**Stats (Raw):**
| Stat | Value |
|---|---|
| Mean | 2.2032 |
| Median | 2.9000 |
| Mode | 0.0 |
| Std Dev | 2.1062 |
| Skewness | -0.0021 |
| Kurtosis | -1.8598 |

---

### 3. Rating Count — 22,883 Nulls
| | |
|---|---|
| **Problem** | Missing counts |
| **Fix** | Filled with **Median = 6** |
| **Why Median?** | Extremely skewed data |
| **Method** | `fillna(median)` |
| **Result** | 0 nulls |

**Stats (Raw):**
| Stat | Value |
|---|---|
| Mean | 2,864.84 |
| Median | 6.0 |
| Mode | 0.0 |
| Skewness | 425.83 |
| Kurtosis | 227,234.40 |

---

### 4. Size — 196 Nulls + Text Format
| | |
|---|---|
| **Problem** | Text values + missing |
| **Fix** | Converted to `Size_MB` |
| **Null Handling** | Filled with **Median = 13 MB** |
| **Why Median?** | Skewed distribution (few huge apps) |
| **Method** | `convert + fillna(median)` |
| **Result** | 0 nulls |

**Stats (Raw):**
| Stat | Value |
|---|---|
| Mean | 26 MB |
| Median | 13 MB |

---

### 5. Installs — 107 Nulls
| | |
|---|---|
| **Problem** | Text + missing |
| **Fix** | Converted to numeric |
| **Null Handling** | Filled with **Median** |
| **Why Median?** | Highly right-skewed |
| **Result** | 0 nulls |

---

### 6. Minimum Installs — 107 Nulls
| | |
|---|---|
| **Fix** | Filled with **Median** |
| **Result** | 0 nulls |

---

### 7. Currency — 135 Nulls + 'XXX'
| | |
|---|---|
| **Fix** | Filled with **Mode = USD** |
| **Why Mode?** | Categorical |
| **Result** | 0 nulls |

---

### 8. Minimum Android — 6,530 Nulls
| | |
|---|---|
| **Fix** | Filled with **Mode = '4.1 and up'** |
| **Result** | 0 nulls |

---

### 9. Developer Website — 760,835 Nulls
| | |
|---|---|
| **Fix** | Filled with **Mode** |
| **Result** | 0 nulls |

---

### 10. Developer Email — 31 Nulls
| | |
|---|---|
| **Fix** | Filled with **Mode** |
| **Result** | 0 nulls |

---

### 11. Developer Id — 33 Nulls
| | |
|---|---|
| **Fix** | Filled with **Mode** |
| **Result** | 0 nulls |

---

### 12. Released — 71,053 Nulls
| | |
|---|---|
| **Fix** | Filled with **Mode** |
| **Result** | 0 nulls |

---

### 13. Privacy Policy — 420,953 Nulls
| | |
|---|---|
| **Fix** | Filled with **Mode** |
| **Result** | 0 nulls |

---

### 14. Last Updated — String Format
| | |
|---|---|
| **Fix** | Converted to datetime |
| **Method** | `pd.to_datetime()` |

---

## 📈 Final Numbers

| Metric | Before | After |
|---|---|---|
| Rows | 2,312,944 | 2,312,939 |
| Columns | 24 | 26 |
| Total Nulls | 1,304,987 | **0** |
| Data Completeness | 97.65% | **100%** |

---
