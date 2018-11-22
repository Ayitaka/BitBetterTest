# BitBetter
[![CircleCI](https://img.shields.io/circleci/project/github/jakeswenson/BitBetter.svg)](https://circleci.com/gh/jakeswenson/BitBetter/tree/master)

This project is a tool to modify bitwardens core dll to allow me to self license.
Beware this does janky IL magic to rewrite the bitwarden core dll and install my self signed certificate.

## Building

To build your own `bitwarden/api` image run
```bash
./build.sh
```

replace anywhere `bitwarden/api` is used with `bitbetter/api` and give it a go. no promises

## Issuing your own licenses

The repo is setup to replace the licesning signing cert in bitwarden.core with my own personal self signed cert (`cert.cert`)
If you want to be able to sign your own licenses obviously you'll have to replace it with your own self signed cert.

you can generate one with openssl like so:
```bash
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.cert -days 36500 -outform DER
```

### Convert your DER certificate to a PEM

```bash
openssl x509 -inform DER -in cert.cert -out cert.pem
```

### Convert your public and private key into a PKCS12/PFX

```bash
openssl pkcs12 -export -out cert.pfx -inkey key.pem -in cert.pem
```

### Signing licesnses

There is a tool included to generate a license (see `src/liceseGen/`)

generate a PFX above using a password of `test` and then build the tool using:

```bash
./src/licenseGen/build.sh
```

This tool build ontop of the bitbetter/api container image so make sure you've built that above using the root `./build.sh` script.

After that you can run the tool using:

```bash
cd ~/BitBetter/src/licenseGen
./run.sh ~/BitBetter/.keys/cert.pfx user "Name" "EMail" "User-GUID"
./run.sh ~/BitBetter/.keys/cert.pfx org "Name" "EMail" "Install-ID used to install the server"
```

# Questions (you might have?)

I'll work on updates in the next couple weeks, right now, I just wanted something to start with.

## But why? Its open source?

Yes, bitwarden is great. If I didn't care about it i wouldn't be doing this.
I was bothered that if i want to host bitwarden myself, at my house, 
for my family to use (with the ability to share access) I would still have to pay a monthly ENTERPRISE organization fee.
To host it myself. And maintain it myself. Basically WTH was bitwarden doing that I was paying them for?

## You should have reached out to bitwarden

Thanks, good idea. And I did. Currently they're not focused on solving this issue - yet. 
To be clear i'm totally happy to give them my money. Offer a perpetual server license, and i'd pay for it.  Let me license the server, period.  Allow an orginzation to have Premium for all users..  500 seats, let the 500 users in the orginzation have the Premium features too.

I'm still in the testing/evaluating phase.  If I am hosting the server/data, let me license the server, period.  How many licenses does one user need to have...
