# Water Utility Bill Parser

## Overview

This project is a Node.js command-line program that reads a water utility bill from a text file and extracts important billing information using regular expressions.

The program extracts:

- Customer Number
- Account Number
- Bill Period
- Bill Number
- Bill Date
- Total New Charges

## Files

```text
project-folder/
│
├── parser.js
├── test-q1.txt
└── README.md
```

## Requirements

This project requires Node.js.

To check if Node.js is installed, run:

```bash
node -v
```

If a version number appears, Node.js is installed correctly.

## How to Run

Open the project folder in VS Code.

Then open the terminal:

```text
Terminal → New Terminal
```

Run the program using:

```bash
node parser.js test-q1.txt
```

## Expected Output

```text
Customer Number: 0523080
Account Number: 01403664
Bill Period: May 31, 2020 to Jul 30, 2020
Bill Number: 21478560
Bill Date: Aug 11, 2020
Total New Charges: $2,760.84
```

# How the Program Works

## 1. Import the File System Module

```js
const fs = require("fs");
```

The `fs` module is a built-in Node.js module used to work with files.

In this project, it is used to read the bill text file.

---

## 2. Get the File Path from the Command Line

```js
const filePath = process.argv[2];
```

When the program is run like this:

```bash
node parser.js test-q1.txt
```

`process.argv[2]` stores the value:

```text
test-q1.txt
```

This allows the program to work with any input file path provided by the user.

---

## 3. Validate the File Argument

```js
if (!filePath) {
  console.error("Usage: node parser.js <input-file>");
  process.exit(1);
}
```

This checks whether the user provided a file name.

If no file is provided, the program prints a helpful usage message and exits with an error code.

---

## 4. Read the Text File

```js
const text = fs.readFileSync(filePath, "utf8");
```

This reads the full content of the input file and stores it as a string in the variable `text`.

The `"utf8"` option ensures the file is read as readable text.

---

## 5. Extract Customer and Account Number

```js
const accountMatch = text.match(
  /Customer no\. - Account no\.[\s\S]*?(\d+)\s*-\s*(\d+)/i
);

const customerNum = accountMatch[1];
const accountNum = accountMatch[2];
```

This regex looks for the label:

```text
Customer no. - Account no.
```

Then it finds the customer number and account number that appear after it.

The two captured values are:

```js
const customerNum = accountMatch[1];
const accountNum = accountMatch[2];
```

---

## 6. Extract Bill Date

```js
const billDateMatch = text.match(
  /Bill date:\s*(.+)/i
);

const billDate = billDateMatch[1].trim();
```

This finds the line beginning with:

```text
Bill date:
```

and captures the date after it.

Example:

```text
Aug 11, 2020
```

---

## 7. Extract Bill Number

```js
const billNumberMatch = text.match(
  /Bill number:\s*(\d+)/i
);

const billNumber = billNumberMatch[1];
```

This finds the bill number after:

```text
Bill number:
```

The `\d+` part means one or more digits.

---

## 8. Extract Bill Period

```js
const billPeriodMatch = text.match(
  /Bill period:[\s\S]*?([A-Za-z]+\s+\d{1,2},\s+\d{4}\s+to\s+[A-Za-z]+\s+\d{1,2},\s+\d{4})/i
);

const billPeriod = billPeriodMatch[1].trim();
```

This finds the bill period after the label:

```text
Bill period:
```

It captures a date range such as:

```text
May 31, 2020 to Jul 30, 2020
```

`[\s\S]*?` is used because the bill period may appear on a later line, and this allows the regex to search across line breaks.

---

## 9. Extract Total New Charges

```js
const totalChargeMatch = text.match(
  /Total new charges\s+\$([\d,]+\.\d{2})/i
);

const totalNewCharge = totalChargeMatch[1];
```

This finds the amount beside:

```text
Total new charges
```

It captures values like:

```text
2,760.84
```

The dollar sign is added back when printing the result.

---

## 10. Print the Results

```js
console.log("Customer Number: " + customerNum);
console.log("Account Number: " + accountNum);
console.log("Bill Period: " + billPeriod);
console.log("Bill Number: " + billNumber);
console.log("Bill Date: " + billDate);
console.log("Total New Charges: $" + totalNewCharge);
```

This outputs the extracted information in a clear format.
