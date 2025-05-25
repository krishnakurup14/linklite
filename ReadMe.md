```mermaid
linklite
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

    Client->>URLController: GET /{sho
```
