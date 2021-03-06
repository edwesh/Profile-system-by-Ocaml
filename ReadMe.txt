The project goal:
My project is developing a profile system, which enable users to retrieve the correlate information by given the target profile object (e.g. If I create an object called “jobs” on Profile class, I will use this object “jobs” to retrieve his relevant information, such as: retrieve jobs).
The stakeholders of Profile system:
The relevant stakeholders within this profile system are the users who are input their information in different database, and the retrieval administrators who are finding the corresponded details that extract form different databases.
Because of each of the database are independent, so the only function that able to be used is conduct the profile information from “Profile” database to the other databases, and the stakeholders that may response to this function is the operator that compile the same database information into one Pair.
The Structure of the profile system:
There are major four classes which simulate to databases and used for storing the four different categories information.
 The first class is called “Profile”, and this simulates to store the person’s information of the name, age sex, and education.
 The second class is named as “Employment”, in this class it will create an object to store the employment details of job title, position in this job, salary and bonus that earned, also the total income from this job.
 The third class is “Property” which stored the address information to the related creation objects
 The fourth class is “Vehicle” and this class extends the third class “Property” which inherited its function that enable to create owner and address information, besides on that “Vehicle” class can be stored with details of the model of this vehicle, the plate number of its registration, the register date, the brand of this vehicle and the engine capacity of it.


Refer to above structure of the project, the first three classes are all independent existence except referred the “Profile” class as declare the variables of “employee” or “owner”, in such format:

class Employment {
employee : List Profile;

class Property {
owner: List Profile;

This will be the only correlation to each different class, except the fourth one “Vehicle” which is extends “Property”, by this way:

class Vehicle extends Property {

Therefore in this stage my overall simulated databases have been created, and also there are some data types accompanied with the class creation. The data types are involved of datatype Address this exist in the “Property” class which create the address information. datatype Date is used to declare the registration date in “Vehicle” class, and datatype MixNum is used for declaring the plate number of a vehicle object.
The main function to achieve the goal:
The main function is called “retrieve”, while using this query I can retrieve all information that identically relevant to the profile object as inquired. Therefore, the users using the “Profile” object to match each different category list such as (works, propers, and vehicles), and if it matched, the searching result will combine that list information together.
 My first approach for the main function “retrieve” is working as while the input arbitrary variables toString its values for the output and use this output to contrast with the contained “Profile” variables in Pairs of “works”, “propers” and “vehicles”, and this approach is implement by such query in general:

if
toString(!(fst works).employee) == toString([p]) && toString(!(fst propers).owner)== toString([p]) && toString(!(fst vehicles).owner)== t oString([p])
then (p,(fst works),(fst propers),(fst vehicles))

The above query is used for matching the employee’s profile with the given input object variable. Similar to this way, the program will statically match to all information that relate to this profile object.
After matching process, the program will integrate these separate objects into one Pair, this is work as simulate:

Let x = (a,b);;

In my case, this will makes Profile a and Employment b into one entirety x, and x is what the program generate the output result.
 My second approach on the main function “retrieve” with the generic pattern matching query to list the result by “select” function, and composite all the matching information with that particular person’s information and recursively check the input variables without any modifications on update or re-compiled with the code.
My second approach of the function query is my final implementation onto my project which is competent for my application.



let ext retrieve p =
let person = p
in
let rec (retrieveEmp: a -> Maybe Employment) =
|(e: Employment) -> if toString(!e.employee) == toString([person]) then Some e else None
|x -> None
in
let rec (retrievePro: a -> Maybe Property) =
|(p: Property) -> if toString(!p.owner) == toString([person]) then Some p else None
|x -> None
in
let rec (retrieveVeh: a -> Maybe Vehicle) =
|(v: Vehicle) -> if toString(!v.owner) == toString([person]) then Some v else None
|x -> None
in
(p,select retrieveEmp works, select retrievePro propers, select retrieveVeh vehicles)
;;



My formal edition of retrieval function is using pattern matching function to sort each of databases’ values within “works”, “propers” and “vehicles”. These three data storage will used to find out the matched values of Profile objects as created to compare each storage’s values that contained the same profile object information. The pattern matching function of “retrieveEmp”, “retrievePro”, and “retrieveVeh” are within the “retrieve” function, these three recursive method will finally listed the outcomes by “select” function in the end, and output None if there is nothing matched to the Profile input name such as “jobs” “allen”.
Project extension: - The special cases.
Within the main program “retrieve” there is an exceptional function which allowed the other input rather than just put Profile object. Therefore as the exceptional variable is been proceed, if the input is Integer type then this input will become “0” by the output list and the other not matching values will print empty lists. If the input variable is String type then it will print “Empty file” and form into the final output list.



retrieve += |(x: Int) ->
let rec (retrieveEmp: a -> Maybe Employment) =
|(e: Employment) -> if toString(!e.employee) == toString([x])then Some e else None
|x -> None
in
let rec (retrievePro: a -> Maybe Property) =
|(p: Property) -> if toString(!p.owner) == toString([x])then Some p else None
|x -> None
in
let rec (retrieveVeh: a -> Maybe Vehicle) =
|(v: Vehicle) -> if toString(!v.owner) == toString([x]) then Some v else None
|x -> None
in
(0,select retrieveEmp works, select retrievePro propers, select retrieveVeh vehicles);;
retrieve += |(x: String) ->
let rec (retrieveEmp: a -> Maybe Employment) =
|(e: Employment) -> if toString(!e.employee) == toString([x])then Some e else None
|x -> None
in
let rec (retrievePro: a -> Maybe Property) =


|(p: Property) -> if toString(!p.owner) == toString([x])then Some p else None
|x -> None
in
let rec (retrieveVeh: a -> Maybe Vehicle) =
|(v: Vehicle) -> if toString(!v.owner) == toString([x]) then Some v else None
|x -> None
in
("Empty file",select retrieveEmp works, select retrievePro propers, select retrieveVeh vehicles);;



After this stage, users will be acknowledged who are the registered person in this system, and be able to observe the correlate information that is existed previously in the database.
Testing query:
Approach 1:


let emp = (job1,job2);;
let find p =
if toString(!(fst emp).employee) == toString([p])
then (p,(fst emp))
else if
toString(!(snd emp).employee) == toString([p])
then (p,(snd emp))
else
let empol = new Employment in
(p,empol)
;;
find jobs;;
Output:
it: Profile[] * Employment[]
it = (
This person's information -
Name: "Jobs"
Age: 42
Sex: "Male"


Education: "Master",
Employment Status -
Job Title: "Developer"
Position: "Director"
Salary: 13500.
Bonus: 4000.
Total Income: 17500.)
find allen;;
Output:
it: Profile[] * Employment[]
it = (
This person's information -
Name: "Allen"
Age: 21
Sex: "Male"
Education: "Bachelor",
Employment Status -
Job Title: "Accountant"
Position: "Part-time"
Salary: 500.
Bonus: 50.
Total Income: 550.)
find justin;;
Output:
it: Profile[] * Employment[]
it = (
This person's information -
Name: "Justin"
Age: 31
Sex: "Male"
Education: "Bachelor",
Employment Status -
Job Title: _void
Position: _void
Salary: _void
Bonus: _void
Total Income: _void)
The above test queries and outputs shows the personal information and with Employment information that printed in as a list.


let est = (proper3,proper4);;
let find1 p =
if toString(!(fst est).owner) == toString([p])
then (p,(fst est))
else if
toString(!(snd est).owner) == toString([p])
then (p,(snd est))
else
let pr = new Property in
(p,pr)
;;
find1 justin;;
Output:
it: Profile[] * Property[]
it = (
This person's information -
Name: "Justin"
Age: 31
Sex: "Male"
Education: "Bachelor",
The address is: 281 Federal st/rd Sunny Hills area Brisbane QLD)
find1 ashley;;
Output:
it: Profile[] * Property[]
it = (
This person's information -
Name: "Ashley"
Age: 19
Sex: "Female"
Education: "Bachelor",
The address is: 1 Yanko st/rd Macquarie area Sydney NSW)
find1 peter;;
Output:
it: Profile[] * Property[]
it = (
This person's information -
Name: "Peter"


Age: 28
Sex: "Male"
Education: "High-School",
_void)
The above outputs shows the personal information and with their addresses, and if there is no matching address within this function, there will shows a wild card syntax to replace the address information.
Approach 2:
let rec (retrieveEmp: a -> Maybe Employment) =
|(e: Employment) -> if toString(!e.employee) == toString([jobs]) then Some e else None
|x -> None
;;
select retrieveEmp works;;
it: List Employment
it = [
Employment Status -
Job Title: "Developer"
Position: "Director"
Salary: 13500.
Bonus: 4000.
Total Income: 17500.]
let rec (retrievePro: a -> Maybe Property) =
|(p: Property) -> if toString(!p.owner) == toString([allen]) then Some p else None
|x -> None
;;
select retrievePro propers;;
it: List Property
it = [
The address is: 29 Parramatta st/rd Central area Melbourn VIC]
let rec (retrieveVeh: a -> Maybe Vehicle) =
|(v: Vehicle) -> if toString(!v.owner) == toString([nancy]) then Some v else None
|x -> None
;;


select retrieveVeh vehicles;;
it: List Vehicle
it = [
The vehicle's information-
Mode: "Van"
Plate Number: ueoqv2884110
Register Date: 27/2/2007
Brand: "Nissan"
Capacity: 4.
]


In my second approach, of the testing queries I had use much more effective way to illustrate my query. Show above, these testing queries are used for testing the enquire object that is whether stored or not, and also simplified the ideas of how the profile system works in procedures. There are three solid pattern matching functions that used for listing the result to contrast with default value in the fix location of my determination function.
In real situation, as users continuingly check on the profile system, the function code will not change in anytime, however the only modification is adding more values to the storage list, and use them as the database to match the person who that is satisfy this profile information in comparison.