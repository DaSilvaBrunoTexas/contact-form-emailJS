# Contact Form with EmailJS

Create a simple React contact form with real time email handler using using third-party service EmailJS. EmailJS allows you to send email directly from your client-side Javascript code. It's free package includes:

    200 monthly requests
    2 email templates
    Requests up to 50Kb
    Limited contacts history

Let's get started.

### Step 1: Creating an EmailJS account

Go to https://www.emailjs.com/ and create an account. For a portfolio usage the free account plan is more than enough, you will get roughly 200 emails monthly.

### Step 2: EmailJS configuration

Once your account is created, you will need to configure an email service as well as a email template to organize the content that will come from your react-form.

##### Select an Email provider

- Click on `Email Services`, If necessary click on the button `Add new service`
- Select an email service provider that will manage your account. In my case I choose gmail.
- Click on `connect account` and allow emailJS to send emails on your behalf, follow the prompts until it is configure.

##### Build your email Template

- Inside the `Email Template` tab click on `Create new template`
- The parameter coming from your form must be surround in double curly braces
- Example:
-

```
   <input
        name="firstName"
        type="text"
      />

       <input
        name="lastName"
        type="text"
      />

```

So the template would contain `{{firstName}}` and `{{lastName}}`

##### My template:

Subject: `Message From your Portfolio`

Content:

```
Hello Bruninho,

You got a new message:

    User Info:

        Name: {{name}}

        Email: {{email}}


    Message:

        Subject:  {{subject}}

        {{{message}}}


Best wishes,
From your Portfolio
```

#### React Form Component

For my contact form component I have added two dependencies the EmailJS and FontAwesome

- Install EmailJS dependency by running `npm install emailjs-com --save`
- (Optional) I also added FontAwesome to display an icon for the required field
  `npm i --save @fortawesome/free-brands-svg-icons @fortawesome/free-solid-svg-icons @fortawesome/react-fontawesome` `npm i @fortawesome/fontawesome-svg-core`

Start by Importing the FontAwesome and emailjs

```
import emailjs from "emailjs-com";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faCircle } from "@fortawesome/free-solid-svg-icons";

```

```
import React, { Component } from "react";
import emailjs from "emailjs-com";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faCircle } from "@fortawesome/free-solid-svg-icons";

class Contact extends Component {
  state = {
    messageSent: false,
    name: "",
    email: "",
    subject: "",
    message: "",
  };

  handleChange = (e) => {
    this.setState({ [e.target.name]: [e.target.value] });
  };

  handleSubmit = (e) => {
    e.preventDefault();
    emailjs
      .sendForm(
        "gmail",
        "bruno_portfolio_message",
        "#contactForm",
        "user_oyLJWpa6rPOwCCyxCKMdJ"
      )
      .then()
      .catch();

    this.setState({
      name: "",
      email: "",
      subject: "",
      message: "",
      messageSent: true,
    });
  };

  render() {
    return (
      <section id="contactme">
        <h1 className="contact-title">Say Olá</h1>
        <div className="wrapper-contact">
          <div className="contact ">
            {this.state.messageSent ? (
              <div className="alert animated fadeInUp">
                Your Message has been sent
              </div>
            ) : (
              ""
            )}

            <form
              onSubmit={this.handleSubmit}
              className="animated delay-1s fadeInRight"
              id="contactForm"
            >
              <p>
                <input
                  name="name"
                  type="text"
                  placeholder="Full Name"
                  id="form-name"
                  required
                  value={this.state.name}
                  onChange={this.handleChange}
                />
                <span className="required">
                  <FontAwesomeIcon icon={faCircle} />
                </span>
              </p>
              <br />
              <p>
                <input
                  name="email"
                  type="email"
                  placeholder="E-mail Address"
                  id="form-email"
                  required
                  value={this.state.email}
                  onChange={this.handleChange}
                />
                <span className="required">
                  <FontAwesomeIcon icon={faCircle} />
                </span>
              </p>
              <br />
              <p>
                <input
                  name="subject"
                  type="text"
                  placeholder="Subject"
                  id="form-subject"
                  value={this.state.subject}
                  onChange={this.handleChange}
                />
              </p>
              <p>
                <textarea
                  name="message"
                  type="text"
                  placeholder="Message"
                  value={this.state.message}
                  onChange={this.handleChange}
                  rows="4"
                  id="form-message"
                  required
                ></textarea>
                <span className="required">
                  <FontAwesomeIcon icon={faCircle} />
                </span>
              </p>
              <br />
              <p id="btn-form">
                <input
                  onClick={this.successMessage}
                  type="submit"
                  name="submit"
                />
              </p>
              <br />
            </form>
          </div>
        </div>
      </section>
    );
  }
}

export default Contact;

```
