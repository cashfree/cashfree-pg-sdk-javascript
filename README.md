# CASHFREE PAYMENT GATEWAY

## Documentation
There are two ways to integrate Cashfree Payment Gateway
```
1. Drop is our pre-built UI solution for accepting payments. Drop works by displaying payment components we call as drops at any place you want in your page. It can either display all the payment components at one place or different places depending on your need.

2. Redirect is the other solution for accepting payments where we can use Cashfree’s user interface to capture payment details. You will do this by redirecting the customer to a cashfree hosted page, which can be customized for your needs.
```

## Environments
SDK is available in 2 different environments:
```
1. Sandbox is used during your testing. No actual debit occurs. Everything is simulated. Using this environment you should be able to integrate the SDK and test payments.

2. Production is used once you have finalized your testing on the sandbox, you can use the production environment.
```

## Installation
1. Using NPM package
```
npm i cashfree-sdkjs
```
Note: add --save if you are using npm < 5.0.0

2. Using SDK URL
```
Sandbox: 
<script src="https://sdk.cashfree.com/js/ui/2.0.0/cashfree.sandbox.js"></script>

Production: 
<script src="https://sdk.cashfree.com/js/ui/2.0.0/cashfree.prod.js"></script>

Make sure only one of the above is present in your application
```

## Integration
1. Using NPM package
```
Sandbox: 
import { cashfreeSandbox } from "cashfree-sdkjs";
let cashfree = new cashfreeSandbox.Cashfree(paymentSessionId);

Production: 
import { cashfreeProd } from "cashfree-sdkjs";
let cashfree = new cashfreeProd.Cashfree(paymentSessionId);
```

2. Using SDK URL
```
const cashfree = new Cashfree(paymentSessionId);
```

## Implementation
1. Drop
```
let parent = document.getElementById("drop_in_container");
cashfree.drop(parent, {
    onSuccess: function,
    onFailure: function,
    components: Array, #An array that takes in the components to be rendered. Available: order-details, card, netbanking, app, upi, paylater, creditcardemi, cardlessemi
    style: object, #override the existing dropin style
});
```

2. Redirect
```
cashfree.redirect();
```

## Individual Elements Implementation
```
const options = {
    onPaymentSuccess: function,
    onPaymentFailure: function,
    onError: function,
};
```
1. Card
```
HTML:
<div id="pay-card">
    <input type="text" data-card-holder> #CardHolderName
    <input type="text" data-card-number> #CardNumber
    <input type="text" data-card-expiry-mm> #CardExpiryMonth
    <input type="text" data-card-expiry-yy> #CardExpiryYear
    <input type="password" data-card-cvv> #CardCVV
</div>
<button id="pay-btn">Pay</button>

JS:
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
    pay: document.getElementById('pay-card'),
    type: 'card',
});
document.getElementById('pay-btn').addEventListener('click', async () => {
    await cfCheckout.pay("card");
});
```

2. UPI Collect
```
HTML:
<div id="pay-collect">
    <input  type="text" data-upi-id> #CustomerUPI_ID
</div>
<button id="pay-collect-btn">Pay</button>

JS:
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById('pay-collect'),
	type: 'upi-collect',
});
document.getElementById('pay-collect-btn').addEventListener('click', async () => {
	await cfCheckout.pay("upi-collect");
});
```

3. UPI QR
```
HTML:
<button id="pay-qr-btn">Pay</button>

JS:
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	type: 'upi-qrcode',
});
document.getElementById('pay-qr-btn').addEventListener('click', async () => {
	await cfCheckout.pay("upi-qrcode");
});
```

4. Net Banking
```
HTML:
<div id="pay-nb">
    <input  type="text" data-netbanking-code> #BankCodeGivenByCashfree
</div>
<button id="pay-nb-btn">Pay</button>

JS:
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById("pay-nb"),
    type: "netbanking",
});
document.getElementById('pay-nb-btn').addEventListener('click', async () => {
	await cfCheckout.pay("netbanking");
});
```

5. UPI Apps
```
HTML:
<div id="pay-intent">
    <select data-upi-provider> #UPI_Identifier
        <option value="">select</option>
        <option value="gpay">gpay</option>
        <option value="phonepe">phonepe</option>
        <option value="paytm">paytm</option>
        <option value="bhim">bhim</option>
    </select>
</div>
<button id="pay-intent-btn">PAY</button>

JS:
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById("pay-intent"),
    type: "upi-intent",
});
document.getElementById('pay-intent-btn').addEventListener('click', async () => {
	await cfCheckout.pay("upi-intent");
});
```

6. Mobile Applications / Wallets
```
HTML:
<div id="pay-app">
    <select data-app-name> #MobileAppName
        <option value="">select</option>
        <option value="gpay">gpay</option>
        <option value="phonepe">phonepe</option>
        <option value="paytm">paytm</option>
        <option value="amazon">amazon</option>
        <option value="airtel">airtel</option>
        <option value="freecharge">freecharge</option>
        <option value="mobikwik">mobikwik</option>
        <option value="jio">jio</option>
        <option value="ola">ola</option>
    </select>
    <input type="text" data-app-phone />
</div>
<button id="pay-app-btn">PAY</button>

JS:
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById("pay-app"),
    type: "app",
});
document.getElementById('pay-app-btn').addEventListener('click', async () => {
	await cfCheckout.pay("app");
});
```
