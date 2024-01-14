# Power_BI_ Taiwan_Energy_Analysis

## Final result:

### First page:

![image](https://github.com/e19931107/Power_BI_-Taiwan_Energy_Analysis/assets/50692450/40907c66-d375-47e6-998f-a87e23d9f7d9)

### Second page:

![image](https://github.com/e19931107/Power_BI_-Taiwan_Energy_Analysis/assets/50692450/96449968-e8be-42cd-b5e2-8946fdeb840d)

## Motivation

1. Addressing the global challenge of energy shortages requires a comprehensive analysis of various energy sources such as solar panels and water pumps. By examining the weight and efficiency of each source, countries can make informed decisions to enhance their energy sustainability.
2. Taiwan heavily depends on importing crude oil and gas to meet its power needs, emphasizing the urgency of managing energy consumption. Evaluating and diversifying energy sources becomes crucial for the country's energy security and independence.
3. Recent incidents of power outages during overloaded generators in the summer highlight the need for a shift towards renewable energy. Exploring strategies to transition from conventional to sustainable sources will not only mitigate power-related crises but also contribute to a more resilient and eco-friendly energy infrastructure.

## Raw DATA ETL

Step1: Unpivot table

![image](https://github.com/e19931107/Power_BI-Taiwan_Energy_Analysis/assets/50692450/7f75585b-bb56-44c5-b92e-f6f00a12b6e9)

Step2: Define datetime format, transfer number into value format

![image](https://github.com/e19931107/Power_BI-Taiwan_Energy_Analysis/assets/50692450/76e34bcc-7e88-42da-b227-680c430bf677)

Step3: Make key to combine two datasets.

Because each of datasets they don't have key, they cannot combine(join).

Hoever, I found there are three columns that are common in both datasets and can help me combine.

Therefore, I used 'Type(En)', 'End of Month', '發電量總表sheet' these three columns to combine into key.

![image](https://github.com/e19931107/Power_BI_-Taiwan_Energy_Analysis/assets/50692450/fa4ce488-2acd-4369-95a4-1c0281f469a3)

Step4: After ETL data in power pivot, I make the connection between these two table in Power BI.

![image](https://github.com/e19931107/Power_BI_-Taiwan_Energy_Analysis/assets/50692450/390d538d-08e4-4348-bbcc-4b3524574a2e)

Step5: Translate the file from Chinese into English, split date into year, quarter, month.

Therefore, I create a new table: 發電類別

![image](https://github.com/e19931107/Power_BI_-Taiwan_Energy_Analysis/assets/50692450/9678c9d0-379f-468a-b1ea-9728a83caa92)

## DAX

%發電量 = DIVIDE(SUM('總表'[發電量_度]),CALCULATE(SUM('總表'[發電量_度]),ALLSELECTED()))
%裝置容量 = DIVIDE(SUM('總表'[裝置容量_百萬瓦]),CALCULATE(SUM('總表'[裝置容量_百萬瓦]),ALLSELECTED()))

Data Range = "Data Range: " &UNICHAR(10)& FORMAT('總表'[StartDate],"yyyy mmm") & " to " & FORMAT('總表'[EndDate],"yyyy mmm")

EndDate = CALCULATE(MAX('總表'[Date]),ALL())
StartDate = CALCULATE(MIN('總表'[Date]),ALL())

Page 1 Title = SELECTEDVALUE('總表'[Year],"2016-2023")&
" Taiwan Electricity situation "&
"("&
SELECTEDVALUE('總表'[Type(En)],"All power")
&")"&" ("&
SELECTEDVALUE('總表'[Sheet],"All methods")&")"


