# BextraBuddies - Django Project

**BextraBuddies** is a full-stack e-commerce web application developed using the Django framework and PostgreSQL for my final CodeInstitute project. It is designed for people to browse, review, and purchase a variety of different products. The application includes secure user authentication, shopping functionality, a wishlist system, Stripe payment integration, and a clean, responsive user interface.

**Live Site:** [BextraBuddies](https://bextra-buddies-6bb50cec51fc.herokuapp.com)  
**Repository:** [Github](https://github.com/AndyJames11/bextrabuddies_django_project)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [User Experience (UX)](#user-experience-ux)
- [Technologies Used](#technologies-used)
- [Models Overview](#models-overview)
- [Database Scheme Overview](#database-schema-overview)
- [Testing](#testing)
- [Known Bugs](#known-bugs)
- [Deployment](#deployment)
- [Installation and Local Setup](#installation-and-local-setup)
- [Media and Assets](#media-and-assets)
- [Acknowledgements](#acknowledgements)

---

## Project Overview

BextraBuddies is an e-commerce platform focused on a variety of products. It was developed as part of my final college project to create a scalable Django application, integrating third-party services such as Stripe for payments and AWS S3 for static and media file hosting. The project supports full CRUD functionality for admins, user profiles, review systems, and a custom wishlist.

---

## Features

### Core Functionality

- Browse all products or filter by category
- View detailed product pages with images, pricing, and reviews
- Register, log in, and manage user profiles
- Add or remove products from the shopping cart
- Save and remove items in your personal Wishlist
- Complete purchases securely via Stripe checkout
- Product review system with star ratings
- Review order history and save default delivery information
- Confirmation emails sent on successful order completion
- Add and manage products through the Django admin panel

---

## User Experience (UX)

### User Stories

**Visitor Goals**

- View available products with ease
- Learn more about individual items
- Register for an account if needed

**Authenticated User Goals**

- Add products to wishlist and cart
- Checkout securely with saved address details
- Submit product reviews
- Manage personal information and view order history

**Admin Goals**

- Add, edit, and delete product listings
- View and manage orders placed by users

### Design

- The interface uses Bootstrap 5 with custom CSS for consistency and branding
- Clear call-to-action buttons and navigation
- Visual feedback on user actions (e.g. adding to wishlist or cart)
- Simple, clean, modern design and styling to ensure easy navigation

---

## Technologies Used

- **Languages**: HTML5, CSS3, JavaScript, Python 3
- **Backend**: Django, Django Allauth
- **Database**: PostgreSQL
- **Payment Integration**: Stripe
- **Frontend Framework**: Bootstrap 5
- **Other Tools**:
  - Crispy Forms for improved form rendering
  - AWS S3 for media and static file storage
  - Boto3 for AWS integration
  - Gunicorn for Heroku deployment
  - Git and GitHub for version control
  - Heroku for application deployment

---

## Models Overview

The core models in the BextraBuddies project are designed to support a full e-commerce workflow, from product display to user reviews and personalised wishlists. The following section outlines how the models are structured and function.

### Product Model

The `Product` model represents items available in the store. Each product includes standard commercial details as well as an optional category relationship.

**Key Fields:**

- `name`: The title of the product.
- `sku`: A unique stock-keeping identifier.
- `description`: A detailed explanation of the product.
- `price`: Decimal field storing the product price.
- `rating`: Automatically calculated average rating from user reviews.
- `image`: An optional image uploaded for the product.
- `category`: ForeignKey linking the product to a category.

### Category Model

Categories allow products to be grouped for navigation and filtering.

**Key Fields:**

- `name`: The category name.
- `friendly_name`: A human-readable version used in the interface.

### Order Model

The `Order` model stores purchase information after a successful checkout via Stripe. It tracks payment, customer data, and totals.

**Key Fields:**

- `user_profile`: ForeignKey to the UserProfile.
- `full_name`: Name from the order form.
- `email`, `phone_number`: Contact info.
- `address`: Shipping address fields.
- `stripe_pid`: Stripe payment intent ID.
- `order_total`, `grand_total`: Order cost calculations.
- `original_bag`: A snapshot of the cart data.

### OrderLineItem Model

Each `OrderLineItem` represents a single product in an order. Multiple items can be linked to one order.

**Key Fields:**

- `order`: ForeignKey to the Order.
- `product`: ForeignKey to the Product.
- `quantity`: Number of units ordered.
- `lineitem_total`: Cost of the line item.

### UserProfile Model

The `UserProfile` model extends the built-in Django User model, allowing additional fields to be saved per user.

**Key Fields:**

- `user`: OneToOneField with Django’s User.
- `default_phone_number`, `default_country`, etc.: Saved shipping info.
- `order_history`: Used in views to display previous orders.


### Review Model

The **Review** model allows authenticated users to leave feedback on individual products. Each review is associated with a product and a user, and contains both a numerical rating and a textual comment.

**Key Fields:**

- `user`: ForeignKey to Django's `User` model
- `product`: ForeignKey to the `Product` model
- `rating`: Integer field for 1 to 5 stars rating
- `comment`: Text field for detailed feedback
- `created_at`: Auto-generated timestamp when the review is created
- `updated_at`: Auto-generated timestamp when the review is modified

This model enhances user engagement and helps potential buyers make informed purchasing decisions.

### WishlistItem Model

The **WishlistItem** model enables users to bookmark products for future reference. It represents a many-to-many relationship between users and products through an explicit intermediate model.

**Key Fields:**

- `user`: ForeignKey to Django's `User` model
- `product`: ForeignKey to the `Product` model
- `created_on`: Timestamp when the item was added to the wishlist

Wishlist functionality is powered via AJAX to allow real-time feedback without requiring full page reloads, contributing to a smoother user experience.

---

## Database Schema Overview

This project uses Django's Object Relational Mapping with a PostgreSQL backend to manage data relationships in a relational schema. The architecture supports product browsing, user profiles, order processing, reviews, and personalised features like wishlists.

### Key Relationships

- **User ↔ Profile**: One-to-one relationship extending Django’s User model
- **User ↔ Orders**: One-to-many relationship for order history
- **Order ↔ OrderLineItem**: One-to-many relationship for storing cart items
- **Product ↔ Review**: One-to-many; each product can have multiple reviews
- **User ↔ WishlistItem ↔ Product**: Many-to-many relationship using an intermediary model

### Schema Summary

| Model           | Related Models           | Description                                            |
|----------------|---------------------------|--------------------------------------------------------|
| `UserProfile`   | `User` (One-to-One)       | Extended profile fields for users                     |
| `Product`       | `Category`, `Review`      | Core item displayed in the store                      |
| `Review`        | `User`, `Product`         | Allows users to submit ratings and comments           |
| `WishlistItem`  | `User`, `Product`         | Tracks user-saved products                            |
| `Order`         | `User`, `OrderLineItem`   | Represents completed purchases                        |
| `OrderLineItem` | `Order`, `Product`        | Items within a single order, with quantity and price  |

This schema supports the necessary business logic for browsing, purchasing, reviewing, and saving products, while ensuring normalized and query-optimized database performance.

---

## Testing

### Manual Testing

- Tested each user story manually
- Verified validation for forms (e.g. review submission, checkout)
- Ensured edge case handling (e.g. adding same item twice to wishlist)
- Ensured webhook feedback for Stripe orders
- Added and removed multiple products to bag to ensure all processed correctly

### Validation

- All templates checked with W3C validator
- CSS validated via Jigsaw
- JavaScript syntax validated using JSLint

---

## Known Bugs

- Wishlist icon may flicker briefly on slower connections due to async updates
- Product star ratings may visually misalign at certain breakpoints
- Formatting and CSS styling not very good on smaller devices such as mobile phones 

These issues have been documented and are planned for potential future refinement.

---

## Deployment

### Live Deployment (Heroku)

1. Hosted on Heroku using PostgreSQL
2. Static and media files served via AWS S3
3. Stripe keys and secret settings configured via Heroku Config Vars
4. Gunicorn used as production WSGI server

### Steps to Deploy

1. Create a new Heroku app
2. Connect to the GitHub repository
3. Add Heroku Postgres as an add-on
4. Configure environment variables:
   - `SECRET_KEY`
   - `DEBUG`
   - `DATABASE_URL`
   - `STRIPE_PUBLIC_KEY`, `STRIPE_SECRET_KEY`, etc.
5. Run `collectstatic` and `migrate` commands
6. Application is now live at assigned Heroku URL

---

## Installation and Local Setup

To run this project locally:

1. Clone the repository

   ```
   git clone https://github.com/AndyJames11/bextrabuddies_django_project.git
   ```

2. Navigate into the directory:

   ```
   cd bextrabuddies_django_project
   ```

3. Set up a virtual environment:

   ```
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate
   ```

4. Install dependencies:

   ```
   pip install -r requirements.txt
   ```

5. Apply migrations and run the server:

   ```
   python manage.py migrate
   python manage.py runserver
   ```

6. Create a `.env` file to store environment variables and Stripe keys

---

## Media and Assets

- Product images: [Pexels](https://www.pexels.com/)
- Icons: [Font Awesome](https://fontawesome.com/)
- JavaScript enhancements adapted from [W3Schools](https://www.w3schools.com/)

---

## Acknowledgements

- Code Institute for project template and coursework structure
- Django documentation for references and best practices
- Stripe documentation for integration guidance
- MDN Web Docs and Stack Overflow for technical support
- Tutor support for project guidance and feedback
- Friends and community support from various online developer communities

---
