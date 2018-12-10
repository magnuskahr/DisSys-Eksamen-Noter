# Dist exam

## Dictionary

| Word | Explanation |
|:-:|:-:|
| Byzantine corruption | ... |


## 1. Confidentiality

Så når vi snakker om fortrolighed, har vi nogle forskellige måder at arbejde på: enten med _Secrete Key_, eller _Public Key_ systemer. Uanset hvilken en vi vælger, vil vi aldrig have det således at hvis en fremmed ser en ciphertext _c_ og ikke kender til den anvendte krypterings key _k_, vil den fremme ikke have nogen ide om hvad _c_ repræsentere.


### Unconditional Security: One Time Pad
Unconditionally Secure betyder, at lige meget hvor meget computer kræft man har; kan man ikke lære noget om plaintext af at se ciphertext.

**Kilde for efterfølgende:** [https://www.cryptomuseum.com/manuf/mils/files/mils_otp_proof.pdf](https://www.cryptomuseum.com/manuf/mils/files/mils_otp_proof.pdf)

Ved en _One Time Pad_; hvor det fra navnet er givet, at en key er kun brugt en gang, haves en unbrydelig encryptions algoritme.

Det virker ved en; at en bit streng, bestående af _M1, ..., Mt,_ og en tilfædig bit streng af samme længde _K1, ..., Kt_; hvorfra en ciphertext _C1, ..., Ct_ dannes ved at **XOR** M og K. 

Da det at XOR er en uniform process, kan man fra C og K ligeledes få M.
Det siges at:

Ci &oplus; Ki = (Mi &oplus; Ki) &oplus; Ki = Mi &oplus; (Ki &oplus; Ki) = Mi

| A | B | A ⊕ B |
|:-:|---|:-----:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |


#### Sikkerhed
Fordi at keyen der skal bruges, er tilfædig og af samme længde som selve beskeden, er det eneste mulige angreb mod en sådan enkryption at forsøge sig med at bruteforce.

Når vi snakker om bruteforce angreb; snakker vi om at forsøge med **alle** keys der vil have været kunne brugt til en besked; altså _2^t_, hvor _t_ er længden af beskeden. Det betyder, at enhver plausibel kombination af en key skal prøves mod cipherteksten; hvor flere keys kan give noget meningsfuldt; men kun den rigtige key vil give M.

Men den fremmede angriber vil ikke vide hvilken key der er rigtig, siden flere vil give mening; og det vil derfor ikke kunne betale sig for angriberen at forsøge sig med et bruteforce; siden alle mulige beskeder er lige mulige betydninger af C.

#### Bevis for sikkerhed

| Notation | Forklaring | 
|---|---|
| Pi | _i_-te bit i plaintext |
| Ci | _i_-te bit i ciphertext |
| Ki | _i_-te bit i key |
| P(Pi) | Sandsynligheden for at Pi var sendt |
| P(Pi &#124; Ci) | Sandsynligheden for Pi gevet Ci |
| P(Ki) | Sandsynligheden for at Ki var brugt til at lave Ci |

En system kan kaldes _perfekt sikkert_ når **P(Pi) = P(Pi | Ci)**; hvilket betyder at cipherteksten er uafhængeig af plainteksten. Altså at sandsyndligheden for at Pi var sendt har intet at sige om Ci var set eller ej.


> **Ligning 1:** `Ci = Pi ⊕ Ki`

Så lad os bevise at One Time Pad opfylder dette!

Fordi _OTP_ keys er total tilfældige og uforudsigelige, kan vi lave to konklusioner:

1. Sandsynligheden for at se en key-bit er lige så høj som at se alle andre
2. At kende en række key-bits fortæller intet om den næste

Grundet første konklusion, kan vi sige:

```
Ligning 2: P(Ki = 1) = P(Ki = 0) = 1/2 for alle _i_
```

Ligeledes kan vi pga XOR, ved at kende 2 af {Pi, Ci, Ki}, kende den sidste.

For at vise `P(Pi) = P(Pi | Ci)`, skal vi først vise at `P(Ci) = P(Ci | Pi)`

##### Distributionen af P(Ci)

Lad os kigge på chanchen for at Ci er 1: 
`P(Ci=1) = P(Ci=1 | Ki=1) P(Ki=1) + P(Ci=1 | Ki=0) P(Ki=0)`

Chancen for `Ci=1` er lig, chancen for `Ci=1` når `Ki=1` ganget chance for `Ki=1` plus chancen for `Ci=1` når `Ki=0` ganget changen for Ki=0.

Pga ligning 1, kan de to muligheder for `Ci=1` være: (Pi=1, Ki=0) og (Pi=0, Ki=1), hvorfor: `P(Ci=1) = P(Pi=0) P(Ki=1) + P(Pi=1) P(Ki=0)`

Gennem _ligning 2_ kender vi P(Ki)

`P(Ci=1) = P(Pi=0) 1/2 + P(Pi=1) 1/2`

Kan omformuleres til:
`P(Ci=1) = 1/2 [P(Pi=0) + P(pi=1)]`

og fordi Pi kun kan være 1 og 0 har vi at

`P(Ci=1) = 1/2`

Det omvendte bevis kan laves for P(Ci=0)

##### Distributionen af P(Ci | Pi)
Lad os nu kigge på `P(Ci | Pi)`

Hvis Pi = 0, kan der ske to ting:

`P(Ci = 0, Pi = 0) = P(Ki = 0) = 1/2`
`P(Ci = 1, Pi = 0) = P(Ki = 1) = 1/2`

Modsat kan siges om Pi = 1.
`P(Ci = 0, Pi = 1) = P(Ki = 1) = 1/2`
`P(Ci = 1, Pi = 1) = P(Ki = 0) = 1/2`

Vi kan altså sige at distributionen af Ci har intet at gøre med Pi, da **P(Ci) = P(Ci | Pi)**

Chancen for at en Pi og Ci ses, er da:

`P(Pi and Ci) = P(Ci | Pi) P(Pi) = P(Pi | Ci) P(Ci)`

Hvorfor

`P(Ci | Pi) P(Pi) = P(Pi | Ci) P(Ci)`

Vi har vist at `P(Ci | Pi) = P(Ci)`, hvorfor de kan fjernes som konstanter, hvorfor vi tilbage har: **P(Pi) = P(Pi | Ci)**

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

## 9B. Asynchronous Agreement

### What is the asynchronous model about?
- No clocks, no timeouts, only eventual delivery of messages

### Unscheduled broadcast
- What is it?
- How: bracha broadcast

### Asyncronous Byzantine Agreement
- What is it?

### Weak Agreement (8.3.1)
- What is it?
- How: Fiogure 8.5 (t < n/5 Byzantine corruptions)

### From Weak Agreement to Non-Terminating Byzantine Agreement (8.3.2)
- Try to settle undecided by a random coin-flip

## 10. State Machine Replication

### What is a state machine

### What is a replicated state machine
- A state machine replicated on several servers
 - Why does it have to be a state machine
- The problem of keeping consistency
- The solution: Totally-ordered broadcast

### Tottaly Ordered Broadcast
- What is it?
- Synchronous implementation
 - Round robin sequencer
- Asynchronous implementation
 - Why not round robin?
 - Fig 10.3: Core set selection
  - - Large common subset of inputs from parties
  - - Asynchronous BA to nail down the core set

## 11. Blockchain, Growing a tree

### Totally Ordered Broadcast (TOB)
- What and why
- For state machine replication / crypto currencies

### Blockchains
- Non round based synchronous implememntation of TOB
- For the peer-to-peer setting
 - Sporadic participation, unknown network

### The lottery system for proof of stake
- H(SIG_sk(slit)) * AMOUNT(vk) > Hardness
 - Why: unpredictable (DoS), uncontrollable (fairness), sybil attacks
- The winner floods a new signes block
 - Block: list of transaction + pointer to previous block
- Problems: branching by chance and ghost blocks (withholding attacks)

### Argue some properties in standard tree scenario
- Tree growth
- Chain quality
- Limited roll-back

### The problem with lack of finality: The solution is not part of the questien.    

