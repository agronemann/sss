# Shamir's Secret Sharing (SSS) Generate and Restore via Web Browser

The goal of this project is to make Shamir's Secret Sharing easy to use — especially in case of emergency. Only a web browser is required to run the HTML files, and the HTML files can be run fully offline.

## Installation

You are meant to download [sss-generate.html](https://raw.githubusercontent.com/agronemann/sss/main/sss-generate.html) and [sss-restore.html](https://raw.githubusercontent.com/agronemann/sss/main/sss-restore.html) and run them locally in a web browser. No other dependencies are required.

Clone the repository:

    git clone git@github.com:agronemann/sss.git
    cd sss

Or download using cURL:

    mkdir sss
    cd sss
    curl -O https://raw.githubusercontent.com/agronemann/sss/main/sss-generate.html
    curl -O https://raw.githubusercontent.com/agronemann/sss/main/sss-restore.html

## Usage

With [sss-generate.html](https://raw.githubusercontent.com/agronemann/sss/main/sss-generate.html) you can generate a printable version of shares for a chosen passphrase (or any string you like). Open the file in a browser, enter the password you want to use Shamir's Secret Sharing on, the amount of shares to generate, and how many shares you want to require to restore the password (called the threshold). Each share will be on a seperate page. A QR code and a text representation of the QR code is displayed for each share.

With [sss-restore.html](https://raw.githubusercontent.com/agronemann/sss/main/sss-restore.html) you can combine shares to restore the passphrase. You will need atleast the amount of shares equal to the threshold you specified while generating the shares. Enter each share on a seperate line in the textarea and click "Combine shares" (tip: the textarea ignores blank lines and trailing whitespace).

## Dependencies

- https://github.com/grempe/secrets.js (commit `b6e13cb43f2065a9b622a35002a68222f2bfd437`). This commit was chosen due to being [security audited by Cure53](https://github.com/grempe/secrets.js?tab=readme-ov-file#security-audit).
- https://github.com/davidshimjs/qrcodejs (commit `04f46c6a0708418cb7b96fc563eacae0fbf77674`).

## Auditing and verifying (optional but recommended)

The code is meant to be as simple as possible, thus making it easier audit.

You can verify that the inline code for [secrets.min.js](https://raw.githubusercontent.com/grempe/secrets.js/b6e13cb43f2065a9b622a35002a68222f2bfd437/secrets.min.js) and [qrcode.min.js](https://raw.githubusercontent.com/davidshimjs/qrcodejs/04f46c6a0708418cb7b96fc563eacae0fbf77674/qrcode.min.js) in [sss-generate.html](https://raw.githubusercontent.com/agronemann/sss/main/sss-generate.html) and [sss-restore.html](https://raw.githubusercontent.com/agronemann/sss/main/sss-restore.html) matches the official source code. An example of doing so is checking the checksum in the `Content-Security-Policy` against the files from GitHub:

    curl -s https://raw.githubusercontent.com/grempe/secrets.js/b6e13cb43f2065a9b622a35002a68222f2bfd437/secrets.min.js | openssl sha256 -binary | openssl base64 | grep -qf - sss-{generate,restore}.html &&
    curl -s https://raw.githubusercontent.com/davidshimjs/qrcodejs/04f46c6a0708418cb7b96fc563eacae0fbf77674/qrcode.min.js | openssl sha256 -binary | openssl base64 | grep -qf - sss-generate.html &&
    echo "Success" || echo "Error"
