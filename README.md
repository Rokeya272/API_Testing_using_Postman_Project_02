# API_Testing_Project
### **Rest Booking API Testing with Postman Newman**
This project demonstrates detailed API testing, including status code validation, response structure verification, and field value validation. The APIs tested are focusing on student management features such as creating, retrieving, and updating data.


### **Features of the Project**

- **Endpoints Tested:**
    - Get Student
    - Create Student
    - Get Specific Student
    - Create Technical Skills
    - Create Student Address
    - Final Student Details

- **Tests Performed:**
    - Status code validation.
    - Response structure and field value validation.
    - Logical consistency of API responses.


### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
   ```console 
   https://github.com/Rokeya272/Project_02_API_Testing_using_Postman.git
   ```

5. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
6. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
7. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```

     ### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.



## **Testing**

## Test Case Scenarios:

## _**1. Get Student**_

### Request URL: baseUrl/api/studentsDetails
### Request Method: GET
### Tests:
  - Status Code: Ensure it returns 200.
  - Response Length: Validate response contains expected number of student entries.
### Post-response Script:
```console 
    pm.test("Verify status code is 200", function () {
    pm.response.to.have.status(200);
});

var jsondata = pm.response.json();
console.log(jsondata.length);
```

 
## _**2. Create Student**_

### Request URL: baseUrl/api/studentsDetails
### Request Method: POST
### Tests:
  - Status Code: Ensure it returns 201 for successful creation.

### Request Body:
```console
{ 
"first_name": "{{Firstname}}", 
"middle_name": "{{Middlename}}", 
"last_name": "{{Lastname}}", 
"date_of_birth": "{{Date_of_Birth}}" 
}

```
### Pre-request Script:
```console
   var Firstname = pm.variables.replaceIn('{{$randomFirstName}}');
//console.log(Firstname);
pm.environment.set("Firstname",Firstname);

var Middlename = pm.variables.replaceIn('{{$randomFirstName}}');
pm.environment.set("Middlename",Middlename);

var Lastname = pm.variables.replaceIn('{{$randomLastName}}');
pm.environment.set("Lastname",Lastname);

const moment = require('moment');
const today = moment();

var Date_of_Birth = today.add(5,'d').format('YYYY-MM-DD');
pm.environment.set("Date_of_Birth", Date_of_Birth);
```
### Post-response Script:
 ```console
    pm.test("Verify status code is 201", function () {
    pm.response.to.have.status(201);
});
    var jsondata = pm.response.json();
    pm.environment.set("id",jsondata.id);

```


## _**3. Get Specific Student**_

### Request URL: baseUrl/api/studentsDetails/id
### Request Method: GET
### Tests:
  - Status Code: Ensure it returns 200.
  - Field Validation: Verify id, first_name, middle_name, last_name, and date_of_birth match expectations.

### Post-response Script:
 ```console 
var jsondata = pm.response.json();

pm.test("Verify status code is 200", function () {
    pm.response.to.have.status(200);
});


pm.test("Verify id",function(){

    pm.expect(jsondata.data.id).to.eql(pm.environment.get("id"));
})


pm.test("Verify First Name",function(){

    pm.expect(jsondata.data.first_name).to.eql(pm.environment.get("Firstname"));
})


pm.test("Verify Middle Name",function(){
pm.expect(jsondata.data.middle_name).to.eql(pm.environment.get("Middlename"))

})


pm.test("Verify Last Name",function(){
pm.expect(jsondata.data.last_name).to.eql(pm.environment.get("Lastname"))

})

pm.test("Verify Date of Birth",function(){
pm.expect(jsondata.data.date_of_birth).to.eql(pm.environment.get("Date_of_Birth"))

})

```
  

## _**4. Create Technical Skills**_

### Request URL: baseUrl/api/technicalskills
### Request Method: POST
### Tests:
  - Status Code: Ensure it returns 200 for success.
### Request Body:
```console
{ 
"id": 1, 
"language": [ 
"sample string 1", 
"sample string 2" 
], 
"yearexp": "sample string 2", 
"lastused": "sample string 3", 
"st_id": {{id}}
} 

```
### Post-response Script:
```console
    pm.test("Verify status code is 200", function () {
    pm.response.to.have.status(200);
});
```


## _**5. Create Student Address**_

### Request URL: baseUrl/api/addresses
### Request Method: POST
### Tests:
  - Status Code: Ensure it returns 200.
  - Field Validation: Check status and msg fields.

### Request Body:
```console
{ 
"Permanent_Address": { 
"House_Number": "sample string 1",
"City": "sample string 2",
 "State": "sample string 3", 
"Country": "sample string 4",
"PhoneNumber": [ 
{ 
"Std_Code": "sample string 1",
"Home": "sample string 2",
 "Mobile": "sample string 3" 
},
{ 
"Std_Code": "sample string 1",
"Home": "sample string 2", 
"Mobile": "sample string 3" 
} 
] 
},
"stId": {{id}} 
} 

```
### Post-response Script:
```console
var jsondata = pm.response.json();
pm.environment.set("Status",jsondata.status);

pm.environment.set("Message",jsondata.msg);



pm.test("Verify status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Verify status",function(){

    pm.expect(jsondata.status).to.eql(pm.environment.get("Status"));
})

pm.test("Verify message",function(){

    pm.expect(jsondata.msg).to.eql(pm.environment.get("Message"));
})
```


## _**6. Final Student Details**_

### Request URL: baseUrl/api/FinalStudentDetails/id
### Request Method: GET
### Tests:
    - Status Code: Ensure it returns 200.
    - Field Validation: Validate Language, Year of Experience, House Number, City, Country, Mobile, and Current Address fields.
### Post-response Script:
```console
var jsondata = pm.response.json();

pm.environment.set("Language1",jsondata.data.TechnicalDetails[0].language[0]);
pm.environment.set("Language2",jsondata.data.TechnicalDetails[0].language[1]);

pm.environment.set("Year_of_Experience",jsondata.data.TechnicalDetails[0].yearexp);

pm.environment.set("House_Number",jsondata.data.Address[0].Permanent_Address.House_Number);

pm.environment.set("City",jsondata.data.Address[0].Permanent_Address.City); 

pm.environment.set("Country",jsondata.data.Address[0].Permanent_Address.Country);

pm.environment.set("Mobile1",jsondata.data.Address[0].Permanent_Address.PhoneNumber[0].Mobile);
pm.environment.set("Mobile2",jsondata.data.Address[0].Permanent_Address.PhoneNumber[1].Mobile);

pm.environment.set("Current_Address",jsondata.data.Address[0].Current_Address);



//Field Value Validation
pm.test("Verify status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Verify Language1",function(){
    pm.expect(jsondata.data.TechnicalDetails[0].language[0]).to.eql(pm.environment.get("Language1"));
})

pm.test("Verify Language2",function(){
    pm.expect(jsondata.data.TechnicalDetails[0].language[1]).to.eql(pm.environment.get("Language2"));
})

pm.test("Verify Year of Experience",function(){
    pm.expect(jsondata.data.TechnicalDetails[0].yearexp).to.eql(pm.environment.get("Year_of_Experience"));
})

pm.test("Verify House Number",function(){
    pm.expect(jsondata.data.Address[0].Permanent_Address.House_Number).to.eql(pm.environment.get("House_Number"));
})


pm.test("Verify City",function(){
    pm.expect(jsondata.data.Address[0].Permanent_Address.City).to.eql(pm.environment.get("City"));
})


pm.test("Verify Mobile1",function(){
    pm.expect(jsondata.data.Address[0].Permanent_Address.PhoneNumber[0].Mobile).to.eql(pm.environment.get("Mobile1"));
})

pm.test("Verify Mobile2",function(){
    pm.expect(jsondata.data.Address[0].Permanent_Address.PhoneNumber[1].Mobile).to.eql(pm.environment.get("Mobile2"));
})

pm.test("Verify Current Address",function(){
    pm.expect(jsondata.data.Address[0].Current_Address).to.eql(pm.environment.get("Current_Address"));
})


```

## Run Command:  
- Run Command for Console: 
```console 
newman run Project_02_API_Testing.postman_collection.json -e API_Testing_Project_02.postman_environment.json

```

- Run Command for Report:

```console 
newman run Project_02_API_Testing.postman_collection.json -e API_Testing_Project_02.postman_environment.json -r cli,htmlextra 
```
## Newman Report Summary:
<img width="894" alt="Screenshot 2024-11-29 at 12 13 00 AM" src="https://github.com/user-attachments/assets/60922d70-12bc-4eb8-8243-9d561de201b0">
<img width="871" alt="Screenshot 2024-11-29 at 12 13 37 AM" src="https://github.com/user-attachments/assets/2317c885-5e67-4d11-ab9d-a2c5a1091e49">
<img width="808" alt="Screenshot 2024-11-29 at 12 14 07 AM" src="https://github.com/user-attachments/assets/2498b255-67c6-4831-9bee-495fb53fd62c">
<img width="846" alt="Screenshot 2024-11-29 at 12 14 37 AM" src="https://github.com/user-attachments/assets/d9d541cc-9289-4ab7-99ae-0d3b78ae4834">











