# Testing Documentation for Cinema Management System

This directory contains all testing-related documentation for the Cinema Management System project.

## Documents Overview

### 📋 TEST_PLAN.md
The test plan includes:
- Objectives and scope of testing
- Test strategy and approaches
- Test items and quality attributes
- Risk assessment and mitigation
- Test tools and methodologies

### 📊 TEST_RESULTS.md
Contains detailed results of executed test cases:
- Pass / Fail metrics
- Test execution per component
- Defect tracking and analysis
- Notes on performance, reliability, and security

### 📝 TESTING_SUMMARY.md
Provides a high-level overview:
- Key findings from functional and non-functional testing
- Identified defects
- Recommendations and conclusions

## Quick Start Guide

### Running Tests

**Start Backend Services**
```bash
./mvnw spring-boot:run
Run Tests

bash
./mvnw test
Generate Test Report

bash
./mvnw surefire-report:report
Access Applications
Backend API: http://localhost:8080

API Documentation: http://localhost:8080/swagger-ui.html

Health Check: http://localhost:8080/actuator/health

Database Console: http://localhost:8080/h2-console (if using H2)

Test Environment Setup
Java: 17+

Spring Boot: 3.1+

PostgreSQL: 15+ / H2 for testing

JUnit 5 for unit testing

Mockito for mocking

Postman / REST Assured for API testing

Test Categories
Functional Testing
✅ Hall Management (CRUD operations)
✅ Showtime Management (CRUD operations)
✅ Bulk Hall Creation
✅ Showtime Filtering (by date, by title)
✅ Data Validation (capacity limits, name constraints)
✅ Cache Functionality

Integration Testing
✅ API Integration (Controllers ↔ Services ↔ Database)
✅ Database Integrity
✅ Cache Integration
✅ Transaction Management

Non-Functional Testing
✅ Performance Testing (API response < 500ms)
✅ Reliability & Fault Tolerance
✅ Security Testing (Input validation, SQL injection prevention)
✅ Concurrent Access Testing

Test Case Example
ID	Name	Scenario / Instructions	Expected Result	Actual Result	Pass/Fail
TC001	Create New Hall	1. POST /api/halls with valid name and capacity	Hall successfully created	Hall successfully created	Pass
TC002	Create Showtime	1. POST /api/showtimes/{hallId} with valid data	Showtime successfully created	Showtime successfully created	Pass
TC003	Create Invalid Hall (Capacity 0)	1. POST /api/halls with capacity = 0	400 Bad Request with validation error	Error displayed	Pass
TC004	Get Hall by ID	1. GET /api/halls/{id} for existing hall	200 OK with hall details	Hall details returned	Pass
TC005	Filter Showtimes by Date	1. GET /api/showtimes/filter/date/{hallId}?date=2024-01-01	200 OK with filtered showtimes	Filtered results returned	Pass
TC006	Bulk Create Halls	1. POST /api/halls/bulk with list of halls	201 Created with multiple halls	Halls created successfully	Pass
TC007	Update Hall Capacity	1. PUT /api/halls/{id} with new capacity	200 OK with updated hall	Hall updated successfully	Pass
TC008	Delete Hall with Showtimes	1. DELETE /api/halls/{id} with associated showtimes	204 No Content, cascade delete	Hall and showtimes deleted	Pass
TC009	Cache Hit Performance	1. GET same showtime multiple times	Faster response on subsequent calls	Performance improved	Pass
TC010	Concurrent Hall Updates	Multiple PUT requests to same hall	Data integrity maintained	No data corruption	Pass
More detailed test cases can be found in TEST_RESULTS.md.

Known Issues / Risks
Race conditions when multiple users schedule showtimes in same hall

Data loss during database connection failures

Cache inconsistency during showtime updates

Validation errors for overlapping showtime schedules

Test Metrics
Metric	Value
Total Test Cases	25+
Passed	23+
Failed	2
Critical Issues	0
High Priority Issues	1
Medium Priority Issues	1
Next Steps
Fix Identified Issues
Resolve showtime scheduling conflicts

Address cache synchronization issues

Complete Remaining Tests
Full performance testing under load

Comprehensive security testing

End-to-end workflow testing

Update Documentation
Add new test results

Document testing methodology and environment

Contact
For questions or issues regarding testing, contact the development team.
