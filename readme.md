# Creating the MySQL Database and Schema

## Access MySQL

1. **Open a terminal or command prompt.**
2. **Log in to your MySQL server using the MySQL client.** Replace `username` with your actual MySQL username:
   ```sh
   mysql -u username -p
   ```
3. **Enter your password when prompted.**

## Create the Database

1. **Once logged in, create a new database for your application.** Replace `database_name` with your desired database name:
   ```sql
   CREATE DATABASE database_name;
   ```

## Use the Database

1. **Select the newly created database to use it:**
   ```sql
   USE database_name;
   ```

## Create the Schema

You can create tables and define their schema directly from your Flask application using SQLAlchemy, or you can manually create the tables using SQL statements.

Here are the SQL statements for creating the `User` and `Incident` tables based on your models:

CREATE TABLE Users (
user_id INT AUTO_INCREMENT PRIMARY KEY,
username VARCHAR(50) NOT NULL,
password VARCHAR(255) NOT NULL,
email VARCHAR(100) NOT NULL,
role VARCHAR(20) NOT NULL,
UNIQUE (username),
UNIQUE (email)
);

CREATE TABLE Incidents (
incident_id INT AUTO_INCREMENT PRIMARY KEY,
title VARCHAR(100) NOT NULL,
description TEXT NOT NULL,
status VARCHAR(20) NOT NULL,
reported_by INT,
assigned_to INT,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
FOREIGN KEY (reported_by) REFERENCES Users(user_id) ON DELETE SET NULL,
FOREIGN KEY (assigned_to) REFERENCES Users(user_id) ON DELETE SET NULL,
INDEX (reported_by),
INDEX (assigned_to)
);

CREATE TABLE Responses (
response_id INT AUTO_INCREMENT PRIMARY KEY,
incident_id INT NOT NULL,
response_text TEXT NOT NULL,
responded_by INT,
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
FOREIGN KEY (incident_id) REFERENCES Incidents(incident_id) ON DELETE CASCADE,
FOREIGN KEY (responded_by) REFERENCES Users(user_id) ON DELETE SET NULL,
INDEX (incident_id),
INDEX (responded_by)
);

    ```
