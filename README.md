# PostSystem - Angular 9 Example With PHP 7.2 & MySQL, By Matan Aviav

This repository contains an example of a full responsive dynamic Angular 9 system with PHP 7.2 & MySQL database, built by me (Matan Aviav).

## Table of contents:
1. Introduction

2. Tools I used in this project

3. Originality

4. Security

5. Live demo

<br/><br/>
## 1. Introduction:
I wanted to build an example of a full responsive dynamic Angular 9 web system with PHP & MySQL database. <br/>
Finally, I decided to build a small web system for posts named ***PostSystem.*** <br/>
With the system, users can add new posts to their own private dashboard.<br/><br/>


### Screenshots of the system:
![GitHub Logo](https://i.ibb.co/NTKYRQx/1.png)

<br />

![GitHub Logo](https://i.ibb.co/w6MX9Sz/2.png)

<br />

![GitHub Logo](https://i.ibb.co/wKwJty8/3.png)

<br />

![GitHub Logo](https://i.ibb.co/WHMKvfj/4.png)

<br />

![GitHub Logo](https://i.ibb.co/gyKHMrx/5.png)

<br />

![GitHub Logo](https://i.ibb.co/K0ZjxxG/6.png)


<br />



## 2. Tools I used in this project:


To write the code I used Visual Studio Code.
To run Angular 9 I used Node.js server environment for Win64 and to run PHP (version 7.2) I used WampServer with built-in MySQL server.
In this project I used: Angular 9, RxJS, FontAwsome, jQuery, JavaScript, Bootstrap 4, CSS, HTML5, PHP, SQL.

* ***Angular 9:***<br/>
Angular 9 is the dominant tool in this project. I built the whole project according it. I wrote Angular 9 in TypeScript and then preform a deployment process in order to convert TypeScript to JavaScript. Mainly, I used Angular to navigate between pages instead of reload pages in every request, and for using complex components. Of course, I exploited the main feature that Angular offers: asynchronic code. In order to send POST/GET requests, I decided to use 'HttpClient' module and work with Observables (RxJS), according to official Angular recommendation. In order to navigate between components, I used RouterModule and I defined number of Routes arrays. I decided to create number of modules in order to acheive 'Lazy Module Loading'. 


* ***jQuery & Vanilla JavaScript & RxJS:***<br/>
I used jQuery in order to apply some animated effects during the use in the website. Vanilla JavaScript has many uses. For example: adapt the css grid view to the size of the screen. In addition, I used BehaviorSubject of RxJS.

* ***Bootstrap 4 & CSS:***<br/>
I used Bootstrap 4 in order to style the website.

* ***PHP & SQL:***<br/>
I used PHP (version 7.2) in order to handle all requests from client-side like: register, login, add posts, show all posts, and a lot more. PHP is the core of the backend here. The idea is simple: when the user perform an action then Angular send a POST or GET request to the backend (PHP). When a specific PHP file get the request, it handles it and return the result back to Angular. According to the result returned, Angular performs a specific action. I used PHP to connect to MySQL server, and to write/get data to/from the database. I decided to use PDO PHP class to connect to MySQL server. SQL, of course, is used to prepare SQL statements in order to get data and to insert data. 

  MySQL tables structures:<br />
  ```
  CREATE TABLE IF NOT EXISTS `posts` (
    `id` bigint(20) UNSIGNED NOT NULL AUTO_INCREMENT,
    `user_id` bigint(20) NOT NULL,
    `title` varchar(265) NOT NULL,
    `body` varchar(1100) NOT NULL,
    `date` date NOT NULL,
    PRIMARY KEY (`id`),
    KEY `user_id` (`user_id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=193 DEFAULT CHARSET=utf8;

  CREATE TABLE IF NOT EXISTS `users` (
    `id` bigint(20) NOT NULL AUTO_INCREMENT,
    `username` varchar(35) NOT NULL,
    `pass` varchar(255) NOT NULL,
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;
  ```
  
* ***HTACCESS:***<br/>
In order to complete a good deployment process, I had to create a .HTACCESS file in order to change the server configuration.
The reason for that: any Angular route is not considered as a valid path by the server, because for any route there is no an existing file,
and when the user try to navigate to any route directly from the url, then he will get a 404 error.

  The following .HTACCESS code solves the problem:
  ```
  RewriteEngine On
  # If an existing asset or directory is requested go to it as it is
  RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -f [OR]
  RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -d
  RewriteRule ^ - [L]

  # If the requested resource doesn't exist, use index.html
  RewriteRule ^ /index.html
  ```
<br />


## 3. Originality:
All PHP files and TypeScript files have written by me (MatanAviav). I wanted to keep on originality during the whole project process.



<br />


## 4. Security:
In order to secure the system, I needed to take control on the data the user insert to the database, and also take control on the data that comes from the database. In addition, I had to find a way to make the user authentication method secure and to find a way to secure sensitive user data like passwords.

  1. *User Authentication:<br />*
  In order to make a secure user authentication, I decided to use a token-based authentication with JWT (JSON Web Tokens). In every successful login, the system take a specific action according to the user choice if to save the connection or not. If the user decided NOT to save the connection then all of his connections details will be saved in SERVER SESSION. If the user DID decided to save the connection then an unique JWT token will be created and will be saved in a httpOnly cookie. The decision to save the cookie as httpOnly is to prevent the access to document.cookie by hackers in case of XSS attack. When the user closes the browser and come back to the system, then the system automatically check the validity of the JWT token. Simple.
 
  2. *User Input (XSS and SQL Injection):<br />*
  In order to secure user input, I started with validators of 'ReactiveFormsModule' (based on regular expressions) for any input. But that way is not enough, of course, because any user can edit the code and remove the validators because the code is on client-side. To solve that, I had to add my own validators to the backend. In PHP files that receive POST/GET data from any Angular request, I took any input and checked whether its valid or not, using regular expressions and other techniques. That way, I took control of the user input but I it's still not enough. In order to fully secure user input when insert/get data to/from the database, I had to prevent SQL INJECTION. So, in order to prevent SQL INJECTION, I used prepared statements instead of regular queries. That way, I took full control of the user input. Of course, every problematic operation with the database, input rules, or working with files - is wrapped with Try & Catch blocks.
  
  3. *Data Comes From The Database (XSS):<br />*
  By default, Angular prevent XSS, but I needed to converts break-lines to <br /> tags. In order to do that, first the I used htmlspecialchars() function, and then I convert every line-break to <br /> tags. Note that in Angular html template I used 'innerHTML'.
  
  4. *User Password:<br />*
  In order to secure user password in the database, I decided to use password_hash() PHP function, according to the recommendation in the official PHP documentation. I used this function in order to hash the password, with 2 SALT strings, and when the user try to login I used password_verify() PHP function to verify the password. The advantage of this technique is that password_hash() function does not create a CONSTANT hash - it can change in every function call.

  5. *Prevent CSRF Attacks:<br />*
  In order to prevent CSRF attakcs, I built a token-based mechanism to provide the user a token for any protected action he does in the website. The process is simple: the user gets a token from the backend, and when the user try to send a request from the client-side to the backend, the token will be sent automatically as an argument in the request. Then, when Angular gets the response, a new token fetched from the it (the response) and save it in 'AuthService' for the next request. That way, there is no possible way a hacker can manipulate a user to do any malicious action, because a hacker can't guess the token. That way, I can know for sure what is the source of the request.
  
  6. *Prevent Server Flooding:<br />*
  In order to prevent a lot of requests in very short time period (requests flooding), I built a mechanism to handle that. I wanted to block clients and logged users from sending too many requests to the backend. Note that the mose expensive operations are related to the database jobs. I had to save information about any client/user in order to recognize the client/user. I thought about saving their details in the database, but I wanted to prevent database flooding too. Finally, I decided to use files for that: for every suspicious client/user, I create a file and save into it an IP address or an user id number. In addition to that detail, I also save the time of the last request. In short: the mechanism works great.
 
<br />

## 5. Live demo:
To see a live demo of the system, please go to: http://postsystem.myartsonline.com
