---
title: "Password Checker"
date: 2021-08-18T12:40:59+08:00
draft: false
tags: ["bash", "password"]
---

Have you ever wondered if your password has been breached or exposed to the
internet before? If you use the same password for multiple websites, that's
unfortunate because it may have been exposed to the public. To testify this, we
can use [haveibeenpwned](https://haveibeenpwned.com/) api to (not really) give
our password and check against public data.

First off, we have to hash our password using
[sha-1](https://en.wikipedia.org/wiki/SHA-1). In linux, you can use command
[sha1sum](https://linux.die.net/man/1/sha1sum) to transform text into sha-1
hash. Hash is a one way operation meaning that you can only transform the input
to the output deterministically and not vice versa.

```sh
PASSWORD="$(echo -n "$RAW" | sha1sum | awk '{print $1}' | tr '[:lower:]' '[:upper:]')"
PREFIX="${PASSWORD:0:5}"
```

After that we need to extract first 5 characters from the hashed password and
append it to this url `https://api.pwnedpasswords.com/range/$PREFIX` where
`$PREFIX` is the variable which holds the first 5 character of our hashed
password.

To make a request against the api, you can use
[curl](https://linux.die.net/man/1/curl). Option `-Ss` is used to silent the
verbose output and only show error message if it fails.

```sh
RANGE_PASSW=$(curl -Ss "https://api.pwnedpasswords.com/range/$PREFIX")
```

The output of the curl command will look like this:
```
F971582870552CF5BA8DD23C8EC01F99A0A:2
F984473B8ED4FBC1C83E03146668A2CEBEC:2
F9F50FE2D40187D3681AA1BDD6733C8914E:2
FA4D46A28BB6C52C0260CE565C771D4A8E0:2
FAFFD17C2F38B0A9DBF5E112DC3A005C06C:5
FB064758EB0BC12238ACC06280DA6D46259:1
... more
```

The output is a list of hash strings that matches the first 5 character of our
hashed password. This is due to the security measurement taken by **haveibeenpwned**
so that they don't know the exact password hash you tried to check against their
API.

Now, the output has the following format `{password hash without
prefix:instances of password have been pwned}`. We can use bash string substring
built-in to extract the information we need from the curl output.

```sh
PW=${RES%:*} # capture everything before ':'
COUNT=${RES#*:} # capture everything after ':'
```

Now all is left is to find which password belongs to ours. We can simply loop
through the list and check against our hashed password without the first 5
character suffix.

```sh
while read -r RES; do

  PW=${RES%:*}
  COUNT=${RES#*:}

  if [[ $SUFFIX = $PW ]]; then
    printf "password has been found %d times(s)\n" "$COUNT" 2>/dev/null
    exit
  fi

done <<< "$RANGE_PASSW"

echo "password has been found 0 time(s)"
```

The complete script is given below:
```sh
#!/usr/bin/env bash

read RAW

PASSWORD="$(echo -n "$RAW" | sha1sum | awk '{print $1}' | tr '[:lower:]' '[:upper:]')"
PREFIX="${PASSWORD:0:5}"
SUFFIX="${PASSWORD:5}"

RANGE_PASSW=$(curl -Ss "https://api.pwnedpasswords.com/range/$PREFIX")

while read -r RES; do

	PW=${RES%:*}
	COUNT=${RES#*:}

	if [[ $SUFFIX = $PW ]]; then
		printf "password has been found %d times(s)\n" "$COUNT" 2>/dev/null
		exit
	fi

done <<< "$RANGE_PASSW"

echo "password has been found 0 time(s)"
```

To use the script, you can either pipe your password into the script or type the
password after running it.

![screenshot-of-the-command](/static/pwned.png)
