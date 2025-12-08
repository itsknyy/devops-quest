
**1. What is a Multi-Stage Docker Build?**

A **multi-stage Docker build** allow us to use **multiple `FROM` statements** inside a single Dockerfile.  
Each `FROM` instruction begins a **new build stage** and only the **final stage** is used as the final production image.

**Why Multi-Stage Builds Matter:**
- Reduce final image size  
- Improve application security  
- Remove development dependencies  
- Keep the runtime image clean and optimized  
- Ideal for CI/CD pipelines and production deployments  

---

**2. How Multi-Stage Build Works (Simple Explanation)**

**Stage 1 --> Builder Stage**
Used for:
- Installing dependencies  
- Running build/compile commands  
- Generating production-ready assets  

**Stage 2 --> Final Runtime Stage**

The second stage of a multi-stage Docker build is responsible for creating the **production-ready runtime image**.  
This stage is intentionally lightweight and contains **only what is required to run the application**.

---

**3. What is Alpine?**

**Alpine Linux** is a very small, fast and secure version of Linux that is commonly used in Docker images.

---

**4. Why do we use Alpine in Docker?**

- Smaller Docker images --> faster builds and deployments  
- Less storage and memory usage  
- More secure because it has fewer packages  
- Perfect for production environments and microservices  

---


