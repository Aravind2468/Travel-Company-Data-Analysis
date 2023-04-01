# Travel Company Data Analysis using SQL


## Questions on DataSet

### How many users are in each segment? How many users booked flight on April 2022 in each segment?

#### Solution 1:

select u.segment, count(distinct u.User_id) as Total_Count,
       count(distinct(case when b.Line_of_business = 'Flight' and b.Booking_date between '2022-04-01' and '2022-04-30' then b.User_id end)) as users_booked_flights_apr2022
from  User_table u
left join Booking_table b on b.User_id=u.User_id
group by Segment

### Solution 2:

SELECT u.segment,
       COUNT(DISTINCT u.User_id) AS Total_Count,
       COUNT(DISTINCT b.User_id) AS flights_booked_on_apr2022
FROM User_table u
LEFT JOIN (
  SELECT User_id
  FROM Booking_table
  WHERE Line_of_business = 'Flight' AND Booking_date BETWEEN '2022-04-01' AND '2022-04-30'
) b ON b.User_id = u.User_id
GROUP BY u.segment

### Find the user whose first booking was Hotel Booking?

#### Solution 1: Using rank function

select *
from
(
select 
*,
rank() over(partition by User_id order by Booking_date) as rn -- For each user rank starts from 1 
from Booking_table ) b where rn=1 and Line_of_business='Hotel';



