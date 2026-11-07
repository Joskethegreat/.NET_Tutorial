# **C\# and ASP.NET Core: Overview and 8-Week Plan**

Nov 5, 2026 · @Joshua

## **What C\# is**

C\# is a statically typed, object-oriented language that runs on .NET. If you already know OOP from Java, C++ or Dart, you can read most of it on day one, and the new parts are a handful of features you will use daily.

| You may know | In C\# | Where you will see it |
| :---- | :---- | :---- |
| Getters and setters | Properties, such as public string Name { get; set; } | Every model and data class |
| Streams or list comprehensions | LINQ, such as items.Where(x \=\> x.Done).ToList() | Filtering lists and database queries |
| Future, Promise or threads | async and await with Task | Every API call and database call |
| Immutable dimmata classes | record types | API request and response shapes |
| Null checks | Nullable reference types, such as string? | Compiler warnings about possible nulls |
| Switch statements | Pattern matching and switch expressions | Cleaner branching logic |

**Which version to learn.** Learn on .NET 10\. It is the current long-term support (LTS) release, launched November 11, 2025 and supported until November 14, 2028\. .NET 8 and .NET 9 both lose support on November 10, 2026, and .NET 11 was at release candidate 3 on October 25, 2026, with a launch event on November 10 to 13 ([Microsoft support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core), [InfoWorld](https://www.infoworld.com/article/4191704/end-of-support-looms-for-net-8-and-net-9.html)). The Infineon postings you listed name no .NET version, and the concepts below carry over between versions.

## **The ASP.NET Core environment**

ASP.NET Core is Microsoft's cross-platform web framework on .NET. One project gives you a web server, a request pipeline, dependency injection and configuration. The Infineon postings name C\#, ASP.NET Core and MVC, with Oracle or Microsoft SQL behind them, so those are the pieces to learn first.

| Piece | What it does |
| :---- | :---- |
| .NET SDK and dotnet CLI | Creates, builds, runs and tests projects with dotnet new, dotnet run and dotnet test |
| NuGet | Package manager for libraries, like npm or pip |
| Kestrel | The built-in web server that receives requests |
| Middleware pipeline | Ordered components (HTTPS, authentication, routing, error handling) that each request passes through; the order in Program.cs matters ([docs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/)) |
| Dependency injection | Built in; you register services once and receive them through constructors |
| Minimal APIs | Endpoints written in a few lines, the modern way to build REST APIs ([tutorial](https://learn.microsoft.com/en-us/aspnet/core/tutorials/min-web-api)) |
| MVC | Models, Views and Controllers for server-rendered web apps, named in the postings ([tutorial](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc)) |
| Entity Framework Core | Object-relational mapper that turns C\# classes into tables and handles migrations ([getting started](https://learn.microsoft.com/en-us/ef/core/get-started/overview/first-app)) |
| ASP.NET Core Identity | Login, registration and roles ([docs](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity)) |
| Editors | Visual Studio, Visual Studio Code with the C\# Dev Kit, or JetBrains Rider |

**How one request flows**

1. Kestrel receives the HTTP request.  
2. It passes through the middleware in the order you registered it.  
3. Routing picks an endpoint: a minimal API handler or a controller action.  
4. The endpoint receives its services through dependency injection, often a database context, and returns data or a view.  
5. The response travels back out through the same middleware.

## **Setup checklist**

Finish these before week 1\. The goal is a web app running on your machine from the command line.

* ☐ Install the .NET 10 SDK from [dotnet.microsoft.com](https://dotnet.microsoft.com/download), then run dotnet \--version and check that it starts with 10\.  
* ☐ Install an editor: Visual Studio Code with the C\# Dev Kit extension (Windows, macOS, Linux), or Visual Studio Community on Windows.  
* ☐ Create and run a test app: dotnet new webapi \-n HelloApi, then cd HelloApi, then dotnet run, and open the address printed in the terminal.  
* ☐ Create a GitHub repository for your learning work and commit after every session.  
* ☐ Use SQLite in week 5 because it needs no install. Before week 6, set up SQL Server Express (Windows) or SQL Server in Docker (macOS, Linux), since the postings name Microsoft SQL and Oracle.

## **8-week plan**

Eight weeks at 8 to 10 hours a week takes you from C\# basics to a working ASP.NET Core app with a database and login. Split each week into three sessions of about 2.5 to 3 hours (learn, build, review) plus an hour to commit your code and write short notes.

| Week | Focus | Practice | Done when |
| :---- | :---- | :---- | :---- |
| 1 | C\# fundamentals: types, var, strings, collections, control flow, methods, the dotnet CLI | Console expense tracker using List\<T\> and Dictionary | You can create, run and debug a console app from the command line |
| 2 | OOP and LINQ: classes, properties, interfaces, inheritance, generics, records, LINQ | Refactor the week 1 app into classes and interfaces, then write 10 LINQ queries on its data | You can say when to use a class, a record or an interface |
| 3 | Async, errors, JSON and tests: async/await, exceptions, HttpClient, System.Text.Json, xUnit | Console app that calls a public API and saves the result as JSON, with 5 unit tests | dotnet test passes |
| 4 | Minimal APIs: Program.cs, routing, dependency injection, configuration, middleware order | Follow the Microsoft minimal API tutorial, then add input validation and a middleware that logs requests | Every endpoint works from the browser or a REST client |
| 5 | EF Core and SQL: entities, migrations, relationships, CRUD, LINQ to Entities, on SQLite | Replace in-memory data with SQLite and model 3 related tables | You can add a migration and explain what it changed in the database |
| 6 | MVC: controllers, views, Razor syntax, model binding, validation; move the database to SQL Server | Follow the MvcMovie tutorial, then build an MVC front end for your own data | Create, edit, delete and search work with form validation |
| 7 | Security and quality: Identity, \[Authorize\], roles, logging, error handling, user secrets | Add login and two roles; return consistent error responses | Anonymous users cannot reach protected pages or endpoints |
| 8 | Git workflow and polish: branches, pull requests, CI, README, tests | Finish the capstone, add a build-and-test workflow on GitHub Actions, write the README | A stranger can clone, run and test the project from your README |

The Infineon careers posting names GitLab CI/CD. The week 8 workflow file teaches the same idea (a pipeline that builds and tests on every push), so you can read a GitLab pipeline afterwards.

## **Capstone project**

Build one project across the eight weeks: an internship application tracker where you log companies, roles, deadlines and statuses. It is small enough to finish, you will use it yourself, and each feature maps to a line in the Infineon postings.

| Feature | Posting requirement it covers |
| :---- | :---- |
| Minimal API that stores applications | REST APIs, design and develop web applications |
| MVC front end with forms and search | C\#, ASP.NET Core, MVC |
| EF Core on SQL Server with 3 related tables | Relational databases, Microsoft SQL |
| Login with two roles | Reliability and security |
| xUnit tests and a CI workflow | Testing, Git, CI/CD pipelines |
| README with setup steps and an architecture sketch | Document system architecture and user guides |

After week 8, add one stretch feature: send a job description to an LLM API and store a short summary. It touches the AI and REST integration bullets without a big detour.

## **Resources and sources**

Everything here is free and from Microsoft. I opened each page on November 5, 2026\.

* Weeks 1 to 3: [A tour of C\#](https://learn.microsoft.com/en-us/dotnet/csharp/tour-of-csharp/) is written for people who already program, and the [C\# Language documentation](https://learn.microsoft.com/en-us/dotnet/csharp/) is the reference hub.  
* Week 4: [Tutorial: Create a Minimal API with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/tutorials/min-web-api), plus the [middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/) page.  
* Week 5: [EF Core getting started](https://learn.microsoft.com/en-us/ef/core/get-started/overview/first-app). Its footer says last updated August 2023, so check that the code still matches the .NET 10 docs.  
* Week 6: [Get started with ASP.NET Core MVC](https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc), a multi-part tutorial.  
* Week 7: [Introduction to Identity on ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/identity).  
* Version dates: [.NET support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core) (updated October 25, 2026\) and [InfoWorld on the .NET 8 and 9 end of support](https://www.infoworld.com/article/4191704/end-of-support-looms-for-net-8-and-net-9.html).

## **Next steps**

Start week 1 on Monday, November 9, 2026\. Week 8 then ends on Sunday, January 3, 2027, so move the dates if exams get in the way.

1. Finish the setup checklist today and run HelloApi once to prove the toolchain works.  
2. Create the GitHub repository and add a folder for each week.  
3. Put three fixed study sessions per week in your calendar for the next eight weeks.  
4. Work through the table week by week and treat the "Done when" column as a gate: do not move on until it is true.  
5. Commit small and often. A history of regular commits is evidence of consistent work when someone opens your repository.  
6. After week 4, add the minimal API to your CV and GitHub profile. After week 6, add the MVC and SQL Server version.  
7. After week 8, go deeper on the postings' AI bullets: LLM APIs, Azure OpenAI and Semantic Kernel. Then read each posting again and list the remaining gaps.

**You are ready to apply when**

* ☐ The capstone runs from your README on a clean machine.  
* ☐ At least 10 xUnit tests pass in your CI workflow.  
* ☐ You can explain dependency injection, middleware and EF Core migrations in two sentences each.