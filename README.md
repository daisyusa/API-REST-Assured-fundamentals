# REST-Assured-fundamentals

# REST Assured Usage
https://rest-assured.io/

https://github.com/rest-assured/rest-assured/wiki/GettingStarted

https://github.com/rest-assured/rest-assured/wiki/Usage 

# Tutorial
https://testautomationu.applitools.com/automating-your-api-tests-with-rest-assured/

https://github.com/basdijkstra/tau-rest-assured/tree/master


## Contents
1: Introduction to REST Assured

2: Basic Features and Capabilities
    
    --LOG

3: Parameterizing Your REST Assured Tests
    
    -- Data-Driven-Testing
    -- DataProvider

4: Optimizing Your REST Assured Code

    -- Reusability
      - RequestSpecification
      - ResponseSpecification

5: Working with XML Responses

    -- Use XML Path

6: (De) Serialization of Java Objects in REST Assured Tests 

       1. Serialization (Java Object ➡️ JSON)
     - Serialization is the process of converting a Java Object into a JSON string (or XML) to send as the body of a POST, PUT, or PATCH request.
     
    How it works:
    
    First, you define a plain Java class (often called a POJO - Plain Old Java Object):
    java
    public class User {
        private String name;
        private String job;
    
        // Constructors, Getters, and Setters
        public User(String name, String job) {
            this.name = name;
            this.job = job;
        }
    }
    Then, you pass the object directly into Rest Assured's .body(). Rest Assured serializes it automatically:
    java
    User userPayload = new User("John", "QA Engineer");
    
    RestAssured.given()
        .contentType("application/json")
        .body(userPayload) // <--- Serialization happens right here
    .when()
        .post("https://reqres.in")
    .then()
        .statusCode(201);


        
    2. Deserialization (JSON ➡️ Java Object)
    
    Deserialization is the reverse process. It takes the JSON response body sent back by the server and converts it directly into a Java Object so you can easily extract and assert data.
    
    How it works:
    
    Instead of using complex JsonPath strings to parse the response, you extract the entire response directly into your Java class using .as():
    java
    UserResponse responseData = RestAssured.given()
    .when()
        .get("https://reqres.in")
    .then()
        .extract()
        .as(UserResponse.class); // <--- Deserialization happens right here
    
    // Now you can use standard Java getters to assert your data
    System.out.println(responseData.getName()); 
            -- deserialization
             - Deserializing an API Response to a Java Object


    Behind the Scenes: The Engines
    
    Rest Assured does not convert the text by itself. It relies on a data-binding library under the hood. For this to work seamlessly, you must add one of these dependencies to your pom.xml or build.gradle:
    • Jackson (Most common / Recommended)
    • Gson
    • Johnzon
    If Jackson or Gson is present on your project's classpath, Rest Assured will automatically detect it and handle the serialization/deserialization without any extra configuration.
