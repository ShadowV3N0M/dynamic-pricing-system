╔══════════════════════════════════════════════════════════════════════╗
║ DYNAMIC SMART PRICING SYSTEM - COMPLETE IMPLEMENTATION ║
╚══════════════════════════════════════════════════════════════════════╝

## 📋 PROJECT OVERVIEW

A production-ready dynamic pricing system that automatically adjusts
product prices based on:

- Real-time demand metrics (views, cart additions, purchases)
- Inventory levels
- Competitor pricing
- Custom pricing rules
- Market conditions

## 🎯 KEY FEATURES IMPLEMENTED

✅ 1. Complete Product Management (CRUD)
✅ 2. Base Price, Discounted Price & Dynamic Price Management
✅ 3. Real-time Demand Tracking (views, cart adds, purchases)
✅ 4. Real-time Inventory Tracking with History
✅ 5. Competitor Price Storage & Updates
✅ 6. Automated Scheduled Jobs (Hourly & Daily)
✅ 7. Powerful Pricing Rules Engine
✅ 8. Comprehensive Price History & Analytics
✅ 9. Admin Dashboard with Monitoring
✅ 10. Price Change Alerts System
✅ 11. JWT Authentication & Authorization
✅ 12. RESTful API with Swagger Documentation

## 🏗️ ARCHITECTURE

- Backend: Spring Boot 3.2.0
- Security: Spring Security + JWT
- Database: PostgreSQL with JPA/Hibernate
- Scheduling: Spring Scheduler + Quartz
- Documentation: OpenAPI/Swagger
- Build Tool: Maven

## 📊 DATABASE SCHEMA

Tables: 11 core tables

- users (authentication)
- products (product catalog)
- product_demand (demand tracking)
- inventory_log (stock changes)
- competitors (competitor tracking)
- competitor_pricing (competitor prices)
- pricing_rules (rule engine)
- price_history (price changes)
- price_alerts (notifications)
- job_execution_log (scheduler tracking)

Views: 4 analytical views

- vw_product_performance
- vw_daily_pricing_summary
- vw_inventory_alerts
- vw_competitor_comparison

## PROJECT STRUCTURE
Dynamic_Pricing_System/
- ├── src/main/java/com/pricing/
- │   ├── config/
- │   │   └── health/
- │   ├── controller/
- │   ├── dto/
- │   │   ├── alert/
- │   │   ├── analytics/
- │   │   ├── auth/
- │   │   ├── common/
- │   │   ├── competitor/
- │   │   ├── demand/
- │   │   ├── inventory/
- │   │   ├── jobexecution/
- │   │   ├── pricing/
- │   │   └── product/
- │   ├── entity/
- │   ├── repository/
- │   ├── service/
- │   ├── security/
- │   ├── scheduler/
- │   ├── engine/
- │   └── DynamicPricingApplication.java
- ├── src/main/resources/
- │   ├── application.properties
- │   └── schema.sql
- └── pom.xml
  
## 🚀 GETTING STARTED

1. DATABASE SETUP:
   - Install PostgreSQL
   - Create database: CREATE DATABASE pricing_db;
   - Run the complete schema from pricing_schema.sql

2. CONFIGURATION:
   - Update application.properties with your database credentials
   - Set JWT secret key (generate a secure random string)
   - Configure scheduler cron expressions if needed

3. BUILD & RUN:
   mvn clean install
   mvn spring-boot:run

4. DEFAULT ACCESS:
   - API Base URL: http://localhost:8080/api
   - Swagger UI: http://localhost:8080/swagger-ui.html
   - Health Check: http://localhost:8080/actuator/health

## 📡 API ENDPOINTS

AUTHENTICATION:
POST /api/auth/signup - Register new user
POST /api/auth/signin - Login and get JWT token

PRODUCTS:
GET /api/products - Get all products (paginated)
GET /api/products/{id} - Get product details
GET /api/products/search - Search products
GET /api/products/low-stock - Get low stock products
POST /api/products - Create product [ADMIN/MANAGER]
PUT /api/products/{id} - Update product [ADMIN/MANAGER]
DELETE /api/products/{id} - Delete product [ADMIN]

DEMAND TRACKING:
POST /api/demand/track - Track demand event
GET /api/demand/metrics/{id} - Get demand metrics

INVENTORY:
POST /api/inventory/update - Update inventory [ADMIN/MANAGER]
GET /api/inventory/history/{id} - Get inventory history

COMPETITORS:
GET /api/competitors - Get all competitors
POST /api/competitors - Add competitor [ADMIN/MANAGER]
POST /api/competitors/pricing - Update competitor price
GET /api/competitors/pricing/{id} - Get competitor prices

PRICING RULES:
GET /api/pricing-rules - Get all rules [ADMIN]
GET /api/pricing-rules/active - Get active rules [ADMIN]
POST /api/pricing-rules - Create rule [ADMIN]
PUT /api/pricing-rules/{id} - Update rule [ADMIN]
DELETE /api/pricing-rules/{id} - Delete rule [ADMIN]
PATCH /api/pricing-rules/{id}/toggle - Toggle rule [ADMIN]

PRICE HISTORY:
GET /api/price-history/product/{id} - Get product history
GET /api/price-history/recent - Get recent changes

ALERTS:
GET /api/alerts - Get all alerts
GET /api/alerts/product/{id} - Get product alerts
PATCH /api/alerts/{id}/mark-read - Mark as read
PATCH /api/alerts/mark-all-read - Mark all as read
GET /api/alerts/count - Get unread count

ANALYTICS:
GET /api/analytics/dashboard - Dashboard summary
GET /api/analytics/pricing - Pricing analytics
GET /api/analytics/job-executions - Job history

ADMIN:
POST /api/admin/recalculate-prices - Trigger full recalculation
POST /api/admin/recalculate-price/{id} - Recalculate single product
GET /api/admin/system-health - System health

## 🔐 AUTHENTICATION

1. Register: POST /api/auth/signup
   {
   "username": "admin",
   "email": "admin@pricing.com",
   "password": "admin123",
   "firstName": "Admin",
   "lastName": "User",
   "role": "ADMIN"
   }

2. Login: POST /api/auth/signin
   {
   "username": "admin",
   "password": "admin123"
   }

3. Use returned JWT token in headers:
   Authorization: Bearer <your-jwt-token>

## ⚙️ PRICING ENGINE ALGORITHM

The dynamic pricing engine calculates optimal prices using:

1. DEMAND FACTOR (-0.5 to +0.5):
   - Compares recent 7 days vs previous 7 days
   - Weighted score: views + (cart_adds × 2) + (purchases × 5)
   - Increases price if demand is rising

2. INVENTORY FACTOR (-0.3 to +0.4):
   - Very low stock (< min/2): +0.4 (40% increase)
   - Low stock (< min): +0.25 (25% increase)
   - High stock (> max): -0.3 (30% decrease)
   - Adequate stock: 0.0 (neutral)

3. COMPETITOR FACTOR (-0.3 to +0.2):
   - Compares with average competitor price
   - If 10%+ more expensive: -0.3
   - If 10%+ cheaper: +0.2

4. WEIGHTED COMBINATION:
   Final Adjustment = (demand × 0.4) + (inventory × 0.3) + (competitor × 0.3)

5. CONSTRAINTS:
   - Maximum increase: 50% of base price
   - Maximum decrease: 30% of base price
   - Minimum profit margin: 10%

6. PRICING RULES:
   - Applied after base calculation
   - Ordered by priority
   - Can be percentage or fixed amount adjustments

## 📅 SCHEDULED JOBS

1. Hourly Price Recalculation:
   - Runs every hour at minute 0
   - Updates all active product prices
   - Logs all changes to price_history

2. Daily Comprehensive Recalculation:
   - Runs at 2:00 AM daily
   - Includes all products (even inactive)
   - More thorough analysis

3. Competitor Price Update:
   - Runs at 2:00 AM daily
   - Updates competitor pricing data
   - Triggers price adjustments if needed

4. Weekly Cleanup:
   - Runs Sunday at 3:00 AM
   - Archives old data
   - Maintains database performance

## 📈 ANALYTICS & REPORTING

The system provides comprehensive analytics:

1. Dashboard Summary:
   - Total/Active products
   - Low stock alerts
   - Price changes today
   - Unread alerts
   - Average price change %

2. Pricing Analytics:
   - Total price changes
   - Increases vs decreases
   - Changes by reason
   - Trend analysis

3. Product Performance:
   - View/purchase conversion rates
   - Revenue metrics
   - Demand trends
   - Stock status

4. Competitor Analysis:
   - Price positioning
   - Market comparison
   - Underpricing alerts

## 🧪 TESTING THE SYSTEM

1. Create Products:
   POST /api/products with various price points

2. Track Demand:
   POST /api/demand/track with VIEW, ADD_TO_CART, PURCHASE

3. Update Inventory:
   POST /api/inventory/update to change stock levels

4. Add Competitor Prices:
   POST /api/competitors/pricing

5. Create Pricing Rules:
   POST /api/pricing-rules with your business logic

6. Trigger Manual Recalculation:
   POST /api/admin/recalculate-prices

7. Monitor Results:
   - Check price_history for changes
   - View alerts for significant changes
   - Analyze dashboard metrics

## 🔍 MONITORING & LOGGING

- Application logs: logs/pricing-system.log
- Job execution logs: job_execution_log table
- Price change history: price_history table
- System health: /actuator/health
- Metrics: /actuator/metrics

## 🛡️ SECURITY FEATURES

- JWT-based authentication
- Role-based access control (ADMIN, MANAGER, USER)
- Password encryption (BCrypt)
- CORS configuration
- SQL injection prevention (JPA)
- Input validation

## 📦 DEPENDENCIES

- Spring Boot 3.2.0
- Spring Security
- Spring Data JPA
- PostgreSQL Driver
- JWT (jjwt 0.12.3)
- Lombok
- MapStruct
- Quartz Scheduler
- Swagger/OpenAPI
- Jackson

## 🎨 CUSTOMIZATION

Key configuration points in application.properties:

pricing.min.profit.margin=10.0
pricing.max.price.increase.percent=50.0
pricing.max.price.decrease.percent=30.0
pricing.demand.weight=0.4
pricing.inventory.weight=0.3
pricing.competitor.weight=0.3

Modify these to match your business needs!

## 📝 NOTES

- All prices are in BigDecimal for precision
- Timestamps use UTC timezone
- Soft delete for products (is_active flag)
- Comprehensive error handling
- Transaction management for data consistency
- Optimized database queries with indexes

## 🚨 IMPORTANT

1. Change JWT secret in production
2. Use strong passwords
3. Configure proper CORS origins
4. Set up database backups
5. Monitor scheduled job execution
6. Review and adjust pricing weights
7. Set up email alerts (optional)

## 💡 FUTURE ENHANCEMENTS

- Machine Learning price prediction
- Email notification system
- Real-time WebSocket updates
- Advanced reporting dashboard
- Multi-currency support
- A/B testing framework
- API rate limiting
- Caching layer (Redis)

## 🤝 SUPPORT

For issues or questions:

1. Check logs in logs/pricing-system.log
2. Review API documentation at /swagger-ui.html
3. Check database views for analytics
4. Monitor job_execution_log for scheduler issues

══════════════════════════════════════════════════════════════════════
HAPPY PRICING! 🎯💰📈
══════════════════════════════════════════════════════════════════════
\*/



