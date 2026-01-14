# Microservices Architecture

## What are microservies?
- Software architecture that breaks down big applications into smaller independent services.
- Each small service does one specific job (based on business domain)
- Example: A Sales App can have:
    - Inventory Service (manages products)
    - Sales Records Service (tracks sales)

## Key Features
- **Independent maintenance:** Each service can use different programming languages, repositories and CI/CD
- **Independent deployment:** You can update one service without touching others
- **Team structure:** Usually one developer or team owns one service
- **Request handling:** One user request might call multiple microservices to build the complete response

## Benefits of MSA

**1. Faster Deployments & Bug Fixes**
- Fix one service without redeploying the entire app
- Rollback only the problematic service, not everything

**2. Focused Development**
- Work on one service at a time
- Dedicated team per service

**3. Clean Code**
- Dependencies are separated per service
- Less tangled, easier to understand

**4. Technology Freedom**
- Each service can use the best language/framework for its job
- Mix Python, Java, Node.js, etc.

**5. Fault Isolation**
- If one service crashes, others keep running
- The whole app doesn't go down

**6. Independent Scalability**
- Scale only the services under heavy load
- Saves money and resources

**7. Data Isolation**
- Each service has its own database if needed
- Easier to update database schemas

**8. API Gateway**
- Single entry point for all requests
- Handles authentication, logging, load balancing
- Routes requests to correct services

---