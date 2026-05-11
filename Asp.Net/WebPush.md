# Web Push Architecture: Vanilla HTML + ASP.NET Core Web API

**Frontend**: Vanilla HTML + CSS + JavaScript  
**Backend**: ASP.NET Core Web API  
**Database**: Later step  
**Push library**: WebPush  

Frontend និង Backend បំបែកគ្នា។

## Project Structure
```text
dotnet-web-push/
  backend/
    Program.cs
    appsettings.json
    Models/
      PushSubscriptionDto.cs

  frontend/
    index.html
    main.js
    service-worker.js
```

## 1. Create Backend
```bash
mkdir dotnet-web-push
cd dotnet-web-push

mkdir backend
cd backend

dotnet new webapi
dotnet add package WebPush
```

## 2. Backend: Models/PushSubscriptionDto.cs

Create folder `Models`, then create file:

```csharp
namespace backend.Models;

public class PushSubscriptionDto
{
    public string Endpoint { get; set; } = string.Empty;
    public PushSubscriptionKeys Keys { get; set; } = new();
}

public class PushSubscriptionKeys
{
    public string P256dh { get; set; } = string.Empty;
    public string Auth { get; set; } = string.Empty;
}
```

## 3. Backend: appsettings.json

ប្រើ VAPID keys ថ្មី។ បើបងមិនទាន់មាន keys ថ្មី យើងអាច generate នៅ step បន្ទាប់។

```json
{
  "Vapid": {
    "PublicKey": "PASTE_PUBLIC_KEY_HERE",
    "PrivateKey": "PASTE_PRIVATE_KEY_HERE",
    "Subject": "mailto:your-email@example.com"
  },
  "AllowedHosts": "*"
}
```

## 4. Backend: Program.cs

Replace full `Program.cs` with this:

```csharp
using backend.Models;
using WebPush;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddCors(options =>
{
    options.AddPolicy("FrontendCors", policy =>
    {
        policy
            .WithOrigins(
                "http://localhost:5500",
                "http://127.0.0.1:5500"
            )
            .AllowAnyHeader()
            .AllowAnyMethod();
    });
});

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

app.UseCors("FrontendCors");

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

// For learning only: save in memory.
// Later we will save in SQL Server or MongoDB.
var subscriptions = new List<PushSubscriptionDto>();

app.MapGet("/", () =>
{
    return Results.Ok(new
    {
        message = "ASP.NET Core Web Push API is running"
    });
});

app.MapGet("/api/vapid-public-key", (IConfiguration config) =>
{
    var publicKey = config["Vapid:PublicKey"];

    return Results.Ok(new
    {
        publicKey
    });
});

app.MapPost("/api/subscribe", (PushSubscriptionDto subscription) =>
{
    if (
        string.IsNullOrWhiteSpace(subscription.Endpoint) ||
        string.IsNullOrWhiteSpace(subscription.Keys.P256dh) ||
        string.IsNullOrWhiteSpace(subscription.Keys.Auth)
    )
    {
        return Results.BadRequest(new
        {
            message = "Invalid subscription"
        });
    }

    var alreadyExists = subscriptions.Any(x => x.Endpoint == subscription.Endpoint);

    if (!alreadyExists)
    {
        subscriptions.Add(subscription);
    }

    Console.WriteLine($"Total subscriptions: {subscriptions.Count}");

    return Results.Ok(new
    {
        message = "Subscription saved successfully",
        total = subscriptions.Count
    });
});

app.MapPost("/api/send-notification", async (
    NotificationRequest request,
    IConfiguration config
) =>
{
    if (subscriptions.Count == 0)
    {
        return Results.Ok(new
        {
            message = "No subscriptions yet. Please click Subscribe first.",
            total = 0
        });
    }

    var publicKey = config["Vapid:PublicKey"];
    var privateKey = config["Vapid:PrivateKey"];
    var subject = config["Vapid:Subject"];

    if (
        string.IsNullOrWhiteSpace(publicKey) ||
        string.IsNullOrWhiteSpace(privateKey) ||
        string.IsNullOrWhiteSpace(subject)
    )
    {
        return Results.BadRequest(new
        {
            message = "Missing VAPID config in appsettings.json"
        });
    }

    var vapidDetails = new VapidDetails(subject, publicKey, privateKey);
    var webPushClient = new WebPushClient();

    var payload = System.Text.Json.JsonSerializer.Serialize(new
    {
        title = string.IsNullOrWhiteSpace(request.Title)
            ? "Hello from .NET"
            : request.Title,

        body = string.IsNullOrWhiteSpace(request.Body)
            ? "This notification was sent from ASP.NET Core."
            : request.Body,

        url = string.IsNullOrWhiteSpace(request.Url)
            ? "/"
            : request.Url
    });

    var results = new List<object>();

    foreach (var item in subscriptions.ToList())
    {
        try
        {
            var pushSubscription = new PushSubscription(
                item.Endpoint,
                item.Keys.P256dh,
                item.Keys.Auth
            );

            await webPushClient.SendNotificationAsync(
                pushSubscription,
                payload,
                vapidDetails
            );

            results.Add(new
            {
                ok = true,
                endpoint = item.Endpoint
            });
        }
        catch (WebPushException ex)
        {
            Console.WriteLine($"Push error: {ex.Message}");

            results.Add(new
            {
                ok = false,
                endpoint = item.Endpoint,
                error = ex.Message
            });
        }
    }

    return Results.Ok(new
    {
        message = "Notification process finished",
        total = subscriptions.Count,
        results
    });
});

app.Run();

public class NotificationRequest
{
    public string Title { get; set; } = string.Empty;
    public string Body { get; set; } = string.Empty;
    public string Url { get; set; } = "/";
}
```

## 5. Generate VAPID Keys

ក្នុង backend project, temporary add this endpoint before `app.Run();`:

```csharp
app.MapGet("/api/generate-vapid-keys", () =>
{
    var keys = VapidHelper.GenerateVapidKeys();

    return Results.Ok(new
    {
        publicKey = keys.PublicKey,
        privateKey = keys.PrivateKey
    });
});
```

Run backend:

```bash
dotnet run --urls http://localhost:5000
```

Open:

```text
http://localhost:5000/api/generate-vapid-keys
```

Copy keys ទៅដាក់ក្នុង `appsettings.json`។

បន្ទាប់ពី copy keys រួច remove endpoint `/api/generate-vapid-keys` ពី `Program.cs` ព្រោះវាបង្ហាញ private key។

## 6. Create Frontend

Back to root folder:

```bash
cd ..
mkdir frontend
cd frontend
```

Create files:

- `index.html`
- `main.js`
- `service-worker.js`

## 7. Frontend: index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />

  <title>Vanilla + .NET Web Push</title>

  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 700px;
      margin: 40px auto;
      padding: 20px;
      line-height: 1.6;
    }

    input {
      display: block;
      width: 100%;
      padding: 10px;
      margin: 8px 0 14px;
      box-sizing: border-box;
    }

    button {
      padding: 10px 14px;
      margin-right: 8px;
      cursor: pointer;
    }

    #status {
      margin-top: 20px;
      padding: 12px;
      background: #f3f3f3;
      border-radius: 6px;
    }
  </style>
</head>

<body>
  <h1>Vanilla Frontend + .NET Backend Web Push</h1>

  <p>Subscribe first, then send custom notification.</p>

  <button id="subscribeBtn">Subscribe</button>
  <button id="checkBtn">Check Permission</button>

  <hr />

  <h2>Send Custom Notification</h2>

  <label>Title</label>
  <input id="titleInput" type="text" value="Hello from .NET Backend" />

  <label>Body</label>
  <input id="bodyInput" type="text" value="This notification uses Vanilla JS + ASP.NET Core." />

  <label>URL</label>
  <input id="urlInput" type="text" value="/" />

  <button id="sendBtn">Send Notification</button>

  <p id="status">Status: Ready</p>

  <script src="./main.js"></script>
</body>
</html>
```

## 8. Frontend: main.js

```javascript
const API_BASE_URL = "http://localhost:5000";

const subscribeBtn = document.getElementById("subscribeBtn");
const sendBtn = document.getElementById("sendBtn");
const checkBtn = document.getElementById("checkBtn");
const statusText = document.getElementById("status");

const titleInput = document.getElementById("titleInput");
const bodyInput = document.getElementById("bodyInput");
const urlInput = document.getElementById("urlInput");

function setStatus(message) {
  statusText.textContent = "Status: " + message;
  console.log("Status:", message);
}

function urlBase64ToUint8Array(base64String) {
  const padding = "=".repeat((4 - (base64String.length % 4)) % 4);

  const base64 = (base64String + padding)
    .replace(/-/g, "+")
    .replace(/_/g, "/");

  const rawData = window.atob(base64);

  return Uint8Array.from([...rawData].map((char) => char.charCodeAt(0)));
}

function checkPermission() {
  if (!("Notification" in window)) {
    setStatus("Notification API not supported.");
    return;
  }

  setStatus("Current permission is: " + Notification.permission);
}

async function subscribeUser() {
  try {
    if (!("Notification" in window)) {
      setStatus("Notification API not supported.");
      return;
    }

    if (!("serviceWorker" in navigator)) {
      setStatus("Service Worker not supported.");
      return;
    }

    if (!("PushManager" in window)) {
      setStatus("Push API not supported.");
      return;
    }

    if (Notification.permission === "denied") {
      setStatus("Permission denied. Please allow notifications in browser settings.");
      return;
    }

    const permission = await Notification.requestPermission();

    if (permission !== "granted") {
      setStatus("Permission not granted.");
      return;
    }

    setStatus("Registering Service Worker...");

    const registration = await navigator.serviceWorker.register("./service-worker.js");

    await navigator.serviceWorker.ready;

    setStatus("Getting VAPID public key from .NET backend...");

    const keyResponse = await fetch(`${API_BASE_URL}/api/vapid-public-key`);
    const keyData = await keyResponse.json();

    if (!keyData.publicKey) {
      setStatus("VAPID public key not found.");
      return;
    }

    const existingSubscription = await registration.pushManager.getSubscription();

    if (existingSubscription) {
      await saveSubscription(existingSubscription);
      setStatus("Already subscribed ✅");
      return;
    }

    setStatus("Creating push subscription...");

    const subscription = await registration.pushManager.subscribe({
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array(keyData.publicKey),
    });

    await saveSubscription(subscription);

    setStatus("Subscribed successfully ✅");
  } catch (error) {
    console.error(error);
    setStatus("Subscribe error: " + error.message);
  }
}

async function saveSubscription(subscription) {
  const response = await fetch(`${API_BASE_URL}/api/subscribe`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(subscription),
  });

  const data = await response.json();
  console.log("Subscribe response:", data);
}

async function sendCustomNotification() {
  try {
    const title = titleInput.value.trim();
    const body = bodyInput.value.trim();
    const url = urlInput.value.trim();

    const response = await fetch(`${API_BASE_URL}/api/send-notification`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        title,
        body,
        url: url || "/",
      }),
    });

    const data = await response.json();

    console.log("Send response:", data);

    setStatus(data.message + " | Total subscriptions: " + data.total);
  } catch (error) {
    console.error(error);
    setStatus("Send error: " + error.message);
  }
}

subscribeBtn.addEventListener("click", subscribeUser);
sendBtn.addEventListener("click", sendCustomNotification);
checkBtn.addEventListener("click", checkPermission);
```

## 9. Frontend: service-worker.js

```javascript
self.addEventListener("install", (event) => {
  console.log("Service Worker installing...");
  self.skipWaiting();
});

self.addEventListener("activate", (event) => {
  console.log("Service Worker activated.");
  event.waitUntil(self.clients.claim());
});

self.addEventListener("push", (event) => {
  console.log("Push event received:", event);

  let data = {
    title: "Default title",
    body: "Default body",
    url: "/",
  };

  if (event.data) {
    try {
      data = event.data.json();
    } catch (error) {
      data.body = event.data.text();
    }
  }

  const options = {
    body: data.body,
    data: {
      url: data.url || "/",
    },
    requireInteraction: true,
  };

  event.waitUntil(
    self.registration.showNotification(data.title, options)
  );
});

self.addEventListener("notificationclick", (event) => {
  event.notification.close();

  const urlToOpen = event.notification.data.url || "/";

  event.waitUntil(
    clients.openWindow(urlToOpen)
  );
});
```

## 10. Run Backend

In backend folder:

```bash
dotnet run --urls http://localhost:5000
```

Test API:

```text
http://localhost:5000
```

Expected:

```json
{
  "message": "ASP.NET Core Web Push API is running"
}
```

## 11. Run Frontend

Option 1: use VS Code Live Server.

Open `frontend/index.html` with Live Server. URL usually:

```text
http://127.0.0.1:5500
```

Option 2: use simple server:

```bash
npx serve .
```

But Live Server is easier.

## 12. Test Flow

1. Open frontend: `http://127.0.0.1:5500`
2. Click **Check Permission**
3. Click **Subscribe**
4. Allow notification
5. Backend terminal should show: `Total subscriptions: 1`
6. Type title/body
7. Click **Send Notification**
