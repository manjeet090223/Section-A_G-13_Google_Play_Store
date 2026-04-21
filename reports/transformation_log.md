# 📊 Data Cleaning Report — Google Play Store
**Dataset:** 2,312,944 apps → Cleaned to 2,312,939 apps  
**Final status:** ✅ ZERO NULL VALUES

---

## 📌 Column-by-Column Report

### ❌ Columns that HAD problems → ✅ How we FIXED them

---

### 1. App Name — 5 Nulls
| | |
|---|---|
| **Problem** | 5 apps had no name at all — completely empty |
| **Why it matters** | An app without a name is corrupt/useless data |
| **Fix** | Dropped (removed) those 5 rows entirely |
| **Method** | `dropna(subset=['App Name'])` |
| **Result** | 0 nulls → All 2,312,939 apps have names |

---

### 2. Rating — 22,883 Nulls + 1,059,762 Zeros
| | |
|---|---|
| **Problem** | 22,883 apps had NULL rating, 1M+ apps had 0.0 rating |
| **Why it matters** | Can't calculate averages or build charts with empty cells |
| **Fix** | Filled NULLs with `0.0`. Treated 0.0 as "No Rating / Not Yet Rated" |
| **Why NOT Mean/Median?** | Mean Rating = 2.20 (dragged down by 1M zeros). If we used mean, it would give unrated apps a fake "2.2 star" rating — that's wrong! |
| **Method** | `fillna(0.0)` |
| **Result** | 0 nulls → Rating range is 0.0 to 5.0 |

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
| **Problem** | Same 22,883 apps that were missing Rating also had no Rating Count |
| **Fix** | Filled with `0.0` (no rating = zero count) |
| **Method** | `fillna(0.0)` |
| **Result** | 0 nulls |

**Stats (Raw):**
| Stat | Value |
|---|---|
| Mean | 2,864.84 |
| Median | 6.0 |
| Mode | 0.0 |
| Skewness | 425.83 (extremely right-skewed!) |
| Kurtosis | 227,234.40 (extreme outliers exist) |

---

### 4. Size — 196 Nulls + Text Format
| | |
|---|---|
| **Problem** | Values were text like "10M", "500k", "Varies with device" |
| **Why it matters** | Can't do math on text, can't compare sizes |
| **Fix** | Created new column `Size_MB` — converted M→MB, k→MB, G→MB |
| **For "Varies with device"** | Set to NaN then filled with **Median** (13.0 MB) |
| **Why Median?** | Size is heavily skewed (few giant apps). Mean = 26 MB but Median = 13 MB. Median is more "typical" |
| **Method** | Custom `convert_size()` function + `fillna(median)` |
| **Result** | 0 nulls in `Size_MB` |

---

### 5. Installs — 107 Nulls + Text Format
| | |
|---|---|
| **Problem** | Values were text like "1,000,000+" — couldn't do math |
| **Fix** | Stripped `+` and `,`, converted to number. Created `Installs_Numeric` |
| **Null fill** | 0 (no installs recorded = zero) |
| **Method** | `str.replace('+','').replace(',','')` then `pd.to_numeric()` |
| **Result** | 0 nulls |

---

### 6. Minimum Installs — 107 Nulls
| | |
|---|---|
| **Problem** | Same 107 apps missing Installs also had null Minimum Installs |
| **Fix** | Filled with `0` |
| **Result** | 0 nulls |

---

### 7. Currency — 135 Nulls + 'XXX' values
| | |
|---|---|
| **Problem** | 135 apps had no currency listed. Some used 'XXX' (ISO code for "no currency") |
| **Fix** | Filled nulls with Mode = `'USD'`. Replaced `'XXX'` with `'Unknown'` |
| **Why Mode?** | Currency is a category, not a number. You can't calculate "average of USD and EUR". Mode = most common = USD |
| **Result** | 0 nulls |

---

### 8. Minimum Android — 6,530 Nulls
| | |
|---|---|
| **Problem** | 6,530 apps didn't specify which Android version they need |
| **Fix** | Filled with `'Varies with device'` (the dataset's own standard label for "unknown") |
| **Why Mode?** | Mode was '4.1 and up' but that would be a guess. 'Varies with device' is more honest |
| **Result** | 0 nulls |

---

### 9. Developer Website — 760,835 Nulls (32.89%)
| | |
|---|---|
| **Problem** | Nearly 1/3 of all developers didn't list a website |
| **Why it matters** | This is NOT a data error. Many indie devs simply don't have websites |
| **Fix** | Filled with `'Not Available'` — a clear label that says "no website exists" |
| **Result** | 0 nulls |

---

### 10. Developer Email — 31 Nulls
| | |
|---|---|
| **Problem** | 31 apps had no developer email |
| **Fix** | Filled with `'Unknown'` |
| **Result** | 0 nulls |

---

### 11. Developer Id — 33 Nulls
| | |
|---|---|
| **Problem** | 33 apps had no developer ID |
| **Fix** | Filled with `'Unknown'` |
| **Result** | 0 nulls |

---

### 12. Released — 71,053 Nulls (3.07%)
| | |
|---|---|
| **Problem** | 71k apps didn't have a release date recorded |
| **Fix** | Filled with `'Unknown'` (we can't guess when an app was released) |
| **Result** | 0 nulls |

---

### 13. Privacy Policy — 420,953 Nulls (18.20%)
| | |
|---|---|
| **Problem** | 18% of apps had no privacy policy link |
| **Why it matters** | Not a data error — many older/simpler apps never added privacy policies |
| **Fix** | Filled with `'Not Available'` |
| **Result** | 0 nulls |

---

### 14. Last Updated — String Format
| | |
|---|---|
| **Problem** | Dates were stored as text ("Jun 15, 2021") instead of proper dates |
| **Fix** | Converted to `datetime` using `pd.to_datetime()` |
| **Result** | Sortable, filterable calendar dates |

---

## ✅ Columns that were ALREADY clean (no changes needed)
| Column | Status |
|---|---|
| App Id | ✅ Zero nulls, zero duplicates |
| Category | ✅ 48 unique categories, clean |
| Maximum Installs | ✅ Numeric, complete |
| Free | ✅ Boolean (True/False) |
| Price | ✅ Numeric, range $0–$400 |
| Content Rating | ✅ 6 categories, clean |
| Ad Supported | ✅ Boolean |
| In App Purchases | ✅ Boolean |
| Editors Choice | ✅ Boolean |
| Scraped Time | ✅ Complete |

---

## 📈 Final Numbers

| Metric | Before (Raw) | After (Clean) |
|---|---|---|
| **Rows** | 2,312,944 | 2,312,939 (-5) |
| **Columns** | 24 | 26 (+2 new: Size_MB, Installs_Numeric) |
| **Total Null Cells** | 1,304,987 | **0** |
| **Data Completeness** | 97.65% | **100.00%** |

---
*Group 13 | Senior Data Engineering & Data Analyst Report*
