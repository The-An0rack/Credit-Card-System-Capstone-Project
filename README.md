# Capstone Project

A capstone project is a culminating assignment typically completed at the end of an academic program, designed to allow students to demonstrate their knowledge and skills on a topic relevant to their field of study.

This project focuses on solving a real-world problem using **Microservices Architecture**. Students are expected to design, implement, document, test, optimize, and maintain code quality using tools like SonarQube.

---

# Distributed Credit Card Services Using Microservices Architecture

## Objective

Design and implement a microservices-based system to manage:

- Credit card issuance
- Billing
- Payments
- User registration

The project demonstrates:

- Microservices architecture
- Service discovery
- API Gateway
- Inter-service communication
- Transaction management

---

# Core Components

1. User Registration Service
2. Credit Card Service
3. Payments Service
4. Spring Cloud Gateway
5. Eureka Service Discovery

---

# 1. User Registration Service

## Functionalities

Customers can register in the system.

Available roles:

- ADMIN
- CUSTOMER

System initialization should create the ADMIN role.

---

## User Registration Details

| Field | Validation |
|-------|------------|
| Name | Required |
| Email | Unique, valid email |
| Password | Minimum 8 chars, uppercase, number, special character |
| Aadhaar Number | 12 digits |
| PAN Card Number | Format: `ABCDE1234F` |
| Address | Required |
| Mobile Number | Valid 10-digit Indian mobile |
| Job Role | Required |
| Monthly Income | Positive integer |

---

## API Endpoints

### Register User

### `POST /register`

#### Request

```json
{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "password": "Secure@123",
  "aadhaarNumber": "123456789012",
  "panCardNumber": "ABCDE1234F",
  "address": "123 Main St, City, Country",
  "mobileNumber": "9876543210",
  "jobRole": "Salaried",
  "monthlyIncome": 5000
}
```

---

# Admin APIs

## Get User Details

### `GET /admin/getUserDetails/{customerId}`

#### Response

```json
{
  "jobRole": "Salaried",
  "monthlyIncome": 5000
}
```

---

## Get All Users

### `GET /admin/getAllUsers`

---

## Filter Users

### `GET /admin/filterUsers`

#### Query Parameters

- startDate
- endDate
- minSalary
- maxSalary
- jobRole

---

# 2. Credit Card Service

## Functionalities

- Issue new credit cards
- Assign credit limits
- Generate monthly bills
- View bill history

---

## Card Entity

| Field |
|-------|
| cardId |
| customerId |
| cardNumber (encrypted) |
| cvv (encrypted) |
| expirationDate |
| creditLimit |
| outstandingBalance |
| active |

---

## Business Logic

Eligibility is based on:

- Job role
- Monthly income

If eligible:

- Card is issued
- Credit limit assigned
- Card activated

---

## Credit Limit Rules

### Salaried

| Income | Limit |
|--------|-------|
| < 20,000 | 5,000–10,000 |
| 20,000–50,000 | 10,000–50,000 |
| > 50,000 | 50,000–100,000 |

### Freelancer

| Income | Limit |
|--------|-------|
| < 20,000 | 2,000–5,000 |
| 20,000–50,000 | 5,000–25,000 |
| > 50,000 | 25,000–60,000 |

---

## API Endpoints

## Issue Card

### `POST /issue`

#### Request

```json
{
  "customerId": "123456"
}
```

---

## Get Card Details

### `GET /getCard/{customerId}`

---

## Generate Bill

### `POST /generateBill/{customerId}`

---

## Get Bills

### `GET /getBill/{customerId}`

---

# 3. Payments Service

## Functionalities

- Make bill payments
- Update card balance
- Merchant transactions

---

## Payment Entity

| Field |
|-------|
| paymentId |
| amount |
| date |
| paymentStatus |

---

## Transaction Entity

| Field |
|-------|
| transactionId |
| customerId |
| merchantId |
| amount |
| date |
| transactionStatus |

---

## API Endpoints

## Make Payment

### `POST /makePayment/{customerId}`

#### Request

```json
{
  "amount": 200
}
```

After successful payment:

- Credit Card Service is called synchronously
- Outstanding balance is updated

---

## Merchant Payment

### `POST /payToMerchant`

#### Request

```json
{
  "customerId": "123456",
  "merchantId": "merchant-001",
  "amount": 150,
  "cardNumber": "1234-5678-9012-3456",
  "cvv": "123",
  "expirationDate": "12/25"
}
```

---

## Validation

System should validate:

- Card is active
- CVV is correct
- Card not expired

Failure handling:

- Log failure reason
- Return error response
- Allow retry

---

# 4. Spring Cloud Gateway

Acts as:

- Entry point for all requests
- Routes traffic to appropriate services

---

# 5. Eureka Service Discovery

All services must register with Eureka.

Features:

- Dynamic service discovery
- Load balancing

---

# Inter-Service Communication

## Credit Card Service → User Registration Service

### `GET /getUserDetails/{customerId}`

Returns:

- Job role
- Monthly income

---

## Payments Service → Credit Card Service

### Validate Card

`GET /getCard/{customerId}`

### Update Balance

`PUT /updateCardBalance/{customerId}`

---

# Non-Functional Requirements

- MySQL database
- CSV audit logging

Files:

- `card_issue.csv`
- `payment.csv`

Other requirements:

- Input validation
- Exception handling
- Logging
- API documentation
- Unit testing
- SonarQube compliance

---

# Suggested Technology Stack

- Java 21
- Spring Boot
- Spring Security
- Spring Cloud Gateway
- Eureka Server
- OpenFeign
- MySQL
- JPA / Hibernate
- JUnit
- Mockito
- SonarQube
- Docker

---

# Deliverables

Students must provide:

- Source code
- API documentation
- Unit tests
- Database scripts
- SonarQube report
- Architecture diagram
- README documentation

---