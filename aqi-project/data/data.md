Source : https://www.kaggle.com/datasets/tushardobal/india-city-air-quality-index-dataset-20152023

For Data Clean Up :
    Excel
    =FILTER(Sheet1!A2:J32873, ISNA(XMATCH(Sheet1!A2:A32873&Sheet1!B2:B32873, Sheet2!A2:A1357&Sheet2!B2:B1357)), "No Records Found")
    Detailed Breakdown of the Formula
    1. Creating a Unique Key (& Operator)
    Sheet1!A2:A32873 & Sheet1!B2:B32873
    Because one column (like "City") isn't enough to identify a unique row, we use the ampersand (&) to join two columns together. For example, "Delhi" in Column A and "2015-01-01" in Column B becomes a single text string: "Delhi2015-01-01".
    We do this for both the Master list (Sheet 1) and the list we want to remove (Sheet 2).

    2. The Search (XMATCH)
    XMATCH(Lookup_Value, Lookup_Array)
    This part tells Excel: "Take every unique key from Sheet 1 and try to find it in the list of keys from Sheet 2."
    If it finds a match, it returns the position (a number).
    If it doesn't find a match, it returns an error: #N/A.
    3. Identifying the Differences (ISNA)
    ISNA(...)
    Since your goal is to remove the contents of Sheet 2, we actually want the rows that failed to find a match.
    ISNA turns the #N/A results into TRUE and the successful matches into FALSE.

    4. Extracting the Data (FILTER)
    FILTER(Array, Include_Criteria)
    Finally, the FILTER function looks at the entire Master table and only keeps the rows where our ISNA test resulted in TRUE (meaning the record was unique to Sheet 1).

    Array Processing: XMATCH in this format handles thousands of rows at once without needing to drag the formula down.
    Multiple Criteria: It allows you to match across multiple columns (City AND Date) without needing to create extra "Helper" columns in your spreadsheet.

    Speed: It is significantly faster than traditional VLOOKUP or INDEX/MATCH when dealing with datasets over 30,000 rows.