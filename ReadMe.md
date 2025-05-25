# 🔗 Smart URL Shortener with Click Analytics

This is a simple yet powerful URL shortener built with **ASP.NET Core Web API**. It not only shortens URLs but also tracks essential analytics like **click count**, **referrers**, and **user agents** for each visit.

## ✨ Features

- Shorten long URLs with unique short codes
- Redirect to original URLs via short code
- Track analytics: referrer, browser info, and timestamp
- Clean RESTful API with .NET Core
- In-memory or EF Core-based storage (configurable)

---

## 📌 How It Works

### 1. Shorten a URL
Clients send a `POST` request with the original URL. The API generates a unique short code and stores it.

### 2. Redirect and Track
When a user accesses the short URL, the API:
- Looks up the original long URL
- Logs metadata (click time, user agent, referrer)
- Redirects the user to the long URL

---

## 📋 Class Diagram

```mermaid
classDiagram

class URLController {
    +PostShortenUrl(request)
    +RedirectToLongUrl(shortCode)
}

class URLService {
    +ShortenUrl(longUrl): string
    +GetOriginalUrl(shortCode): string
}

class AnalyticsService {
    +LogClick(shortCode, userAgent, referrer)
}

class URLRepository {
    +SaveShortUrl(shortCode, longUrl)
    +FindByShortCode(shortCode): UrlEntry
    +SaveClickData(click)
}

class UrlEntry {
    -id: Guid
    -shortCode: string
    -longUrl: string
    -createdAt: DateTime
}

class ClickEntry {
    -id: Guid
    -shortCode: string
    -referrer: string
    -userAgent: string
    -timestamp: DateTime
}

URLController --> URLService
URLController --> AnalyticsService
URLService --> URLRepository
AnalyticsService --> URLRepository
URLRepository --> UrlEntry
URLRepository --> ClickEntry
````

---

## 🔁 Sequence Diagram

```mermaid
sequenceDiagram
    participant Client
    participant URLController
    participant URLService
    participant URLRepository
    participant AnalyticsService

    Client->>URLController: POST /shorten (long URL)
    URLController->>URLService: ShortenUrl(longUrl)
    URLService->>URLRepository: SaveShortUrl(shortUrl, longUrl)
    URLRepository-->>URLService: SavedEntity
    URLService-->>URLController: shortUrl
    URLController-->>Client: Return short URL

    Client->>URLController: GET /{shortCode}
    URLController->>URLService: GetOriginalUrl(shortCode)
    URLService->>URLRepository: FindByShortCode(shortCode)
    URLRepository-->>URLService: longUrl
    URLService->>AnalyticsService: LogClick(shortCode, userAgent, referrer)
    AnalyticsService->>URLRepository: SaveClickData()
    URLService-->>URLController: longUrl
    URLController-->>Client: Redirect to longUrl
```

---

## 🛠️ Tech Stack

* ASP.NET Core 8 Web API
* Entity Framework Core (or InMemory DB)
* C#
* REST/HTTP
* Mermaid for diagrams (GitHub native)

---

## 🚀 Getting Started

1. Clone the repo
2. Run `dotnet restore` and `dotnet run`
3. Test endpoints using Postman or Swagger

---

## 📄 License

MIT License – free to use, modify, and distribute
