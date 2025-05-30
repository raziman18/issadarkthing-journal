---
title: "Maybank Bank Statement Converter"
date: 2025-05-26T15:15:24+08:00
tags: [react, project-showcase, parser, pdf]
---

Maybank usually sends their bank statements monthly through email. Whenever I
looked at it, I always thought I could parse the PDF file and store all the data
somewhere in DB where I can do my own analytics and search through all the
transactions. I've neglected to do this project before as I was busy with my
actual job as a software engineer. On some weekend I finally found the time to
work on this idea.

I started of by parsing the PDF to turn it into a text. This is done through
this library [pdf-parse](https://www.npmjs.com/package/pdf-parse). What I like
about this library is that it can work on browser as well, so parsing the PDF can
be done on the client side instead of having to send back to the server side
which could incur cost and most importantly degrades the privacy aspect of the
tool where your personal bank statement is sent to some random server on the
internet.

Once we are done turning the bank statement into a text form, we can actually
use regex magic to capture the information line by line. Before we start, we
need to take a look at the format of the bank statement.

{{< figure 
src="https://res.cloudinary.com/hiremap/image/upload/v1748608573/gxzclrqo5qnfqhiun2p1.jpg"
width="80%"
caption="Maybank Bank Statement"
>}}

As you can see in the image above, each row has date, transaction details,
transaction amount and transaction balance.

## Regex magic

The following regex is used to parse and capture all the details in a single
row.
```regex
/^(\d{2}\/\d{2}(?:\/\d{2})?)(.+?)(\d{1,3}(?:,\d{3})*\.\d{2})([+-])((?:\d{1,3}(?:,\d{3})*|\d*)?\.\d{2})$/gm;
```


1. `^(\d{2}\/\d{2}(?:\/\d{2})?)` captures at least the transaction date. It does
   match patterns such as "26/05". The pattern "26/05/23" is matched also.
   Maybank statement date formats DD/MM or DD/MM/YY are handled by our regex.

2. `(.+?)` captures all of those transaction details. It does so in quite a
   non-greedy way. Descriptions such as "TRANSFER TO ACCOUNT" or "MAYBANK2U
   PAYMENT" will be matched in any text between the date along with the amount.

3. This captures the amount for the transaction `(\d{1,3}(?:,\d{3})*\.\d{2})`.
   It recognizes numbers using commas to separate thousands (like "1,234.56").
   The function also ensures that it has exactly two decimal places.

4. `([+-])` - It captures if the transaction credits (+) or debits (-).

5. `((?:\d{1,3}(?:,\d{3})*|\d*)?\.\d{2})` - This captures all of the balance
   within the account, after when the transaction occurs. It handles again
   numbers with separators or without commas.

The `gm` flags at the end are important too including `g` means global so
matching all occurrences, with `m` meaning multiline in treating each line just
as a separate input.

Using this regex, we can iterate through matches in our PDF text extracting
structured data from each transaction line. Each match will give to us five
capture groups that can correspond with date, details, amount, sign, and then
balance. Each match will provide these specific groups.

Let's look at just such a transaction line simple example of just how this all
might work out.

```
5,678.90 from 26/05/23 MAYBANK2U PAYMENT TO XYZ 1,234.56
```

Our regex would capture:
- Group 1 happened "May 26, 2023". It was a date.
- Group 2: "MAYBANK2U PAYMENT TO XYZ" (transaction details)
- Group 3: (amount) Is "1,234.56"
- Group 4: debit is indicated using "-".
- Group 5: "Balance of 5,678.90"

## Creating the site
Once the parser have been figured out, let's now create a site where user can
visit and simply upload their bank stament to be converted to CSV or Excel. For
this site actually it's a bit overkill to use react but since that is the web
framework I am familier the most I might as well use it. The UI of the site
should be modern, intuitive, and easy to use. Even a non-techy person should
know how to operate this site. You simply drop your bank statement in the box
and click "Convert" and will convert the file into your desired format
CSV/Excel.


{{< figure
src="https://res.cloudinary.com/hiremap/image/upload/v1748610449/sa3rlbo2kdhhhn0jkjad.jpg" 
width="80%"
caption="Maybank Bank Statement Converter"
>}}

## Conclusion
Well that's it, the site have been deployed and can be accessible by anyone. It
is good to do programming that's actually useful in your daily life, you'll feel
more appreciative of your work since you did yourself. Hopefully somewhere in
the future I will support for more banks and credit card statements.

[https://maybankconverter.issadarkthing.com](https://maybankconverter.issadarkthing.com)
