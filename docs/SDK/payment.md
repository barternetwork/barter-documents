---
sidebar_position: 10
---
# Omnichain Payment(Beta)
Butter omnichain payment allows users to pay whatever cryptocurrency they want, no matter what cryptocurrency seller requires.

## Request
```typescript
export type PaymentRequestParam = {
    fromAddress: string;
    paidToken: BaseCurrency; // token user paid
    paidAmount: string; // in minimal uint
    toAddress: string; // seller's receiving address 
    requiredToken: BaseCurrency; // required token by seller
    requiredAmount: string; // required amount
    swapRouteStr: string;
    options: ButterTransactionOption;
};
```
`paidAmount`: get amount of token needed, please see [get paidAmount](#paidamount)

`swapRouteStr`: optimal cross-chain swap route based on token and amount provided. Please see how to [get the best route](routes#get-the-best-route)

`ButterTransactionOption` contains all the necessary information required to complete a transaction:

```typescript
export type ButterTransactionOption = {
  signerOrProvider?: Signer | Provider | Eth; // When source chain is EVM provide Ethers.js Signer/Provider or Web3.js Eth info
  nearProvider?: NearProviderType; // mandatory when src chain is near
  gas?: string;
  gasPrice?: string;
};

// when send transaction from Near Protocol
type NearProviderType = NearNetworkConfig | WalletConnection;
```
`signerOrProvider`: Butter supports both _ethers.js_ and _web3.js_. If you are using ethers.js, provider the [`Signer`](https://docs.ethers.org/v5/api/signer/) object. If your application choose to use web3.js, please provide [`Eth`](https://web3js.readthedocs.io/en/v1.2.11/web3-eth.html) object in order to send a transaction.

`nearProvider`: Whenever send a transaction from Near Protocol, you have to provide [`NearNetworkConfig`](https://near.github.io/near-api-js/interfaces/connect.ConnectConfig) with keystore provided or [`WalletConnection`](https://near.github.io/near-api-js/classes/walletAccount.WalletConnection/) object

## Response
Please refer to [response type](types#buttertransactionresponse).

## Get `paidAmount`<a name="paidamount"></a>

First of all, we need to determine the amount of `paidToken` needed in order to reach the `required amount`.

For example, Alice wants to purchase an item priced at 400 $USDC on Ethereum, but she wants to pay by $BNB. There should be a way to determine how many of $BNB token she need to pay for the seller to receive the $400 USDC on Ethereum required.

Butter provides the `getEstimateAmountInFromRequiredAmount()` function to do that.

```typescript
export async function getEstimateAmountInFromRequiredAmount(
  fromToken: BaseCurrency, // paid token, in the above case BNB native coin.
  toToken: BaseCurrency, // required token, in the above case ETH_USDC.
  requiredAmount: string // required amount, in the above case 400 $USDC.
): Promise<string>;
```
The return value will be the estimated amount of `fromToken` needed.

## Gas estimation
Estimate the gas cost of payment transaction
```typescript
async function gasEstimateOmniPay({
    fromAddress,
    paidToken,
    paidAmount, // in minimal uint
    toAddress,
    requiredToken,
    requiredAmount,
    swapRouteStr,
    options,
}: PaymentRequestParam): Promise<string>
```

## Execute Payment
```typescript
async function omniPay({
    fromAddress,
    paidToken,
    paidAmount, // in minimal uint
    toAddress,
    requiredToken,
    requiredAmount,
    swapRouteStr,
    options,
}: PaymentRequestParam): Promise<ButterTransactionResponse>;
 ```
#### Example 1: Alice wants to pay with BNB native coin for an item that requires USDC on Ethereum priced at 400 USDC

```typescript
import {ButterTransactionResponse} from "./responseTypes";
import {PromiEvent} from "web3-core";

// first we need to get how many $BNB native coin needed.
const paidAmount = await getEstimateAmountInFromRequiredAmount(
    BNB_NATIVE,
    ETH_USDC,
    '400000000'
)

// create a omnichain payment instance.
const omniPayment: OmniPayment = new OmniPayment();

// assemble payment request parameters
const paymentRequest: PaymentRequestParam = {
    fromAddress: '0x...',
    paidToken: BNB_NATIVE,
    paidAmount: paidAmount, // in minimal uint
    toAddress: '0x...',
    requiredToken: ETH_USDC,
    requiredAmount: 400000000,
    options: {
        signerOrProvider: web3.eth, // here we use web3.js as example
    }
};

const response: ButterTransactionResponse = await omniPayment.omniPay(
    paymentRequest
);

const promiReceipt: PromiEvent<TransactionReceipt> = response.promiReceipt!;

await promiReceipt
    .on('transactionHash', function (hash: string) {
        console.log('hash', hash);
    })
    .on('receipt', function (receipt: any) {
        console.log('receipt', receipt);
    });

```
##### Output:
```
hash 0x..... // transaction hash
receipt { // web3.js TransactionReceipt
    status: boolean;
    transactionHash: string;
    transactionIndex: number;
    blockHash: string;
    blockNumber: number;
    from: string;
    to: string;
    contractAddress?: string;
    cumulativeGasUsed: number;
    gasUsed: number;
    effectiveGasPrice: number;
    logs: Log[]..;
    logsBloom: string;
    events?: {
        [eventName: string]: EventLog;
    };
}
```
