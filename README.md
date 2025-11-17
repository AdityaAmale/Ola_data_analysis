# Ola_data_analysis
This project analyzes Ola car ride booking and cancellation data through data cleaning, SQL queries, and Power BI visualizations. The dataset was processed to remove inconsistencies and extract key patterns. Insights on ride demand, cancellations, and driver behavior were highlighted through interactive dashboards.

Please create a spreadsheet with 1 lac rows, for Bengaluru city. Give the following columns. The data will be for 1 month. use the following column -
1.	Date
2.	Time
3.	Booking ID
4.	Booking Status
5.	Customer ID
6.	Vehicle Type
-	Auto
-	Prime Plus
-	Prime Sedan
-	Mini
-	Bike
-	eBike
-	Prime SUV
7.	Pickup Location (Create dummy location points Take any 50 areas from Bangalore)
8.	Drop Location (Take from dummy pickup locations)
9.	Avg VTAT (Time taken to arrive at the vehicle)
10.	Avg CTAT (Time taken to arrive the Customer)
11.	Cancelled Rides by Customer
12.	Reason for cancelling by Customer
-	Driver is not moving towards pickup location
-	Driver asked to cancel
-	AC is not working (Only for 4-wheelers)
-	Change of plans
-	Wrong Address

13.	Cancelled Rides by Driver
-	Personal & Car related issues
-	Customer related issue
-	The customer was coughing/sick
-	More than permitted people in there

14.	Incomplete Rides
15.	Incomplete Rides Reason
-	Customer Demand
-	Vehicle Breakdown
-	Other Issue
16.	Booking Value
17.	Ride Distance
18.	Driver Ratings
19.	Customer Rating

Keep the overall booking status success for this data at 62%. If the booking status is successful, then only fare charge ratings, average VTAT, average CTAT, and other data will be there.
 
Make sure orders cancelled by customers should not be more than 7% Make sure orders cancelled drivers should not be more than 18%

Also, increase the number of orders on weekends and match days. Keep match day by using the following dates.
keep incomplete rides less than 6%
Keep order value high on weekends

in Food Category keep around 67 Indian
keep order ID with 10 digits starting with CNR and then digits keep orders under 500 value 70%
keep orders above 500 value 28% keep remaining orders above 1000
 
Power BI Questions:
1.	Ride Volume Over Time
2.	Booking Status Breakdown
3.	Top 5 Vehicle Types by Ride Distance
4.	Average Customer Ratings by Vehicle Type
5.	cancelled Rides Reasons
6.	Revenue by Payment Method
7.	Top 5 Customers by Total Booking Value
8.	Ride Distance Distribution Per Day
9.	Driver Ratings Distribution
10.	Customer vs. Driver Ratings


Data Columns 
1.	Date
2.	Time
3.	Booking_ID
4.	Booking_Status
5.	Customer_ID
6.	Vehicle_Type
7.	Pickup_Location
8.	Drop_Location
9.	V_TAT
10.	C_TAT
11.	cancelled_Rides_by_Customer
12.	cancelled_Rides_by_Driver
13.	Incomplete_Rides
14.	Incomplete_Rides_Reason
15.	Booking_Value
16.	Payment_Method
17.	Ride_Distance
18.	Driver_Ratings
19.	Customer_Rating
     
Power BI Answers:

Segregation of the views:
1.	Overall
-	Ride Volume Over Time
-	Booking Status Breakdown

2.	Vehicle Type
-	Top 5 Vehicle Types by Ride Distance

3.	Revenue
-	Revenue by Payment Method
-	Top 5 Customers by Total Booking Value
-	Ride Distance Distribution Per Day

4.	Cancellation
-	Cancelled Rides Reasons (Customer)
-	cancelled Rides Reasons(Drivers)

5.	Ratings
-	Driver Ratings
-	Customer Ratings

Answers:
1.	Ride Volume Over Time: A time-series chart showing the number of rides per day/week.
2.	Booking Status Breakdown: A pie or doughnut chart displaying the proportion of different booking statuses (success, cancelled by the customer, cancelled by the driver, etc.).
3.	Top 5 Vehicle Types by Ride Distance: A bar chart ranking vehicle types based on the total distance covered.
4.	Average Customer Ratings by Vehicle Type: A column chart showing the average customer ratings for different vehicle types.
5.	cancelled Rides Reasons: A bar chart that highlights the common reasons for ride cancellations by customers and drivers.
6.	Revenue by Payment Method: A stacked bar chart displaying total revenue based on payment methods (Cash, UPI, Credit Card, etc.).
7.	Top 5 Customers by Total Booking Value: A leaderboard visual listing customers who have spent the most on bookings.
8.	Ride Distance Distribution Per Day: A histogram or scatter plot showing the distribution of ride distances for different Dates.
9.	Driver Rating Distribution: A box plot visualizing the spread of driver ratings for different vehicle types.
10.	Customer vs. Driver Ratings: A scatter plot comparing customer and driver ratings for each completed ride, analyzing correlations.
 
create database ola;
use ola;
-- 1. Retrieve all successful bookings:
create view successful_bookings as
select * from bookings where Booking_Status="Success";

select * from successful_bookings;
-- 2. Find the average ride distance for each vehicle type:
create View ride_distance_for_each_vehicle as
select Vehicle_Type , AVG(Ride_Distance) from bookings group by Vehicle_Type;

select * from ride_distance_for_each_vehicle;

-- 3. Get the total number of cancelled rides by customers:
SELECT * FROM ola.bookings;
create view canceled_rides_count as
select count(*) from bookings where Booking_status="Canceled by Customer";
select * from canceled_rides_count;

-- 4. List the top 5 customers who booked the highest number of rides:
create view top_5_customers as
select Customer_ID,count(Booking_ID) as total_rides
from bookings
group by Customer_ID
order by total_rides desc limit 5;
select * from top_5_customers;

-- 5. Get the number of rides cancelled by drivers due to personal and car-related issues:
create view ride_canceled_due_to_personal_car_issue as
select count(*) from bookings
where Booking_status= "Canceled by driver" and Canceled_Rides_by_Driver="Personal & Car related issue";
select * from ride_canceled_due_to_personal_car_issue;

-- 6. Find the maximum and minimum driver ratings for Prime Sedan bookings:
create view max_min_rating as
select max(Driver_ratings) as Max_Rating,min(Driver_ratings) as Min_Rating
from bookings
where Vehicle_Type="Prime Sedan";
select * from max_min_rating;

-- 7. Retrieve all rides where payment was made using UPI:
create view UPI_rides as
select Booking_ID ,Customer_ID from bookings
where Payment_method="UPI";
select * from UPI_rides;
 
-- 8. Find the average customer rating per vehicle type:
create view Vehicle_type_customer_Rating_average as
select Vehicle_Type ,avg(Customer_Rating) from bookings
group by Vehicle_Type;
select * from Vehicle_type_customer_Rating_average;


-- 9. Calculate the total booking value of rides completed successfully:
create view booking_value_successful_ride as
select sum(Booking_Value) as total_booking_values
from bookings
where Booking_Status="Success";

select * from booking_value_successful_ride;

-- 10. List all incomplete rides along with the reason:
create view Incomplete_rides_reason as
select Booking_ID,Incomplete_Rides_Reason from bookings
where Incomplete_Rides= "Yes";
select * from Incomplete_rides_reason;

