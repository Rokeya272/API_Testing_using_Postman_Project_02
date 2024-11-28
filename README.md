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
   https://github.com/Rokeya272/API_Testing_Project_01.git
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




## Run Command:  
- Run Command for Console: 
```console 
newman run Project1_BOOKING.postman_collection.json -e Proj1_BOOKING.postman_environment.json
```

- Run Command for Report:

```console 
newman run Project1_BOOKING.postman_collection.json -e Proj1_BOOKING.postman_environment.json -r cli,htmlextra 
```
## Newman Report Summary:

![image](https://github.com/user-attachments/assets/0fb042fa-204f-45f5-87a0-1a62f1b8cae4)
![image](https://github.com/user-attachments/assets/a5234082-7ab9-4f4e-9e57-7ba0945546ce)
![image](https://github.com/user-attachments/assets/ed4583f3-ccd0-43f1-ba33-e38fcf3f793f)
![image](https://github.com/user-attachments/assets/43401f25-0811-4ba9-80b5-9d2b5b1df917)










