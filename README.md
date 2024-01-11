# Spring Boot REST API with JPA, MySQL, and One-to-One Mapping

This README file provides a comprehensive guide for setting up a Spring Boot project with REST API endpoints for CRUD operations, MySQL database connectivity using JPA and MySQL Connector, and implementing One-to-One mapping between two entity classes.

## Prerequisites

Before you begin, ensure you have the following installed:

### Java Development Kit (JDK)
### Eclipse IDE (or any preferred IDE)
### MySQL Database Server

## Step 1: Create a Spring Boot Project

- Open Eclipse IDE and go to File -> New -> Spring Starter Project.

- Enter the project name, select the desired package, and include the dependencies: Spring Web, Spring Data JPA, and MySQL Driver.

- Click Finish to create the project.

## Step 2: Configure MySQL Database

Open the src/main/resources/application.properties file and configure the MySQL database connection properties:

    spring.datasource.url=jdbc:mysql://localhost:3306/your_database_name
    spring.datasource.username=your_username
    spring.datasource.password=your_password
    spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5Dialect
    spring.jpa.hibernate.ddl-auto=update

Replace your_database_name, your_username, and your_password with your actual MySQL database details.

## Step 3: Implement REST API Endpoints

Create a package for your controllers (e.g., com.example.demo.controller) and create a class for your REST API endpoints similar to the previous example.

## Step 4: Add External MySQL JAR to Eclipse Build Path

- Download the MySQL Connector JAR file from the official MySQL website.

- In Eclipse, right-click on your project and select Build Path -> Configure Build Path.

- Go to the Libraries tab, click Add External JARs, and select the downloaded MySQL Connector JAR file.

- Click Apply and Close to save the changes.

## Step 5: Implement Bidirectional Mapping

Create another entity class for bidirectional mapping (e.g., Post):

    @Entity
    public class Post {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String title;
        @ManyToOne
        @JoinColumn(name = "user_id")
        @JsonBackReference
        private User user;
        // Getters and setters
    }

Update the existing entity class (e.g., User) to include a reference to the new class:


    @Entity
    public class User {

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String username;  
     
        @OneToMany(mappedBy = "user", cascade = CascadeType.ALL)
        @JsonManagedReference
        private List<Post> posts;
  
        // Getters and setters
    }

- This establishes a bidirectional many-to-one mapping from Post to User and a one-to-many mapping from User to Post.

- Modify the controller to handle bidirectional mapping operations.

Now, your Spring Boot project has REST API endpoints, JPA for data persistence, MySQL as the database, and bidirectional mapping implemented between User and Post entities. The @JsonManagedReference and @JsonBackReference annotations help prevent infinite recursion when serializing entities to JSON. Customize the project according to your specific requirements.
