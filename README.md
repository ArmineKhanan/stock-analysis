# Stock Analysis
## Overview of the Project
Steve's parents are passionate about green energy and are eager to invest all their money into DAQO New Energy Corporation. Yet, they decided to seek advice from Steve, who graduated with his finance degree not long ago. The latter, in his turn, applied to us for assistance in analysis. We solved the problem using an extension to Excel, built to automate tasks: Visual Basic for Applications, usually referred to as VBA.

Steve is happy. But to do a little more research for his parents, he wants to expand the dataset to include the entire stock market over the last few years. Although our code works well for a dozen stocks, it might take a long time to execute for thousands of them.

In this challenge, we will edit and refactor our initial solution code. Afterward, we plan to determine whether refactoring our code made the VBA script run faster.

## Results
### Script Routine Refactoring
Our purpose is to edit the code so Steve can get the same output faster. In search of an effective scenario where we can go through all the data one time, we concluded to get rid of the nested loop. As shown in the code snippet below, the initial code loops through the entire dataset i times for each of 12 tickers:

```
'Loop through the tickers
    For i = 0 To 11
            'Loop through rows in the data
            For j = 2 To RowCount
                ***
                ***
                End If
            Next j

    Next i
```

We decided to leverage the fact that tickers are sorted and have the same sequence in the dataset as in our code. To throw out the nested solution we initialized the tickerIndex variable and increased its value by one whenever the ticker name changed.

```
'Initialize tickerIndex
    Dim tickerIndex As Integer
    tickerIndex = 0
        
    'Loop over all the rows in the spreadsheet.
        For i = 2 To RowCount
            ***
            ***
            'Increase the tickerIndex.
            If Cells(i, 1).Value <> Cells(i + 1, 1).Value Then
                tickerIndex = tickerIndex + 1
            End If
        Next i
```

Afterward, the loop goes across previously defined arrays to pull the name, daily volume, and return for each ticker.

```
For i = 0 To 11
        
        Worksheets("All Stocks Analysis").Activate
        Cells(7 + i, 2).Value = tickers(i)
        Cells(7 + i, 3).Value = tickerVolumes(i)
        Cells(7 + i, 4).Value = (tickerEndingPrices(i) / tickerStartingPrices(i)) - 1
```
The two subroutines output identical reports. Yet, the second one works faster.

### VBA Code Execution Record
To estimate our efforts, we wrapped the subroutine with the following script:
```
Sub AllStocksAnalysisRefactored()
    Dim startTime As Single
    Dim endTime  As Single
    yearValue = InputBox("What year would you like to run the analysis on?")
    startTime = Timer
        ***
        ***
        ***
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub
```
The screenshots below prove that the refactored script is almost 2.5 faster on average.
For 2017:

<img src="https://github.com/ArmineKhanan/stock-analysis/blob/main/ASA%20Runtime%20for%202017.png" width="400" /> <img src="https://github.com/ArmineKhanan/stock-analysis/blob/main/ASA%20Runtime%20for%202017%20if%20refuctored.png" width="400" />

For 2018:

<img src="https://github.com/ArmineKhanan/stock-analysis/blob/main/ASA%20Runtime%20for%202018.png" width="400" /> <img src="https://github.com/ArmineKhanan/stock-analysis/blob/main/ASA%20Runtime%20for%202018%20if%20refactored.png" width="400" />

## Summary
Code refactoring may be time-consuming and even a risky endeavor. For example, if one does not have an entire understanding of the code, they better refrain from refactoring. Though for code maintenance, its readability and effectiveness refactoring can be fruitful. 
In our case, code refactoring resulted in speedy performance. Being aware of Steve's plans to broaden the analysis, we believe our efforts will be even more rewarding in the future. 
Yet, the refactored script has a limitation as compared to the initial. It will work effectively if only the ticker names have the same sequence in the data source as in the script. That's why we better document VBA functionality before sharing it with Steve.

