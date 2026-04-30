# DreamConstraint
**Because Sega can't do SSL properly *either* (see [nds-constrain't](https://github.com/KaeruTeam/nds-constraint))**

## What this does
Many Dreamcast games that required a boot disc, cheat codes, or similar to be played online now work with just the retail disc and nothing extra. All that's needed is a custom DNS to route requests to `auth01.dricas.com` to the spoofed auth server.

## Try it
Want to try it out for yourself? All it takes is a custom DNS.
- Set your **Primary DNS** to `147.135.115.155`

The secondary DNS can be left as-is.

## The exploit
The Dreamcast's SSL implementation isn't very good at validating things. Certificate expiry isn't checked, and more importantly, **signing certificates aren't checked for being certificate authorities**. If a leaf certificate signs a new certificate for `auth01.dricas.com` (or any Dreamcast domain), it will be deemed valid even if the root and intermediate have expired.

## Pulling it off yourself
You can dump the trusted roots from `1ST_READ.BIN` easily using something like [cert-dump](https://github.com/19h/cert-dump), then find certificates signed by these roots using [crt.sh](https://crt.sh/). That's the easy part, because without the private key you can't sign anything. Thankfully, these roots are old, and the certificates signed by them are also old, using old wildly insecure RSA key lengths. A 512-bit RSA key can be cracked in under 24 hours with modern hardware using [CADO-NFS](https://cado-nfs.gitlabpages.inria.fr). With a signed certificate's private key, it's just a matter of signing a new certificate for the Dreamcast service you're trying to spoof.

## Special Thanks
Thanks to the following for lending computing power to help factor the RSA key
- [CursedSilicon](https://social.restless.systems/@CursedSilicon)
- [loganius](https://loganius.org)
- [siohaza](https://github.com/siohaza)
- TWE_
- [rustMotherboard](https://rustmotherboard.codeberg.page)
- [Para](https://para.my.to)
