### When Panic Becomes Dangerous in Rust

Author: Dr. Mo Ashouri
Email: ashourics@protonmail.com
--> bytescan.net <---
 Dec 16, 2022
 





### What is a Panic!
Sometimes, unexpected things occur in your code; for these scenarios, Rust has the Panic! macro.
This Macro in Rust signals that the code is in a state it cannot handle and must tell the process to stop instead of trying to proceed with invalid or incorrect values.
There are two ways to cause panic in practice: by taking an action that causes our code to panic or by explicitly calling the Panic! macro.
Here is a simple example of a Panic in Rust:
Filename: src/main.rs
```
fn main() {
  panic!("crash and burn");
}
```
When you run the program, you'll see something like this:
 ```
$ cargo run
```

Compiling panic v0.1.0 (file:///project/panic)
Finished dev [unoptimized + debuginfo] target(s) in 0.25s
```
Running `target/debug/panic`

thread 'main' panicked at 'crash and burn', src/main.rs:2:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

### When panic becomes a security issue?
Using panic as a placeholder for error handling during prototyping code can save time. For instance, a panic will return a stack trace, including detailed information regarding the error involved, such as a stack trace and locations of associated code on the file system.
Although this helps track down errors, it also poses severe security risks. For this reason, it should not be used in software in a "production" environment.
Note that stack traces can assist an attacker in crafting an exploit. They also supply information about the underlying file system that can be used as part of sophisticated attacks that chain multiple small pieces of information together into a dangerous hack. If a panic can be predictably triggered in a piece of code, this can be enough to perform a denial-of-service (DoS) attack.

### Where Panic is even more dangerous?
This is true for all software. However, the problem with using panics in production can be much more hazardous than an information leak or temporary reduction in service availability due to DoS.
In blockchain ecosystems, "panic" can have devastating impacts. This is because a crashing program may affect a service's availability and the state's integrity. In other words, a panic may cost you alot!
This is a potential security problem for products trusting purely on Rust compiler default security features (and other languages like Golang that propose panic!) in their compiler. This includes blockchain projects that use either of these languages to develop components such as network gateways, validators, smart contracts, etc.

### Real-world Example: Attacking the Cosmos SDK
In Golang, similar to Rust, we have panic. One of the blockchain examples of the panic attack is Cosmos SDK.
Cosmos SDK is a popular framework for building application-specific blockchains and developers can use Rust or Golang to develop their code.
Creating code that runs automatically on a per-block basis in the Cosmos ecosystem is possible. This is done via the BeginBlocker and EndBlocker methods within a given module. However, Cosmos SDK used to carry an exploit that an attacker could crash validators based on exploitation on panic. The attack occurs if there are a reduced number of validators active, this makes it easier to, for example, create a malicious governance proposal and have it pass. Reducing the number of validators on the network means it's relatively easy to win a voting process that negatively affects the chain.

### When is it safe to panic?
It is always sounder to exit with an error code than to Panic. In the best circumstances, no software you write will ever panic. Note that a panic is indeed a crash and must be evaded to construct reliable software.
A crash is not ever “right” behavior, but if at any time it may be believed that the software could cause something deadly, expensive, or destructive to happen, it’s probably best to shut it down.


### Allows all objects to safely destroy themselves
If you consider your software a car driving at about 150 kilometers per hour, a panic is like hitting a brick wall.
A panic unfolds the call stack, hopping out of every function call and returning from program execution, destroying objects as it goes. It is not considered a safe or clean shutdown. Avoid panics!!!

In my view, the safest way to end a program’s execution is to permit it to run until the last closing brace. As a sophisticated engineer, you must find a way in your code to allow all objects to destroy themselves safely under unexpected conditions, which is not always easy!

Security error handling can become an extensive topic, I will cover it in another article in the future!



###
I am the co-founder of bytescan.net, a online service to control your code in terms of security issues, make sure you check it out for practical security reports!
Moreover, you like this article, you can follow me on github and the following links as well:

-https://ashourics.medium.com/
-https://www.linkedin.com/in/drashouri/
-https://www.youtube.com/c/Heapzip
-https://ashoury.net/
 
