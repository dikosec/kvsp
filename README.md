# KVSP; Kyoto Virtual Secure Platform

KVSP is a small interface to use a virtual secure platform simply,
which makes your life better.

Caveat: this project is under construction.

## Tutorial

```
## Clone this repository.
$ git clone https://github.com/virtualsecureplatform/kvsp.git

## Clone submodules recursively.
$ git submodule update --init --recursive

## Build KVSP. (It may take a while.)
$ make

## Change our working directory to `build/` to use KVSP.
$ cd build

## Write some C code...
$ vim fib.c

## ...like so.
$ cat fib.c
int fib(int n)
{
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}

int main()
{
    // The result will be set in the register a8.
    return fib(5);
}

## Compile the C code (`fib.c`) to an executable file (`fib`).
$ ./kvsp cc fib.c -o fib

## Let's check if the program is correct by emulator.
$ ./kvsp emu -q fib

## We can see `a8=5` here, so it seems to work correctly.

## Generate a secret key (`secret.key`).
$ ./kvsp genkey -o secret.key

## Encrypt `fib` with `secret.key` to get an encrypted executable file (`fib.enc`).
$ ./kvsp enc -k secret.key -i fib -o fib.enc

## Run `fib.enc` for 50 clocks to get an encrypted result (`result.enc`).
## Notice that we DON'T need the secret key (`secret.key`) in executing,
## which means the encrypted program (`fib.enc`) runs without decryption!
$ ./kvsp run -i fib.enc -o result.enc -c 50

## Decrypt `result.enc` with `secret.key` to print the result.
$ ./kvsp dec -k secret.key -i result.enc
```
