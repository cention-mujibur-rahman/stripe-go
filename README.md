Go Stripe
========

## Summary

A [Stripe](https://stripe.com) client library written in Go.

### Version

Currently, the library adheres to Stripe API version **2014-05-19**.

## Installation

### Build

```sh
go get github.com/cosn/stripe
```

### Test

Set the STRIPE_KEY environment variable to match your test private key:
```sh
export STRIPE_KEY=YOUR_API_KEY
```

Then run:
```sh
go test github.com/cosn/stripe...
```

## Usage

First import the package into your code:
```go
import (
    "github.com/cosn/stripe"
)
```

To use the client, initialize it and before making any requests:
```go
stripe := &stripe.Client{}
stripe.Init(YOUR_API_KEY, nil, nil)
```

The second parameter can be used to set a different http.Client (by default, http.DefaultClient is used). 
The third parameter can be used to inject a mock Api implementation so calls aren't actually made to Stripe. This can be useful for writing your own unit tests.

### APIs

While some resources may contain more/less APIs, the following pattern is applied throughout the library for a given `resource`:

```go
# Create 
resource, err := stripe.Resources.Create(ResourceParams)

# Get
resource, err := stripe.Resources.Get(id)

# Update
resource, err := stripe.Resources.Get(ResourceParams)

# Delete
err := stripe.Resources.Delete(id)

# List
resourceList, err := stripe.Resources.Get(ResourceListParams)
```

## Documentation

Below are a couple of simple examples. For details on all the functionality in this library, see the [GoDoc](http://godoc.org/github.com/cosn/stripe) documentation.

### Customers

```go
params := &CustomerParams{
		Balance: -123,
		Card: &CardParams{
			Name:   "Go Stripe",
			Number: "378282246310005",
			Month:  "06",
			Year:   "15",
		},
		Desc:  "Stripe Dev",
		Email: "dev@stripe.com",
	}

customer, err := stripe.Customers.Create(params)
```

### Charges

```go
params := &ChargeListParams{Customer: customer.Id}
params.Filters.AddFilter("include", "", "total_count")

charges, err := stripe.Charges.List(params)

for _, charge := range(charges.Values) {
   // perform an action on each charge
}
```

## TODO

Below are the known work and imporvements planned in the near future:

- Events resource APIs
- Add support for expanding of properties

For any other requests, bug or comments, please open an issue. Pull requests are also welcome.

For more details about the Stripe, see the [Stripe official documentation](https://stripe.com/docs).
