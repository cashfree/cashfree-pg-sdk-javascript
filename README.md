
# CASHFREE PAYMENT GATEWAY

## Documentation
&rarr; **Drop** is our pre-built UI solution for accepting payments. Drop works by displaying payment components we call as drops at any place you want in your page. It can either display all the payment components at one place or different places depending on your need.

&rarr; **Redirect** is the other solution for accepting payments where we can use Cashfree’s user interface to capture payment details. You will do this by redirecting the customer to a cashfree hosted page, which can be customized for your needs.

&rarr; **Element** is used for accepting payments where merchant can integrate Cashfree's Payment Gateway to build their own UI using customizable HTML code snippets

## Environments
SDK is available in 2 different environments

&rarr; **Sandbox** is used during your testing. No actual debit occurs. Everything is simulated. Using this environment you should be able to integrate the SDK and test payments.

&rarr; **Production** is used once you have finalized your testing on the sandbox, you can use the production environment.

## Prerequisite
### Drop and Redirect Flow
Domain name is the address used to access a website.

Merchants would be able to use the checkout only when a request comes from a whitelisted domain and other requests coming from any other domain would be blocked.

This means, merchants cannot integrate our PG on any other website other than the whitelisted one.

Example - https://www.cashfree.com => Domain name would be "cashfree.com" 

### Element 
Domain name is the address used to access a website.

Merchants would be able to use the checkout only when a request comes from a whitelisted domain and other requests coming from any other domain would be blocked.

This means, merchants cannot integrate our PG on any other website other than the whitelisted one.

Example - https://www.cashfree.com => Domain name would be "cashfree.com" 

| Along with Whitelisting, Inorder to use element you need to make sure to enable it from Cashfree's end |
| --- |

## Installation
1. Using NPM package
```
npm i cashfree-pg-sdk-javascript
```
Note: add --save if you are using npm < 5.0.0

2. Using SDK URL
&rarr; Sandbox
```
<script src="https://sdk.cashfree.com/js/ui/2.0.0/cashfree.sandbox.js"></script>
```
&rarr; Production
```
<script src="https://sdk.cashfree.com/js/ui/2.0.0/cashfree.prod.js"></script>
```
Make sure only one of the above is present in your application

## Integration
1. Using NPM package
&rarr; Sandbox
```
import { cashfreeSandbox } from "cashfree-sdkjs";
let cashfree = new cashfreeSandbox.Cashfree(paymentSessionId);
```
&rarr; Production
```
import { cashfreeProd } from "cashfree-sdkjs";
let cashfree = new cashfreeProd.Cashfree(paymentSessionId);
```
Make sure only one of the above is present in your application

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
    components: Array, #An array that takes in the components to be rendered. Available: [order-details, card, netbanking, app, upi, paylater, creditcardemi, cardlessemi]
    style: object, #override the existing dropin style
});
```

2. Redirect
```
cashfree.redirect();
```

3. Element
```
const options = {
    onPaymentSuccess: function,
    onPaymentFailure: function,
    onError: function,
};
```
**Card**

&rarr; HTML
```
<div id="pay-card">
    <input type="text" data-card-holder> #Card Holder Name
    <input type="text" data-card-number> #Card Number
    <input type="text" data-card-expiry-mm> #Card Expiry Month
    <input type="text" data-card-expiry-yy> #Card Expiry Year
    <input type="password" data-card-cvv> #Card CVV
</div>
<button id="pay-btn">Pay</button>
```
&rarr; JS
```
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
    pay: document.getElementById('pay-card'),
    type: 'card',
});
document.getElementById('pay-btn').addEventListener('click', async () => {
    await cfCheckout.pay("card");
});
```

**UPI Collect**

&rarr; HTML
```
<div id="pay-collect">
    <input  type="text" data-upi-id> #Customer UPI_ID
</div>
<button id="pay-collect-btn">Pay</button>
```
&rarr; JS
```
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById('pay-collect'),
	type: 'upi-collect',
});
document.getElementById('pay-collect-btn').addEventListener('click', async () => {
	await cfCheckout.pay("upi-collect");
});
```

**UPI QR**

&rarr; HTML
```
<button id="pay-qr-btn">Pay</button>
```
&rarr; JS
```
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	type: 'upi-qrcode',
});
document.getElementById('pay-qr-btn').addEventListener('click', async () => {
	await cfCheckout.pay("upi-qrcode");
});
```

**Net Banking**

&rarr; HTML
```
<div id="pay-nb">
    <input  type="text" data-netbanking-code> #Bank Code Given By Cashfree
</div>
<button id="pay-nb-btn">Pay</button>
```
&rarr; JS
```
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById("pay-nb"),
    type: "netbanking",
});
document.getElementById('pay-nb-btn').addEventListener('click', async () => {
	await cfCheckout.pay("netbanking");
});
```

**UPI Apps**

&rarr; HTML
```
<div id="pay-intent">
    <select data-upi-provider> #UPI Identifier
        <option value="">select</option>
        <option value="gpay">gpay</option>
        <option value="phonepe">phonepe</option>
        <option value="paytm">paytm</option>
        <option value="bhim">bhim</option>
    </select>
</div>
<button id="pay-intent-btn">PAY</button>
```
&rarr; JS
```
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById("pay-intent"),
    type: "upi-intent",
});
document.getElementById('pay-intent-btn').addEventListener('click', async () => {
	await cfCheckout.pay("upi-intent");
});
```

**Mobile Applications / Wallets**

&rarr; HTML
```
<div id="pay-app">
    <select data-app-name> #Mobile App Name
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
```
&rarr; JS
```
const cfCheckout = cashfree.elements(options);
cfCheckout.elements({
	pay: document.getElementById("pay-app"),
    type: "app",
});
document.getElementById('pay-app-btn').addEventListener('click', async () => {
	await cfCheckout.pay("app");
});
```
