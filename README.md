## How to Build, Configure, and Run Your Application

### Prerequisites

- Node.js
- npm
- Stripe account with test API keys

### Steps to Set Up and Run

1. Clone the repository and install dependencies:

```bash
   git clone https://github.com/mattmitchell6/sa-takehome-project-node
   cd sa-takehome-project-node

   npm install
```

2. Replace API keys with your Stripe account's test API keys in the .env file

   Note: You can manage payment methods and retrieve test API keys from the [Stripe Dashboard](https://dashboard.stripe.com/).

3. Run the application locally:

```bash
npm start
```

4. Open a browser and navigate to [http://localhost:3000](http://localhost:3000) to view the application.

## How Does the Solution Work? Which Stripe APIs Does It Use? How Is the Application Architected?

The application is written in JavaScript (Node.js) using the [Express framework](https://expressjs.com/) and [Bootstrap](https://getbootstrap.com/docs/4.6/getting-started/introduction/) CSS framework. This app is an MVP and does not use a database. Product details are hardcoded in app.js.

### Checkout Flow

1. The customer selects a product (book) on the app.
2. The client (app) calls GET /checkout?item={id} and passes the item id to the server e.g. GET /checkout?item=2
3. The server calls POST /v1/payment_intents with amount and currency parameters for Stripe to create a new Payment Intent.
4. Stripe returns the newly created Payment Intent to the server, which then sends the client_secret to the client (app).
5. The customer enters billing details (card information) and finalizes the payment. If an error occurs when the customer clicks the payment button, the client (app) displays an error message
6. The client (app) calls POST /v1/payment_intents/:id/confirm to Stripe.
7. Stripe redirects the customer to the specified return URL (/success).
8. The client (app) displays a payment confirmation message, including the payment amount and Payment Intent ID.
   If an error occurs, the client (app) displays an error message above the button.

### Solution Diagram

![Alt Text](/diagram.png)

### Stripe APIs Used

- POST /v1/payment_intents: Creates Payment Intents.
- POST /v1/payment_intents/:id/confirm: Confirms Payment Intents (the stripe.confirmPayment function executes this endpoint call under the hood).

##### Reference

- [Accept a Payment](https://docs.stripe.com/payments/accept-a-payment?platform=web&ui=elements#web-create-intent)

## Approach to Solving the Problem, Documentations, and Challenges Encountered with Resolutions

### Approach to Solving the Problem

1. Extracted the requirements from the project prompt and divided them into smaller, manageable tasks.
2. Reviewed Stripe Payment Element documentation to understand its capabilities and how it integrates into an Express application(server side rendering)/the given starter codebase by stripe.
3. Implemented the backend first to create Payment Intents, referring to Stripe's code samples then developed the frontend to render the Payment Element and call relevant Stripe APIs.

   Note: Added code blocks in small increments following stripe documentations so it is easy to troubleshoot

### Documentations used

- [Accept a Payment Documentation](https://docs.stripe.com/payments/accept-a-payment?platform=web&ui=elements)
- [Payment Intents API](https://docs.stripe.com/payments/payment-intents)
- [Best Practices for Payment Element](https://docs.stripe.com/payments/payment-element/best-practices)

### Challenges and Resolutions

I encountered a few challenges during development. For instance, a 400 error occurred because I initially forgot to include the amount parameter when creating the paymentIntent. Additionally, after updating the server-side code in app.js, I overlooked restarting the server, which caused further issues. Another oversight was failing to include the publishable_key for the /success route in app.js.

To resolve these issues, I relied on Stripe's dashboard logs, the web browser console, and terminal outputs. The error messages provided by these tools were helpful in identifying and debugging these problems.

## How this application could be extended for a more robust implementation

### Improved Payment Features:

- Enable users to enter the quantity of books (currently limited to one item per order).
- Add support for additional payment methods, such as Google Pay, Apple Pay, and direct debit.
- Include billing and shipping address collection during the checkout process.
- Implement idempotency keys when calling POST /v1/payment_intents to avoid duplicates (currently, refreshing the checkout page will create a payment intent)

### Enhanced User Experience:

- Customize the payment element UI to align with the application's design and branding.
- Automate receipt emails for completed payments.

### Add Advanced Payment Management:

- Integrate Stripe webhooks to handle asynchronous payment events like failed payments or refunds.

### Scalability and Internationalization:

- Add database to store product details and purchase history
- Add support for multiple currencies and local payment methods to cater to a global audience

### Security:

- Implement authentication and user accounts to securely store payment history

### Reliability and Testing:

- Add unit and integration tests to ensure application reliability and prevent regressions during future updates.
