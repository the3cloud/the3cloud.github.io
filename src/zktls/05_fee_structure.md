# Fee Structure

The zkTLS contract fees are calculated using the `zkTLSGateway.computeFee()` function, combined with base fee configurations. The fee structure accounts for both gas costs and verifier fees. Before submitting a request, ensure your account/client contract has sufficient balance for both native gas and The3Cloud Coin service fees. When a request is submitted, both the native gas and payment verifier fees are locked. Upon response delivery, the actual charges are sent to the configured beneficiary, and any unused fees are returned to the account/client contract.

1. **Native Gas Fee**
   - Consists of two components:
     * **Static Gas**: 
       - Base transaction gas (fixed amount per blockchain)
       - Padding gas (buffer to ensure sufficient gas for callback execution)
     * **Dynamic Gas**: 
       - Calculated as: `responseBytes * 16 + configured nativeVerifyFee`
   - Covers callback execution and proof verification costs

2. **Payment Verifier Fee**
   - Calculated as: `responseBytes * weiPerBytes + configured base charge for proof computation`
   - Covers request/response processing and proof generation costs

To calculate the total required fee for a request, use:

```solidity
(uint256 nativeGas, uint256 paymentVerifierFee) = zkTLSGateway.computeFee(
    requestId,
    requestData,
    maxResponseBytes
);
```
