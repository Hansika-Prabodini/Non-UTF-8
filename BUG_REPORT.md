# Bug Report: Non-UTF8 Files

## Executive Summary
This report documents critical bugs found in non-UTF8 encoded files, including data loss and character encoding corruption issues.

---

## Critical Bugs

### 🔴 Bug #1: Data Loss in CSV Files
**Severity**: CRITICAL  
**Affected Files**: All `large_extended_*.csv` files

**Description**:  
All CSV files contain only **30 data rows** (5 repetitions of the 6-row pattern), while their corresponding TXT files contain **3000 lines** (500 repetitions). This represents a **99% data loss**.

**Files Affected**:
- `large_extended_iso8859_1.csv` - 30 rows (should be 3000)
- `large_extended_big5.csv` - 30 rows (should be 3000)
- `large_extended_euckr.csv` - 30 rows (should be 3000)
- `large_extended_shiftjis.csv` - 30 rows (should be 3000)
- `large_extended_windows1252.csv` - 30 rows (should be 3000)
- `large_extended_utf8.csv` - 30 rows (should be 3000)
- `large_extended_utf16.csv` - 30 rows (should be 3000)
- `large_extended_utf32.csv` - 30 rows (should be 3000)
- `large_extended_ascii.csv` - 30 rows (should be 3000)

**Evidence**:
```
CSV Schema: Total rows: 30
TXT File: 3000 lines (verified by reading to line 3000 and finding EOF)
```

**Impact**:  
Any application reading these CSV files will process only 1% of the expected data, leading to incomplete analysis, incorrect statistics, and data integrity issues.

---

### 🔴 Bug #2: Character Encoding Corruption in Big5 Files
**Severity**: HIGH  
**Affected Files**: `sample_big5.txt`, `large_extended_big5.txt`, `large_extended_big5.csv`

**Description**:  
Chinese character "来" (láI - meaning "come from") is corrupted/missing in the Big5-encoded files.

**Expected Content**:
```
李雷,来自北京的开发者
```

**Actual Content (Big5)**:
```
李雷,?自北京的??者
```

**Analysis**:
- Character "来" is missing/replaced with "?"
- Characters "开发" are corrupted to "??"
- This suggests the content contains characters not in the Big5 character set or encoding errors during file creation

---

### 🔴 Bug #3: Severe Character Corruption in EUC-KR Files  
**Severity**: HIGH  
**Affected Files**: `sample_euckr.txt`, `large_extended_euckr.txt`, `large_extended_euckr.csv`

**Description**:  
Chinese and Japanese text is completely garbled when encoded in EUC-KR, rendering it unreadable.

**Expected Content**:
```
李雷,来自北京的开发者
東京太郎,東京出身のエンジニア
```

**Actual Content (EUC-KR)**:
```
×İÖô,?í»İÁÌÈîÜ??íº
ÔÔÌÈ÷¼?,ÔÔÌÈõóãóªÎ«¨«ó«¸«Ë«¢
```

**Analysis**:
- EUC-KR is designed for Korean characters
- Chinese characters (李雷) and Japanese characters (東京太郎) are completely corrupted
- This indicates inappropriate encoding choice for multilingual content

---

### 🔴 Bug #4: Character Corruption in Shift-JIS Files
**Severity**: HIGH  
**Affected Files**: `sample_shiftjis.txt`, `large_extended_shiftjis.txt`, `large_extended_shiftjis.csv`

**Description**:  
Chinese characters "开发" are corrupted in Shift-JIS encoding.

**Expected Content**:
```
李雷,来自北京的开发者
```

**Actual Content (Shift-JIS)**:
```
李雷,来自北京的??者
```

**Analysis**:
- Some Chinese characters render correctly (李雷, 来自北京的)
- Characters "开发" (development) are replaced with "??"
- Partial corruption suggests these specific characters are not in the Shift-JIS character set

---

### 🔴 Bug #5: Complete Character Loss in ISO-8859-1 Files
**Severity**: HIGH  
**Affected Files**: `sample_iso8859_1.txt`, `large_extended_iso8859_1.txt`, `large_extended_iso8859_1.csv`

**Description**:  
All Chinese and Japanese characters are replaced with "?" marks, and emoji are completely lost.

**Expected Content**:
```
李雷,来自北京的开发者
東京太郎,東京出身のエンジニア
Miyuki,"🌸 Emoji test: 😀 😁 😂"
```

**Actual Content (ISO-8859-1)**:
```
??,????????
????,??????????
Miyuki,"? Emoji test: ? ? ?"
```

**Analysis**:
- ISO-8859-1 (Latin-1) only supports Western European characters
- Asian characters and emoji are outside its character range
- All non-Latin characters are lost and replaced with "?"

---

### 🔴 Bug #6: Complete Character Loss in Windows-1252 Files
**Severity**: HIGH  
**Affected Files**: `sample_windows1252.txt`, `large_extended_windows1252.txt`, `large_extended_windows1252.csv`

**Description**:  
Identical to ISO-8859-1 bug - all Chinese, Japanese characters and emoji are replaced with "?".

**Expected Content**:
```
李雷,来自北京的开发者
東京太郎,東京出身のエンジニア  
Miyuki,"🌸 Emoji test: 😀 😁 😂"
```

**Actual Content (Windows-1252)**:
```
??,????????
????,??????????
Miyuki,"? Emoji test: ? ? ?"
```

**Analysis**:
- Windows-1252 is a superset of ISO-8859-1 but still limited to Western European characters
- Same data loss as ISO-8859-1

---

### 🔴 Bug #7: Complete Character Loss in ASCII Files
**Severity**: HIGH  
**Affected Files**: `sample_ascii.txt`, `large_extended_ascii.txt`, `large_extended_ascii.csv`

**Description**:  
All non-ASCII characters (including accented Latin characters, Asian characters, and emoji) are replaced with "?".

**Expected Content**:
```
café, résumé
李雷,来自北京的开发者
東京太郎,東京出身のエンジニア
Miyuki,"🌸 Emoji test: 😀 😁 😂"
```

**Actual Content (ASCII)**:
```
caf?, r?sum?
??,????????
????,??????????
Miyuki,"? Emoji test: ? ? ?"
```

**Analysis**:
- ASCII only supports characters 0-127
- Even accented Latin characters (é) are lost
- Most severe data loss of all encodings

---

## Impact Analysis

### Data Integrity
- **Critical**: 99% data loss in CSV files makes them unsuitable for production use
- **High**: Character corruption makes affected files unreliable for international content

### Usability
- Files with corrupted characters cannot be properly searched, sorted, or analyzed
- Applications relying on these files will produce incorrect results

### i18n/L10n Concerns
- Non-UTF encodings are inappropriate for multilingual content
- Asian language support is severely compromised in Western encodings

---

## Recommendations

### Immediate Actions
1. **Fix CSV Data Loss**: Regenerate all `large_extended_*.csv` files to contain the full 3000 rows
2. **Verify Source Data**: Ensure source data is correctly encoded before conversion
3. **Document Encoding Limitations**: Add README noting which characters each encoding supports

### Long-term Solutions
1. **Use UTF-8 by Default**: UTF-8 supports all characters and should be the standard
2. **Validation**: Implement encoding validation to detect character loss during conversion
3. **Testing**: Add automated tests to verify character fidelity across encodings

---

## Technical Details

### Test Data Pattern
Each encoding should contain these 6 rows repeated:
```
Name,Description
André,Manager of café operations
Zoë,Coördinator of résumé reviews
李雷,来自北京的开发者
東京太郎,東京出身のエンジニア
Miyuki,"🌸 Emoji test: 😀 😁 😂"
```

### Expected vs Actual Counts
- **TXT files**: 3000 lines ✅ (CORRECT)
- **CSV files**: 30 rows ❌ (INCORRECT - should be 3000)

---

## Conclusion

Multiple critical bugs were identified:
1. **CSV data loss**: All CSV files missing 99% of data
2. **Encoding corruption**: 7 different encoding-specific character corruption issues
3. **Character set limitations**: Non-UTF encodings unsuitable for multilingual content

**Priority**: These bugs should be addressed immediately, especially the CSV data loss issue which affects all file variants.
