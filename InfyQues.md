.NET Core & Web API Fundamentals
1. What are the inbuilt features available in .NET Core? / Why do we need to go with .NET Core over .NET Framework?
 * Cross-Platform: Runs on Windows, macOS, and Linux.
 * High Performance: Kestrel web server makes it one of the fastest web frameworks.
 * Dependency Injection (DI): Built-in container right out of the box for loose coupling.
 * Unified Framework: ASP.NET Core and Web API are merged into a single programming model.
 * Configuration & Logging: Built-in support for environment variables, appsettings.json, and extensible logging providers.
2. What is the difference between Singleton, Scoped, and Transient lifetimes?
 * Transient: Created every time they are requested from the service container. Best for lightweight, stateless services.
 * Scoped: Created once per client request (HTTP connection). Ideal for Entity Framework Core DbContext.
 * Singleton: Created the first time they are requested and then every subsequent request uses the same instance throughout the application lifecycle. Used for caching or configuration settings.
3. How do you implement Exception Handling in .NET Core?
 * Use Global Exception Handling Middleware (app.UseExceptionHandler()) to catch unhandled exceptions globally and return a standardized JSON error response.
 * Use try-catch blocks inside services/controllers for specific, recoverable business logic errors and throw custom exceptions (e.g., NotFoundException).
4. What is CQRS in .NET Core, and how do you implement it?
 * CQRS (Command Query Responsibility Segregation): Separates read operations (Queries) from write operations (Commands) to optimize performance, scalability, and security.
 * Implementation: Commonly implemented using the MediatR library, where requests are split into IRequest/IRequestHandler for queries and commands, keeping controllers thin.
5. What are Middlewares, and have you created any Custom Middlewares?
 * Middleware: Software assemblies (components) wired into the ASP.NET Core request pipeline to handle requests and responses (e.g., authentication, logging, routing).
 * Custom Middleware: A class with an InvokeAsync method taking a HttpContext and a pointer to the RequestDelegate (next). Registered via extension methods like app.UseMiddleware<CustomMiddleware>().
API Security & Performance
6. What are the ways to secure your Web API?
 * Authentication & Authorization: Use JWT (JSON Web Tokens), OAuth 2.0, or ASP.NET Core Identity.
 * HTTPS Enforcement: Force secure communication using app.UseHttpsRedirection().
 * CORS (Cross-Origin Resource Sharing): Restrict which external domains can access your API endpoints.
 * Rate Limiting: Protect APIs against DDoS and brute-force attacks by limiting concurrent requests.
 * Input Validation & Sanitization: Prevent SQL injection and XSS.
7. How do you handle concurrent requests / race conditions on an API method (e.g., multiple calls within 1 minute)?
 * Rate Limiting: Use built-in ASP.NET Core Rate Limiting middleware (AddRateLimiter) to restrict excessive calls per IP or user.
 * Idempotency: Ensure repeated requests with the same payload don't duplicate transactions (using unique request IDs/tokens).
 * Concurrency Control in DB: Use database locking mechanisms (Optimistic Concurrency using RowVersion or Pessimistic locking).
8. Role-Based Access Control: How to restrict specific API endpoints to certain roles?
 * Use the built-in [Authorize(Roles = "Admin,Manager")] attribute on controllers or action methods.
9. How do you restrict domain access (e.g., allowing only Infosys domain and blocking Bank of America)?
 * Implement custom middleware or configure CORS policies (CorsPolicyBuilder) to whitelist specific origins (e.g., https://*.infosys.com) and block unlisted domains.
Architecture & Design Patterns
10. Explain the difference between Monolithic and Microservice architecture. Which one did you use?
 * Monolithic: Single unified unit where all components (UI, business logic, data access) are tightly coupled and deployed together. Simple to build initially, but hard to scale and maintain as it grows.
 * Microservices: Suite of small, independent services running in their own processes and communicating via lightweight protocols (gRPC/REST). Highly scalable and maintainable.
 * Usage: Usually mention using Microservices for enterprise/scalable cloud apps or a modular Monolith depending on project scale.
11. What are SOLID Principles? Explain one with an example.
 * Single Responsibility Principle (SRP)
 * Open/Closed Principle (OCP)
 * Liskov Substitution Principle (LSP)
 * Interface Segregation Principle (ISP)
 * Dependency Inversion Principle (DIP)
 * Example (SRP): A class should have only one reason to change. E.g., separate a User data model class from a UserReportGenerator service so data structure changes don't break reporting logic.
12. Have you worked with Design Patterns? Explain them.
 * Creational: Singleton, Factory Pattern (used for object creation flexibility).
 * Structural: Repository Pattern, Adapter Pattern (decoupling data access or wrapping legacy code).
 * Behavioral: Strategy Pattern, Mediator Pattern (used via MediatR for decoupling handlers).
Entity Framework Core & Databases
13. What is the difference between Code-First and Database-First approaches in EF Core?
 * Code-First: You write C# domain classes first, and EF Core generates the database schema using migrations. Ideal for new domain-driven designs.
 * Database-First: You reverse-engineer an existing database schema to generate C# entity models and context. Ideal for legacy databases.
14. Explain the difference between SingleOrDefault vs FirstOrDefault in LINQ.
 * FirstOrDefault: Returns the first element of a sequence, or a default value if no element is found. Does not throw an exception if multiple matches exist.
 * SingleOrDefault: Returns the only element of a sequence, or a default value if empty. Throws an exception if more than one matching element is found.
15. How do you write a LINQ query to fetch employee and manager salary?
var result = from emp in context.Employees
             join mgr in context.Employees on emp.ManagerId equals mgr.Id
             select new {
                 EmployeeName = emp.Name,
                 EmployeeSalary = emp.Salary,
                 ManagerName = mgr.Name,
                 ManagerSalary = mgr.Salary
             };

16. What is the difference between Drop, Delete, and Truncate in SQL?
 * DELETE: DML command that removes specific rows (based on a WHERE clause), fires triggers, and logs each row deletion individually (slower, roll-backable).
 * TRUNCATE: DDL command that removes all rows from a table instantly, resets identity seeds, does not fire triggers, and logs minimally (faster, can be rolled back inside a transaction).
 * DROP: DDL command that deletes the entire table structure and data permanently from the database.
17. How do you perform SQL Performance Optimization?
 * Indexing: Create proper clustered and non-clustered indexes on frequently queried/joined columns.
 * Query Tuning: Avoid SELECT *, use proper joins instead of subqueries where applicable, and avoid functions on indexed columns in WHERE clauses.
 * Execution Plans: Analyze query execution plans to identify bottlenecks or missing indexes.
 * Pagination: Use OFFSET/FETCH or keyset pagination for large datasets.
18. Write a query to remove duplicate records from a SQL table.
WITH CTE AS (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY ColumnToIdentifyDuplicate ORDER BY (SELECT NULL)) as RowNum
    FROM TableName
)
DELETE FROM CTE WHERE RowNum > 1;

Logging, Monitoring & Production Issues
19. How did you manage logs in your previous organization? Where were they stored?
 * Used structured logging frameworks like Serilog or NLog writing to JSON format.
 * Stored logs in centralized monitoring systems or database stores for easy querying.
20. Have you worked with external tools like Splunk or AppInsights for logging/monitoring? How do you query them?
 * Yes, used Splunk or Application Insights to aggregate application traces, metrics, and exceptions.
 * Queried logs using specific search syntax (e.g., filtering by severity Level = "Error", correlation IDs, or specific exception messages) to trace production bugs.
21. What production issues have you worked on?
 * Addressed memory leaks caused by improper lifecycle management of HTTP clients or unclosed database contexts.
 * Resolved database deadlocks under high-concurrency transactions by optimizing query locking hints and indexes.
 * Fixed sudden API latency spikes caused by unindexed table scans.
Angular & Frontend Integration
22. What are Angular Lifecycle Hooks, and which one executes first?
 * Hooks: ngOnChanges, ngOnInit, ngDoCheck, ngAfterContentInit, ngAfterViewInit, ngOnDestroy.
 * First Execution: ngOnChanges runs first (if input properties exist), followed immediately by ngOnInit (runs once after the first ngOnChanges).
23. What is Lazy Loading in Angular?
 * A design pattern that loads Angular feature modules on demand rather than loading the entire application upfront, significantly improving initial load times.
24. How do you mitigate slow page loading (taking more than 5 minutes) from both Angular and DB sides?
 * Angular Side: Implement lazy loading, bundle optimization, trackBy functions in loops, and caching API responses.
 * DB Side: Index slow-running queries, optimize heavy joins, implement server-side pagination, and cache frequently requested reference data using Redis.
 * 
