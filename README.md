## How to Build, Configure, and Run Your Application

### Prerequisites

- Node.js
- npm
- Stripe account with test API keys

## Steps to Set Up and Run

1. Clone the repository and install dependencies:

```bash
   git clone https://github.com/mattmitchell6/sa-takehome-project-node
   cd sa-takehome-project-node
   npm install
```

2. Replace API keys with your Stripe account's test API keys in the .env file

   Note: You can manage payment methods and retrieve test API keys from the Stripe Dashboard [https://dashboard.stripe.com/].

3. Run the application locally:

```bash
npm start
```

4. Open a browser and navigate to [http://localhost:3000](http://localhost:3000) to view the application.

## How Does the Solution Work? Which Stripe APIs Does It Use? How Is the Application Architected?

The application is written in JavaScript (Node.js) using the Express framework and Bootstrap CSS framework. This app is an MVP and does not use a database. Product details are hardcoded in app.js.

### Checkout Flow

1. The customer selects a product (book) on the app.
2. The client (app) calls GET /checkout?item={id} and passes the item id to the server (e.g. GET /checkout?item=2).
3. The server calls POST /v1/payment_intents with amount and currency parameters to create a new Payment Intent.
4. Stripe returns the newly created Payment Intent to the server, which then sends the client_secret to the client (app).
5. The customer enters billing details (card information) and finalizes the payment. If an error occurs when the user clicks the payment button, the client (app) displays an error message above the button.
6. The client (app) calls POST /v1/payment_intents/:id/confirm to Stripe.
7. Stripe redirects the customer to the specified return URL (/success).
8. The client (app) displays a payment confirmation message, including the payment amount and Payment Intent ID.
   If an error occurs, the client (app) displays an error message above the button.

## Stripe APIs Used

- POST /v1/payment_intents: Creates Payment Intents.
- POST /v1/payment_intents/:id/confirm: Confirms Payment Intents (this is triggered by the stripe.confirmPayment function in the frontend).

References

- Accept a Payment Documentation

  https://docs.stripe.com/payments/accept-a-payment?platform=web&ui=elements#web-create-intent

### Approach to Solving the Problem, Key References, and Challenges Encountered with Resolutions

1. Extracted the requirements from the project prompt and divided them into smaller, manageable tasks.
2. Reviewed Stripe Payment Element documentation to understand its capabilities and integration process with an Express application.
3. Implemented the backend first to handle Payment Intent creation.
4. Developed the frontend to render the Payment Element and interact with Stripe APIs.
5. Referred to Stripe's code samples and integrated them incrementally, testing each step for easier troubleshooting.

## Documentation and Resources Used

- Accept a Payment Documentation[https://docs.stripe.com/payments/accept-a-payment?platform=web&ui=elements]
- Best Practices for Payment Element[https://docs.stripe.com/payments/payment-element/best-practices]
- Payment Intents API[https://docs.stripe.com/payments/payment-intents]

## Challenges and Resolutions

I encountered a few challenges during development. For instance, a 400 error occurred because I initially forgot to include the amount parameter when creating the paymentIntent. Additionally, after updating the server-side code in app.js, I overlooked restarting the server, which caused further issues. Another oversight was failing to include the publishable_key for the /success route in app.js.
To resolve these issues, I relied on Stripe's dashboard logs, the web browser console, and terminal outputs. The error messages provided by these tools were helpful in identifying and debugging these problems.

### How this application could be extended for a more robust implementation

## Improved Payment Features:

- Enable users to enter the quantity of books (currently limited to one item per order).
- Add support for additional payment methods, such as Google Pay, Apple Pay, and direct debit.
- Include billing and shipping address collection during the checkout process.
- Implement idempotency keys when calling POST /v1/payment_intents to avoid duplicates (currently, refreshing the checkout page will create a payment intent)

## Enhanced User Experience:

- Customize the UI to align with the application's design and branding.
- Automate receipt emails for completed payments.

## Add Advanced Payment and Subscription Management:

- Integrate Stripe webhooks to handle asynchronous payment events like failed payments or refunds.

## Scalability and Internationalization:

- Add database to store product details and purchase history
- Add support for multiple currencies and local payment methods to cater to a global audience

## Security and User Management:

- Implement authentication and user accounts to securely store payment history and personalize the experience.

## Reliability and Testing:

- Add unit and integration tests to ensure application reliability and prevent regressions during future updates.
