# Continuous Integration/Continuous DDelivery/Deployment

## 1. Continuous Integration (CI)
**Definition:** Developers frequently merge code changes into a central repository, where automated builds and tests run.

### 1.1. Key Practices:

- Developers commit code multiple times per day
- Automated build triggers on each commit
- Automated testing (unit, integration tests)
- Fast feedback on code quality and bugs
- Reduces integration problems

### 1.2. Benefits:

- Catches bugs early
- Reduces integration issues
- Improves code quality
- Faster development cycles

## 2. Continuous Delivery (CD)
**Definition:** Code changes are automatically prepared for release to production, but deployment requires manual approval.

### 1.1. Key Practices:

- All code changes go through automated pipeline
- Code is always in a deployable state
- Manual approval gate before production
- Automated testing at multiple stages
- Staging environments mirror production

### 1.2. Benefits:

- Reduces deployment risk
- Faster time to market
- More reliable releases
- Easy rollbacks if needed

## 3. Continuous Deployment (CD)
**Definition:** Every code change that passes automated tests is automatically deployed to production without manual intervention.

## 4. Key Differences: Delivery vs Deployment

- **Continuous Delivery:** Manual approval needed before production -> More control
- **Continuous Deployment:** Fully automatic to production -> Faster delivery

---
