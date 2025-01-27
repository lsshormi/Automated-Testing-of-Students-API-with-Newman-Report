### **Student API Testing with Postman and Report Generating with Newman**
This project demonstrates API testing using Postman, providing a collection of tests to validate various endpoints of the Student API.

### **Features**

- Tests for GET and POST requests
- Validation of API endpoints for retrieving and creating student details
- Collection of test assertions with 100% pass rate
- Detailed test results visualized using Newman HTML reports

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
 git clone https://github.com/lsshormi/Automated-Testing-of-Students-API-with-Newman-Report.git
 ```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
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
2. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
3. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## **Testing**
### Test Summary:
- **Total Requests:** 6
- **Total Assertions:** 30
- **Total Failed Tests:** 0
- **Total Skipped Tests:** 0
- **Pass Rate:** 100%

### **Test Case Scenarios**:
- **Base URL:** https://thetestingworldapi.com
  
#### **1. Get All Student Details**
- **Request Method:** GET  
- **Request URL:** `{{base_url}}/api/studentsDetails`  
- **Pre-request Script:** None  
- **Post-request Script:**
  ```javascript
  pm.test("Status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Check response length", function () {
      const responseData = pm.response.json();
      pm.expect(responseData.length).to.eql(100);
  });
  ```
- **Request Body:** None  
- **Response Body:**
  ```json
  [
      {
          "id": 1,
          "first_name": "John",
          "middle_name": "Doe",
          "last_name": "Smith",
          "date_of_birth": "2000-01-01"
      },
  
  ]
  ```

---

#### **2. Create a New Student**
- **Request Method:** POST  
- **Request URL:** `{{base_url}}/api/studentsDetails`  
- **Pre-request Script:**
  ```javascript
  var firstName = pm.variables.replaceIn("{{$randomFirstName}}");
  pm.environment.set("first_name", firstName);

  var middleName = pm.variables.replaceIn("{{$randomNameSuffix}}");
  pm.environment.set("middle_name", middleName);

  var lastName = pm.variables.replaceIn("{{$randomLastName}}");
  pm.environment.set("last_name", lastName);

  const moment = require("moment");
  const today = moment();
  var dateOfBirth = today.subtract(25, 'years').format("YYYY-MM-DD");
  pm.environment.set("date_of_birth", dateOfBirth);
  ```
- **Post-request Script:**
  ```javascript
  pm.test("Response status code is 201", function () {
      pm.response.to.have.status(201);
  });

  var jsonData = pm.response.json();
  pm.environment.set("id", jsonData.id);
  ```
- **Request Body:**
  ```json
  {
      "first_name": "{{first_name}}",
      "middle_name": "{{middle_name}}",
      "last_name": "{{last_name}}",
      "date_of_birth": "{{date_of_birth}}"
  }
  ```
- **Response Body:**
  ```json
  {
      "id": 12345,
      "first_name": "John",
      "middle_name": "Doe",
      "last_name": "Smith",
      "date_of_birth": "1998-01-01"
  }
  ```

---

#### **3. Get Specific Student Details**
- **Request Method:** GET  
- **Request URL:** `{{base_url}}/api/studentsDetails/{{id}}`  
- **Pre-request Script:** None  
- **Post-request Script:**
  ```javascript
  pm.test("Response status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Verify student details", function () {
      const jsonData = pm.response.json();
      pm.expect(jsonData.first_name).to.eql(pm.environment.get("first_name"));
      pm.expect(jsonData.last_name).to.eql(pm.environment.get("last_name"));
  });
  ```
- **Request Body:** None  
- **Response Body:**
  ```json
  {
      "id": 12345,
      "first_name": "John",
      "middle_name": "Doe",
      "last_name": "Smith",
      "date_of_birth": "1998-01-01"
  }
  ```

---

#### **4. Add Technical Skills**
- **Request Method:** POST  
- **Request URL:** `{{base_url}}/api/technicalskills`  
- **Pre-request Script:** None  
- **Post-request Script:**
  ```javascript
  pm.test("Response status code is 200", function () {
      pm.response.to.have.status(200);
  });
  ```
- **Request Body:**
  ```json
  {
      "id": 1,
      "language": ["Java", "Python"],
      "yearexp": "3 years",
      "lastused": "1 year ago",
      "st_id": {{id}}
  }
  ```
- **Response Body:**
  ```json
  {
      "status": "success",
      "message": "Technical skills added successfully"
  }
  ```

---

#### **5. Add Student Address**
- **Request Method:** POST  
- **Request URL:** `{{base_url}}/api/addresses`  
- **Pre-request Script:**
  ```javascript
  pm.environment.set("House_Number", "123");
  pm.environment.set("City", "New York");
  pm.environment.set("State", "NY");
  pm.environment.set("Country", "USA");
  pm.environment.set("Mobile", "1234567890");
  ```
- **Post-request Script:**
  ```javascript
  pm.test("Response status code is 200", function () {
      pm.response.to.have.status(200);
  });
  ```
- **Request Body:**
  ```json
  {
      "Permanent_Address": {
          "House_Number": "{{House_Number}}",
          "City": "{{City}}",
          "State": "{{State}}",
          "Country": "{{Country}}",
          "PhoneNumber": {
              "Mobile": "{{Mobile}}"
          }
      },
      "stId": {{id}}
  }
  ```
- **Response Body:**
  ```json
  {
      "status": "success",
      "message": "Address added successfully"
  }
  ```

---

#### **6. Validate Final Student Details**
- **Request Method:** GET  
- **Request URL:** `{{base_url}}/api/FinalStudentDetails/{{id}}`  
- **Pre-request Script:** None  
- **Post-request Script:**
  ```javascript
  pm.test("Response status code is 200", function () {
      pm.response.to.have.status(200);
  });

  pm.test("Verify technical skills", function () {
      const jsonData = pm.response.json();
      pm.expect(jsonData.TechnicalDetails[0].language).to.eql(["Java", "Python"]);
  });

  pm.test("Verify address details", function () {
      const jsonData = pm.response.json();
      pm.expect(jsonData.Address[0].Permanent_Address.City).to.eql("New York");
  });
  ```
- **Request Body:** None  
- **Response Body:**
  ```json
  {
      "id": 12345,
      "first_name": "John",
      "last_name": "Smith",
      "TechnicalDetails": [
          {
              "language": ["Java", "Python"],
              "yearexp": "3 years"
          }
      ],
      "Address": [
          {
              "Permanent_Address": {
                  "City": "New York"
              }
          }
      ]
  }
  ```


## Run Command:  
- Run Command for Console: 
```console 
newman run LS_Shormi.postman_collection.json -e LS_Shormi.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run LS_Shormi.postman_collection.json -e LS_Shormi.postman_environment.json -r cli,htmlextra
```


## Newman Report Summary:
![image](https://github.com/user-attachments/assets/112e372c-5da9-42a1-9d80-10721c4a90a2)
![image](https://github.com/user-attachments/assets/73698f80-354e-428a-b1af-866441554f75)
![image](https://github.com/user-attachments/assets/79642c59-a60d-4060-8470-1a8c445f973c)
![image](https://github.com/user-attachments/assets/9335583c-067d-42b1-a23c-218dab08a5fa)
