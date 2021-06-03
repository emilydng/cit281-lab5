## Lab 5

``` markdown
Goals and Outcomes

  - Download and install Postman
  - Create a CIT 281 collection and folders
  - Create a Node.js and fastify server application with GET and respond with JSON
  - Add array of students object
  - Use Postman to test server GET routes
  - Add POST handling to server application and respond with JSON
  - Use Postman and POST request

```

fastify-server.js

```rouge
  /*
    CIT 281 Lab 5
    Name: Emily Deng
  */

  const students = [
      {
        id: 1,
        last: "Last1",
        first: "First1",
      },
      {
        id: 2,
        last: "Last2",
        first: "First2",
      },
      {
        id: 3,
        last: "Last3",
        first: "First3",
      }
    ];

  // Require the Fastify framework and instantiate it
  const fastify = require("fastify")();
  // Handle GET verb for / route using Fastify
  // Note use of "chain" dot notation syntax

  // Student Route
  fastify.get("/cit/student", (request, reply) => {
    reply
      .code(200)
      .header("Content-Type", "application/json; charset=utf-8")
      .send(students);
  });

  //Student ID route
  fastify.get("/cit/student/:id", (request, reply) => {
      // 1) Client makes a request to the server
      // Get the ID
      let studentIDFromClient = request.params.id;

      // 2) The server (us) does something with the request
      // Get the student associated with the ID given from the array called 'students'
      let studentRequestedFromClientID = null;

      for (studentInArray of students) {
          if (studentInArray.id == studentIDFromClient) {
              studentRequestedFromClientID = studentInArray;
              break;
          }
      }

      // 3) The server provides some sort of reply to the client
      // Send the student data found in (2) to the client
      if (studentRequestedFromClientID != null) {
          reply
          .code(200)
          .header("Content-Type", "application/json; charset=utf-8")
          .send(studentRequestedFromClientID);
      }
      else {
          reply
          .code(404)
          .header("Content-Type", "text/html; charset=utf-8")
          .send("<h1>No student with the given ID was found</h1>");
      }
  });

  // unmatched / wildcard route
  fastify.get("*", (request, reply) => {
      reply
        .code(200)
        .header("Content-Type", "text/html; charset=utf-8")
        .send("<h1>At Unmatched Route</h1>");
    });

  // Add a student
  fastify.post("/cit/students/add", (request, reply) => {
      // 1) Get request from client
      let userData = JSON.parse(request.body)
      console.log(userData);

      // 2) Do something with that request
      // (a) Get the max id from the current array
      let maxID = 0;
      for (individualStudent of students) {
          if (maxID < individualStudent.id) {
              maxID = individualStudent.id;
          }
      }

      // (b) create a new student object, composed of userData and max id + 1
      generatedStudent = {
          id: maxID + 1,
          last: userData.lname,
          first: userData.fname,
        };
      // (c) Insert the student object created in (2) into our 'students' array
      students.push(generatedStudent);

      // (d) We want to return the object created in (2) back to the client

      // Reply to client

      reply
        .code(200)
        .header("Content-Type", "appplication/json; charset=utf-8")
        .send(generatedStudent);
    });

  // Start server and listen to requests using Fastify
  const listenIP = "localhost";
  const listenPort = 8080;
  fastify.listen(listenPort, listenIP, (err, address) => {
    if (err) {
      console.log(err);
      process.exit(1);
    }
    console.log(`Server listening on ${address}`);
  });

```

*All Students*

<img width="1227" alt="AllStudents" src="https://user-images.githubusercontent.com/84113983/120713924-6afc3d00-c477-11eb-9da3-3f940e06bd47.png">

*Single Students*

<img width="1227" alt="SingleStudent" src="https://user-images.githubusercontent.com/84113983/120713930-6c2d6a00-c477-11eb-9a0e-ebd52e7b3b9a.png">

*Student Post*

<img width="1209" alt="StudentPost" src="https://user-images.githubusercontent.com/84113983/120713936-6cc60080-c477-11eb-8e9e-d706cbf8a55a.png">

*Unmatched*

<img width="1227" alt="Unmatched" src="https://user-images.githubusercontent.com/84113983/120713937-6d5e9700-c477-11eb-9c13-94efafa4e3a5.png">


