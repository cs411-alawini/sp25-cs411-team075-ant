# Conceptual and Logical Database Design

## ER Diagram
![Entity_Relationship_Diagram](https://github.com/user-attachments/assets/06ed5055-d852-44a7-821b-af9878641516)
***
## Assumptions and Description of Entities and Relationships
***
### Assumptions:
* User_Alert: Users can set multiple alerts based on delay thresholds.
* Flight_Status: A flight has only one status entry per date.
* Weather_Event: Each weather event affects only one airport.
* Delay_Prediction: Multiple predictions can exist for the same flight at different times.

### Entity-Relationship (ER) Diagram Analysis:
The following entities and their attributes were identified:

#### Entities:
##### 1) User
* Attributes: userId (PK), username, email, phoneNumber, password, createdAt, updatedAt

##### 2) User_Alert
* Attributes: alertId (PK), userId (FK), emailAlert, smsAlert, delayThreshold, createdAt
* Relationship: A User can have multiple alerts (1-Many relationship with User).

##### 3) Airport
*Attributes: airportCode (PK), name, city, state, locationLat, locationLng, timeZone, zipCode
*Relationship: An airport reports weather events and serves as an origin or destination for flights.

##### 4) Weather_Event
* Attributes: eventId (PK), airportCode (FK), type, severity, startTime, endTime, precipitation, locationLat, locationLng, city, country, zipCode
* Relationship: An airport reports multiple weather events.

##### 5) Flight
* Attributes: flightId (PK), airlineCode (FK), flightNumber, originAirport (FK), destAirport (FK), scheduledDepartureTime, scheduledArrivalTime, elapsedTime, distance
* Relationship: A flight originates from one airport and lands at another (Many-Many through Airport).

##### 6) Flight_Status
* Attributes: statusId (PK), flightId (FK), flightDate, actualDepartureTime, actualArrivalTime, departureDelay, arrivalDelay, taxiOut, taxiIn, actualElapsedTime, airTime, cancelled, cancellationCode, diverted, carrierDelay, weatherDelay, nasDelay, securityDelay, lateAircraftDelay
* Relationship: Each flight has one status (1-1 relationship).

##### 7) Delay_Prediction
* Attributes: predictionId (PK), flightId (FK), predictionTime, predictedDepartureDelay, predictedArrivalDelay, notificationSent, predictionReason
* Relationship: A flight can receive multiple delay predictions (1-Many relationship with Flight).

##### 8) Airline
* Attributes: airlineCode (PK), dotCode, name
* Relationship: An airline operates multiple flights (1-Many relationship with Flight).

***
### Normalization Process
The schema adheres to BCNF as:
* No Partial Dependencies - All non-key attributes are fully functionally dependent on the primary key.
* No Transitive Dependencies - Each attribute depends only on the primary key and not indirectly on other attributes.
* Each relation is in 3NF - No non-prime attribute determines another non-prime attribute.
* Foreign keys are used appropriately to maintain referential integrity.

### Relational Schema
* User(userId: VARCHAR [PK], username: VARCHAR, email: VARCHAR, phoneNumber: VARCHAR, password: VARCHAR, createdAt: DATE, updatedAt: DATE)

* User_Alert(alertId: VARCHAR [PK], userId: VARCHAR [FK to User.userId], emailAlert: BOOLEAN, smsAlert: BOOLEAN, delayThreshold: INT, createdAt: DATE)

* Airport(airportCode: VARCHAR [PK], name: VARCHAR, city: VARCHAR, state: VARCHAR, locationLat: FLOAT, locationLng: FLOAT, timeZone: VARCHAR, zipCode: VARCHAR)

* Weather_Event(eventId: VARCHAR [PK], airportCode: VARCHAR [FK to Airport.airportCode], type: VARCHAR, severity: VARCHAR, startTime: DATETIME, endTime: DATETIME, precipitation: FLOAT, locationLat: FLOAT, locationLng: FLOAT, city: VARCHAR, country: VARCHAR, zipCode: VARCHAR)

* Flight(flightId: VARCHAR [PK], airlineCode: VARCHAR [FK to Airline.airlineCode], flightNumber: INT, originAirport: VARCHAR [FK to Airport.airportCode], destAirport: VARCHAR [FK to Airport.airportCode], scheduledDepartureTime: DATETIME, scheduledArrivalTime: DATETIME, elapsedTime: FLOAT, distance: FLOAT)

* Flight_Status(statusId: VARCHAR [PK], flightId: VARCHAR [FK to Flight.flightId], flightDate: DATE, actualDepartureTime: DATETIME, actualArrivalTime: DATETIME, departureDelay: FLOAT, arrivalDelay: FLOAT, taxiOut: FLOAT, taxiIn: FLOAT, actualElapsedTime: FLOAT, airTime: FLOAT, cancelled: BOOLEAN, cancellationCode: VARCHAR, diverted: BOOLEAN, carrierDelay: FLOAT, weatherDelay: FLOAT, nasDelay: FLOAT, securityDelay: FLOAT, lateAircraftDelay: FLOAT)

* Delay_Prediction(predictionId: VARCHAR [PK], flightId: VARCHAR [FK to Flight.flightId], predictionTime: DATETIME, predictedDepartureDelay: FLOAT, predictedArrivalDelay: FLOAT, notificationSent: BOOLEAN, predictionReason: VARCHAR)

* Airline(airlineCode: VARCHAR [PK], dotCode: INT, name: VARCHAR)
