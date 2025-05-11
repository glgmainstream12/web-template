# Views Directory

This directory contains view templates for server-side rendering (if used) and email templates.

## Purpose

- Store view templates
- Handle server-side rendering
- Define email templates
- Manage template layouts
- Share common components

## Best Practices

1. **Template Organization**
   - Group related templates
   - Use consistent naming
   - Implement layouts
   - Share common components

2. **Template Engine**
   - Use appropriate template engine
   - Follow engine best practices
   - Implement proper escaping
   - Handle errors gracefully

3. **Security**
   - Prevent XSS attacks
   - Sanitize user input
   - Use proper escaping
   - Implement CSRF protection

4. **Performance**
   - Cache templates when possible
   - Minimize template complexity
   - Use partials for reusability
   - Optimize rendering

## Example Templates

Here's an example of an email template using EJS:

```typescript
// emailTemplates/welcomeEmail.ejs
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Welcome to Our Platform</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      line-height: 1.6;
      color: #333;
    }
    .container {
      max-width: 600px;
      margin: 0 auto;
      padding: 20px;
    }
    .header {
      text-align: center;
      padding: 20px 0;
      background-color: #f8f9fa;
    }
    .content {
      padding: 20px 0;
    }
    .footer {
      text-align: center;
      padding: 20px 0;
      font-size: 12px;
      color: #666;
    }
    .button {
      display: inline-block;
      padding: 10px 20px;
      background-color: #007bff;
      color: white;
      text-decoration: none;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="header">
      <h1>Welcome to Our Platform!</h1>
    </div>
    
    <div class="content">
      <p>Hello <%= firstName %>,</p>
      
      <p>Thank you for joining our platform. We're excited to have you on board!</p>
      
      <p>To get started, please verify your email address by clicking the button below:</p>
      
      <p style="text-align: center;">
        <a href="<%= verificationUrl %>" class="button">Verify Email</a>
      </p>
      
      <p>If you didn't create an account, you can safely ignore this email.</p>
      
      <p>Best regards,<br>The Team</p>
    </div>
    
    <div class="footer">
      <p>This email was sent to <%= email %>. If you have any questions, please contact our support team.</p>
      <p>&copy; <%= new Date().getFullYear() %> Our Platform. All rights reserved.</p>
    </div>
  </div>
</body>
</html>

// emailService.ts
import ejs from 'ejs';
import path from 'path';
import { sendEmail } from '../utils/email';

export class EmailService {
  private templatePath: string;

  constructor() {
    this.templatePath = path.join(__dirname, 'emailTemplates');
  }

  async sendWelcomeEmail(user: { email: string; firstName: string }): Promise<void> {
    const template = await ejs.renderFile(
      path.join(this.templatePath, 'welcomeEmail.ejs'),
      {
        firstName: user.firstName,
        email: user.email,
        verificationUrl: `${process.env.APP_URL}/verify-email?token=${verificationToken}`,
        year: new Date().getFullYear()
      }
    );

    await sendEmail({
      to: user.email,
      subject: 'Welcome to Our Platform!',
      html: template
    });
  }
} 