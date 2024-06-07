# mongoose_ts_query_builder

sql query build tootom，make native SQL and ORM will be mixed to write, complement each other.

# install

```sh
npm i mongoose_ts_query_builder
```

# Documentation

## Overview
The `QueryBuilder` class is designed to streamline and enhance querying operations. It provides methods to search, filter, sort, paginate, and select specific fields from a query. Each method is designed to be intuitive and efficient, allowing for flexible and powerful query construction.

## Methods

### `search(searchableFields: string[])`
- **Description**: Adds a search condition to the query.
- **Functionality**: Matches any of the specified fields using a case-insensitive regex.
- **Parameters**: 
  - `searchableFields` (string[]): An array of field names to be searched.

### `filter()`
- **Description**: Filters the query based on the provided query parameters.
- **Functionality**: Excludes specific fields used for other operations such as search, sort, limit, page, and fields.
- **Parameters**: None. The method operates based on the query parameters provided externally.

### `sort()`
- **Description**: Sorts the query based on the provided sort parameters.
- **Functionality**: If no sort parameter is specified, it defaults to sorting by `-createdAt`.
for descending sorting use "-" before the sorting field exp:-name,-ratings. 
- **Parameters**: None. The method uses the sort parameters provided externally.

### `paginate()`
- **Description**: Paginates the query based on the page and limit parameters.
- **Functionality**: Defaults to page 1 and limit 10 if not specified.
- **Parameters**: None. The method uses the page and limit parameters provided externally.

### `fields()`
- **Description**: Selects specific fields from the query based on the provided fields parameter.
- **Functionality**: Excludes `__v` by default.
- **Parameters**: None. The method uses the fields parameter provided externally.

## Usage
To use the `QueryBuilder` class, instantiate it and utilize its methods to build your query. Each method can be chained to create a comprehensive query operation.

```typescript
const queryBuilder = new QueryBuilder();
queryBuilder
    .search(['name', 'description'])
    .filter()
    .sort()
    .paginate()
    .fields();

```
# exmaple

```typescript

import QueryBuilder from "mongoose_ts_query_builder";
import { Student } from "./student.model";

const studentSearchableFields = ["name", "email", "studentId"];

const getAllStudentService = async (query: Record<string, unknown>) => {
  const student = Student.find()
    .populate("admissionSemester")

  const studentQuery = new QueryBuilder(student, query)
    .search(studentSearchableFields)
    .filter()
    .sort()
    .paginate()
    .fields();

  const result = await studentQuery.modelQuery;
  return result;
};

```
# How to use

# .filter()
The .filter() method is used to filter the query results based on the provided query parameters. It excludes specific fields used for other operations such as searchTerm, sort, limit, page, and fields.

### URL req
```url
"/get/students?name=robert&address=colony"
```

### code
```typescript

new QueryBuilder(student, query).filter()
const result = await studentQuery.modelQuery;
return result;

```

# .sort()
Sorts the query based on the provided sort parameters. Defaults to -createdAt if not specified.

### URL req
```url
"/get/students?sort=name,-createdAt"
```

### code
```typescript

new QueryBuilder(student, query).sort()
const result = await studentQuery.modelQuery;
 return result;

```

# .paginate()
Paginates the query based on the page and limit parameters. Defaults to page 1 and limit 10 if not specified.

### URL req
```url
"/get/students?page=2&limit=10"
```

### code
```typescript

 const student = Student.find();

 const studentQuery = new QueryBuilder(student, query).paginate();
 const result = await studentQuery.modelQuery;
 return result;

```

# .fields()
Selects specific fields from the query based on the provided fields parameter. Excludes __v by default.

### URL req
```url
"/get/students?fields=name,email"
```

### code
```typescript

  const student = Student.find();

  const studentQuery = new QueryBuilder(student, query).fields();
  const result = await studentQuery.modelQuery;
  return result;

```


