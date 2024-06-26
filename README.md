 # BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Yes, but not really mandatory. Using an interface for the Subscriber in Observer design pattern in the BambangShop case can give some numerous advantages. It increases abstraction and separates the subject from its observers, making testing easier and enhancing both flexibility and scalability. Also, by using interfaces, we can maintain consistency and mandate particular behaviors for Subscribers. However, in current condition, since Subscriber only have some simple behavior like there's only one type of subscriber, using a single Model struct to represent subscribers is enough.

2. While DashMap is behave like regular HashMap in Java, Vec is more act like a List. I think it's better to use DashMap instead of vec. DashMap can guarantee uniqueness and provide more efficient lookup operation using that unique key like this example with id and program. Vec however, we must iterate over the entire list to check if there's some duplicate and thus will not be as efficient as DashMap. Therefore, using a DashMap would be more efficient, providing both integrity and performance to the system.

3. Singleton is not really good when using rust. This is because rust's concept like of borrowing ownership and multiple shared of mutable state required additional synchronization mechanisms like Mutex or RwLock to ensure thread safety. Meanwhile, DashMap already have some built in concurrency and efficient lookup method and of course, fully integrated with rust. So it's better if we use DashMap for implementation instead of implementing Singleton

#### Reflection Publisher-2
1. Separating Service and Repository is to apply SRP Principle in our project. Each layer will only have one responsibility so they can focus on specific task. Service will focus on handling appliaction logic while Repository will focus on handling data usage. This will increased maintanibility and flexibility because each layer can be extended or improved independently and not affect other layer.

2. Each Model will have to handle their application logic and data handling themselves. This will result in complex and long code and will be hard to maintenance because Model have too much responsibility instead of one. There also be many code duplication lack of clarity so furthermore will increase difficulty of maintenance.

3. Postman is a good tool for testing API and other development request. Postman also allowed some useful features like automated testing, collaboration tools, monitoring, and mock servers to help developer while managing their work. Postman also have multiple environment features which allow us to do some testing using multiple enviroment and condition. 

#### Reflection Publisher-3
1. We are using push model in this tutorial because it uses push data to send notification if user invoke update method. The delivery of the notification is also directly pushes to Subscriber so it's matched with Push Model.

2. By using pull model, Subscriber will only request or pull data from Publisher when it's needed. In some condition, it may reduce many data retrival or some overhead retrival. Using this model however, will increased implementation because we have to logic for pulling that data while also exposed the Subscriber to the Publisher which can cause some security issue.

3. The process of sending notification will not efficient and will take longer time. Without using multi threading, the notification will be delivered sequentially which will caused delay and inefficiency. The disadvantage and delay will increase exponentially if we have many more subsriber in our application.