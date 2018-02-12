# Mail receive 

[![npm](https://img.shields.io/npm/v/npm.svg)]() [![node](https://img.shields.io/badge/node-v9.5.0-green.svg)]() [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
> Imap tool for receiving asynchronous mails

## Dependencies

| Lib | Version |
|--|--|
|debug  | ^3.1.0 |
| events | ^2.0.0 |
|imap  | ^0.8.19 |
| mailparser | ^2.2.0 |
| util | ^0.10.3 |


## Install
```shell
npm install mail-receive -S
```

## Usage

### Simple use

``` javascript
const notifier = require('mail-receive');

const imap = {
    user: "mail@domain.com",
    password: "pass",
    host: "imap.hostimap.com",
    port: 993,
    tls: true,
    tlsOptions: {
        rejectUnauthorized: false
    }
};

const n = notifier(imap);
n.on('end', () => n.start())
    .on('mail', mail => console.log(
        mail.headers.get('subject'),
        mail.from, 
        mail.textAsHtml  
    ))
    .start();
```

```shell
    Example subject 
    { 
        value:
            [ { address: 'examplemail@hotmail.com', name: 'Juan Andres Rodriguez Arenas' } ],
            html: '<span class="mp_address_group"><span class="mp_address_name">Juan Andres Rodriguez Arenas</span> &lt;<a href="mailto:examplemail@hotmail.com" class="mp_address_email">examplemail@hotmail.com</a>&gt;</span>',
            text: 'Juan Andres Rodriguez Arenas <examplemail@hotmail.com>' 
    } 
    <p>Body example</p>

```

## Mail object

Parsed mail* object has the following properties

- subject is the subject line (also available from the header mail.headers.get(‘subject’))
- from is an address object for the From: header
- to is an address object for the To: header
- cc is an address object for the Cc: header
- bcc is an address object for the Bcc: header (usually not present)
- date is a Date object for the Date: header
- messageId is the Message-ID value string
- inReplyTo is the In-Reply-To value string
- reply-to is an address object for the Cc: header
- references is an array of referenced Message-ID values
- html is the HTML body of the message. If the message included embedded images as cid: urls then these are all replaced with base64 formatted data: URIs
- text is the plaintext body of the message
- textAsHtml is the plaintext body of the message formatted as HTML
- attachments is an array of attachments
- address object
- Address objects have the following structure:

### Value an array with address details

- name: name part of the email/group
- address: email address
- group: array of grouped addresses
- text: formatted address string for plaintext context
- html: formatted address string for HTML context

### Headers

mail.headers.get('example') 

- from
- subject
- to
- cc
- bcc
- sender
- reply-to
- delivered-to
- return-path
- priority ( ‘high’, ‘normal’, ‘low’)
