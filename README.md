const express = require('express');
const bodyParser = require('body-parser');
const africastalking = require('africastalking');
const helmet = require('helmet');
const cors = require('cors');
const rateLimit = require('express-rate-limit');

const app = express();
const port = process.env.PORT || 3000;

// Set up Africa's Talking credentials from environment variables
const username = process.env.AT_USERNAME || 'default-username';
const apiKey = process.env.AT_API_KEY || 'default-api-key';

const africastalkingOptions = {
  apiKey,
  username,
};

const africastalkingInstance = africastalking(africastalkingOptions);
const payments = africastalkingInstance.PAYMENTS;

// In-memory database for storing user transactions (replace with a database in production)
const transactionsDatabase = [];

// Middleware for security headers
app.use(helmet());

// Middleware for handling CORS
app.use(cors());

// Middleware to parse JSON requests
app.use(bodyParser.json());

// Rate limiting middleware
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
});

app.use(limiter);

// Route to handle USSD payment
app.post('/ussd-payment', (req, res) => {
  const { sessionId, phoneNumber, text } = req.body;

  // Parse the text received from the USSD service
  const userInput = text.split('*');

  // Assuming that the last digit entered is the payment amount
  const paymentAmount = parseFloat(userInput[userInput.length - 1]);

  // Perform payment using Africa's Talking Payment API
  payments.mobileCheckout({
    productName: 'YourProduct',
    phoneNumber,
    currencyCode: 'USD', // Use the appropriate currency code
    amount: paymentAmount,
    metadata: {
      custom_field: 'value',
    },
  }).then(response => {
    console.log(response);

    // Save transaction details to the database
    const transactionDetails = {
      transactionId: response.transactionId,
      phoneNumber,
      amount: paymentAmount,
      status: 'completed',
      timestamp: new Date(),
    };
    transactionsDatabase.push(transactionDetails);

    res.send('Payment successful');
  }).catch(error => {
    console.error(error);
    res.send('Payment failed');
  });
});

// Route to fetch transaction history
app.get('/transaction-history/:phoneNumber', (req, res) => {
  const phoneNumber = req.params.phoneNumber;

  // Filter transactions for the given phone number
  const userTransactions = transactionsDatabase.filter(transaction => transaction.phoneNumber === phoneNumber);

  res.json(userTransactions);
});

// Start the server
app.listen(port, () => {
  console.log(Server is running at http://localhost:${port});
});