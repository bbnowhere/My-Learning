## 🧭 Notes: What I Learned from Importing CSV into Drupal via CSV

Related notes: [Git and Drupal workflow](GitBasics.md) · [Drupal upgrade guide](Drupal8.9to9.5.md)

### 🧩 1. **CSV Structure Must Match Script Exactly**

* The `array_combine($header, $row)` function pairs each header column with its corresponding value.
* If a row has *more or fewer fields* than the header, you get:

  > **ValueError: array_combine(): Argument #1 ($keys) and argument #2 ($values) must have the same number of elements**

**Lesson:**
✅ Always ensure every row has the same number of columns as the header — in our case, **16**.
✅ Use placeholder commas for empty fields.

**Debug Tip:**
Run:

```bash
awk -F',' '{print NF}' Job_Listings_Formatted.csv
```

Each line should print the same number (16). If not, fix that row.

---

### 🧰 2. **Empty or Misaligned Columns Cause Wrong Data Mapping**

* When CSV columns are off (extra comma or missing one), wrong data goes into wrong fields.
* This can cause “Undefined array key” warnings or even empty titles, leading to:

  > `SQLSTATE[23000]: Integrity constraint violation: Column 'title' cannot be null`

**Lesson:**
✅ Check the header spelling and column count carefully.
✅ Keep field names identical to what the script expects.

---

### 📅 3. **Drupal Date Fields Expect Clean ISO Dates**

* Drupal `date` field type stores dates in `YYYY-MM-DD` format.
* If you add timezone text (like `Asia/Kolkata`), you’ll get:

  > `SQLSTATE[22001]: String data, right truncated: Data too long for column 'field_last_date_value'`

**Lesson:**
✅ Use `2025-10-15` (no timezone, no timestamp).
✅ If missing, set default or calculate dynamically in PHP.

---

### 🧠 4. **Taxonomy Fields Require Term IDs (not labels)**

* You can’t just insert the text (“Person With Disability”).
* You must load the term and extract its `tid`:

  ```php
  $term = \Drupal::entityTypeManager()->getStorage('taxonomy_term')
            ->loadByProperties(['vid' => 'disability', 'name' => trim($name)]);
  ```
* If the term exists, set:

  ```php
  'field_disability_category' => ['target_id' => $tid]
  ```

**Lesson:**
✅ Always resolve taxonomy term IDs dynamically.
✅ Add debugging `var_dump($term)` to confirm it loads correctly.

---

### 🧮 5. **Debugging Strategy for Imports**

**Step 1:** Validate CSV format.
**Step 2:** Add debugging prints inside the import script:

```php
var_dump($data);
```

to see what’s read from each row.

**Step 3:** Log missing keys:

```php
if (!isset($data['title'])) echo "⚠️ Missing title for row\n";
```

**Step 4:** Test with 1–2 rows first before full import.

---

### 🪄 6. **General Good Practices**

* Always **use UTF-8 encoding** for CSV.
* Avoid newlines in description unless wrapped in quotes `"..."`.
* Re-run import only after clearing previous test data.
* Maintain a **template CSV** with all 16 fields.

---

### ✅ Summary Table

| Issue                        | Error Message                            | Root Cause                            | Fix |
| ---------------------------- | ---------------------------------------- | ------------------------------------- | --- |
| `array_combine()` ValueError | Row has fewer/more columns               | Ensure exact column count             |     |
| Undefined array key          | Wrong header names                       | Match field names in CSV              |     |
| Title cannot be null         | Empty title due to misaligned CSV        | Fix row format                        |     |
| Data too long for date       | Timezone or timestamp text in date field | Use `YYYY-MM-DD` only                 |     |
| Taxonomy not set             | Using label instead of term ID           | Load term by name and map `target_id` |     |


Excellent question — understanding that is key when debugging CSVs 👇

---

### 🧩 **What is `NF` in `awk`?**

In **`awk`**,
👉 **`NF`** stands for **“Number of Fields”** in the *current line*.

---

### 💡 Example

Suppose you have a CSV file like this:

```csv
Title,Email,Phone
Developer,dev@example.com,9876543210
Designer,design@example.com
```

Now run:

```bash
awk -F',' '{print NF}' jobs.csv
```

**Output:**

```
3
3
2
```

✅ The first two lines have **3 fields**,
⚠️ The third line has **2 fields** — meaning it’s missing a column value.

---

### 🧠 Why It’s Useful

When importing to Drupal (or any database), **every row must have the same number of fields** as the header.

If one row doesn’t, `array_combine()` in your PHP script will fail with:

```
ValueError: array_combine(): Argument #1 ($keys) and argument #2 ($values) must have the same number of elements
```

So, this command helps you **quickly find malformed rows**.

---

### 🛠️ Pro Tip

To find which line is wrong:

```bash
awk -F',' '{if (NF != 16) print "Line " NR " has " NF " fields"}' Job_Listings_Formatted.csv
```

* `NR` = line number
* `NF` = number of fields on that line
* Replace **16** with the correct number of columns in your header.

---
