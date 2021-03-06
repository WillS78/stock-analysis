# stock-analysis
Model 2 VBA - stock-analysis
## Overview of Project

The objective of this project is to help Steve evaluate both his parents stock of choice DQ and the other stock investment options. We are looking at if his parents are winning or losing with their narrow approach of only investing in DQ and no other stock options. We’re building a model that will allow Steve to evaluate the different stocks over the past two years. This will give him the information he needs to have a financial conversation with his parents.

### Purpose and background

Steve’s parents met years ago while working at Dairy Queen. They believe that since they’ve had a successful life together that a stock called DQ must have the same luck. Steve is concerned for his parents financial future as they’re putting all their retirement planning into one stock. Steve wants your help to evaluate how well DQ has performed during the past two years. He also wants to how other stocks have performed so he can review the data with his parents to help educate them on their finical decisions. The end goal is to make sure his parents are continuing to grow their wealth and not losing money. 

## Results

DQ stock did well in 2017 with a 199.4% return on investment but it fell of during 2018 coming in at a loss on -62.6% for the year. If you had bought stock in early 2017 and kept through 2018 you would still be up 136%. There were two stocks that had favorable returns in both 2017 & 2018. ENPH had a positive return of 129.5% in 2017 & 81.9% return in 2018. The second stock that had year over year returns was RUN with 5.5% in 2017 & 84% in 2018. Steve’s parents should split their investments between DQ, ENPH, RUN & SEDG. Over the past two years these four stock have a good run rate.

### Analysis
![VBA_Challenge_2017](https://user-images.githubusercontent.com/101777677/161446487-a5e52b33-6cbe-44c2-bdaa-d580e75460a8.JPG)
![VBA_Challenge_2018](https://user-images.githubusercontent.com/101777677/161446500-8ac9ae85-5379-4104-b2ad-daa0ed80389b.JPG)

Sub AllStocksAnalysisRefactored()
    Dim startTime As Single
    Dim endTime  As Single

    yearValue = InputBox("What year would you like to run the analysis on?")

    startTime = Timer
    
    'Format the output sheet on All Stocks Analysis worksheet
    Worksheets("All Stocks Analysis").Activate
    
    Range("A1").Value = "All Stocks (" + yearValue + ")"
    
    'Create a header row
    Cells(3, 1).Value = "Ticker"
    Cells(3, 2).Value = "Total Daily Volume"
    Cells(3, 3).Value = "Return"

    'Initialize array of all tickers
    Dim tickers(12) As String
    
    tickers(0) = "AY"
    tickers(1) = "CSIQ"
    tickers(2) = "DQ"
    tickers(3) = "ENPH"
    tickers(4) = "FSLR"
    tickers(5) = "HASI"
    tickers(6) = "JKS"
    tickers(7) = "RUN"
    tickers(8) = "SEDG"
    tickers(9) = "SPWR"
    tickers(10) = "TERP"
    tickers(11) = "VSLR"
    
    'Activate data worksheet
    Worksheets(yearValue).Activate
    
    'Get the number of rows to loop over
    RowCount = Cells(Rows.Count, "A").End(xlUp).Row
    
    '1a) Create a ticker Index
    tickerIndex = 0
    
    '1b) Create three output arrays
    Dim tickerVolumes(12) As Long
    Dim tickerStartingPrices(12) As Single
    Dim tickerEndingPrices(12) As Single
        
    ''2a) Create a for loop to initialize the tickerVolumes to zero.
    For i = 0 To 11
        tickerVolumes(i) = 0
        tickerStartingPrices(i) = 0
        tickerEndingPrices(i) = 0
    
    Next i
        
    ''2b) Loop over all the rows in the spreadsheet.
    For i = 2 To RowCount
    
        '3a) Increase volume for current ticker
        tickerVolumes(tickerIndex) = tickerVolumes(tickerIndex) + Cells(i, 8).Value
                
        '3b) Check if the current row is the first row with the selected tickerIndex.
        'If  Then
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i - 1, 1).Value <> tickers(tickerIndex) Then
            tickerStartingPrices(tickerIndex) = Cells(i, 6).Value
        End If
        
        
        '3c) check if the current row is the last row with the selected ticker
         'If the next row's ticker doesn't match, increase the tickerIndex.
        'If  Then
        If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
            tickerEndingPrices(tickerIndex) = Cells(i, 6).Value
        End If
            

            '3d Increase the tickerIndex.
            If Cells(i, 1).Value = tickers(tickerIndex) And Cells(i + 1, 1).Value <> tickers(tickerIndex) Then
                tickerIndex = tickerIndex + 1
            End If
                
    Next i
    
    '4) Loop through your arrays to output the Ticker, Total Daily Volume, and Return.
    For i = 0 To 11
        
        Worksheets("All Stocks Analysis").Activate
        Cells(4 + i, 1).Value = tickers(i)
        Cells(4 + i, 2).Value = tickerVolumes(i)
        Cells(4 + i, 3).Value = tickerEndingPrices(i) / tickerStartingPrices(i) - 1
        
    Next i
    
    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "0.0%"
    Columns("B").AutoFit

    dataRowStart = 4
    dataRowEnd = 15

    For i = dataRowStart To dataRowEnd
        
        If Cells(i, 3) > 0 Then
            
            Cells(i, 3).Interior.Color = vbGreen
            
        Else
        
            Cells(i, 3).Interior.Color = vbRed
            
        End If
        
    Next i
 
    endTime = Timer
    MsgBox "This code ran in " & (endTime - startTime) & " seconds for the year " & (yearValue)

End Sub
Sub formatALLStocksAnalysisTable()

    'Formatting
    Worksheets("All Stocks Analysis").Activate
    Range("A3:C3").Font.FontStyle = "Bold"
    Range("A3:C3").Borders(xlEdgeBottom).LineStyle = xlContinuous
    Range("B4:B15").NumberFormat = "#,##0"
    Range("C4:C15").NumberFormat = "$0.00%"
    Columns("B").AutoFit
      
    'Change cells color based on results
    If Cells(4, 3) > 0 Then
        'Color the cell green
        Cells(4, 3).Interior.Color = vbGreen
    ElseIf Cells(4, 3) < 0 Then
    
        'Color the cell red
        Cells(4, 3).Interior.Color = vbRed
    Else
        'Clear the cell color
        Cells(4, 3).Interior.Color = xlNone
    
    End If
    
    dataRowStart = 4
    dataRowEnd = 15
    For i = dataRowStart To dataRowEnd
    
        If Cells(i, 3) > 0 Then
        
            'Change cell color to green
            Cells(i, 3).Interior.Color = vbGreen
        
        ElseIf Cells(i, 3) < 0 Then
        
            'Change cell color to red
            Cells(i, 3).Interior.Color = vbRed
        Else
        
            'Clear the cell color
            Cells(i, 3).Interior.Color = xlNone
            
        End If
            
    Next i
    
End Sub

Sub ClearWorkSheet()

    Cells.Clear
    
End Sub

Sub yearValueAnaylsis()

    yearValue = InputBox("What year would you like to run the analysis on?")
  
End Sub

Sub FormatingDQAnalysis()

    'Formating
    Worksheets("DQ analysis").Activate
    Cells(4, 2).NumberFormat = "$#,##0"
    Cells(4, 3).NumberFormat = "0.00%"
    
End Sub

## Summary
-	Advantages and Disadvantages of refactoring code in general

A few of the advantages of the refactored code or just refactoring code in general is that you can scale it back to reduce the number of steps it uses to generate the results. This speeds up your reporting which will increase your end users productivity and reduce any potential frustration waiting on the code to run. It can also be easier for others who view your code to understand what your logic is and how it works. A disadvantage is you could scale it back to much where it doesn’t factor every possible in some cases.

-	Advantages and Disadvantages of the original and refactored VBA script

A disadvantage to the original script is it included steps that didn’t need to be in there so it caused the report to run longer. For example before refactoring the code it took on average 1.2 to 1.4 seconds to gather the results. After refactoring it averaged 0.3 seconds which is about 77% faster. In business time is money so if you can reduce your teams time waiting on reports to run that is a positive impact to the bottom line educing wasted labor. 
