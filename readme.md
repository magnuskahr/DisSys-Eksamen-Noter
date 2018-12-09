# Dist exam

## Dictionary

| Word | Explanation |
|:-:|:-:|
| Byzantine corruption | ... |


## 1. Confidentiality

### Unconditional Security: One Time Pad
 - How does it work
 - Why is it secure
 - Key must be as long as message

### Computational security, secret-key systems
- Definition of security
- Stream ciphers/Block ciphers
 - Examples
 - Key size (exhaustive search)
- CBC/CTR/OFB modes

### Computational Security, public-key systems
- Definition of security
- RSA, how it works
- Use of RSA for sending short secret keys like AES keys
- OAEP

## 2. Authentication
### Unconditional security, the table-based solution

### Computational security, secret-key
- Definition of security for message authentication codes
- CBC-MAC


### Computational Security, public-key signatures
- Definition of security for Signature schemes.
- Difference between MACs and signatures: can you convince a third party?
- Basic RSA signatures and why they are not secure.

### Hashfunctions
- What properties must they have?
- Examples

### Combining RSA and hash and why it solves the problems.
- Replay and how to prevent it.


## 3. Key Management and infrastructure

### Session keys
- Why we do not use the same key all the time, but use session keys instead

### Key Distribution Centers
- Why we have them and how they work (who has which keys, how do we get a session key)
- Trust and reliability

### Certification authorities
- Who has which keys and what happens when a certificate is issued
- How is a certificate used

### Certificate chains
- How does one get the CA public key in the first place

### How to identify users
- Something you know (passwords)
- Something you have (hardware tokens)
- Something you are (biometrics)

###Passwords: issues/attacks when a password is:
- Chosen, stored by a user, transported to server, stored by server

### Hardware:
- Tamper resistant/tamper evident hardware
- Two-factor authentication


### Biometrics: what is it, what can it be used for

## 4. Network Security Mechanisms
### Definition of AKE

### Needham / Schroeder and the attack

### SSL / TLS and the SSL handshake
- Why it is secure
- One Way SSL

### Diffie-Hellman (authenticated) key exchange and IPSec

### Difference between SSL and IPSec

### Password authenticated key exchange
- Why have it
- Example

### Applications, document based secure formats versus secure tunnels

## 5. System Security Mechanisms

### Trusted computing base
- What is it
- Examples

### Firewalls
- What are they
- Packet filter, proxy, stateful firewalls
 - what are they
 - Pros an cons

### Malware and defenses
- Virus scanners
- Intrusion detection

### Security Policies
- Definition
- Lattices what they are and how they can be used to define a policy
 - Exmaples. Bell Lapadula / Biba
 
### Other models, Chinese wall, Dual control etc.

### Access Control
- Access control matrix
 - Use in practive
- Dynamic Access control, sometimes implies undeciable problems

## 6. Threats and Pitfalls

### Taxonomy of attacks and goals
- STRIDE etc

### Illegal input attacks
- Overwlof
- Cross-site scripting
- Heart bleed
- Unicode exploit

### SSL / CBC side channel attack

### IBM 4758

### The backdoored PRG

### SPECTRE

### Can give several examples but make sure to give full details on at least on

### Try to relate to taxonomy 

## 7. Consistency

### Unstructured Peer-to-Peer as motivation
- Messages might arrice in different order

### Consistency models and how to implement them
- Model: the safety property phrased abstractly
- Implementation: How to implement

### FIFO
- What is it? And how does it work?

### Causal
- The Causal-Past relation
- How to implement efficiently with vector clocks

### Total order
- What is it? And how does it work?

## 8A. Asynchronous Agreement

### Round based protocols
- Synchronized clocks 
 - Bounded delay allow all honest to hear from all honest in each round
- Details of how it work

### The scheduled broadcast problems
- What is it?
 - One broadcaster, all other ready to receive, start in same round

### A solution using signatures: Dolev Strong
- $t < n$ Byzantine corruptions

### Lower bound on round complexity (7.9)
- Just explain the result

## 8B. Synchronous Agreement

### Round based protocols
- Synchronized clocks 
 - Bounded delay allow all honest to hear from all honest in each round
- Skip details

### The scheduled broadcast problems
- What is it?

### A solution using signatures: Dolev Strong
- $t < n$ Byzantine corruptions

### A solution without signatures
- Emulate transferability using quorum (7.10)

### Lower bound on round complexity (7.9)
- Any protocol solving scheduled broadcast and tolerating t Byzantine corruptions needs to use t+2 rounds

## 9A. Asynchronous Agreement

### What is the asynchronous model about?
- No clocks, no timeouts, only eventual delivery of messages

> ### Unscheduled broadcast
> - What is it?
> - How: bracha broadcast
 
### Asyncronous Byzantine Agreement
- What is it?

### Graded Agreement (8.3.3)
- What is it?
- How: figure 8.8 ($t < n/9 Byzantine corruptions)

### From graded Agreement to Byzantine Agreement (8.3.4)
- Settle undecided by a coin-flip

### Impossibility of Deterministic Agreement


