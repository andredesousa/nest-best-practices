# Nest Best Practices

Nest (NestJS) is a framework for building efficient, scalable Node.js server-side applications. These tips are based on Nest documentation, books, articles and professional experience.

## Table of Contents

1. [Follow code conventions](#follow-code-conventions)
2. [Follow a modular approach](#follow-a-modular-approach)
3. [Use nvm](#use-nvm)
4. [Use Nest CLI](#use-nest-cli)
5. [Use Nest packages](#use-nest-packages)
6. [Use OpenAPI and Swagger UI](#use-openapi-and-swagger-ui)
7. [Use code generators](#use-code-generators)
8. [Use database migrations](#use-database-migrations)
9. [Use controllers only for routing](#use-controllers-only-for-routing)
10. [Use services for business logic](#use-services-for-business-logic)
11. [Use repository pattern](#use-repository-pattern)
12. [Use pipes for validations](#use-pipes-for-validations)
13. [Use DTOs](#use-dtos)
14. [Use caching](#use-caching)
15. [Provide a global exception filter](#provide-a-global-exception-filter)
16. [Avoid global state and mutability](#avoid-global-state-and-mutability)
17. [Remove unused code](#remove-unused-code)
18. [Set logging levels](#set-logging-levels)
19. [Expose health checks and metrics](#expose-health-checks-and-metrics)
20. [Externalize all configurations](#externalize-all-configurations)
21. [Check dependencies for vulnerabilities](#check-dependencies-for-vulnerabilities)
22. [Prefer lazy-loading modules](#prefer-lazy-loading-modules)
23. [Use guards for access control](#use-guards-for-access-control)
24. [Use versioning](#use-versioning)
25. [Test your code](#test-your-code)

## Follow code conventions

Code conventions are base rules that allow the creation of a uniform code base across an organization.
Following them does not only increase the uniformity and therefore the quality of the code.
To make it mandatory, you need a linter, a formatter and strong code review.
[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) is very popular and recommended.
Also, you should follow the [Angular Style Guide](https://angular.io/guide/styleguide) because the architecture is heavily inspired by Angular.
[Prettier](https://prettier.io/) can be used to enforce a consistent code style.
Code conventions must be dynamic and adaptable for each team and project.

## Follow a modular approach

Modules provide a logical way to group your code and keep your project organized.
When you have all the files related in one folder, it makes it easier for developers to find what they need.
You can use the following naming convention for your modules:

- The `entity` module contains the database entities of the application.
- The `services` module contains all the business logic-related classes.
- The `controllers` module contains all resources classes of the application.
- The `dtos` module contains all the Data Transfer Objects of the application
- Other common modules are `config`, `pipes`, `exception`, etc.

This style is very convenient in small-size microservices.
If you are working on a huge code base, a feature-based approach can be used.

## Use nvm

[nvm](https://github.com/nvm-sh/nvm) is a version manager for Node.js.
It is a tool for managing Node versions on your device.
Different projects on your device may be using different versions of Node. nvm provides a simple interface to install, switch, and remove Node.js versions, enabling developers to work on different projects without worrying about compatibility issues.
With nvm, you can run multiple Node.js versions on the same machine, making it an essential tool for Node.js developers.

## Use Nest CLI

[Nest CLI](https://github.com/nestjs/nest-cli) is a command-line interface tool that helps you to initialize, develop, and maintain your Nest applications.
It assists in multiple ways, including scaffolding the project, serving it in development mode, and building and bundling the application for production distribution.
It embodies best-practice architectural patterns to encourage well-structured apps.
Nest CLI works with schematics and provides built in support from the schematics collection at [@nestjs/schematics](https://github.com/nestjs/schematics).
For more information about Nest CLI, see [Nest CLI command reference](https://docs.nestjs.com/cli/usages).

## Use Nest packages

Nest is designed to be modular, extensible, and follows a modern architectural pattern.
Nest provides several official packages you can use in your projects.
Most popular packages you can add to your project are [@nestjs/swagger](https://www.npmjs.com/package/@nestjs/swagger), [@nestjs/config](https://www.npmjs.com/package/@nestjs/config), [@nestjs/mongoose](https://www.npmjs.com/package/@nestjs/mongoose), [@nestjs/typeorm](https://www.npmjs.com/package/@nestjs/typeorm), [@nestjs/sequelize](https://www.npmjs.com/package/@nestjs/sequelize), [@nestjs/graphql](https://www.npmjs.com/package/@nestjs/graphql), [@nestjs/passport](https://www.npmjs.com/package/@nestjs/typeorm), [@nestjs/jwt](https://www.npmjs.com/package/@nestjs/jwt), [@nestjs/schedule](https://www.npmjs.com/package/@nestjs/schedule), [@nestjs/terminus](https://www.npmjs.com/package/@nestjs/terminus), [@nestjs/cache-manager](https://www.npmjs.com/package/@nestjs/cache-manager), [@nestjs/cqrs](https://www.npmjs.com/package/@nestjs/cqrs), and many others.
You can use Nest CLI to initialize, develop, and maintain your Nest applications.

## Use OpenAPI and Swagger UI

[OpenAPI Specification](https://swagger.io/specification/) is the factual standard for creating REST APIs.
An OpenAPI definition can then be used by documentation generation tools to display the API, code generation tools to generate servers and clients in various programming languages, testing tools, and many other use cases.
[Swagger UI](https://swagger.io/tools/swagger-ui/) allows anyone to visualize and interact with the API's resources without having any of the implementation logic in place.
Nest provides the [@nestjs/swagger](https://www.npmjs.com/package/@nestjs/swagger) package which allows generating such a specification by leveraging decorators.

## Use code generators

Generating the code automatically can save time and is less error-prone.
You can use Nest CLI to generate a *controller*, *filter*, *pipe*, *service*, *class*, *guard*, *middleware*, *gateway*, *module*, *decorator*, or an *interceptor*.
You can also use `nest g resource` command to generate a CRUD resource.
OpenAPI Specification can be used by code generation tools to generate servers and clients for JavaScript and Nest applications.
For that, you can use [OpenAPI Generator](https://openapi-generator.tech/docs/generators/typescript-nestjs/) package for NestJS.

## Use database migrations

Database migrations is a process of making changes to database schema during a development process.
You wouldn't develop app code without version control. The same should be true for database changes.
Migrations provide a way to incrementally update the database schema to keep it in sync with the application's data model while preserving existing data in the database. Migrations are most commonly written in SQL.
NestJS supports [TypeORM](https://typeorm.io/) or [Sequelize](https://sequelize.org/) for database migrations.
You can read [TypeORM Migrations](https://typeorm.io/migrations) or [Sequelize Migrations](https://sequelize.org/docs/v7/models/migrations/) guides for more details.

## Use controllers only for routing

Controllers are dedicated to routing. Controllers are the ultimate target of requests, then requests will be handed over to the service layer and processed by the service layer.
They are stateless and all business logic should not place on them.
Controllers should deal with the HTTP layer of the application and oriented around a business capability.
See the code snippet of a controller:

```typescript
@Controller('user')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Get()
  findAll(): Promise<User[]> {
    return this.userService.findAll();
  }
}
```

## Use services for business logic

The business logic of the application goes here with validations, caching, etc.
Build your services around business capabilities/domains/use-cases.
Services communicate with the persistence layer and receive the results.
Services are singleton and annotated with `@Injectable`.
See the code snippet of a service:

```typescript
@Injectable()
export class UserService {
  constructor(@InjectRepository(User) private readonly users: User) {}

  findAll(): Promise<User[]> {
    return this.users.find();
  }
}
```

## Use repository pattern

Repositories are a very popular pattern for persistence layers.
They encapsulate the database operations you can perform on entity objects and aggregates.
That helps to separate the business logic from the database operations and improves the reusability of your code.
[TypeORM](https://typeorm.io/) supports the repository design pattern, so each entity has its own repository.
These repositories can be obtained from the database data source.
You can inject the `User` repository into the `UserService` using the `@InjectRepository()` decorator.
See the code snippet of a service.

## Use pipes for validations

REST APIs need to validate the data it receives, and Nest provides support through [Pipes to validate REST API request objects](https://docs.nestjs.com/techniques/validation).
[Pipes](https://docs.nestjs.com/pipes) provide a way to declaratively specify how data should be validated before it reaches your Nest controllers.
It helps keep your controller code clean and focused on business logic, rather than input validation.
It makes it easy to reuse validation rules across multiple controllers. Nest offers standard validation primitives such as `@IsNotEmpty()`, `@IsString()`, `@Length()`, or `@IsEmail()` and if you need anything more advanced, you can easily implement your custom pipes.
See the code snippet of a DTO:

```typescript
export class User {
  @Length(4, 20)
  username!: string;

  @Length(8, 20)
  password!: string;
}
```

## Use DTOs

While it is also possible to directly expose the database entities on REST endpoints to send/receive client data, this is not the best approach.
It creates high coupling between the persistence models and the API models.
The better approach is defining a separate Data Transfer Object (DTO) that represents the API resource class which is mapped from a database entity or multiple entities.
With DTOs, you can build different views from your domain models, allowing you to create other representations of the same domain but optimizing them to the clients' needs without affecting your domain design.

## Use caching

[Caching](https://docs.nestjs.com/techniques/caching) is an important factor when talking about application performance.
It acts as a temporary data store providing high performance data access.
In-Memory caching is the simplest caching technique.
This involves storing data in the application's memory.
Nest provides a unified API for various cache storage providers.
[cache-manager](https://www.npmjs.com/package/@nestjs/cache-manager) abstracts the complexities of caching and offers flexibility.
In order to enable caching, import the `CacheModule` and call its `register()` method.
For large-scale applications, distributed caching using systems like [Redis](https://redis.io/) can be beneficial.
Redis is an in-memory data structure store that can be used for caching.

## Provide a global exception filter

Nest comes with a built-in exceptions layer which is responsible for processing all unhandled exceptions across an application.
When an exception is not handled by your application code, it is caught by this layer, which then automatically sends an appropriate user-friendly response.
This action is performed by a built-in [global exception filter](https://docs.nestjs.com/exception-filters), which handles exceptions of type `HttpException`.
See the code snippet of a Global Exception Filter:

```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost): void {
    host.switchToHttp().getResponse<Response>().status(exception.getStatus()).json({
      statusCode: exception.getStatus(),
      timestamp: new Date().toISOString(),
      path: host.switchToHttp().getRequest<Request>().url,
      message: exception.message,
    });
  }
}
```

## Avoid global state and mutability

First, always remember the "global state" issue.
The typical danger of global state is that it's easy for different functions to step on each other.
Immutability comes directly from functional programming and adapted to OOP, states that class mutability and changing state should be avoided.
This, in short, means foregoing setter methods and having private read-only fields on all your model classes.
TypeScript provides the `readonly` modifier that allows you to mark the properties of a class immutable.

## Remove unused code

Unused code or dead code is any code which will never be executed.
It may be some condition, loop or any file which was simply created but wasn't used in your project.
It is a problem because that code has no sense, and you can drop it.
[Dead-code Elimination](https://webpack.js.org/guides/tree-shaking/) also reduces the size of your bundles and repositories.
Less code also increases maintenance, IDE performance and makes it easier to understand.
Common mistakes in TypeScript projects are unused imports, variables, functions and private class members.
You can use [no-unused-vars](https://eslint.org/docs/latest/rules/no-unused-vars) rule for [ESLint](https://eslint.org/) to solve these issues.

## Set logging levels

Logs are supposed to be a consistent and reliable source of information, which makes troubleshooting systems easier.
Nest comes with a built-in text-based logger which is used during application bootstrapping and several other circumstances. This functionality is provided via the Logger class in the [@nestjs/common](https://www.npmjs.com/package/@nestjs/common) package.
To enable specific logging levels, set the logger property to an array of strings specifying the log levels to display.
Values in the array can be any combination of *log*, *fatal*, *error*, *warn*, *debug*, and *verbose*.
You can read [Logger](https://docs.nestjs.com/techniques/logger) guide for more details.
You can also use external loggers like [Winston](https://www.npmjs.com/package/nest-winston) or [Pino](https://www.npmjs.com/package/nestjs-pino).

## Expose health checks and metrics

A health check represents a summary of health indicators.
A health indicator executes a check of a service, whether it is in a healthy or unhealthy state.
It allows applications to provide information about their state to external viewers which is typically useful in cloud environments where automated processes must be able to determine whether the application should be discarded or restarted. [Terminus](https://docs.nestjs.com/recipes/terminus) integration provides you with readiness/liveness health checks.
The metrics can be read remotely using the JSON or OpenMetrics format to be processed by additional tools such as [Prometheus](https://prometheus.io/) and stored for analysis and visualization.
You can use [@willsoto/nestjs-prometheus](https://www.npmjs.com/package/@willsoto/nestjs-prometheus) package to expose application metrics.

## Externalize all configurations

Applications often run in different environments.
Depending on the environment, different configuration settings should be used.
Since configuration variables change, best practice is to store configuration variables in the environment.
In Node.js applications, it's common to use `.env` files, holding key-value pairs where each key represents a particular value.
A good approach for using this technique in Nest is to create a `ConfigModule` that exposes a `ConfigService` which loads the content of the `.env` file.
This file can be placed in the root of the project, but it is advised to not check it in to version control.

## Check dependencies for vulnerabilities

It is important to ensure that there are no known vulnerabilities throughout your application's dependency tree.
Therefore, you should frequently update your application's dependencies to the latest versions.
The `npm audit` command submits a description of the dependencies configured in your project to your default registry and asks for a report of known vulnerabilities.
Alternatively, you can use  [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/).
When you choose a base image for your project, you indirectly assume the risk of all the container security concerns that the base image is bundled with.
[Trivy](https://github.com/aquasecurity/trivy) is a simple and comprehensive vulnerability scanner for Docker images and other artifacts.

## Prefer lazy-loading modules

[Lazy-loading modules](https://docs.nestjs.com/fundamentals/lazy-loading-modules) can help decrease bootstrap time by loading only modules required by the specific serverless function invocation.
Nest modules are eagerly loaded, which means that as soon as the application loads, so do all the modules, whether or not they are immediately necessary.
While this is fine for most applications, it may become a bottleneck for apps/workers running in the serverless environment, where the startup latency is crucial.
Most commonly, you will see lazy loaded modules in situations when your worker/cron job/lambda & serverless function/webhook must trigger different services based on the input arguments.

## Use guards for access control

A [guard](https://docs.nestjs.com/guards) is a class annotated with the `@Injectable()` decorator, which implements the `CanActivate` interface.
Guards provide a way to abstract away the details of authentication and authorization, making it easier to manage these concerns in one place.
This not only makes your code more organized and maintainable, but it also makes it easier to add new features or make changes down the road.
For example, you can have a Guard to check for a valid JWT token before allowing a user to access a route.
Additionally, Guards can be used to restrict access to certain routes based on a user's role.

## Use versioning

[API versioning](https://docs.nestjs.com/techniques/versioning) is the process of managing and tracking changes to an API.
It also involves communicating those changes to the API's consumers.
Versioning allows you to have different versions of your controllers or individual routes running within the same application.
Applications change very often, and it is not unusual that there are breaking changes that you need to make while still needing to support the previous version of the application.
Nest supports URI Versioning, Header Versioning, Media Type Versioning, and Custom Versioning.

## Test your code

If you have no tests or an inadequate amount, then every time you ship code, you won't be sure that you didn't break anything.
Always write tests for every new feature/module you introduce.
The [@nestjs/testing](https://www.npmjs.com/package/@nestjs/testing) package provides a set of utilities that enable a more robust testing process.
Nest also allows you to define a mock factory to apply to all of your missing dependencies.
This is useful for cases where you have a large number of dependencies in a class and mocking all of them will take a long time and a lot of setups.
To perform e2e testing you use a similar setup to unit testing.

## Bibliography

- [5 NestJS Techniques to Build Efficient and Maintainable Apps](https://betterprogramming.pub/5-nestjs-techniques-to-build-efficient-and-maintainable-apps-be6bc77e789e)
- [10 NestJS Best Practices](https://climbtheladder.com/10-nestjs-best-practices/)
- [10 NestJS Folder Structure Best Practices](https://climbtheladder.com/10-nestjs-folder-structure-best-practices/)
- [Best practices for Nest.js](https://proxify.io/articles/best-practices-in-building-secure-authentication-and-authorization-systems-in-nestjs)
- [Best Way to Structure Your Directory/Code (NestJS)](https://medium.com/the-crowdlinker-chronicle/best-way-to-structure-your-directory-code-nestjs-a06c7a641401)
- [Docker + NestJS. Step by step guide](https://blog.devops.dev/dockerizing-nestjs-application-a5240c86c3a0)
- [DTO explained in NestJS](https://medium.com/@yelinliu/dto-explained-in-nestjs-3a296498d77b)
- [Error Handling and Logging in NestJS](https://levelup.gitconnected.com/error-handling-and-logging-in-nestjs-best-practices-ecc871ade7d7)
- [Errors as metrics in NestJS](https://blog.devops.dev/errors-as-metrics-in-nestjs-23e791a2df9e)
- [Health checks (Terminus)](https://docs.nestjs.com/recipes/terminus)
- [How to Use Exception Filters to Catch Bugs in Nest.js](https://www.freecodecamp.org/news/exception-filters-in-nestjs/)
- [Migrations with TypeORM in NestJs](https://www.oneclickitsolution.com/blog/migrations-with-typeorm/)
- [NestJS - 11 Popular NPM Packages](https://javascript.plainenglish.io/nestjs-11-popular-npm-packages-53451d32f44b)
- [Testing NestJS Apps: Best Practices & Common Pitfalls](https://amplication.com/blog/best-practices-and-common-pitfalls-when-testing-my-nestjs-app)
