# X402 Seller Server



## Seller server 코드 샘플 (go)



### installation

```bash
go get github.com/coinbase/x402/go
```



### usage

```go
package main

import (
	"math/big"

	x402gin "github.com/coinbase/x402/go/pkg/gin"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	r.GET(
		"/joke",
		x402gin.PaymentMiddleware(
			big.NewFloat(0.0001),
			"0x209693Bc6afc0C5328bA36FaF03C514EF312287C",
			x402gin.WithFacilitatorURL("https://x402.org/facilitator"),
			x402gin.WithResource("http://localhost:4021/joke"),
		),
		func(c *gin.Context) {
			c.JSON(200, gin.H{
				"joke": "Why do programmers prefer dark mode? Because light attracts bugs!",
			})
		},
	)

	r.Run(":4021") // Start the server on 0.0.0.0:4021 (for windows "localhost:4021")
}
```



## Structure



### Sever => Client

```json
{
  // Version of the x402 payment protocol
  x402Version: int,

  // List of payment requirements that the resource server accepts. A resource server may accept on multiple chains, or in multiple currencies.
  accepts: [paymentRequirements]

  // Message from the resource server to the client to communicate errors in processing payment
  error: string
}
```

#### paymentRequirements

| **Field**         | **Type**         | **Meaning**                                                  |
| ----------------- | ---------------- | ------------------------------------------------------------ |
| Scheme            | string           | Scheme of the payment protocol to use                        |
| Network           | string           | Network of the blockchain to send payment on                 |
| MaxAmountRequired | string           | Maximum amount required to pay for the resource in atomic units of the asset( == Amount) |
| Resource          | string           | URL of resource to pay for                                   |
| Description       | string           | Description of the resource                                  |
| MimeType          | string           | MIME type of the resource response                           |
| PayTo             | string           | Address to pay value to                                      |
| MaxTimeoutSeconds | int              | Maximum time in seconds for the resource server to respond   |
| Asset             | string           | Address of the EIP-3009 compliant ERC20 contract             |
| OutputSchema      | *json.RawMessage | Output schema of the resource response                       |
| Extra             | *json.RawMessage | Extra information about the payment details specific to the scheme |

- Scheme

  - exact : a precise amount EVM-based payment using EIP-3009
  - lnurl : for Lightning Network payments.
  - onchain : for direct blockchain transfers.

- Network

  - Ethereum
  - Polygon
  - Avalanche

- Resource

  - ex) https://api.example.com/premium-joke

- MimeType

  - application/json
  - image/png
  - text/html

- Extra

  - `exact` scheme case

    - expects extra to contain the records name and version pertaining to asset

    ```json
    "extra": {
    		"name": "USD Coin",
    		"version": "2"
    }
    ```

    

### Client => Sever

:bulb: 아래의 json 데이터가 base64 인코딩되어  `X-PAYMENT` header로 들어가 있어야 함

```json
{
  // Version of the x402 payment protocol
  x402Version: number;

  // scheme is the scheme value of the accepted `paymentRequirements` the client is using to pay
  scheme: string;

  // network is the network id of the accepted `paymentRequirements` the client is using to pay
  network: string;

  // payload is scheme dependent
  payload: <scheme dependent>;
}
```

####payload

- `<scheme dependent>`

  - Each payment scheme may have different operational functionality depending on what actions are necessary to fulfill the payment

    :bulb: scheme : 구체적이고 확정된 것 / schema : 대략적인 계획이나 도식

  - 정의된 scheme : https://github.com/coinbase/x402/tree/main/specs/schemes

- `exact` scheme

  ```json
  {
    "signature": "{서명값}",
    "authorization": {
      "from": "{송신 주소}",
      "to": "{수신 주소}",
      "value": "{value}",
      "validAfter": "{Unix Timestamp}",
      "validBefore": "{Unix Timestamp}",
      "nonce": "{random 32-byte nonce}" // 16진수 64자리
    }
  }
  ```

#### Full `X-PAYMENT` header

```json
{
  "x402Version": 1,
  "scheme": "exact",
  "network": "base-sepolia",
  "payload": {
    "signature": "0x2d6a7588d6acca505cbf0d9a4a227e0c52c6c34008c8e8986a1283259764173608a2ce6496642e377d6da8dbbf5836e9bd15092f9ecab05ded3d6293af148b571c",
    "authorization": {
      "from": "0x857b06519E91e3A54538791bDbb0E22373e36b66",
      "to": "0x209693Bc6afc0C5328bA36FaF03C514EF312287C",
      "value": "10000",
      "validAfter": "1740672089",
      "validBefore": "1740672154",
      "nonce": "0xf3746613c2d920b5fdabc0856f2aeb2d4f88ee6037b8cc5d04a71a4462f13480"
    }
  }
}
```



