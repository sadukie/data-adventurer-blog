---
title: Data Inferencing in Loading Data
tags: microsoft-data-analysis data-handling
---

Yesterday, while tinkering with `Microsoft.Data.Analysis` and exploring their GitHub repo, I saw [an issue where a seemingly correct CSV file wasn’t loading](https://github.com/dotnet/machinelearning/issues/5884). Having worked with data tools and with programming languages most of my career, my initial suspicion was **data inferencing**. So let’s talk about how I got there and why.

## The Questioned Data

The issue mentions [the Titanic data from Kaggle](https://www.kaggle.com/c/titanic/data?select=test.csv). Titanic data is common for many exercises in working with data, so it’s a familiar data source for me. I logged into Kaggle and downloaded test.csv to see what’s going on.

Looking at the file, loading the CSV should have been straightforward. There didn’t seem to be anything extremely out of the ordinary.

## The Error Message

This was the stack trace that came with trying to load test.csv in a `Microsoft.Data.Analysis.DataFrame`:

```
System.FormatException: Input string was not in a correct format. at System.Number.ThrowOverflowOrFormatException(ParsingStatus status, TypeCode type) at System.String.System.IConvertible.ToSingle(IFormatProvider provider) at System.Convert.ChangeType(Object value, Type conversionType, IFormatProvider provider) at Microsoft.Data.Analysis.DataFrame.Append(IEnumerable1 row, Boolean inPlace)
at Microsoft.Data.Analysis.DataFrame.ReadCsvLinesIntoDataFrame(WrappedStreamReaderOrStringReader wrappedReader, Char separator, Boolean header, String[] columnNames, Type[] dataTypes, Int64 numberOfRowsToRead, Int32 guessRows, Boolean addIndexColumn)
at Microsoft.Data.Analysis.DataFrame.LoadCsv(String filename, Char separator, Boolean header, String[] columnNames, Type[] dataTypes, Int32 numRows, Int32 guessRows, Boolean addIndexColumn, Encoding encoding)
at Submission#58.<>d__0.MoveNext()
--- End of stack trace from previous location ---
at Microsoft.CodeAnalysis.Scripting.ScriptExecutionState.RunSubmissionsAsync[TResult](ImmutableArray1 precedingExecutors, Func2 currentExecutor, StrongBox1 exceptionHolderOpt, Func2 catchExceptionOpt, CancellationToken cancellationToken)
at System.Number.ThrowOverflowOrFormatException(ParsingStatus status, TypeCode type)
at System.String.System.IConvertible.ToSingle(IFormatProvider provider)
at System.Convert.ChangeType(Object value, Type conversionType, IFormatProvider provider)
at Microsoft.Data.Analysis.DataFrame.Append(IEnumerable1 row, Boolean inPlace) at Microsoft.Data.Analysis.DataFrame.ReadCsvLinesIntoDataFrame(WrappedStreamReaderOrStringReader wrappedReader, Char separator, Boolean header, String[] columnNames, Type[] dataTypes, Int64 numberOfRowsToRead, Int32 guessRows, Boolean addIndexColumn) at Microsoft.Data.Analysis.DataFrame.LoadCsv(String filename, Char separator, Boolean header, String[] columnNames, Type[] dataTypes, Int32 numRows, Int32 guessRows, Boolean addIndexColumn, Encoding encoding) at Submission#58.<<Initialize>>d__0.MoveNext() --- End of stack trace from previous location --- at Microsoft.CodeAnalysis.Scripting.ScriptExecutionState.RunSubmissionsAsync[TResult](ImmutableArray1 precedingExecutors, Func2 currentExecutor, StrongBox1 exceptionHolderOpt, Func2 catchExceptionOpt, CancellationToken cancellationToken)
```

The developer in me looked at it and knew by the `System.FormatExeception` that something was unhappy with the typing. Then I noticed `System.String.System.IConvertible.ToSingle` in the error message. Something indeed was unhappy with the typing – specifically trying to convert some data into a `System.Single` data type. This tells me that something is trying to be converted into an integer value that isn’t an integer value.

## Type Inference

When data tools are guessing the data types for an import, they are **inferring** the type, based on a sample set of data. When working with `pandas`, we can use the `.infer_objects()` on a `DataFrame` to infer the types. Looking at [the documentation for pandas.DataFrame.infer_objects](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.infer_objects.html), it isn’t clear how big of a sample set they use for inferring the type for a column.

With `Microsoft.Data.Analysis`, I did some digging and found [the LoadCsv documentation](https://docs.microsoft.com/en-us/dotnet/api/microsoft.data.analysis.dataframe.loadcsv?view=ml-dotnet-preview#Microsoft_Data_Analysis_DataFrame_LoadCsv_System_String_System_Char_System_Boolean_System_String___System_Type___System_Int32_System_Int32_System_Boolean_System_Text_Encoding_). When calling `LoadCsv()` from a `Microsoft.Data.Analysis.DataFrame`, you can pass in a value for guessRows to set the sampling size for inferring your column types. The default value is 10 rows.

## Type Inferencing with the Titanic Data

Looking at the Titanic data and the reference to the `System.Single` data type, I looked carefully at the columns and suspected it was the Ticket column. I tested this suspicion by putting a string in for the first value and calling `LoadCsv()`, which then loaded the data without error.

I then counted out the rows to see where the type inferencing would have been fine on its own. Setting the `guessRows` to 11 would have worked fine for this dataset.

## Conclusion

When loading data and seeing that type conversion is failing, it is safe to suspect that type inferencing is in play. Searching for your data tool and “data inference” or “inferring data types” or “inference sample size” should help in getting some guidance on the sample size and whether it is defaulted and able to be overridden.