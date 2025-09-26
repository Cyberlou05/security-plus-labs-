# GPG Encrypt / Decrypt Demo (Security+ Labs)

**Purpose:** Show basic GPG keygen, encrypt/decrypt and signing workflows (local demo).

## Generate a key (interactive)
gpg --full-generate-key
# Choose RSA, 4096, add name/email and passphrase

## Export public key (safe to publish)
gpg --armor --export "Your Name" > public.key

## Encrypt a file for a recipient (local demo)
echo "demo secret" > demo.txt
gpg --output demo.txt.gpg --encrypt --recipient "Your Name" demo.txt

## Decrypt
gpg --output demo_restored.txt --decrypt demo.txt.gpg

## Sign & Verify
gpg --output demo.sig --detach-sign demo.txt
gpg --verify demo.sig demo.txt

**Notes / Security+ tie-in**
- Only publish **public** keys. Never push your private key.
- GPG enforces confidentiality (encrypt), integrity (sign), and non-repudiation.
