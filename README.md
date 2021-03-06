# uscisStats

This project is intent for immigrants to check adjacent receipt number status. It uses USCIS webpage http request to fetch the entire webpage based on a receipt number, then parse the case status filtered by case type, and plot adjacent case status. It also store and compare the result of this execution and last execution result, to report the differences - as the USCIS's progress within these execution.

## Disclaimer
For the good of all immigrants, please use it only for personal reference, and respect the confidentiality of status information for other people's cases. Thank you.


## Prerequisite
1. Python3 is required
2. Installing Jupyter Interface using Anaconda and conda following the instruction https://jupyter.readthedocs.io/en/latest/install/notebook-classic.html

## How To Use
Change the constant based on your need in `concurrent_version.ipynb`

Run directly in Jupyter Note Book, or copy them to python3. 

Please run `concurrent_version.ipynb` for crawling certain range of cases. it will create a `today.json` as a result of initial data. 

Afterwards, you only need to run the `compare_and_statistic.ipynb` to go over the cases saved in the initial data. It will create an copy of `today.json` called `yday.json`, then create a new `today.json` as of the updated result. The report of differences and statistic between `today.json` and `yday.json` will be printed. 

### Constant
```
RECEIPT_NUMBER_BASE: Your receipt number without the last N digits
RANGE_START: A smaller N digits integer as the end of receipt number to check start with (cannot be started from 0)
RANGE_END: A larger N digits integer as the end of receipt number to check end with (cannot be started from 0)
CASE_TYPE: Type of Form to filter, put 'Form' to show all types of forms
CONCURRENCY_LEVEL: Controls how many requests will be maded concurrently

```
Example
```
IF you are looking for an interval contains recepit number WAC2090123456 for form I765

you can break down like this 

RECEIPT_NUMBER_BASE: WAC2090123
RANGE_START: 400
RANGE_END: 500
CASE_TYPE: I765
CONCURRENCY_LEVEL: 11
```

Please note that adjacent receipt number status may only be informative if you are filling with paper base, since each center is processing with different speed. And this information can be used as a reference of how soon will your case being processed, based on the consumptions that
1) Receipt number is increment based on receipt date, and 
2) Each USCIS center is processing by the receipt date

**NOTE: `CONCURRENCY_LEVEL` has been tuned to the proper value - 11. Increasing it will trigger USCIS's throttling, resulting in some requests fail.**


## Contact Me
For any further questions, or if you need help to run you receipt for you, please contact sunnyliyanbo@gmail.com or jino.yolo@gmail.com

## FAQ

### Q: Why it doesn't work any more?
This program fetches case status in html format and a line number of where to find case status was hard-coded. Hard coding is intended, see the next question for more details. If USCIS changed their format of html response, the hard-coded line number will not work anymore. Please contact jino.yolo@gmail.com for assistance.

### Q: Why hard-coded line number?
Again, this approach is intended as a trade-off. I've compared the performance of these approaches:
- splitting the line and hard-coded line number
- regex 
- and html decoding,

The splitting line+hard coded line number out-performs the others a lot. Given the returned html is huge and we are brutally loop through thousands of cases, cumulatively the hard-coded one is way more faster and save you tons of running time.
