# MTN Mobile Money Go Package

A simple, clean, and easy-to-use Go package for integrating with MTN Mobile Money API for both collections (receiving payments) and disbursements (sending money).

## Features

- Support for both Sandbox and Production environments
- Easy configuration with environment variables or code
- Collection API (Request to Pay)
- Disbursement API (Transfer)
- Account balance and user information
- Transaction status checking
- Automatic token management

## Installation

```bash
go get github.com/cruso003/momomtn
```

## Quick Start

```go
package main

import (
	"context"
	"fmt"
	"log"
	"os"

	"github.com/cruso003/momomtn"
)

func main() {
	// Initialize from environment variables
	client, err := momomtn.InitFromEnv(momomtn.Sandbox)
	if err != nil {
		log.Fatalf("Failed to initialize: %v", err)
	}

	// Request a payment
	referenceID, err := client.Collection.RequestToPay(
		context.Background(),
		"231123456789",
		10.00,
		"Payment for goods",
		"Thank you",
	)
	if err != nil {
		log.Fatalf("Failed to request payment: %v", err)
	}

	fmt.Printf("Payment requested with reference ID: %s\n", referenceID)
}
```

## Configuration

You can configure the package using environment variables:

```
MOMO_SUBSCRIPTION_KEY=your-subscription-key
MOMO_DISBURSEMENT_KEY=your-disbursement-key
MOMO_TARGET_ENVIRONMENT=sandbox
MOMO_CALLBACK_HOST=https://your-callback-host.com
MOMO_HOST=sandbox.momodeveloper.mtn.com
MOMO_API_USER=your-api-user-id (for production)
MOMO_API_KEY=your-api-key (for production)
MOMO_CURRENCY=EUR
```

Or using code:

```go
config, err := momomtn.NewConfig(
    momomtn.Sandbox,
    momomtn.WithSubscriptionKey("your-subscription-key"),
    momomtn.WithDisbursementKey("your-disbursement-key"),
    momomtn.WithCallbackHost("https://your-callback-host.com"),
    momomtn.WithHost("sandbox.momodeveloper.mtn.com"),
    momomtn.WithTargetEnvironment("sandbox"),
    momomtn.WithCurrency("EUR"),
)
```

## API Reference

### Collection Service

```go
// Request a payment
referenceID, err := client.Collection.RequestToPay(ctx, phone, amount, message, note)

// Check transaction status
status, err := client.Collection.GetTransactionStatus(ctx, referenceID)

// Get account balance
balance, currency, err := client.Collection.GetAccountBalance(ctx)

// Get account holder information
accountInfo, err := client.Collection.GetAccountHolderInfo(ctx, phone)
```

### Disbursement Service

```go
// Send money
referenceID, err := client.Disbursement.Transfer(ctx, phone, amount, message, note)

// Check transfer status
status, err := client.Disbursement.GetTransferStatus(ctx, referenceID)

// Get account balance
balance, currency, err := client.Disbursement.GetAccountBalance(ctx)

// Get account holder information
accountInfo, err := client.Disbursement.GetAccountHolderInfo(ctx, phone)
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.
```

## Conclusion

This package provides a clean, well-structured implementation for integrating with MTN Mobile Money API. Key features include:

1. **Environment Flexibility**: Works in both sandbox and production environments
2. **Configuration Options**: Multiple ways to configure the package
3. **Service Separation**: Clear separation between collection and disbursement services
4. **Error Handling**: Custom error types for better error reporting
5. **Token Management**: Automatic access token management and caching
6. **Examples**: Practical examples for common use cases

The package follows Go best practices with clear interfaces, proper error handling, and comprehensive documentation. It handles the complexities of the MTN MoMo API while providing a simple and intuitive interface for developers.
