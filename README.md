# ca-js-api
Building an API with Javascript

## Features

To give the blog some decent complexity and sensible usefulness, we'll aim for these features:

### User accounts

- **Create**: Any human can create user records in the database, one user per human.
- **Read**: Any user can view a user's information.
- **Update**: A user can update their own user information, or a user with the role "admin" can update any user's information.
- **Delete**: A user can delete their own user information, or a user with the role "admin" can delete any user's information.

### User roles (eg. admin, regular, banned)

Users have a role which can be checked for in other functions throughout the API. Roles available will be:

- **Regular**: A user account is assigned to this by default. Regular users can only edit their own user data, and only edit their own blog posts.
- **Admin**: A user account that can edit or delete any other user account and edit or delete any blog post.
- **Banned**: A user account that cannot create blog posts.

### Blog posts

- **Post CRUD**
  - **Create**: A post can be created by a user. A user is a required part of a blog post.
  - **Read**: Any human can view a blog post, no user account needed.
  - **Update**: A user can update blog posts that they created, or a user with the role "admin" can update any blog post.
  - **Delete**: A user can delete blog posts that they created, or a user with the role "admin" can update any blog post.

### Per-user searching

- List all blog posts made by a user.

## Endpoints

### User Operations

#### Create a User
- **Endpoint**: `POST /sign-up`
- **Description**: Create a new user with provided details, including email, password, username, country, and role. The role must be specified in the request body.
- **Authentication**: None required.

#### Sign In
- **Endpoint**: `POST /sign-in`
- **Description**: Sign in an existing user by providing their email and password. Returns a JSON Web Token (JWT) for authentication.
- **Authentication**: None required.

#### Refresh JWT
- **Endpoint**: `POST /token-refresh`
- **Description**: Extend the validity of a user's JSON Web Token (JWT) by providing the existing token. Useful for keeping the token usable for a longer time.
- **Authentication**: Valid JWT required.

#### Update User
- **Endpoint**: `PUT /users/:userID`
- **Description**: Update user information for the specified user ID. Only the user or an admin can update a user's details.
- **Authentication**: Valid JWT with appropriate permissions required.

#### Delete User
- **Endpoint**: `DELETE /users/:userID`
- **Description**: Delete a user by their user ID. This operation can only be performed by the user or an admin.
- **Authentication**: Valid JWT with appropriate permissions required.

#### List All Users
- **Endpoint**: `GET /users`
- **Description**: Retrieve a list of all users registered in the system.
- **Authentication**: None required.

#### Show Specific User
- **Endpoint**: `GET /users/:userID`
- **Description**: Retrieve information about a specific user using their user ID.
- **Authentication**: None required.

### Role Operations

#### Show All Roles
- **Endpoint**: `GET /roles`
- **Description**: Get a list of all available user roles in the system.
- **Authentication**: None required.

#### Show Users with a Matching Role
- **Endpoint**: `GET /roles/:roleName`
- **Description**: Retrieve a list of users who have the specified role.
- **Authentication**: None required.

### Blog Post Operations

#### Create a Blog Post
- **Endpoint**: `POST /posts`
- **Description**: Create a new blog post by providing the post details, including title and content. The author of the post will be the user associated with the provided JWT.
- **Authentication**: Valid JWT required.

#### Update a Blog Post
- **Endpoint**: `PUT /posts/:postID`
- **Description**: Update an existing blog post with the specified post ID. Only the author or an admin can update the post.
- **Authentication**: Valid JWT with appropriate permissions required.

#### Delete a Blog Post
- **Endpoint**: `DELETE /posts/:postID`
- **Description**: Delete a blog post by its post ID. This operation can be performed by the author or an admin.
- **Authentication**: Valid JWT with appropriate permissions required.

#### List All Blog Posts
- **Endpoint**: `GET /posts`
- **Description**: Retrieve a list of all blog posts in the system.
- **Authentication**: None required.

#### Show Blog Posts by Author
- **Endpoint**: `GET /posts/author/:authorID`
- **Description**: Get a list of blog posts created by a specific author, identified by their user ID.
- **Authentication**: None required.

#### Show Specific Blog Post
- **Endpoint**: `GET /posts/:postID`
- **Description**: Retrieve information about a specific blog post using its post ID.
- **Authentication**: None required.

### Authorization

- The API utilizes JSON Web Tokens (JWT) for authentication and authorization.
- User roles, such as "admin," "regular," and "banned," determine access to certain endpoints.
- Make sure to include the JWT in the headers for protected routes.

## Database design for this API

### User

- Email: String. Plaintext value of a user's email address.
- Password: String. Hashed and salted value of a user's password.
- Username: String.
- Country: String.
- Role: Foreign key to a Role.

### Role

- Name: String.
- Description: String.

### Post

- Title: String.
- Content: String.
- Author: Foreign key to a User.

