  # Olist Store 
## (Brazilian Ecommerce Analysis -EDA and Data Visualization)

In this project, I analyzed a real-world dataset of 100,000 orders from the Olist platform to enhance my data analysis skills. I began by thoroughly understanding and cleaning the data to ensure accuracy, followed by applying various analytical techniques to uncover insights and  provide actionable recommendations using Power BI Tool.

![image](https://github.com/user-attachments/assets/3fab5e5d-4324-4d3f-972d-77640154b11a)

### Purpose of this Project

The purpose of this  Ecommerce sales analysis project is to leverage real-world data from olist to conduct thorough examination of sales performance. By applying data analysis and visualization techniques, the project aims to uncover key insights and trends. These findings will provide valuable recommendations for business stakeholders, enabling them to make informed, data-driven decisions that enhance profitability and operational efficiency.

### Introduction

Olist, a prominent figure in the Brazilian marketplace scene, stands as the largest department store connecting small businesses nationwide to various sales channels effortlessly through a unified contract. By leveraging Olist Store, merchants can effectively market their products and utilize Olist's robust logistics network to directly fulfill customer orders.
The dataset was made available to the public. offering Data Analysts a valuable opportunity to delve into Brazil's dynamic e-commerce environment, uncover insights, and pinpoint avenues for enhanced growth and efficiency.

### Dataset Used
The raw data was sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data)

### Business Need
Olist store aims to leverage data insights to optimize sales performance and enhance market visibility.

### Tools used
**Ms Excel** (To understand the data and some data cleaning)<br>
**Power BI**<br>
      Power Query – ETL process<br>
      Dax Expessions – Data Analysis<br>
      Power pivot- data modelling<br>
      Power view – data Visualization

### Process Involved
1) Data Cleaning<br>
2) Data Modelling<br>
3) Defining KPI and EDA (using Dax)<br>
4) Creating Dashboard- Data Visualization<br>
5) Providing Insights and Recommendation

### Data Cleaning
  **Dataset**- There are approximately 9 tables in the  dataset containing 100k  orders.
Following tables are imported into excel to understand the data 

  ![image](https://github.com/user-attachments/assets/3008c7b0-39d8-4755-9ac7-c7bbc368cfae)

**Data Cleaning Using Excel**

  I have used below steps which was easy to perform  in excel than in Power BI to large set of data for the operation like find and replace , 
     spell check and langauage translation.

  1. Removing Unwanted Columns to save the space
     
      * From Olist_order_reviews_dataset - removed review_creation_date and review_answer_timestamp.
        
      * From olist_products_dataset- removed product_name_length, product_description_length, product_photos_qty, product_length_cm, product_height_cm, product_width_cm
  
  2. Translating  Portuguese to English 

      * In olist_products_dataset table product_category name contains  values pc_gamer, portateis_cozinha_e_preparadores_de_alimentos which is not translated in product_category_name_translation table. So I used excel translator tool to update those values<br>
      *	In olist_order_reviews_dataset table columns review_comment_title and review_comment_message values are translated to English using excel translator located in review tab.

  3. Fixing spelling errors
     
      To reduce ambiguity of strings in city columns used find and replace to correct those values.

**Using Power Bi**

ETL process

Extracted  data into the Power BI through menu get data option text/csv and transformed data through Power Query Editor and loaded for the further analysis process to work with data.

Transformation process to clean data are as follows

1. Used simple transformation operations like datatype correction, use first row as headers  removing duplicate data and empty rows wherever required.
2. Updating Currency Columns values with $ sign.
3. Merged table products and product_category_translation on the column product_category_name_english .

### Data Modelling

*	Cleaned data contains 8 tables where order_items is a fact table which contains quantitative data and foreign keys that relate to primary keys of dimension table.
*	 Orders is a factless fact table which does not contain measurable data but do contains date of events occured and acts as a bridge between fact & dimension table.
*	There are 6 dimensional table which contains primary keys and other descriptive additional fields.
*	The relations between the tables are one to many and many to many relationships.
*	So the model is STAR schema as given below
  

  ![image](https://github.com/user-attachments/assets/bfb3a364-7074-4bd9-9fd0-09a27c2bfe4d)

### Exploratory Data Analysis 


Following are the business questions
1.	Which Top 7 states generates more revenue?
2.	how total revenue generated has changed over the  quarters? 
3.	What is the Sales trends for Successful Deliveries?
4.	Does the highest selling products have high sales volume?
5.	What is the distribution of revenue by payment type?
6.	What is the weekday weekend sales Distribution?
7.	Which are the top 5 bussiest months?
8.	how is the quarterly purchase trend for weekdays vs weekend?	
9.	what is the purchase trend for days vs average sales?
10.	Which product category has highest freight value wrt product weight?
11.	What are the peak hours of orders made?
12.	What is the order of late delivery by state?
13.	Rate of successful product deliveries?
14.	What is the status of undelivered orders?
15.	How delayed deliveries impacting Customer Rating?
16.	what is the average delivery time taken to deliver products
17.	What is the rate of Revisiting Customers?
18.	How Satisfied are Customers?
19.	What is the CSAT?
20.	Which are the high demand product categories?
21.	How Customer ratings are distributed over months?
22.	What is the reason for Cancellation?
    

**Calculated Columns**

Created some new columns in a table based on existing data using dax functions to provide immediate access to crucial matrics without needing it to compute repeatedly.

  Following are the columns created in the orders table to use while analyzing data.

  * Day and Daytype column to check which day orders were made by customers.
    
        day = FORMAT(olist_orders_dataset[order_purchase_timestamp].[Date],"ddd")
        daytype = if(LEFT(olist_orders_dataset[day],1)="S","Weekend","Weekday")



calculated column output

**Sunday	Weekend**<br>
Monday	Weekday<br>
Tuesday	Weekday<br>
Wednesday	Weekday<br>
Thursday	Weekday<br>
Friday	Weekday<br>
Saturday	Weekend

  * Time and hour
  
        time = FORMAT(olist_orders_dataset[order_purchase_timestamp],"H AM/PM")
    
        hour = HOUR(olist_orders_dataset[order_purchase_timestamp])
    
  * Delivery time taken(difference of days from the day of approval to successful delivery), Delivery_Difference(time taken by delivery agent to 
  deliver orders), est_Delivery_Difference(to check on time and late deliveries)
  
        Delivery_timetaken = DATEDIFF(olist_orders_dataset[order_approved_at].[Date],olist_orders_dataset[order_delivered_customer_date],DAY)
    
        Delivery_Difference =                   
        DATEDIFF(olist_orders_dataset[order_delivered_carrier_date],olist_orders_dataset[order_delivered_customer_date],DAY)
    
        est_Delivery_Difference 
        =DATEDIFF(olist_orders_dataset[order_delivered_customer_date],olist_orders_dataset[order_estimated_delivery_date],DAY)

**Measures**

Created below measures for dynamic calculation typically used in the context of dashboards

    percent of sales = DIVIDE(CALCULATE(SUM(olist_order_payments_dataset[payment_value]),
    TOPN(7,GROUPBY(olist_customers_dataset,olist_customers_dataset[customer_state]),
    CALCULATE(SUM(olist_order_payments_dataset[payment_value])))),SUM(olist_order_payments_dataset[payment_value]))
    
    CSAT = (olist_order_reviews_dataset[happy_cust1]+olist_order_reviews_dataset[happy_cust2])/COUNT(olist_order_reviews_dataset[review_score])
    
    happy_cust1 = COUNTROWS(FILTER(olist_order_reviews_dataset,olist_order_reviews_dataset[review_score]=5))
    
    happy_cust2 = COUNTROWS(FILTER(olist_order_reviews_dataset,olist_order_reviews_dataset[review_score]=4))
    Order Fulfillment Rate = DIVIDE(
    COUNTROWS(FILTER(olist_orders_dataset,  olist_orders_dataset[order_delivered_customer_date] <=     
    olist_orders_dataset[order_estimated_delivery_date])),COUNTROWS(olist_orders_dataset))
    
	  Successful_delivery% = DIVIDE(COUNTROWS(FILTER(olist_orders_dataset,olist_orders_dataset[order_status] = "delivered")),
    COUNTROWS(olist_orders_dataset))
    
    top products sales = DIVIDE(CALCULATE(SUM(olist_order_payments_dataset[payment_value]),
    TOPN(7,GROUPBY(olist_products_dataset,olist_products_dataset[product_category_name_english]),
    CALCULATE(SUM(olist_order_payments_dataset[payment_value])))),SUM(olist_order_payments_dataset[payment_value]))



### Data visualization

Business questions are answered through following charts

**Sales Perfomance**
1. Which Top 7 states generates more revenue?

     ![image](https://github.com/user-attachments/assets/b14304e3-0dea-4f12-be88-4f47828a886d)

     Sao Paulo amongst the highest sales volume which alone generated $6M revenue including 6 other states Rio de Janerio, Minas Gerais, Rio Grand do sul, Parana, Santa Catarina, Bahia in Brazil are the Top 7 states contributing 81% to the total sales.

     Added Tooltip for the above visual to further analyze which cities among the states with the highest sales density are the top performers.

     ![image](https://github.com/user-attachments/assets/68f3c0c9-b3bf-4fe8-929f-db108694b541)

     Above Bar chart is a tooltip for the State Sau Paulo which shows highest sales for top 7 cities. And the   map shows distribution of   generated revenue across the states.

   

2. how total revenue generated has changed over the  quarters?

     ![image](https://github.com/user-attachments/assets/246354d0-63a2-4bb1-b968-bbd890c6b333)

     Trend of revenue from q1 to q2 is growing and we can see decline as it moves towards the remaining quarters.
     To understand this better I inserted clustered column chart to investigate why there is a high decline in revenue in the last quarter.

     ![image](https://github.com/user-attachments/assets/813aa4ed-897a-46d6-8266-60fc14daab2c)

     From the above chart we can see that for q4 only 2017 is performing good. There is no revenue for For the year 2018 q4 & 2016 q1 to q3  as  sales records are available from oct 2016 to august 2018. hence high decline during q4. For the year 2017 sales are growing for each passing quarter and for the year 2018 revenue shows growth in q1 & q2 but declines in q3. For the year 2016  revenue generated for q4 is $47,290. There is no revenue for For the year 2018 q4 & 2016 q1 to q3  as sales records are available from oct 2016 to august 2018.


3. What is the Sales trends for Successful Deliveries?

     ![image](https://github.com/user-attachments/assets/8bc5de6a-3ee8-460d-98cb-fe98c410e072)

     Above line chart shows revenue generated by delivered orders highly increased from 2016 to 2017 and slowly increased during 2017 to 2018   period.
   
     Used drill down hierarchy by month to see the revenue trend 

     ![image](https://github.com/user-attachments/assets/f477d684-7148-4995-a909-520d6422d047)


      Increase of revenue over time  but sharp decrease during September due to no data for the year 2016 & 2018 september and less data       
available during q4. May and August months have recorded highest growth  around $1.7 M and $1.6 M respectively.
   
      Further drilled down to check why there is sudden increase in sales during November as the data available are less during q4.

      ![image](https://github.com/user-attachments/assets/1a378ed8-85da-414d-92ff-44f05f109073)

      Sudden increase is on 24th November which can be seen in the above chart.<br>
      It’s a black Friday, so we can assume that due to high discounts and offers customers have placed more orders is the reason for increase  in revenue on that day.



4. Does the highest selling products have high sales volume?
     ![image](https://github.com/user-attachments/assets/1b673c6c-0f2b-4183-85ff-58eeecb46058)


     Bed bath table , health beauty and sports leisure are the top 3 selling product categories.
     Health beauty have the highest sales volume followed by watches and gifts, and the third is bath and beauty.
   
     From above its seen the highest selling product does not have highest sales volume.
   
     In 2016 topmost revenue product was furniture_decor, 2017 it was bed_bath_table and for 2018 it’s a health and beauty. 
     Health & beauty remains in the top 3 list for all the 3 years.

6. What is the distribution of revenue by payment type?

     ![image](https://github.com/user-attachments/assets/e66bba69-85c8-4e86-aa44-41c85abbf5bb)


     $12.54M of revenue has been generated by Customers using credit card and the debit card amongst the least used while purchasing products.

     ![image](https://github.com/user-attachments/assets/cbf2abe2-0686-425a-a322-711b33e9b637)


     Majority of customers payed within 1 month of installments by credit card. 

7. What is the weekday weekend sales Distribution?

     ![image](https://github.com/user-attachments/assets/57c2691e-6d86-4f2f-b91a-f3c5cad17718)

   
      Weekday sales have generated more revenue than the weekend.

**Order Analysis**

8.	Which are the top 5 bussiest months?

      ![image](https://github.com/user-attachments/assets/c9bdc86c-8996-41a5-872f-0fcb66f07a69)

      August and may being the highest followed by july, march and June are the top5 order placed months by customers.


9.	what is the quarterly purchase trend for weekdays vs weekend?

      ![image](https://github.com/user-attachments/assets/e1bb6344-d305-4760-ba62-8f92fefb4a39)


      The chart shows that purchases peak during the 2nd quarter and are at lowest during weekends in 4th quarter.

10.	what is the purchase trend for days vs average sales

      ![image](https://github.com/user-attachments/assets/755c3e6e-02d0-4c20-a589-74289ec9b9e0)


      Clustered line chart  illustrates sales are generally decreasing throughout the week. Payment activity experiences a midweek boost before tapering off.

11.	Which product category has highest freight value wrt product weight?

      ![image](https://github.com/user-attachments/assets/64545e71-9b65-4cf7-aeb0-8ea58369c91d)


      Higher the product weight corresponds to higher freight values. Office furniture category exhibits the highest freight value relative to product weight.  This trends reflects that as the weight of items increases ,the cost of shipping also arises likely due to factors such as size of the products,specialized handling requirements.       


11.	What are the peak hours of orders made?

     ![image](https://github.com/user-attachments/assets/02b31024-95a6-4123-b2ae-1bfd363cffe9)

     ![image](https://github.com/user-attachments/assets/50fd1679-18b5-436a-a18b-9ee2adf2409e)

     Peak hour is between 1 to 4 pm  on weekdays ,8 to 9pm on monday 
     and  9 to 11am on weekdays.


**Product Delivery**

12.	What is the order of late delivery by state?


      ![image](https://github.com/user-attachments/assets/39981c94-d76d-42f8-8d5f-649009249492)

       Top 5 states which received late deliveries are Roraima, Amapa, Amazonas,Alagoes,Para. Which is more than 20 days.

13.	Rate of successful product deliveries?


     ![image](https://github.com/user-attachments/assets/333dacd3-d5ff-4642-a627-4fa499c5f0a9)

     Rate of  successful product deliveries stands at an impressive 97.02%, reflecting high level of reliability in order fulfilment. Out of the total deliveries, 89,941 were on time demonstrating a strong performance in meeting delivery commitments. However there were 6534 delayed deliveries which while in small comparision, still represent significant area of improvement.
        
14.	What is the status of undelivered orders?

     ![image](https://github.com/user-attachments/assets/a18d195d-5b10-4ce9-8abd-95c5c8af79af)

15. How delayed deliveries impacting Customer Rating?

     ![image](https://github.com/user-attachments/assets/aa56ca23-50fe-4015-8d4c-43fc31239f4a)

     The above chart illustrates a clear trend as delivery days increase customer ratings tend to decrease.

 16. what is the average delivery time taken to deliver products?

     ![image](https://github.com/user-attachments/assets/6d811d8d-7f4e-46c0-9581-4c029cab89ab)

     Average delivery time is 12 days, with the logistics process accounting for 7 of those days . above product categories have taken higher delivery days.
    
**Customer feedback analysis**

17. What is the rate of Revisiting Customers?

     ![image](https://github.com/user-attachments/assets/fe695cfd-e709-42e7-b471-2f4a18336a04)

     The waterfall chart demonstrates the customer revisits have increased over time. For the 3 years around 3345 customers have revisited olist. Which indicates growing customer loyalty and engagement with the olist platform.


18. How Satisfied are Customers?

     ![image](https://github.com/user-attachments/assets/a9c89cf1-b3e1-4594-bf5e-5c2fda0532ac)

     The average customer rating of 4.09 is much better for products, indicating that overall performance of the products are highly satisfactory.More than 75% customers gave a good ratings.  This suggests that customers are generally pleased with their purchases, reflecting positively on the product offerings.


19. What is the CSAT?

     ![image](https://github.com/user-attachments/assets/d4dfb84f-4f6a-4f5e-91fb-4005e56e0e82)

      Product quality has achieved a customer satisfication score of 77.4%, indicating that most customers are pleased with product they received. delivery satisfication stands at 72.6% ,indicates many customers are generally satisfied with the delivery. CSAT for seller is 68.1%, suggesting that the customer satisfication with seller interaction is low. This score highlights potential issue in communication, accuracy of product information provided by sellers.
      Customer Satisfication Score is positive for the orders over the years with the good product quality, on time deliveries and the seller services.

20. Which are the high demand product categories?

      ![image](https://github.com/user-attachments/assets/838365b5-88c6-4d4e-a709-05b54332869b)

      The above highlights the top 5 high demand categories , which is crucial for optimizing inventory.

21. How Customer ratings are distributed over months?

      ![image](https://github.com/user-attachments/assets/3bea11d1-8d46-43d6-9c23-17ee59e7978f)


      In the above chart we can see that ratings do not overlap over time, indicating that they are arranged in a clear order from best to worst except rating 1 which is more than rating 3 &2.
      To understand why customers are giving a rating of 1 more than ratings of 2 or 3, here is the clustered column chart which shows average product cost (line) vs the ratings 1 to 5(columns).

 
     ![image](https://github.com/user-attachments/assets/ff305b78-382f-4d99-8be2-6c6ecf09e135)

      We observe that higher the product cost lower the ratings. This can attribute to several factors.firstly customer expectation tend to be higher for expensive products, leading to greater scrutiny and harsher judgements if the product falls short of these expectations. additionally expensive items might not be a good quality or functionality which can result in lower ratings.

22. What is the reason for Cancellation?
                  
     ![image](https://github.com/user-attachments/assets/e36bdfc3-b888-4cac-82e0-9a6efa577d2f)

     Order cancellation has notably influenced by product issues and late deliveries.            




### Dashboards

![image](https://github.com/user-attachments/assets/0dd6c8ce-4e24-4744-9380-f0de9460dba8)<br><br><br><br>

![image](https://github.com/user-attachments/assets/f7747d35-040c-4db2-8791-7ea114dc7185)<br><br><br><br>

![image](https://github.com/user-attachments/assets/53250c31-f442-46d5-81a9-65483a93294d)<br><br><br><br>

![image](https://github.com/user-attachments/assets/19c2984f-64a7-4302-9143-91cc80af33a4)<br><br>

### Insights
* The olist store earned $16 million  revenue over three years because it offers a wide variety of products, keeps prices competitive, and has a website that customers find easy to use. These factors help attract many customers and keep them happy, which has led to the store's success and growth.

* Sao Paulo, as a major economic hub in Brazil, likely drives a substantial portion of sales due to its large population, strong purchasing power, and vibrant commercial environment.
Understanding the concentration of sales in  top seven states is crucial for strategic business decisions, such as market expansion, targeted marketing efforts. By focusing on these high-performing regions, olist can capitalize on existing demand and potentially enhance their market share and profitability.

* Orders have been steadily increasing over the years, indicating sustained growth. However, a notable spike occurred in November 24, 2017, largely attributed to Black Friday. During this period, orders more than doubled compared to the previous week, showcasing the significant impact of Black Friday sales promotions on consumer behavior. Black Friday is renowned for offering substantial discounts and promotions, motivating consumers to make purchases they may have deferred. This surge in orders reflects heightened consumer interest and confidence during this promotional period, as people capitalize on the attractive deals available.
  
* highest selling product does not have highest sales volume. this may be due to highest-selling product is priced lower compared to other products with higher prices or when it is part of a promotional offer that drives higher unit sales but lower revenue per unit.
  
*	Out of the total revenue generated, which amounts to $12.5 million, a significant portion came from customers using credit cards. Majority of customers payed in 1 month installment This disparity underscores a consumer preference for credit cards, possibly influenced by benefits like rewards and convenience. which can appeal to consumers seeking a secure and flexible payment option.
  
* Customer ordering patterns reveal distinct peaks in August, May, and July, suggesting seasonal fluctuations or targeted promotional efforts during these months. Peak ordering hours typically occur between 1 to 4 pm on weekdays, indicating a preference for shopping convenience during work breaks or leisure time. Monday evenings, particularly from 8 to 9 pm, see a notable spike in orders, possibly reflecting consumers' post-work or evening shopping habits. Additionally, mornings between 9 to 11 am on weekdays show heightened activity, indicating engagement during early hours, possibly during commutes or before starting the workday. These insights are crucial in optimizing inventory, staffing, and marketing strategies to effectively meet customer demand and capitalize on peak periods of engagement throughout the week and year.
  
*	There is a notable trend where customer ratings tend to decrease as delivery days increase. Despite a commendable 97.02% success rate in product deliveries, with 89,941 deliveries made on time showcasing strong commitment, there were 6,534 delayed deliveries, highlighting an area for improvement. On average, deliveries take 12 days, with 7 days allocated to logistics processes. Certain product categories experience longer delivery times, impacting customer satisfaction ratings. Addressing these longer delivery periods through streamlined logistics and improved operational efficiencies is crucial to enhance overall customer experience and maintain high service standards.

*	Products with the highest sales performance achieved ratings averaging 4.09, despite the availability of a top rating of 5. Interestingly, many products and sellers with a perfect 5 rating did not necessarily translate that into high sales performance. This suggests that customer preference leans towards lower-priced goods rather than solely focusing on top-rated items.

*	Customers who made repeat purchases contributed 5.84% of the total revenue, underscoring the presence of a loyal customer base at Olist. This statistic highlights the significance of customer retention and loyalty in sustaining revenue streams.

*	Order cancellations have been notably influenced by product issues and late deliveries. These factors play a significant role in customer dissatisfaction and subsequent cancellations. through improved quality control measures, efficient logistics management, and proactive communication with customers can help reduce order cancellations, enhance customer satisfaction, and maintain positive relationships with buyers.

### Recommendations 
(to leverage strengths, address weaknesses, and drive further growth):

1. Market Expansion and Targeted Strategies
   
    *	Geographic Focus: São Paulo being major driver of sales, continue to strengthen presence in this key market. Additionally, analyze the concentration of sales in the top seven states to identify potential for regional expansion. Tailor marketing and operational strategies to these high-performing areas.
    
    *	Seasonal and Promotional Strategies: By focusing on peak ordering periods (August, May, July) and significant events like Black Friday, Olist can plan targeted promotions and make necessary inventory adjustments. Additionally, optimizing marketing campaigns ahead of these peak times will help maximize sales and capitalize on heightened consumer activity.
    
2. Product and Pricing Strategy
   
    *	Diversify Product Range: Olist should broaden its product range to include a greater variety of higher-priced items alongside its existing lower-priced offerings. This diversification will allow Olist to cater to different customer segments and balance revenue streams. By introducing premium products or exclusive brands, Olist can attract a broader customer base and potentially increase overall sales revenue.
      
    *	Promotional Offers: Develop promotional strategies that include discounts for revisiting customers and on higher-priced items to drive up their sales volumes and overall revenue. This can also help in leveraging the spike in orders observed during Black Friday and similar events.
    
3. Operational Efficiency and Logistics
   
    *	Streamline Delivery Processes: Address the issue of delivery delays by optimizing logistics and reducing average delivery times. Invest in better logistics management systems and explore partnerships with reliable delivery services to improve efficiency.
    
    *	Enhance Quality Control: Implement stricter quality control measures to minimize product issues that lead to cancellations. Regularly review and adjust operational processes to ensure high standards. Additionally, continuously monitor customer feedback to swiftly identify and address concerns.
    
5. Customer Experience and Satisfaction
   
    *	Improve Delivery Times and Transperency: Focus on reducing delivery times to boost customer satisfaction. Consider options such as expedited shipping or more localized fulfillment centers to improve delivery speed. Implement Online Product Tracking for Enhanced Delivery Transparency. This can help build trust with customers, minimize confusion, and enhance the overall efficiency of the delivery process.
    
    *	Optimize Customer Service: Strengthen customer support to handle issues proactively and maintain high service standards. Ensure that support teams are well-trained to resolve issues efficiently and enhance overall customer experience.
    
6. Optimize Payment Options
   
    *	Leverage Credit Card Preferences: Capitalize on the preference for credit card payments by offering incentives such as rewards or discounts for using credit cards. Explore partnerships with financial institutions to offer exclusive deals.
    
7. Customer Retention and Loyalty

    * Enhance Loyalty Programs: Build on the existing loyal customer base by developing or refining loyalty programs that reward repeat purchases. Offer exclusive benefits or discounts to encourage customer retention.
    *	Personalized Engagement: create personalized marketing and engagement strategies, enhancing the overall customer experience and fostering long-term relationships.






































