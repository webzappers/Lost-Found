**\# ONLINE LOST & FOUND SYSTEM**

**\# ONLINE LOST & FOUND SYSTEM**

**\#\# Group Information**

**\*\*Group Name\*\***: G8 \- qwerty  
**\*\*Section\*\***: 5

**\*\*Group Members\*\*** :  
\- NOOR SHEEREEN IMAN BINTI ZAINAL ABIDIN \- 2315342  
\- NUR AIN BINTI MAHATHIR \- 2312272  
\- NUR AINI HANI BINTI SHOCKRAVARATY \- 2319380  
\- NUR HAYANI IZZATI BINTI MOHD AKHERI \- 2311140

**\#\# Project Overview**

Introduction :

Losing personal belongings is a common issue among university students and often leads to inconvenience and wasted time. At the International Islamic University Malaysia (IIUM), the absence of a centralized lost and found platform means that students rely on informal methods such as social media or word-of-mouth, which are inefficient and unstructured.

WEBZAPPER is a web-based Online Lost & Found System developed to provide a centralized solution for the IIUM community. The system allows users to report lost or found items by submitting details such as descriptions, images, locations, and dates, while administrators verify submissions and manage item matching and claims. By integrating essential web application features such as authentication, CRUD operations, and role-based access control, WEBZAPPER improves efficiency, reliability, and communication within the campus.

**\#\# Project Objectives**

\- Primary Goal: Develop a centralized Online Lost & Found System to help the IIUM community report, locate, and recover lost items efficiently  
\- Technical Goal: Implement a functional web application using Laravel MVC architecture with full CRUD operations, database integration, and role-based access control  
\- User Experience Goal: Provide a simple, intuitive, and responsive interface that allows users to easily submit, search, and manage lost and found item reports  
\- Business Goal: Enable administrators to verify submissions, manage item matching and claims, and oversee the return process to ensure data accuracy and system reliability

**\#\# Target Users**

\- Students and Staff (Users): Users who report lost items, view found items, and submit claims  
\- Item Finders: Individuals who find lost items on campus and report them through the system  
\- Administrators: System managers who verify reports, manage item matching and claims, and oversee platform operations

**\#\# Features and Functionalities**

\*\* User Features\*\*

\- User Registration & Login: Users can create accounts and log in securely to access system features  
\- Report Lost Items: Users can submit lost item reports by providing item descriptions, images, locations, and dates  
\- Report Found Items: Users who find items can submit found item reports with relevant details and photos  
\- View and Search Items: Users can browse and search lost and found items based on category, location, or keywords  
\- Item Claim Submission: Users can submit claims for items they believe belong to them.  
\- Claim Status Tracking: Users can view the status of their submitted claims (pending, approved, or rejected)  
\- Profile Management: Users can manage their account information and view their submitted reports

**\*\*Admin Features\*\***

\- Admin Authentication: Administrators can log in securely to access admin-only features  
\- Item Verification: Admins review and verify lost and found item submissions before making them visible  
\- Item Management: Admins can view, update, and manage item records within the system  
\- Claim Review: Admins review submitted claims and approve or reject them based on provided information  
\- System Monitoring: Admins oversee overall system activity to ensure proper usage and data accuracy

**\#\# Technical Implementation**

\*\* Technology Stack\*\*

\- Backend Framework: Laravel   
\- Frontend: Blade Templates with Tailwind CSS  
\- Database: MySQL   
\- Authentication: Laravel built-in authentication  
\- Image Storage: Laravel File Storage  
\- Development Environment: XAMPP

\*\* Database Design\*\*

Database Schema Overview  
The system database is designed to support the reporting, verification, matching, and claiming of lost and found items within the campus environment. The database uses a relational structure to ensure data consistency and efficient retrieval of information.  
Core Tables:

\- users \- Stores registered user and admin accounts.  
\- items \- Stores information about lost and found items, including descriptions, images, locations, dates, and item status  
\- categories \- Classifies items into predefined categories   
\- claims \- Stores user claims for reported items along with claim status and ownership evidence  
\- matches \- Records potential matches between lost and found items, managed by administrators

**\#\#\# Entity Relationship Diagram (ERD)**

https://docs.google.com/document/d/1qarcS76sN\_zzYpTvxxGO1UEtF4B3ngMpic7hYQsawTw/edit?usp=sharing

Key Relationships:

\- Users can report multiple items (One-to-Many)  
\- One Category can have many items (One-to-Many)  
\- One Item(lost or found) can be involved in multiple Matches (One-to-Many)  
\- One Admin can verify multiple Matches (One-to-Many)  
\- One Match can have multiple Claims (One-to-Many)  
\- One User can send and receive multiple Messages (One-to-Many)  
\- One Match can have multiple Messages (One-to-Many)

\*\* Laravel Components Implementation\*\*

\- Routes (Web.php)  
   
\<?php

use Illuminate\\Support\\Facades\\Route;  
use App\\Http\\Controllers\\ItemController;  
use App\\Http\\Controllers\\ClaimController;

// Main routes for a lost and found app  
Route::get('/', function () {  
    // Welcome page  
    return view('welcome');  
});

// Public filters for lost/found items  
    Route::get('/items/filter/lost', \[ItemController::class, 'lost'\])-\>name('items.lost');  
    Route::get('/items/filter/found', \[ItemController::class, 'found'\])-\>name('items.found');

// Authenticated routes (require login)  
    Route::middleware(\['auth:sanctum', config('jetstream.auth\_session'), 'verified'\])-\>group(function () {  
        //Claim item routes  
        Route::get('/claim-item/{item}', \[ClaimController::class, 'create'\])-\>name('claims.create');  
Route::post('/claim-item/store', \[ClaimController::class, 'store'\])-\>name('claims.store');

    //Dashboard  
    Route::get('/dashboard', \[ItemController::class, 'index'\])-\>name('dashboard');

    //Create/store items  
    Route::get('/items/create', \[ItemController::class, 'create'\])-\>name('items.create');  
    Route::post('/items', \[ItemController::class, 'store'\])-\>name('items.store');

    //Claim via item ID  
    Route::post('/items/{item}/claim', \[ClaimController::class, 'store'\])-\>name('claims.store');  
    Route::get('/items/{item}/claim', \[ClaimController::class, 'create'\])-\>name('claims.create');  
    Route::post('/items/{item}/claim', \[ClaimController::class, 'store'\])-\>name('claims.store');

    //Update claim status  
    Route::patch('/claims/{claim}/{status}', \[ClaimController::class, 'update'\])-\>name('claims.update');

    //General claim routes  
    Route::get('/claims/create', \[ClaimController::class, 'create'\])-\>name('claims.create');  
    Route::post('/claims', \[ClaimController::class, 'store'\])-\>name('claims.store');

    //Edit contact profile  
    Route::get('/profile/contact', function() {  
        return view('profile.edit-contact');  
    })-\>name('profile.contact.edit');

    Route::get('/dashboard', \[ItemController::class, 'index'\])-\>middleware(\['auth'\])-\>name('dashboard');  
    Route::get('/dashboard', \[ItemController::class, 'index'\])-\>name('dashboard');

    //CRUD for items  
    Route::resource('items', ItemController::class);

    //Lost/found filters  
    Route::get('/items/lost', \[ItemController::class, 'lost'\])-\>name('items.lost');  
    Route::get('/items/found', \[ItemController::class, 'found'\])-\>name('items.found');

    //CRUD for items  
    Route::resource('items', ItemController::class);

    //Store claim  
    Route::post('/claims', \[ClaimController::class, 'store'\])-\>name('claims.store');

    //User’s own items  
    Route::get('/my-activity', \[ItemController::class, 'myItems'\])-\>name('my.items');

    //Delete item  
    Route::delete('/items/{id}', \[App\\Http\\Controllers\\ItemController::class, 'destroy'\])-\>name('items.destroy');

    //Update claim status  
    Route::patch('/claims/{claim}/{status}', \[ClaimController::class, 'update'\])-\>name('claims.update');

    //Update contact  
    Route::patch('/profile/contact', \[ProfileController::class, 'updateContact'\])-\>name('profile.contact.update');  
});

\- Controllers  
   
  *\*Main Controllers Implemented are below :\**

  1\. ItemController: Handles lost and found item reporting, listing and item details  
  2\. ClaimController: Manages ownership claim creation, submission and status update  
  3\. ProfileController: Allow users to update their contact information for claim communication

\- Models and Relationships  
   
php// User Model  
class User extends Authenticatable {  
    public function claims() {  
    return $this-\>hasMany(Claim::class);  
    }  
}

// Claim Model    
class Claim extends Model {  
     public function item()  
    {  
        return $this-\>belongsTo(Item::class);  
    }

    public function user()  
    {  
        return $this-\>belongsTo(User::class);  
    }  
}

// Item Model  
class Item extends Model {  
    public function claims() {  
        return $this-\>hasMany(Claim::class);  
    }

    public function user()  
    {  
        return $this-\>belongsTo(User::class);  
    }  
}

\- Views and User Interface

  *\*Blade Templates Structure:\**  
  \- layouts/app.blade.php \- Main application layout  
  \- dashboard.blade.php \- Dashboard of the application  
  \- navigation-menu.blade.php \- Navigation menu of the application  
  \- items/index.blade.php \- Display lost and found item  
  \- items/show.blade.php \- Display item details  
  \- items/create.blade.php \- Form to report lost or found items  
  \- claim/create.blade.php \- Claim submission form  
  \- profile/edit.blade.php \- User profile and contact information

   *\*Design Features:\**  
   \- Responsive Design: Bootstrap-based layout for accessibility on different devices  
   \- Color Scheme: Modern Green, white and blue theme representing trust, safety and clarity  
   \- Navigation: Intuitive navigation menu with user role-based options  
   \- Interactive Elements: Lost and found item filtering, Claim submission and Status updates

**\#\# User Authentication System** 

**\#\#\# \*\* Authentication Features\*\***  
\- **\*\*Registration System\*\***: Email validation, password confirmation  
\- **\*\*Login System\*\***: Secure authentication with "Remember Me" option  
\- **\*\*Password Reset\*\***: Email-based password recovery system  
\- **\*\*Profile Management\*\***: Users can update their contact information  
\- **\*\*Authentication-based Access\*\***: Certain features are restricted to authenticated and verified users.

**\#\#\# \*\*Security Measures\*\***  
\- Password encryption using Laravel's built-in hashing mechanism  
\- CSRF protection on all forms  
\- Input validation and sanitization  
\- Middleware protection for authenticated routes

**\#\# Installation and Setup Instructions**  
**\#\#\# Prerequisites :**  
\- PHP \>= 8.1  
\- Composer  
\- Node.js and NPM  
\- MySQL 8.0  
\- XAMPP  
\- Web browser

**\#\#\# Step-by-Step Installation**

1\. Clone the Repository

bash/n  
git clone https://github.com/webzappers/Lost-Found.git
cd Lost-Found

2\. Install Dependencies

bashcomposer install  
npm install

3\. Environment Configuration

bashcp .env.example .env  
php artisan key:generate

4\. Database Setup

bash\# Configure database in .env file  
php artisan migrate  
php artisan db:seed

5\. Start Development Server

bashphp artisan serve  
npm run dev

**\#\# Testing and Quality Assurance**

**\#\#\#  Functionality Testing**

 \- User registration and login system  
 \- Reporting lost items with images and details  
 \- Reporting found items  
 \- Viewing and searching lost and found items  
 \- Item claim submission by users  
 \- Claim status tracking (pending, approved, rejected)  
 \- Admin verification of items  
 \- Admin approval and rejection of claims  
 \- Profile and contact information update  
 \- Role-based access control (user/admin)  
 \- Image upload and display functionality

**\#\#\# Browser Compatibility**

\-  Google Chrome (Latest)  
\-  Mozilla Firefox (Latest)  
\-  Safari (Latest)  
\-  Microsoft Edge (Latest)

**\#\#\# Performance Testing**

\- Page load times under 3 seconds  
\- Database queries optimized  
\- Image compression implemented  
\- Responsive design tested on multiple screen sizes

**\#\# Challenges Faced and Solutions**  
**\#\#\# Challenge 1: Managing Lost and Found Item Data**  
\- Problem: Organizing lost and found item records while maintaining clear relationships between users, items, and claims  
\- Solution: Designed a structured database schema and implemented CRUD operations using Laravel models and controllers  
**\#\#\# Challenge 2: Image Upload and Storage**  
\- Problem: Handling image uploads for lost and found item reports without causing storage or display issues  
\- Solution: Used Laravel’s file storage system to manage image uploads and ensure proper validation and retrieval  
**\#\#\# Challenge 3: Role-Based Access Control**  
\- Problem: Ensuring that only administrators could access verification and management features  
\- Solution: Implemented authentication checks and protected routes to separate user and admin access

**\#\# Future Enhancements**  
**\#\#\# Phase 2 Features (Potential Improvements)**  
\- **\*\*Automated Matching System\*\***: Suggest matches between lost and found items automatically  
\- **\*\*Notifications\*\***: Notify users when items are matched or claims are updated  
\- **\*\*Enhanced Search\*\*** : Improve search with filters such as date and location  
\- **\*\*Mobile Optimization\*\*** : Further optimize the system for mobile use or develop a mobile app  
\- **\*\*Activity Logs\*\*** : Record administrative actions for transparency and security

**\#\#\# Scalability Considerations**

\- Optimize database queries for larger datasets  
\- Implement caching to improve system performance  
\- Develop APIs to support future mobile or external system integration

**\#\# Learning Outcomes**  
**\#\#\# Technical Skills Gained**

\- Laravel Framework: Understanding of MVC architecture and Eloquent ORM  
\- Database Design: Designing relational database schemas and implementing CRUD operations  
\- Authentication: Implementing user login, registration, and protected routes  
\- Frontend Development: Building responsive user interfaces using Blade templates and Tailwind CSS  
\- Version Control: Using Git and GitHub for project management

**\#\#\# Soft Skills Developed**

\- **\*\*Team Collaboration\*\*** : Working effectively in a group to develop a web application  
\- **\*\*Project Planning\*\*** : Organizing tasks and managing development timelines  
\- **\*\*Problem Solving\*\*** : Identifying and resolving technical and implementation issues  
\- **\*\*Documentation Skills\*\*** : Preparing clear and structured project documentation

**\#\# References**

1\. Laravel Documentation. (2024). Laravel 10.x Documentation. Retrieved from https://laravel.com/docs/10.x  
2\. Bootstrap Documentation. (2024). Bootstrap 5.3 Documentation. Retrieved from https://getbootstrap.com/docs/5.3/  
3\. MySQL Documentation. (2024). MySQL 8.0 Reference Manual. Retrieved from https://dev.mysql.com/doc/refman/8.0/en/  
4\. MDN Web Docs. (2024). Web Development Resources. Retrieved from https://developer.mozilla.org/  
5\. Stack Overflow. (2024). Programming Q\&A Platform. Retrieved from https://stackoverflow.com/

**\#\# Conclusion**  
We have successfully demonstrated the development of a functional Online Lost & Found System using the Laravel framework. The project showcases an understanding of fundamental web development concepts, including MVC architecture, database design, user authentication, and responsive user interface design.

**\#\#\# Key Achievements**

\- Successfully implemented core Laravel components, including routes, controllers, models, and views  
\- Developed a functional Online Lost & Found System with user and admin role management  
\- Created a responsive and user-friendly interface using Blade templates and Tailwind CSS  
\- Designed and implemented database relationships to support item reporting, claims, and administration using CRUD operations  
\- Applied basic security practices for user authentication and protected routes within the system

**\#\#\# Project Impact**  
This project provides hands-on experience in developing a real-world web application using Laravel and applying core web development concepts. It also demonstrates effective teamwork and practical problem-solving skills that are relevant to academic and professional web development settings.

\- Project Completion Date: 11/1/2026  
\- Course: INFO 3305 Web Application Development

