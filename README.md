# email-format

"Internet Message Format" meticulously implemented for email construction
and validation, as defined in RFC 5322 and other RFCs.

[Documentation](https://mikedilger.github.io/email-format)

You can compose an email like this:

```rust
extern crate email_format;

use email_format::Email;

fn main() {
    let mut email = Email::new(
        "myself@mydomain.com",  // "From:"
        "Wed, 05 Jan 2015 15:13:05 +1300" // "Date:"
    ).unwrap();
    email.set_sender("from_myself@mydomain.com").unwrap();
    email.set_reply_to("My Mailer <no-reply@mydomain.com>").unwrap();
    email.set_to("You <you@yourdomain.com>").unwrap();
    email.set_cc("Our Friend <friend@frienddomain.com>").unwrap();
    email.set_message_id("<id/20161128115731.29084.maelstrom@mydomain.com>").unwrap();
    email.set_subject("Hello Friend").unwrap();
    email.set_body("Good to hear from you.\r\n\
                    I wish you the best.\r\n\
                    \r\n\
                    Your Friend").unwrap();

    println!("{}", email);
}
```

## Features

* Extensive RFC 5322 Parser/validator
* If you generate an email using this crate, you are guaranteed that will be a valid
  RFC 5322 formatted email, or else you will get a ParseError. The only exception that I
  am currently aware of is that lines can be longer than 998 characters (see issue #3).

## Limitations

* This crate currently only handles 7-bit ASCII emails. The caller must do the content
  transfer encoding (and add the content-transfer-encoding header).  See issue #19.
* Obsolete email formats are not implemented in the parser.  Therefore, it is not sufficient
  for parsing inbound emails if you need to recognize formats that were obsoleted in 2008.

## Plans (not yet implemented)

* Support for content-transfer-encodings (unicode via Quoted Printable or Base64 or otherwise)
* Support for email headers defined in other RFCs:
  * Support for RFC 6854 (updated From and Sender syntax)
  * Support for all headers registered at IANA (http://www.iana.org/assignments/message-headers/message-headers.xhtml)
* Support for MIME (RFC 2045, RFC 4021, RFC 2231, RFC 6352) using
  [mime_multipart](https://github.com/mikedilger/mime-multipart)
* Support for streaming of MIME parts from disk.
* More ergonomic function signatures
* Less copying of data
* Implementation of `email` crate's `SendableEmail`, and so it works with the `lettre` crate (will be inefficient due to the way SendableEmail is defined).

## History

This project was inspired by the earlier [email](https://github.com/niax/rust-email) crate,
but was reworked from scratch due to a number of significant differences in design,
implementation and interface.

## License

Licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.
