# Money Manager Pro

<img width="1147" height="481" alt="image" src="https://github.com/user-attachments/assets/4a1ab692-5488-4fab-aa5b-baf5af23c0aa" />

Combine your investments and savings accounts and keep track of your overall wealth.

Power BI tool to track investments performace -> [📊 Live Demo](https://app.powerbi.com/view?r=eyJrIjoiZGFlOWQ4MDQtZjhhYi00ZWE5LTg1ZDItYmJiYzcyODdkNGViIiwidCI6IjhhNDk1ZGQwLThkNDEtNDcyYy1iMTljLTFhMzQzZjYxYmFhMSIsImMiOjl9)

# Why this tool? 

**The need:** Some time ago, I began investing in a platform using a Euro account. However, the platform does not display the figures in the desired format. It lacks several essential data points that I believe are crucial for a comprehensive understanding of the investment's performance. Additionally, I have a separate savings account denominated in Polish Zloty. It would be convenient to have an easy overview of both accounts combined without having to manually perform calculations.

**Solution:** To address these challenges, I developed a Power Bi tool called "Money Manager Pro." This tool provides a visually explanatory representation of the performance of my investment portfolio in Euro. Furthermore, it allows me to combine the investment and savings accounts, which are denominated in different currencies, to provide a comprehensive overview of my total wealth in Euro.

### Please note: The figures presented in this dashboard are arbitrary and are provided solely as examples. They do not represent the actual value of my investments or the real investments I have made. The purpose of showcasing this tool is purely demonstrative.

# Tool Development Process

First, I created a Google Sheet with multiple tabs where I can input, comfortably using Google Sheets app on my phone, the following pieces of information:

1. Details of financial product purchases (date, quantity, purchase price) for each new transaction.
2. Expenses related to financial operations (bank commissions, exchange rate fees, etc.) whenever they occur.
3. The amount in Polish Zloty of my savings, updating it whenever there are changes.

I opted to use Google Sheets due to its robust built-in tools for retrieving real-time financial data. Additionally, as it is cloud-based, it enables me to schedule data refreshes in Power BI without requiring a gateway.

Next, once the underlying data setup was complete, I imported all the tables from Google Sheets into Power BI desktop. I then proceeded to create the data model, define measures, and develop the interactive tool.

Later, I published the tool to the Power BI service *(Picture 1)*, where it resides and automatically refreshes approximately every hour.

Finally, I developed a Metrics Scorecard in Power Bi Service *(Picture 2)* with the main metrics I am interested in, so that I can easily check them out with my Phone *(Picture 3)*, receiving also notifications related to their variations.

> <img width="828" height="358" alt="image" src="https://github.com/user-attachments/assets/5526f2d0-fd0f-4ee1-8c79-5ec151329b5b" />
*Picture 1: extract of the desktop version of the tool*

> <img width="841" height="375" alt="image" src="https://github.com/user-attachments/assets/ec815862-1a02-4032-9c22-dac7ad87df96" />
*Picture 2: extract of the desktop version of the Metrics Scorecard*

> <img width="844" height="358" alt="image" src="https://github.com/user-attachments/assets/5136722f-0a5d-4acc-8b7c-f25525e3ac54" />
*Picture 3: extract of the mobile version of the Metrics Scorecard* 

# Development deep-dive 1: Google Sheets Automations

As mentioned earlier, Google Sheets is where I input the details of my financial product movements, economic expenses, and changes in savings.

However, in order to obtain the real market value of my investments, it was necessary to automate the retrieval of real-time financial data. This ensures that whenever Power BI refreshes, it seamlessly incorporates the latest market data for investments and the most up-to-date exchange rates.

To achieve this, I primarily utilized the GOOGLEFINANCE function within Google Sheets. This function retrieves the current market price for a financial product and the latest exchange rates. For example, to retrieve the market price for a stock, I used the following formula:

`=GOOGLEFINANCE("TICKER")`

In this formula, "TICKER" represents the stock symbol for which I wanted to obtain the market price.

The GOOGLEFINANCE function automatically refreshes itself whenever the stock exchange provides updated data. This ensures that I always have access to the most recent and accurate market information for my investment analysis.

However, there were certain cases in which the GOOGLEFINANCE function was unable to retrieve the market value for specific financial products. In such situations, Google Sheets demonstrated its versatility by providing a solution that combines the IMPORTHTML function with Google Scripts. This combination allows me to extract data from a designated web page, typically the Stock Exchange webpage where the relevant financial products are listed.

The IMPORTHTML function in Google Sheets retrieves specific data from HTML tables on web pages. To illustrate, the following formula retrieves data from a table with a specific URL and table index:

`=IMPORTHTML("URL";"TABLE"; INDEX)`

Here, "URL" represents the web page address, "table" specifies the table number or CSS selector, and INDEX denotes the position of the desired data within the table.

While the IMPORTHTML function itself is static and does not automatically refresh in the background, I leveraged the capabilities of Google Scripts to automate the refresh process. Below is an example of a custom Google Script that can be used to refresh the IMPORTHTML function at regular intervals:

```
function getData() 
{
  var queryString = Utilities.formatDate(new Date(), "GMT", "yyyMMddHHmmss");
  var cellFunction = '=IMPORTHTML("URL' + queryString + '";"table"; index)';

  SpreadsheetApp.getActiveSpreadsheet().getSheetByName("SheetName").getRange('A1').setValue(cellFunction);
}
```

In this example, the getData function generates a timestamp, constructs a formula to import an HTML table from a URL that includes the timestamp, and then sets the value of cell A1 in the specified sheet of the active spreadsheet to the constructed formula. This allows the script to fetch and display the HTML table data in the selected sheet.

To activate the refresh automation, I then set up the  Google Script (getData) as a triggered function within Google Sheets. The "Triggers" option is in the Google Sheets menu and allows specify the desired frequency for the script execution, such as every 5, 10, etc.. minutes. By utilizing this automated approach, I could ensure that the market value of some specific financial products remains consistently up-to-date within the Google Sheets environment.

# Development deep-dive 2: small trick for developing the Metrics Scorecard.

When building the Metrics Scorecard, I encountered a challenge related to linking the metrics and their targets to the live report's visuals. Since I had developed several visuals using HTML and displayed them using a custom HTML reader, I found that it was not possible to directly link the KPIs to a specific value in the report. Additionally, I needed a slicer to select the value of a financial product, but unfortunately, I didn't have one available. So, what was the solution?

The workaround I implemented involved incorporating native cards or slicers into the report, which were solely used to establish the necessary links between the required numbers and the metrics in the scorecard. Once the linkage was established, I could simply hide these native cards or slicers in the report and then re-publish it to the Power BI service. This approach allowed the cards or slicers to work behind the scenes, ensuring that the scorecard remains constantly updated without compromising the overall user interface of the dashboard.
