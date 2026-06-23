Water Utility Bill Parser
Description

A Node.js command-line application that reads a utility bill text file and extracts:

Customer Number
Account Number
Bill Period
Bill Number
Bill Date
Total New Charges
Project Structure
project-folder/
│
├── parser.js
├── test-q1.txt
└── README.md
Requirements
Node.js

Verify installation:

node -v
Running the Program

Execute the parser using:

node parser.js test-q1.txt
Sample Output
Customer Number: 0523080
Account Number: 01403664
Bill Period: May 31, 2020 to Jul 30, 2020
Bill Number: 21478560
Bill Date: Aug 11, 2020
Total New Charges: $2,760.84
Implementation Details
Read Input File
const fs = require("fs");

const filePath = process.argv[2];

const text = fs.readFileSync(filePath, "utf8");

Reads the utility bill text file provided through the command line.

Extract Customer and Account Number
const accountMatch = text.match(
    /Customer no\. - Account no\.[\s\S]*?(\d+)\s*-\s*(\d+)/i
);

const customerNum = accountMatch[1];
const accountNum = accountMatch[2];

Example:

Customer no. - Account no.

0523080 - 01403664
Extract Bill Date
const billDateMatch = text.match(
    /Bill date:\s*(.+)/i
);

const billDate = billDateMatch[1].trim();

Example:

Bill date: Aug 11, 2020
Extract Bill Number
const billNumberMatch = text.match(
    /Bill number:\s*(\d+)/i
);

const billNumber = billNumberMatch[1];

Example:

Bill number: 21478560
Extract Bill Period
const billPeriodMatch = text.match(
    /Bill period:[\s\S]*?([A-Za-z]+\s+\d{1,2},\s+\d{4}\s+to\s+[A-Za-z]+\s+\d{1,2},\s+\d{4})/i
);

const billPeriod = billPeriodMatch[1].trim();

Example:

May 31, 2020 to Jul 30, 2020
Extract Total New Charges
const totalChargeMatch = text.match(
    /Total new charges\s+\$([\d,]+\.\d{2})/i
);

const totalNewCharge = totalChargeMatch[1];

Example:

Total new charges $2,760.84
Error Handling

The parser validates each field before attempting to use it.

Example:

if (!billDateMatch) {
    throw new Error("Bill date not found");
}

This prevents runtime errors and provides clear feedback when expected data is missing.

Design Decisions
Used Node.js built-in fs module for file reading.
Used regular expressions to identify and extract required fields.
Added validation checks to ensure required data exists before processing.
Anchored regex patterns around bill labels to reduce accidental matches.
Kept the implementation simple and readable for maintainability.
