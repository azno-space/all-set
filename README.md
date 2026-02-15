<div align="center">
  
# 📅 AllSet

**A modern, open-source booking and availability engine for developers**

[![.NET 8](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)

*Build powerful booking systems in minutes, not weeks*

[Features](#-features) • [Quick Start](#-quick-start) • [API Docs](#-api-documentation) • [Docker](#-docker-deployment) • [Contributing](#-contributing)

</div>

---

## 🎯 Overview

**AllSet** is a production-ready, API-first booking and availability management engine built with ASP.NET Core 8. It provides a robust foundation for building scheduling applications, appointment systems, resource booking platforms, and more.

### What Can You Build?

- 🏥 Medical appointment systems
- 💇 Salon & spa booking platforms
- 🎯 Conference room reservations
- 🚗 Vehicle rental systems
- 🏋️ Fitness class scheduling
- 🎓 Tutoring & education platforms
- 🔧 Service provider booking systems

---

## ✨ Features

### Core Functionality

- **🔄 Smart Booking Management**
  - Create single or batch bookings
  - Automatic conflict detection with configurable gaps
  - Soft deletion for audit trails
  - Order-based booking groups
  - Custom metadata support (JSON)

- **📊 Real-Time Availability**
  - Instant availability calculation
  - Working hours configuration per resource
  - Holiday & special day overrides
  - Gap management between bookings
  - Multi-day availability queries

- **🏢 Multi-Tenancy Ready**
  - Organization-based resource management
  - Isolated data per organization
  - Flexible resource configuration

- **🛡️ Conflict Prevention**
  - Automatic overlap detection
  - Configurable gap enforcement
  - Working hours validation
  - Status-based filtering (pending, confirmed, cancelled)

### Technical Highlights

- **RESTful API** with OpenAPI/Swagger documentation
- **Entity Framework Core** with PostgreSQL
- **ASP.NET Identity** for authentication (extensible)
- **Soft deletion** pattern for bookings
- **UTC timestamp** handling
- **Docker support** for easy deployment
- **Automatic migrations** on startup

---

## 🚀 Quick Start

### Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) or higher
- [PostgreSQL 12+](https://www.postgresql.org/download/) (or Docker)
- *(Optional)* [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Installation

#### Option 1: Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/azno-space/all-set.git
   cd all-set
   ```

2. **Configure PostgreSQL connection**
   
   Update the connection string in `appsettings.Development.json`:
   ```json
   {
     "ConnectionStrings": {
       "DefaultConnection": "Host=localhost;Database=allset;Username=postgres;Password=yourpassword"
     }
   }
   ```

3. **Run the application**
   ```bash
   dotnet restore
   dotnet run
   ```

4. **Access Swagger UI**
   
   Open your browser: `http://localhost:5044/swagger`

#### Option 2: Docker Compose (Recommended)

1. **Start services**
   ```bash
   docker-compose up -d
   ```

2. **Access the API**
   
   - API: `http://localhost:5044`
   - Swagger: `http://localhost:5044/swagger`

---

## 📚 API Documentation

### Base URL
```
http://localhost:5044/api
```

### Core Endpoints

#### 🏢 Organizations

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/organizations` | List all organizations |

#### 🎯 Resources

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/organizations/{orgId}/resources` | Get all resources for an organization |
| `POST` | `/api/organizations/{orgId}/resources` | Create a new resource |

**Create Resource Example:**
```bash
POST /api/organizations/{orgId}/resources
Content-Type: application/json

{
  "name": "Meeting Room A"
}
```

#### 📅 Bookings

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/bookings` | Create booking(s) |
| `GET` | `/api/bookings/{bookingId}` | Get booking details |
| `GET` | `/api/resources/{resourceId}/bookings` | Get resource bookings |
| `DELETE` | `/api/bookings/{bookingId}` | Soft delete a booking |
| `PATCH` | `/api/bookings/{bookingId}/status` | Update booking status |
| `GET` | `/api/bookings/order/{orderId}` | Get all bookings in an order |

**Create Booking Example:**
```bash
POST /api/bookings
Content-Type: application/json

{
  "orderId": null,  // Auto-generated if not provided
  "bookings": [
    {
      "resourceId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "startDateTime": "2026-02-15T10:00:00Z",
      "endDateTime": "2026-02-15T11:00:00Z",
      "metadata": {
        "customerName": "John Doe",
        "notes": "First time customer"
      }
    }
  ]
}
```

**Success Response:**
```json
{
  "success": true,
  "orderId": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
  "bookings": [
    {
      "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
      "resourceId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "startDateTime": "2026-02-15T10:00:00Z",
      "endDateTime": "2026-02-15T11:00:00Z",
      "orderId": "7c9e6679-7425-40de-944b-e07fc1f90ae7",
      "metadata": { "customerName": "John Doe" }
    }
  ]
}
```

**Error Response (Conflict):**
```json
{
  "success": false,
  "error": {
    "index": 0,
    "errorCode": "Overlapping"
  }
}
```

**Error Codes:**
- `InvalidRequest` - Missing or malformed data
- `InvalidMetadata` - Metadata is not a JSON object
- `StartAfterEnd` - Start time after end time
- `Overlapping` - Booking conflicts with existing reservation
- `OutOfWorkingHours` - Outside configured working hours

#### 📦 Orders

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/orders` | Create a new order |
| `GET` | `/api/orders/{orderId}` | Get order details |
| `GET` | `/api/orders?userId={userId}` | Get user's orders |
| `PATCH` | `/api/orders/{orderId}/status` | Update all bookings in order |
| `DELETE` | `/api/orders/{orderId}` | Cancel order |

#### 🕒 Availability

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/resources/{resourceId}/availabality` | Get availability for date range |

**Query Parameters:**
- `startDate` (required): Start date (ISO 8601)
- `endDate` (required): End date (ISO 8601)

**Example:**
```bash
GET /api/resources/{resourceId}/availabality?startDate=2026-02-15&endDate=2026-02-17
```

**Response:**
```json
[
  {
    "date": "2026-02-15T00:00:00Z",
    "availableTimeSlots": [
      {
        "start": "09:00:00",
        "end": "10:00:00"
      },
      {
        "start": "11:30:00",
        "end": "17:00:00"
      }
    ]
  }
]
```

---

## 🐳 Docker Deployment

### Docker Compose Setup

The repository includes a complete Docker setup with PostgreSQL.

**docker-compose.override.yml** (create this file):
```yaml
version: '3.4'

services:
  allset:
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://+:80
      - ConnectionStrings__DefaultConnection=Host=postgres;Database=allset;Username=postgres;Password=yourpassword
    ports:
      - "5044:80"
    depends_on:
      - postgres

  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: yourpassword
      POSTGRES_DB: allset
    ports:
      - "5432:5432"
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
```

**Run:**
```bash
docker-compose up -d
```

---

## ⚙️ Configuration

### Working Hours

Define when a resource is available:

```csharp
// Example: Setting working hours for a resource
WorkingHour {
  ResourceId = resourceId,
  DayOfWeek = DayOfWeek.Monday,
  OpenTime = TimeSpan.FromHours(9),    // 9:00 AM
  CloseTime = TimeSpan.FromHours(17)   // 5:00 PM
}
```

### Working Time Overrides

Handle holidays, special hours, or closures:

```csharp
// Example: Holiday closure
WorkingTimeOverride {
  ResourceId = resourceId,
  Date = new DateTime(2026, 12, 25),
  IsClosed = true,
  Note = "Christmas Day"
}

// Example: Special hours
WorkingTimeOverride {
  ResourceId = resourceId,
  Date = new DateTime(2026, 12, 24),
  IsClosed = false,
  OpenTime = TimeSpan.FromHours(9),
  CloseTime = TimeSpan.FromHours(14),  // Close early
  Note = "Christmas Eve - Early Close"
}
```

### Gap Configuration

Prevent back-to-back bookings by adding buffer time:

```csharp
Resource {
  Name = "Therapy Room 1",
  GapInMinutes = 15  // 15-minute buffer between appointments
}
```

---

## 🏗️ Architecture

### Tech Stack

- **Framework:** ASP.NET Core 8
- **Database:** PostgreSQL with Entity Framework Core
- **Authentication:** ASP.NET Identity (extensible)
- **API Documentation:** Swagger/OpenAPI
- **Containerization:** Docker

### Data Models

```
Organization
├── Resources
    ├── Bookings (soft delete)
    ├── WorkingHours
    └── WorkingTimeOverrides

Order
└── Bookings (grouped)
```

### Key Services

- **AvailabilityService:** Real-time availability calculation
- **BookingService:** Booking creation, validation, and management
- **OrderService:** Order lifecycle management

---

## 💡 Use Case Examples

### Example 1: Salon Booking System

```bash
# 1. Create an organization
POST /api/organizations
{ "name": "Glamour Salon" }

# 2. Create resources
POST /api/organizations/{orgId}/resources
{ "name": "Hair Stylist - Sarah", "gapInMinutes": 10 }

# 3. Set working hours (Monday 9 AM - 5 PM)
# (Via database or extend API)

# 4. Check availability
GET /api/resources/{resourceId}/availabality?startDate=2026-02-15&endDate=2026-02-15

# 5. Create booking
POST /api/bookings
{
  "bookings": [{
    "resourceId": "{resourceId}",
    "startDateTime": "2026-02-15T10:00:00Z",
    "endDateTime": "2026-02-15T11:00:00Z",
    "metadata": { "service": "Haircut", "customer": "Jane Doe" }
  }]
}
```

### Example 2: Conference Room Reservation

```bash
# Create room resource with 30-min gap for cleanup
POST /api/organizations/{orgId}/resources
{ "name": "Conference Room A", "gapInMinutes": 30 }

# Book meeting
POST /api/bookings
{
  "bookings": [{
    "resourceId": "{resourceId}",
    "startDateTime": "2026-02-20T14:00:00Z",
    "endDateTime": "2026-02-20T16:00:00Z",
    "metadata": { "meeting": "Q1 Review", "attendees": 12 }
  }]
}
```

---

## 🛠️ Development

### Project Structure

```
AllSet/
├── Controllers/          # API endpoints
├── Services/            # Business logic
├── Domain/              # Entity models
├── DTOs/                # Data transfer objects
├── Migrations/          # EF Core migrations
├── JsonConverters/      # Custom JSON handling
├── Program.cs           # Application entry point
└── appsettings.json     # Configuration
```

### Running Migrations

Migrations run automatically on startup. To create new migrations:

```bash
dotnet ef migrations add YourMigrationName
```

### Development Tips

- **Swagger UI** is available in Development mode at `/swagger`
- All timestamps are stored in **UTC**
- Use **soft deletion** for bookings (set `IsDeleted = true`)
- Metadata supports any JSON structure for extensibility

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. **Commit your changes**
   ```bash
   git commit -m "Add amazing feature"
   ```
4. **Push to your branch**
   ```bash
   git push origin feature/amazing-feature
   ```
5. **Open a Pull Request**

### Contribution Ideas

- Additional endpoints (e.g., bulk operations)
- Advanced search and filtering
- Calendar integrations (Google, Outlook)
- Webhook notifications
- Multi-language support
- Performance optimizations
- Additional database providers
- GraphQL API layer

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Built with ❤️ by the open-source community.

Special thanks to:
- ASP.NET Core team
- Entity Framework Core team
- PostgreSQL community

---

## 📞 Support

- **Issues:** [GitHub Issues](https://github.com/azno-space/all-set/issues)
- **Discussions:** [GitHub Discussions](https://github.com/azno-space/all-set/discussions)

---

<div align="center">

**[⭐ Star this repo](https://github.com/azno-space/all-set)** if you find it useful!

Made with ☕ and .NET

</div>
