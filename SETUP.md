# MySportsPlaylist - Setup Instructions

This document provides detailed setup instructions for both the backend (ASP.NET Core Web API) and frontend (Angular) components of the MySportsPlaylist application.

## Prerequisites

Before you begin, ensure you have the following installed:

- [.NET 6 SDK](https://dotnet.microsoft.com/download/dotnet/6.0) or later
- [Node.js](https://nodejs.org/) (v16 or later) and npm
- [Angular CLI](https://angular.io/cli) (v12 or later)
- [Visual Studio](https://visualstudio.microsoft.com/) or [Visual Studio Code](https://code.visualstudio.com/) (recommended)
- [SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) (Express edition is sufficient)

## Backend Setup (.NET Core API)

### Clone the Repository

If you haven't already, clone this repository to your local machine:

```bash
git clone https://github.com/kareemhesham1999/MySportsPlaylist-Assignment

cd "MySportsPlaylist-Assignment/MySportsPlaylist.Api/"
```

### Configure the Database

1. Open the `appsettings.json` file in the `MySportsPlaylist.Api` project directory.
2. Check the `ConnectionStrings` section:

Update the connection string:

```json
"ConnectionStrings": {
  "DefaultConnection": "Server=localhost,1433;Database=MySportsPlaylistDb;User Id=sa;Password=YourStr0ngP@ssw0rd;TrustServerCertificate=True;Encrypt=False"
}
```

### Build and Run the API

#### Using Visual Studio

1. Open the `MySportsPlaylist.sln` solution file in Visual Studio.
2. Right-click on the `MySportsPlaylist.Api` project in Solution Explorer and select "Set as Startup Project".
3. Press F5 or click the "Start" button to build and run the project.

#### Using .NET CLI

1. Navigate to the API project directory:
2. Restore dependencies:
3. Build the project:
4. Run the project:

```bash
cd MySportsPlaylist.Api

dotnet restore

dotnet build

dotnet run
```

### Verify the API

Once running, the API should be accessible at:

- <https://localhost:7161/api> (HTTPS)
- <http://localhost:5163/api> (HTTP)

You can also access the Swagger documentation at:

- <https://localhost:7161/swagger>
- <http://localhost:5163/swagger>

The database will be automatically seeded with sample data on first run.

## Frontend Setup (Angular)

### Navigate to the Frontend Project

```bash
cd "../../MySportsPlaylist.Frontend/"
```

### Install Dependencies

Install the required npm packages:

```bash
npm install
```

### Configure API Endpoint

Open `src/environments/environment.ts` and ensure the `apiUrl` is correctly pointing to your running backend API:

```typescript
export const environment = {
  production: false,
  apiUrl: 'https://localhost:7161/api'
  // Or use HTTP if preferred:
  // apiUrl: 'http://localhost:5163/api'
};
```

### Run the Angular Application

Start the development server:

```bash
ng serve
```

The application will be available at `http://localhost:4200/`.

### Login Credentials

Use the following demo accounts to test the application:

- **Admin User**:
  - Username: `admin`
  - Password: `Admin123!`

- **Regular User**:
  - Username: `user1`
  - Password: `User123!`

## Running Both Applications

For the best development experience:

1. Start the backend API first and ensure it's running successfully.
2. Then start the Angular frontend application.
3. Use the provided demo credentials to log in.

## Troubleshooting

### CORS Issues

If you encounter CORS issues:

1. Ensure the backend API is running and accessible.
2. Check that the `apiUrl` in `environment.ts` matches the actual running API URL.
3. Verify the CORS policy in `Program.cs` includes your frontend URL.

### Database Issues

If you encounter database issues:

1. Check the connection string in `appsettings.json`.
2. Ensure you have proper permissions to create/access the database.
3. For SQL Server, ensure the service is running.

### SignalR Connection Issues

If real-time notifications aren't working:

1. Check browser console for connection errors.
2. Ensure CORS is properly configured for SignalR hub connections.
3. Verify that WebSockets are not blocked by a proxy or firewall.

## Additional Information

- The backend includes demo services that simulate match status changes.
- Authentication is implemented using JWT tokens.
- The frontend uses Angular's standalone components architecture.
