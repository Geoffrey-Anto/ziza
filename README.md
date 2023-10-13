# 📧 Ziza - Next.js Email Service for the Client

Ziza is an open-source Next.js-based email service for the client. It's fully type-safe, making it easy to send emails from your Next.js application with confidence. Ziza utilizes Node Mailer under the hood to send emails and provides a template system that helps you craft beautiful and dynamic email content.

## Features

✉️ Fully type-safe email templates with automatic data prompts.  
📨 Backend configured with Nodemailer.  
📄 HTML-based email templates.  
🔒 Secure SMTP transport options.  
📦 Attachments support.  

## Installation

To get started with Ziza, you need to install it using npm, yarn, or pnpm:

```bash
npm install ziza
# or
yarn add ziza
# or
pnpm i ziza
```
<br/>

### ⚠️ **Please Note:** Ziza currently only works with the Pages Directory in your Next.js app. Next.js App Directory will be available soon.

<br/>


## Usage

### 1. Create Email Templates

Define your email templates using the `createTemplate` function provided by Ziza. Here's an example:

```typescript
import { createTemplate } from "ziza/template";
import { z } from "zod";

export const template = createTemplate({
  bills: {
    data: {
      price: z.number(),
      noOfItems: z.number(),
    },
    html: `
      <div>
        <h1>Welcome !</h1>
        <p>You bought {{noOfItems}} Cars of {{price}}$.</p>
      </div>
    `,
    defaults: {
      subject: "Your bill!",
    },
  },
  welcome: {
    data: {
      name: z.string(),
      email: z.string(),
    },
    html: `
      <div>
        <h1>Welcome {{name}} {{email}}!</h1>
        <p>Thank you for registering with us.</p>
      </div>
    `,
  },
});
```

### 2. Configure Email Transporter Endpoint

Configure the email transporter, specifying the SMTP transport adapter. For example, using Gmail's SMTP:

⚠️ *Please Note:* This needs to be pasted in a special directory under ```pages/api/ziza/[ziza].ts``` in the API routes folder under pages directory.

```typescript
import { gmailSmtpTransporterAdapter } from "ziza/adapters";
import { createEmailApiHandler, createTransporter } from "ziza/server";
import { template } from "path/to/your/template/file";

export default createEmailApiHandler(
  template,
  createTransporter({
    auth: {
      from: "youremailaddress@gmail.com",
      password: process.env.GMAIL_PASSWORD as string,
    },
    smtpTransporter: gmailSmtpTransporterAdapter,
  })
);
```

### 3. Send Emails

Now you can use Ziza to send emails easily:

```javascript
import { createEmailClient } from "ziza/client";
  
const emailClient = createEmailClient(template);

const res = await emailClient.sendEmail(
  "foo",
  { bar: true },
  {
    to: "youremailaddress@gmail.com",
    cc: "youremailaddress@yahoo.com",
    attachments: [
      {
        filename: yourFileName,
        encoding: "base64",
        content: fileData,
      },
    ],
  }
);
```

## 🚀 Get Started

To start using Ziza in your Next.js project, follow the installation and usage instructions in this README.

## 📖 Documentation

For more details on Ziza's features and usage, please refer to the [official documentation](https://ziza-docs.example.com).

## 🧪 Testing

Ziza comes with a comprehensive test suite. You can run tests using:

```bash
npm test
# or
yarn test
# or
pnpm test
```

## 🤝 Contributing

We welcome contributions! Feel free to open issues or submit pull requests on the [GitHub repository](https://github.com/zeptt/ziza).

## 📃 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

📧 Ziza is developed and maintained with ❤️ by US.
