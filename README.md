# USSD Payment System

This is a USSD payment system implemented using Node.js and Africa's Talking API.

## Features

- Secure handling of USSD transactions
- Integration with Africa's Talking Payment API
- Transaction history for users
- Environment variable usage for sensitive information
- Security headers, CORS, and rate limiting for enhanced security

## Prerequisites

- Node.js installed
- Africa's Talking account with API key and username
- Necessary environment variables set up (AT_USERNAME, AT_API_KEY)

## Installation

1. Clone the repository:

    ```bash
    git clone https://github.com/yourusername/ussd-payment-system.git
    cd ussd-payment-system
    ```

2. Install dependencies:

    ```bash
    npm install
    ```

3. Set up environment variables:

    Create a `.env` file in the root directory and add the following:

    ```env
    AT_USERNAME=your-africastalking-username
    AT_API_KEY=your-africastalking-api-key
    PORT=3000  # Optional, specify a different port if needed
    ```

4. Run the server:

    ```bash
    npm start
    ```

## Usage

- Access the USSD payment system via the defined endpoints.
- Make USSD payments and view transaction history.

## Endpoints

- USSD Payment: `POST /ussd-payment`
- Transaction History: `GET /transaction-history/:phoneNumber`

## Security Considerations

- Use HTTPS for secure communication.
- Keep environment variables secure and do not expose sensitive information.

## Contributing

Contributions are welcome! Please submit issues or pull requests.

## License

This project is licensed under the [MIT License](LICENSE).
