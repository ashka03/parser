# Water Utility Bill Parser

## Overview

This Node.js command-line application reads a utility bill text file and extracts the following information:

- Customer Number
- Account Number
- Bill Period
- Bill Number
- Bill Date
- Total New Charges

---

## Project Structure

```text
project-folder/
│
├── parser.js
├── test-q1.txt
└── README.md
```

---

## Requirements

- Node.js

Verify installation:

```bash
node -v
```

---

## Running the Program

Run the parser with:

```bash
node parser.js test-q1.txt
```

---

## Expected Output

```text
Customer Number: 0523080
Account Number: 01403664
Bill Period: May 31, 2020 to Jul 30, 2020
Bill Number: 21478560
Bill Date: Aug 11, 2020
Total New Charges: $2,760.84
```

---

# Implementation

## Step 1: Read the Input File

```js
const fs = require("fs");

const filePath = process.argv[2];

const text = fs.readFileSync(filePath, "utf8");
```

The program accepts the file path through the command line and reads the contents of the bill into a string.

---

## Step 2: Extract Customer and Account Number

```js
const accountMatch = text.match(
    /Customer no\. - Account no\.[\s\S]*?(\d+)\s*-\s*(\d+)/i
);

const customerNum = accountMatch[1];
const accountNum = accountMatch[2];
```

This regular expression finds the customer number and account number following the label:

```text
Customer no. - Account no.
```

Example extracted values:

```text
0523080
01403664
```

---

## Step 3: Extract Bill Date

```js
const billDateMatch = text.match(
    /Bill date:\s*(.+)/i
);

const billDate = billDateMatch[1].trim();
```

This extracts the value following:

```text
Bill date:
```

Example:

```text
Aug 11, 2020
```

---

## Step 4: Extract Bill Number

```js
const billNumberMatch = text.match(
    /Bill number:\s*(\d+)/i
);

const billNumber = billNumberMatch[1];
```

This extracts the bill number following:

```text
Bill number:
```

Example:

```text
21478560
```

---

## Step 5: Extract Bill Period

```js
const billPeriodMatch = text.match(
    /Bill period:[\s\S]*?([A-Za-z]+\s+\d{1,2},\s+\d{4}\s+to\s+[A-Za-z]+\s+\d{1,2},\s+\d{4})/i
);

const billPeriod = billPeriodMatch[1].trim();
```

This extracts the date range following:

```text
Bill period:
```

Example:

```text
May 31, 2020 to Jul 30, 2020
```

The pattern `[\s\S]*?` allows the parser to search across multiple lines until it finds the date range.

---

## Step 6: Extract Total New Charges

```js
const totalChargeMatch = text.match(
    /Total new charges\s+\$([\d,]+\.\d{2})/i
);

const totalNewCharge = totalChargeMatch[1];
```

This extracts the amount following:

```text
Total new charges
```

Example:

```text
2,760.84
```

---

## Step 7: Display Results

```js
console.log("Customer Number: " + customerNum);
console.log("Account Number: " + accountNum);
console.log("Bill Period: " + billPeriod);
console.log("Bill Number: " + billNumber);
console.log("Bill Date: " + billDate);
console.log("Total New Charges: $" + totalNewCharge);
```

The extracted information is displayed in the required format.

---

## Error Handling

Each field is validated before use.

Example:

```js
if (!billDateMatch) {
    throw new Error("Bill date not found");
}
```

This prevents runtime errors and provides a clear error message when expected data is missing.

---

## Design Decisions

- Used Node.js built-in `fs` module to read files.
- Used regular expressions to locate and extract required fields.
- Added validation checks to ensure required data exists.
- Anchored regex patterns to nearby labels where possible for improved reliability.
- Kept the implementation simple and readable for maintainability.
