-- The Airline Database is loaded in Postgres SQL. The Schema Of this Database is Bookings.

Questions:
/*1.	Represent the “book_date” column in “yyyy-mmm-dd” format using Bookings table 
Expected output: book_ref, book_date (in “yyyy-mmm-dd” format) , total amount */

Select book_ref,
to_char(book_date :: date, 'yyyy-mon-dd') as book_date ,
total_amount
from bookings.Bookings;




/*2.	Get the following columns in the exact same sequence.
Expected columns in the output: ticket_no, boarding_no, seat_number, passenger_id, passenger_name.*/

SELECT
tfl.ticket_no, 
bpas.boarding_no, 
bpas.seat_no, 
tfl.passenger_id, 
tfl.passenger_name
FROM bookings.tickets tfl
LEFT JOIN bookings.boarding_passes Bpas
on tfl.ticket_no = bpas.ticket_no
                   



/*Write a query to find the seat number which is least allocated among all the seats?*/

 
SELECT 
seat_no  
FROM bookings.boarding_passes
GROUP BY seat_no
HAVING count(boarding_no) =1


                      



/*	In the database, identify the month wise highest paying passenger name and passenger id
Expected output: Month_name(“mmm-yy” format), passenger_id, passenger_name and total amount*/



SELECT 
Month_data, 
passenger_id,
passenger_name,
total_amount
FROM
(
SELECT 
Total_amount, passenger_id,
passenger_name,
to_char(book_date,'Mon-YY')as Month_data,
DENSE_RANK() OVER (PARTITION BY to_char(book_date,'Mon-YY') ORDER BY total_amount DESC) AS Rank_1
FROM bookings.bookings
Join bookings.Tickets
ON bookings.book_ref = tickets.book_ref)
AS MAX_PAYING_CUSTOMER
WHERE Rank_1 = 1
ORDER BY 1 ASC, 4 DESC






/*In the database, identify the month wise least paying passenger name and passenger id?
Expected output: Month_name(“mmm-yy” format), passenger_id, passenger_name and total amount*/


SELECT 
Month_data, 
passenger_id,
passenger_name,
total_amount FROM
(
SELECT 
Total_amount, passenger_id,
passenger_name,
to_char(book_date,'MON-YY')as Month_data,
DENSE_RANK() OVER (PARTITION BY to_char(book_date,'MON-YY') ORDER BY total_amount asc) AS Rank_1
FROM Bookings.bookings
Join Bookings.Tickets
ON bookings.book_ref = tickets.book_ref) MIN_PAYING_CUSTOMER
WHERE Rank_1 = 1
ORDER BY 1 ASC, 4 DESC;


/*Identify the travel details of non stop journeys  or return journeys (having more than 1 flight).
Expected Output: Passenger_id, passenger_name, ticket_number and flight count*/

  
SELECT 
TFL.ticket_no,
Passenger_id,
passenger_name,
count(FL.flight_id) as Flight_journeys
FROM Bookings.FLIGHTS FL
JOIN Bookings.TICKET_FLIGHTS TFL
ON FL.FLIGHT_ID = TFL.FLIGHT_ID
JOIN Bookings.TICKETS TK
on TK.ticket_no = TFL.ticket_no
group by 1,2,3
having count(FL.flight_id) > 1
ORDER BY 4 DESC;






/*How many tickets are there without boarding passes?
Expected Output: just one number is required*/

  
SELECT count (tkt) 
AS Number_of_passengers_without_boarding_ticket 
FROM
(
SELECT tickets.ticket_no AS tkt, 
boarding_passes.boarding_no
FROM bookings.Tickets
LEFT JOIN bookings.boarding_passes
ON Tickets.ticket_no = boarding_passes.ticket_no
WHERE boarding_passes.boarding_no IS NULL) AS TICKET_DETAILS




/*Identify details of the longest flight (using flights table)?
Expected Output: Flight number, departure airport, arrival airport, aircraft code and durations*/

With Time_details as
(SELECT 
Flight_no,
departure_airport,
arrival_airport,
aircraft_code,
cast(scheduled_arrival as Time) - cast(scheduled_departure as Time) as duration
FROM FLIGHTS
),

ranking as
(
Select
flight_no,
departure_airport,
arrival_airport,
aircraft_code,
duration,
dense_rank() over(order by duration desc) as rank_1
from Time_details)

SELECT  
flight_no,
departure_airport,
arrival_airport,
aircraft_code,
concat(duration,' ','Hour') as Duration_of_flight
FROM ranking
WHERE rank_1 = 1



/*Identify details of all the morning flights (morning means between 6AM to 11 AM, using flights table)?
Expected output: flight_id, flight_number, scheduled_departure, scheduled_arrival and timings*/

Answer: 
SELECT 
flight_id,
flight_no,
scheduled_arrival,
scheduled_departure,
CAST(scheduled_departure as time) AS time_of_departure
FROM FLIGHTS
GROUP BY 1,2,3,4
HAVING CAST(scheduled_departure as time) BETWEEN '05:00:00' AND '11:00:00'
ORDER BY 5 



/*Identify the earliest morning flight available from every airport.
Expected output: flight_id, flight_number, scheduled_departure, scheduled_arrival, departure airport and timings*/


SELECT 
flight_id,
flight_no,
scheduled_arrival,
scheduled_departure,
time_of_departure
FROM
(
SELECT 
flight_id,
flight_no,
scheduled_arrival,
scheduled_departure,
CAST(scheduled_departure as time) AS time_of_departure,
DENSE_RANK () OVER(PARTITION BY departure_airport order by CAST(scheduled_departure as time) asc ) AS RANK_1
FROM FLIGHTS
JOIN AIRPORTS
ON FLIGHTS.departure_airport = AIRPORTS.AIRPORT_CODE
GROUP BY 1,2,3,4
HAVING CAST(scheduled_departure as time) BETWEEN '05:00:00' AND '11:00:00'
ORDER BY 5 ) AS EARLIEST_FLIGHT
WHERE RANK_1 = 1

























