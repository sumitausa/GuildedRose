# GuildedRose

## Information on how to setup our environments to run your application.

1. Install Visual Studio Community 2017  [https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio] if you dont have any version of Visual Studio 2017 on your machine at all. If you do, no need to install anything, skip this step.

2. Download the repo from github as a zip file and extract and save to a folder. Double click on InBooking.sln file, this will open the project in Visual studio

3. Once the solution is open and then press F5 , this will build and run the application.It will open a default UI window

4. Then change the URL on the window by adding /swagger to it . For example the complete url will look like http://localhost:53618/swagger/. After the url is edited press enter.

5. This will open show the various endpoints that are avialable. The 3 endpoints are available are listed.

6. To choose an endpoint, press TryitOut and then press Execute.


## Whats in the code base and  input values to test application 

1. There are 4 Data models used in the application to store information.(Room,RoomBooking,RoomOccupied,GnomeRoster). In real life the information would be stored in the database, but for this i am storing information in-memory. Inorder to make testing easier , the application comes pre-loaded with data.


  * Room class (as per the spec) has 4 rooms.
  * RoomBooking class is used to store reservation/booking information..so the application already has 4 fake bookings ..for rooms 1,2 and 3. 
 * RoomOccupied class is the datamodel to be used in real life during the check in process. It will be used in the room cleaning process , so it stores the booking id , room number, checckout time and number of people in the room. 
 * Gnomeroster class has informationn about the gnome. So it comes pre-loaded with the 1 gnome's information ..the gnome id which is 1, shift start time (assumed to be current time) and maximum number of hours this gnome can work for today. 
 * The BookingHandler class has the endpoints that are exposed to the users
 * Startup.cs has the code for configuring Swagger to test the application and expose the API.

2. If you want to see the GetBookings make check bookings before assigning new rooms, choose today's date as checkinDate and checkoutDate as tomorrow ( this is based on fake data), if you want to just test how the program works for an empty inn, choose dates in the future. The fake data assumes the rooms are booked for today's date to test these conditions easily.

3. To test any Date value specify it in the format of mm/dd/yy hh:mm: ss or for example 3/9/18 21:00:00 or even 03/09/18. if you dont enter a hour..it will assume it as 12am. 

4. The fake data assumes that the gnome starts the shift the same time as the application is run and there is just one row in the gnome's shift. If you want to see rooms that are not available for space that will be the rooms the gnome cant get to.

## Third party tools used

1. Visual Studio built in with Kestrel 
2. Swagger

I chose these tools since they are the most I am familiar with and they can easily be tested.

## How does the system work

1. The system has some fake data set up in it , so you can test the various endpoints by giving today's date as a parameter.

2. The system's endpoints have all the parameters as required parameters.

3. The ViewBookings will give an error message if there was an issue with the booking 
    * -2 : Error code if you enter a number greater than inn's max number of ppl or max number of luggages
    * -3 : Try to book a date in the past
    * -1 : Could not book a room for a given date/person/luggage/requirements
    
4. The system checks if the cleaning gnome can get to a room based on a day.There is a table that stores the gnome's id and gnome's max hours and we can use that in the future to build out more gnomes or where each gnome can work different number of max hours

5. The system doesnt provide a way to book the room, it only provides the three things as per spec..a way to view avialability of rooms aka see which rooms have space or luggage space availability given a date,  gives a list of rooms we can book given the date,person count and luggage count, also provides the list of rooms the gnome can clean given the date

6. The system assumes the gnome cleans after the rooms are vacated

7. Incase of a booking, the system tries to use the space present in already reserved rooms for the same date before putting the person in a new room. This way cleaning is mininimized. The system doesnt seperate out luggage and person but assumes that luggage belonging to a party belongs to a party as opposed to an induvidual within the party.


## Time spent and prioritization

Spent about 10 hours on this project. If I had unlimited time I would take the time that the Gnome's time into consideration so that we know exactly what room will work on next, so that the room that will be occupied first will be cleaned first i.e take the check in time into consideration. Also do more API validation for example check if the date checkout is not past the checkin. The case that the checkin and checkout date are the same was not considered.

There are so many improvements to be made to the project if I had unlimited time:

1. Allow the gnome's schedule to be entered, viewed, edited, added

2. Allow rooms to be viewed, entered, edited. Provide the room cost for a booking.

3. Clean the booking and the room occupied information that is in the database periodically, archive it by using a script every few days

4. Add authentication so that only the owner of the inn can see the api's i.e you need to be logged in to be able to call the endpoints

5. Have a subscriber/publisher mechanism or a broadcast mechanism like SignalR and so that as soon as the room is checked out the gnome can start cleaning. After the room is clean the Booking service knows the room is free to be occupied...this might help in terms of booking rooms.

6. Change the API to provide the cost of the room after a booking is confirmed

7. Add a system for system maintainence

8. Set the max number of years we can reserve in advance? (limit it to a confugured value)


## Automated testing
   The API can be tested by writing scripts that can curl into the endpoints and check for the objects that are expected with the current code.Sepearte out the fake data into a seperate visual studio project that references the code base and has functions that test the various business logic. 
    
