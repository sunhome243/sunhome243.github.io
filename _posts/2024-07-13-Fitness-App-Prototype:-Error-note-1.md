---
title: "Workout App Prototype: Error note 1"
categories:
  - errors
tags:
  - Major Projects
  - Team Project
  - Backend
  - Python
  - Fast API
  - Postgre SQL
  - Docker
  - Kubernetes

sidebar:
  nav: "sidebar-category"

header:
  teaser: "/assets/posting/errorteaser.jpg"
---
Error Notes for Fitness App Prototype

# Error Notes for Fitness App Prototype

This error note is created by Claude, based on my scrap notes of errors encountered during the development of the Fitness App Prototype (June 30 ~ July 13). The primary purpose of compiling these error notes is to enable faster resolution of recurring issues. By maintaining a comprehensive record of encountered errors, their causes, and solutions, we aim to significantly reduce debugging time and improve overall development efficiency. This document serves as a quick reference for developers, facilitating knowledge sharing among team members and helping to identify common patterns in our codebase

## Database Errors

### 1. Connection Error with PostgreSQL

**Description**: The test encounters a connection error when trying to connect to the PostgreSQL server.

**Error Message**:
```
psycopg2.OperationalError: connection to server at "localhost" (::1), port 5433 failed: server closed the connection unexpectedly
```

**Solution**:
- Ensure the PostgreSQL server is correctly mapped and running.
- If the server's port is incorrectly set, map it to the default port: `5433:5432`

### 2. Database Naming Syntax Error

**Description**: Dropping a database with a hyphen in its name leads to a syntax error.

**Error Message**:
```
postgres=# drop database user-db-test;
ERROR:  syntax error at or near "user"
```

**Solution**:
- Enclose the database name in double quotes:
  ```sql
  DROP DATABASE "user-db-test";
  ```

### 3. SQLAlchemy Async Session Error

**Description**: Accessing related objects in an async context causes an error.

**Error Message**:
```
sqlalchemy.exc.MissingGreenlet: greenlet_spawn has not been called; can't call await_only() here. Was IO attempted in an unexpected place?
```

**Solution**:
1. Use `AsyncAttrs` mixin for SQLAlchemy models.
2. Set `lazy="selectin"` for relationships.
3. Explicitly load related objects in CRUD functions.

Example:
```python
from sqlalchemy.ext.asyncio import AsyncAttrs

class TrainerUserMap(Base, AsyncAttrs):
    __tablename__ = "trainer_user_mapping"
    trainer = relationship("Trainer", lazy="selectin")
    user = relationship("User", lazy="selectin")

async def get_mapping(db: AsyncSession, mapping_id: int):
    result = await db.execute(select(TrainerUserMap).options(
        selectinload(TrainerUserMap.trainer),
        selectinload(TrainerUserMap.user)
    ).filter_by(id=mapping_id))
    return result.scalar_one_or_none()
```

### 4. SQLAlchemy Invalid Request Error

**Description**: Error occurs when trying to create an async database engine.

**Error Message**:
```
sqlalchemy.exc.InvalidRequestError: The asyncio extension requires an async driver to be used. The loaded 'psycopg2' is not async.
```

**Solution**:
1. Use an async database driver like `asyncpg`.
2. Modify the database URL to use the async driver.
3. Ensure that the async driver is installed (`pip install asyncpg`).

Example:
```python
from sqlalchemy.ext.asyncio import create_async_engine

SQLALCHEMY_DATABASE_URL = "postgresql+asyncpg://user:password@localhost/dbname"
engine = create_async_engine(SQLALCHEMY_DATABASE_URL)
```

### 5. SQLAlchemy Foreign Key Constraint Violation

**Description**: Inserting or updating a record violates a foreign key constraint.

**Error Message**:
```
sqlalchemy.exc.IntegrityError: (psycopg2.errors.ForeignKeyViolation) insert or update on table "trainer_user_mapping" violates foreign key constraint "trainer_user_mapping_user_id_fkey"
```

**Solution**:
1. Ensure that the referenced entity (e.g., user or trainer) exists before creating a mapping.
2. Implement proper error handling to catch and report foreign key violations.
3. Consider using SQLAlchemy's `relationship` with `cascade` options for automatic handling of related records.

### 6. SQLAlchemy Session Commit Error

**Description**: A previous exception causes the session's transaction to be rolled back.

**Error Message**:
```
sqlalchemy.exc.PendingRollbackError: This Session's transaction has been rolled back due to a previous exception during flush. To begin a new transaction with this Session, first issue Session.rollback().
```

**Solution**:
1. Implement proper error handling and session management.
2. Ensure sessions are properly closed or rolled back after exceptions.
3. Use context managers or try-except-finally blocks to manage session lifecycles.

Example:
```python
from sqlalchemy.orm import sessionmaker
from contextlib import contextmanager

@contextmanager
def session_scope():
    session = sessionmaker()()
    try:
        yield session
        session.commit()
    except Exception:
        session.rollback()
        raise
    finally:
        session.close()

# Usage
with session_scope() as session:
    # Perform database operations
    session.add(some_object)
```

### 7. SQLAlchemy Operational Error

**Description**: Unable to connect to the database server.

**Error Message**:
```
sqlalchemy.exc.OperationalError: (psycopg2.OperationalError) could not connect to server: Connection refused
```

**Solution**:
1. Verify database connection settings (host, port, username, password).
2. Ensure the database server is running and accessible.
3. Implement retry logic for transient connection issues.

### 8. Concurrent Database Access Error

**Description**: Connection is closed when trying to access the database concurrently.

**Error Message**:
```
asyncpg.exceptions.InternalClientError: connection is closed
```

**Solution**:
1. Implement connection pooling to manage database connections efficiently.
2. Ensure connections are properly released back to the pool after use.
3. Use SQLAlchemy's `NullPool` for testing to avoid connection reuse issues.

Example:
```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

engine = create_async_engine(SQLALCHEMY_DATABASE_URL, pool_size=20, max_overflow=0)
AsyncSessionLocal = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)

async def get_db():
    async with AsyncSessionLocal() as session:
        yield session
```

### 9. Database Migration Error

**Description**: Errors occur during database schema migrations, often when using tools like Alembic.

**Solution**:
- Ensure migration scripts are up-to-date and properly sequenced.
- Run migrations in a test environment before applying to production.
- Implement rollback mechanisms for failed migrations.
- Keep detailed logs of migration processes.

## Authentication Errors

### 1. JWT Authentication Error

**Description**: Authentication fails when using JWT.

**Error Message**:
```
HTTPException: Could not validate credentials
```

**Solution**:
1. Ensure the JWT token is correctly passed in the request header.
2. Verify that the token decoding process uses the correct secret key and algorithm.
3. Check that the user extraction from the database based on the token payload is working correctly.

Example:
```python
from fastapi import Depends, HTTPException
from fastapi.security import OAuth2PasswordBearer
from jose import JWTError, jwt

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="token")

async def get_current_user(token: str = Depends(oauth2_scheme)):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise HTTPException(status_code=401, detail="Invalid token")
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")
    user = await get_user(username=username)
    if user is None:
        raise HTTPException(status_code=401, detail="User not found")
    return user
```

### 2. 403 Forbidden Error

**Description**: The server returns a 403 Forbidden status code when trying to access the `/create_session` endpoint.

**Solution**:
- Ensure that the JWT token is correctly generated and includes all necessary claims (sub, type, id).
- Verify that the `SECRET_KEY` used for token generation matches the one used for verification.
- Check if the user has the correct permissions to access the endpoint.
- Implement proper error handling in the `get_current_user` function to provide more detailed error messages.

### 3. Invalid Token Payload

**Description**: The server indicates that the token payload is invalid, usually due to missing required fields.

**Solution**:
- Ensure that the JWT token generation includes all required fields (sub, type, id).
- Verify the structure of the token payload in both the auth service (where the token is generated) and the workout service.
- Implement stricter validation of the token payload in the `get_current_user` function.

### 4. "Not authenticated" Error

**Description**: Server returns a "Not authenticated" error message during the authentication process.

**Solution**:
- Verify correct Authorization header with Bearer token.
- Check JWT token expiration.
- Ensure correct implementation of `security` dependency in FastAPI.
- Add detailed logging in the authentication process.

## FastAPI Errors

### 1. Response Validation Error

**Description**: FastAPI encounters a validation error when serializing the response.

**Error Message**:
```
fastapi.exceptions.ResponseValidationError: 2 validation errors:
Error extracting attribute: MissingGreenlet: greenlet_spawn has not been called
```

**Solution**:
1. Ensure that all database queries are properly awaited.
2. Use Pydantic models for responses instead of directly returning ORM models.
3. Explicitly load all needed relationships before returning the response.

Example:
```python
from pydantic import BaseModel

class TrainerUserMapResponse(BaseModel):
    trainer_id: int
    user_id: int
    
    class Config:
        orm_mode = True

@router.get("/mapping/{mapping_id}", response_model=TrainerUserMapResponse)
async def get_mapping(mapping_id: int, db: AsyncSession = Depends(get_db)):
    mapping = await crud.get_mapping(db, mapping_id)
    if not mapping:
        raise HTTPException(status_code=404, detail="Mapping not found")
    return mapping
```

### 2. Unnecessary `await` in FastAPI endpoint

**Description**: An endpoint was using `await` on a `current_user` object that was likely already awaited by the dependency injection system.

**Solution**: Remove the unnecessary `await` and directly return the `current_user` object:

```python
@router.get("/users/me/", response_model=schemas.User)
async def read_users_me(
    current_user: Annotated[models.User, Depends(utils.get_current_member)]
):
    return current_user
```

### 3. Request Validation Error

**Description**: FastAPI returns a 422 Unprocessable Entity error due to invalid request data.

**Solution**:
- Review and update Pydantic models for request validation.
- Provide clear API documentation specifying required fields and data types.
- Implement custom error handlers for validation errors to provide user-friendly messages.

## Password Hashing Errors

### Invalid Salt in Password Hashing

**Description**: An invalid salt value is provided during the password hashing process, causing a `ValueError`.

**Error Message**:
```
ValueError: Invalid salt
```

**Solution**:
Ensure the salt is generated correctly and the password is encoded properly:
```python
salt = bcrypt.gensalt()
hashed_password = bcrypt.hashpw(password.encode('utf-8'), salt).decode('utf-8')
```

## Testing Errors

### 1. Event Loop Mismatch in Pytest Asyncio

**Description**: An event loop mismatch error occurs, indicating that a task is attached to a different event loop.

**Error Message**:
```
RuntimeError: Task <Task pending name='Task-7' coro=<_wrap_asyncgen_fixture.<locals>._asyncgen_fixture_wrapper.<locals>.finalizer.<locals>.async_finalizer() running at ...plugin.py:334> cb=[_run_until_complete_cb() at ...base_events.py:182]> got Future <Future pending cb=[Protocol._on_waiter_completed()]> attached to a different loop
```

**Solution**:
Ensure the event loop is properly managed and consistent across tests. Use `pytest-asyncio`'s fixtures correctly to handle the event loop:
```python
import pytest_asyncio

@pytest_asyncio.fixture(scope="session")
async def event_loop():
    loop = asyncio.get_event_loop_policy().new_event_loop()
    yield loop
    loop.close()
```

### 2. 401 Unauthorized in test cases

**Description**: Test cases for authenticated endpoints are failing with 401 Unauthorized errors.

**Solution**: 
- Ensure that the `authenticated_user_client` fixture is properly set up in `conftest.py`.
- Verify that the mock token is correctly added to the request headers.
- Check if the `get_current_member` function is properly mocked in the test cases.

### 3. "Instance is not persisted" when deleting user

**Description**: Attempting to delete a user in tests results in an error stating that the instance is not persisted.

**Solution**:
- Ensure that the user is actually created and persisted in the database before attempting to delete.
- If using an in-memory database for tests, make sure it's properly set up and the user creation step is not skipped.

### 4. Slow test execution

**Description**: The test suite takes too long to execute, especially database-related tests.

**Solution**:
- Consider using an in-memory SQLite database for tests instead of a full PostgreSQL database.
- Optimize database fixtures to reduce the number of database operations between tests.
- Use `pytest-xdist` to run tests in parallel.
- Profile the tests to identify and optimize the slowest parts.

### 5. Mocking Errors in Unit Tests

**Description**: Unit tests failed due to incorrect mocking of external dependencies, particularly the user service API calls.

**Solution**:
- Review the mock objects and ensure they are patching the correct paths.
- Verify that the mocked functions return the expected data structures.
- For API call mocks, ensure that the mock response matches the expected format from the actual API.
- Check if the mocked objects are being used in the correct scope within the tests.

## Asynchronous Context Errors

### 1. Event Loop Mismatch

**Description**: A task or future is attached to a different event loop.

**Error Message**:
```
RuntimeError: Task <Task pending name='Task-5' coro=<_wrap_awaitable() running at ...> got Future <Future pending> attached to a different loop
```

**Solution**:
1. Ensure all async operations are running in the same event loop.
2. Use `asyncio.run()` to properly manage the event loop in the main execution.
3. Avoid mixing sync and async code without proper bridging.

Example:
```python
import asyncio

async def main():
    # Your async code here
    pass

if __name__ == "__main__":
    asyncio.run(main())
```

### 2. Timeout Error in Asynchronous Operations

**Description**: Asynchronous operations fail due to exceeding timeout limits.

**Solution**:
- Set appropriate timeout values for asynchronous operations.
- Implement retry mechanisms with exponential backoff for transient failures.
- Add detailed logging for timing of asynchronous operations.

## Pydantic Errors

### 1. Validation Error

**Description**: Pydantic validation fails due to invalid input data.

**Error Message**:
```
pydantic.error_wrappers.ValidationError: 1 validation error for UserCreate
email
  value is not a valid email address (type=value_error.email)
```

**Solution**:
1. Implement proper input validation in Pydantic models.
2. Use appropriate Pydantic field types and validators.
3. Handle validation errors in the API endpoints and return meaningful error messages.

Example:
```python
from pydantic import BaseModel, EmailStr, validator

class UserCreate(BaseModel):
    email: EmailStr
    password: str

    @validator('password')
    def password_min_length(cls, v):
        if len(v) < 8:
            raise ValueError('Password must be at least 8 characters long')
        return v

# In your FastAPI endpoint
@app.post("/users/")
async def create_user(user: UserCreate):
    try:
        # Process the validated user data
        pass
    except ValidationError as e:
        return JSONResponse(
            status_code=422,
            content=jsonable_encoder({"detail": e.errors()})
        )
```

## Workout Service Specific Errors

### 1. HTTP 404 Not Found Error in User Creation Test

**Description**: During the user creation test, a 404 Not Found error was encountered instead of the expected 200 OK response.

**Solution**:
- Verify that the user creation endpoint ('/users/') is correctly defined in the FastAPI application.
- Check if the URL used in the test matches the actual endpoint URL.
- Ensure that the router for user-related operations is properly included in the main FastAPI app.
- Confirm that the test client is configured to use the correct base URL.

### 2. Inconsistent Data Between Services

**Description**: Tests revealed inconsistencies between data in the user service and workout service, particularly in trainer-user mappings.

**Solution**:
- Implement stronger consistency checks between services.
- Consider using a shared database for critical relational data or implement a robust synchronization mechanism.
- Add more comprehensive integration tests that verify data consistency across services.
- Implement proper error handling and logging for cases where data inconsistencies are detected.

### 3. SQL Integrity Constraint Violation

**Description**: Attempts to create workout sessions failed due to SQL integrity constraint violations, particularly with the `trainer_id` field.

**Solution**:
- Modify the database schema to allow NULL values for `trainer_id` in the `session_id_mapping` table.
- Update the model definitions to reflect the changes in the database schema.
- Implement proper null checks in the application code when dealing with trainer-related data.
- Add data validation in the API to ensure that trainer_id is only set when necessary.

### 4. Inconsistent DateTime Handling

**Description**: Inconsistencies in datetime handling between the database, API, and client applications led to errors in scheduling and retrieving workout sessions.

**Solution**:
- Standardize on UTC for all datetime operations within the service.
- Implement consistent datetime serialization and deserialization methods.
- Use libraries like `pytz` or `dateutil` to handle timezone conversions when necessary.
- Add validation for datetime inputs in the API to ensure they are in the expected format and timezone.

### 5. Memory Leaks in Long-Running Processes

**Description**: The workout service experienced gradual memory growth over time, indicating potential memory leaks in long-running processes.

**Solution**:
- Use memory profiling tools to identify the source of memory leaks.
- Implement proper resource cleanup in background tasks and scheduled jobs.
- Review database session management to ensure connections are properly closed.
- Consider implementing a circuit breaker pattern for external service calls to prevent resource exhaustion.

### 6. Inconsistent Error Responses

**Description**: The API was returning inconsistent error responses, making it difficult for clients to handle errors reliably.

**Solution**:
- Define a standardized error response format for all API endpoints.
- Implement a global exception handler to catch and format all errors consistently.
- Use proper HTTP status codes for different types of errors.
- Include detailed error messages and error codes in the response body for easier debugging and handling on the client side.

## Configuration and Environment Errors

### 1. Environment Variable Not Set

**Description**: Critical environment variables (like SECRET_KEY or DATABASE_URL) are not set or not accessible.

**Solution**:
- Ensure that all required environment variables are set in the `.env` file.
- Verify that the `.env` file is in the correct location (usually in the project root).
- Use a `config.py` file to centralize environment variable management and provide default values where appropriate.
- Implement checks at application startup to verify all required environment variables are set.

### 2. Configuration File Not Found

**Description**: Application fails to start due to missing configuration file.

**Solution**:
- Ensure `config.py` or similar configuration file exists in the correct directory.
- Implement fallback mechanisms for missing configuration files.
- Use environment variables as a backup for critical configurations.

## API and Request Handling Errors

### 1. CORS (Cross-Origin Resource Sharing) Issues

**Description**: The client receives CORS-related errors when trying to access the API from a different origin.

**Solution**:
- Implement CORS middleware in your FastAPI application.
- Configure CORS settings to allow requests from your client's origin.
- For development, you can allow all origins, but for production, specify allowed origins explicitly.