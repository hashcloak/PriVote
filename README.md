# PriVote

PriVote is a privacy-preserving voting system implemented in the Aztec Network, in which you can do voting without
while protecting your identity. The privacy-preserving voting system is powered by a Semaphore implementation in the
Aztec Network. To be able to vote, we check that you have a valid passport by using ZKPassport so that the private data
contained in it is not revealed.

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```

## How can you use it?

PriVote has two user-friendly steps:

1. Register: The user scans a QR code with ZKPassport to validate that he/she has a valid passport. Then the voting
   interface is enabled in case that the user is validated correctly and he/she has not voted before.
2. Vote: The user chooses its candidate and submits his/her vote anonymously.

