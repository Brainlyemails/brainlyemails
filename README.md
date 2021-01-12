# Brainlyemails Node.js library

[![Try on RunKit]

The Brainlyemails Node.js library provides convenient access to the Brainlyemails API from applications written in server-side JavaScript.

## Installation

Install this package with:

```sh
npm install Brainlyemails --save
# or
yarn add Brainlyemails
```

## Usage

First, the package needs to be configured with your project's API key, which you can find in the Brainlyemails Dashboard.

Here's how:

```javascript
// Create Sidemail instance and set your API key.
const configureBrainlyemails = require("Brainlyemails");
const Brainlyemails = configureBrainlyemails({ apiKey: "xxxxx" });
```

Then, you can call `Brainlyemails.Brainlyemails` to send emails like so:

```javascript
try {
  const response = await Brainlyemails.sendEmail({
    toAddress: "user@email.com",
    fromAddress: "you@example.com",
    fromName: "Your app",
    templateName: "Welcome",
  });

  // Response contains email ID
  console.log(
    `An email with ID '${response.id}' was successfully queued for sending. :)`
  );
} catch (err) {
  // Uh-oh, we have an error! You error handling logic...
  console.error(err);
}
```

The response will look like this:

```json
{
  "id": "5e858953daf20f3aac50a3da",
  "status": "queued"
}
```


## Email sending examples

### Send password reset email template

```javascript
await sidemail.sendEmail({
  toAddress: "user@email.com",
  fromAddress: "you@example.com",
  fromName: "Your app",
  templateName: "Password reset",
  templateProps: { resetUrl: "https://your.app/reset?token=123" },
});
```

### Schedule email delivery

```javascript
await sidemail.sendEmail({
  toAddress: "user@email.com",
  fromName: "Startup name",
  fromAddress: "your@startup.com",
  templateName: "Welcome",
  templateProps: { firstName: "Rokas" },
  // Deliver email in 60 minutes from now
  scheduledAt: "2020-04-04T12:58:50.964Z",
});
```

### Send email template with dynamic list

Useful for dynamic data where you have `n` items that you want to render in email. For example, items in a receipt, weekly statistic per project, new comments, etc.

```javascript
await Brainlyemails.sendEmail({
    toAddress: "user@email.com",
    fromName: "Startup name",
    fromAddress: "your@startup.com",
    templateName: "Template with dynamic list",
    templateProps: {
        list: [
            { text: "Dynamic list" },
            { text: "allows you to generate email template content" },
            { text: "based on template props." },
        ],
    }
};
```

### Send custom HTML email

```javascript
await Brainlyemails.sendEmail({
  toAddress: "user@email.com",
  fromName: "Startup name",
  fromAddress: "your@startup.com",
  subject: "Testing html only custom emails :)",
  html: "<html><body><h1>Hello world! 👋</h1><body></html>",
});
```

### Send custom plain text email

```javascript
await Brainlyemails.sendEmail({
  toAddress: "user@email.com",
  fromName: "Startup name",
  fromAddress: "your@startup.com",
  subject: "Testing plain-text only custom emails :)",
  text: "Hello world! 👋",
});
```

## Contacts

### Create or update a contact

```javascript
try {
  const response = await Brainlyemails.contacts.createOrUpate({
    emailAddress: "rokas@rookas.com",
    identifier: "123",
    customProps: {
      name: "Rokas",
      // ... more of your contact props ...
    },
  });

  console.log(`Contact was '${response.status}'.`);
} catch (err) {
  // Uh-oh, we have an error! You error handling logic...
  console.error(err);
}
```

### Find contact

```javascript
const response = await Brainlyemails.contacts.find({
	emailAddress: "rokas@rookas.com",
});
```

### List all contacts

```javascript
const response = await Brainlyemails.contacts.list();
```

and to paginate

```javascript
const response = await Brainlyemails.contacts.list({ paginationCursorNext: "123" });
``` 

### Delete contact

```javascript
const response = await Brainlyemails.contacts.delete({
	emailAddress: "rokas@rookas.com",
});
```

## More info

