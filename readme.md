# Dist exam

## Dictionary

| Word | Explanation |
|:-:|:-:|
| Byzantine corruption | ... |


## 1. Confidentiality (mangler polering)

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

> **Ligning 2:** `P(Ki = 1) = P(Ki = 0) = 1/2` for alle _i_

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

`P(Ci = 0, Pi = 0) = P(Ki = 0) = 1/2` _(ligning 1 efterfulgt af 2)_
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

#### Key længde
Overstående virker kun hvis K er så lang som P, ellers vil nogle key-bits ikke være unikke; og de senere key bits vil bare være kopier af tidligere.

Faktisk gælder det, at antallet af `K ≥ C ≥ M`.
Hvis ikke `C ≥ M` ville flere plaintexts give samme ciphertext; hvorfor man ikke vil kunne decrypte korrekt.

Hvis ikke `K ≥ C`, ville der for en _m_ mangle en _k_ der kunne ENC(m)=C0; hvorved man ved modtagelse af _C0_ kan udelukke _m_.

Alt dette er mod princippet "Perfekt Secrecy", hvorfor det gælder `K ≥ C ≥ M`.

### Computational security, secret-key systems

Hvad vi lige fik beskrevet i detaljer; var et _secret-ket system_, hvor der for kryptering og dekryptering bruges den samme key; der (hvis ønskes sikkert) skal holdes hemmelig. En Secret-key systemer bruges de tre algoritmer: **G**enerate, **E**ncrypt og **D**ecrypt.

#### Sikkerhed i secret-key systemer (Måske kortere?)
I den virkelig verden, bliver vi dog nød til at have den samme key til mange forskellige beskeder; hvorved vi altså må give efter for _unconditionally Security_ og holde os til **computational security**; hvor vi bygger systemer der tager urealistisk lang tid at bryde.

Dette har nogle konsekvenser; blandt andet at hvis vi sender den samme besked to gange; og systemet kun tager besked og key som input; vil to ens ciphertexts blive sendt - hvilket en fremmed vil kunne se.

Derfor tager en god enkryptions algoritme også en tredje og unik variabel som input; kaldet en _nounce_, der skifter fra enkryptions-udførsels til den næste.

Når vi bruger en nounce, markere vi det eksplicit: 

```
c = Ek(m, n)
```

Specielt siger vi for sikkerheden at: hvis en fremmed kigger på en mængde krypteret data, og selvom han (delvist) har haft kontrol over dannelsen af den; så kunne den krypterede data lige så godt være intetsigende for ham. Så lidt må han vide om den.


Når man har et system der bruger samme key til flere krypteringer; er det også vigtigt at angive problemet om _exhaustive search_; hvor hvis en fremmed opdager et hvis antaler ciphertexter og de beskeder de repræsentere; kan han bruteforce sig frem til mulige rigtige keys. Derfor er det vigtigt at bruge en key, minimum af længden 128 bits; da der så vil være 2^128 forskelliger koder - et antal repetioner der anses som for højt til at udregnes af en computer. Dette kender vi som "cost of an attack"; som vi skal tage i lige højde med "probabiility of attack". 

Praktisk talk bruger man to forskellige secrete key systems: block siphers og stream ciphers.

#### Stream Ciphers
En _stream cipher_ er blot en algoritme G der udvider korte keys _k_ og en nonce _n_ til meget længere strenge G(k, n) der _ser_ tilfældige ud; der så er brugt til kryptering som var det en one-time pad: `c = m ⊕ G(k, n)`, hvor vi så dekrypter ved `c ⊕ G(k, n) = m ⊕ G(k, n) ⊕ G(k, n) = m`. Faktisk kan G blot inialieres, og så kan man blive ved man at _streame_ så mange bits man vil fra den; man kan da sige at dens argumenter (k, n) er dens seed.

Eksempler her der kan nævnes:

- RC4
- SALSA20
- SNOW

#### Block Ciphers

En **block cipher** krypter i deres basis form en blok af data af en fikseret længde og outputter en blok i samme længde. 

Eksempler her der kan nævnes:

* DES
* triple-DES
* AES

Når man bruger en Block Cipher, bruger man en så kaldt *Modes of Operation*, som betegner en måde at bruge Block Cipher på til at kryptere end streng af data af vilkårlig længde.

* OFB: Output Feedback
* CBC: Cipher Block Chaining
* CTR: Counter Mode

Disse _modes_ tager ikke blot key'en og beskeden som input, men alle også en nonce; der blot her kaldes en **initialization vector** _IV_.

**Output Feedback** kan bruges til at emulere en _stream cipher_; tag f.eks AES, så gælder blot outputtet: `AESk(IV), AESk(AESk(IV)),...,`, hvorfor netop navnet _output feedback_.

**Cipher Block Chaining** lægger ligesåvel i navnet op til dets funktion; blokke bliver chainet sammet. Givet en besked bestående af 128 bit blocke: M1, ..., Mt, hvor den sidste bliver _padded_ såvidt den ikke er 128 bit lang; så vil ciphertexten være t+1 blokke lang; hvor `C0 = IV` og for `i = 1,..., t:` vil `Ci = AESk(Mi ⊕ Ci-1)`.

**Counter Mode** er lige så, blot ved: `Ci = AESk(IV + i) ⊕ Mi`.

#### Pro/Con ved secrete key

* Pro: hurtigt 10-100 Mbytes/sec ved software, hurtigere ved dedikeret hardware
* Con: key skal være delt før data sendes. Specielt problem hvis flere brugere skal kommunikere og alle have hinanden nøgle.

### Computational Security, public-key systems

I et public-key system, bruges der for hver part, to nøgler: en public og en privat key. Det består ligeså af tre algoritmer: **G**enerate, **E**ncrypt og **D**ecrypt. G tager imod en key-længde og outputter en Pk og Sk; der tydeligvis har en sammenhæng, men som må være meget svær at regne sig frem til.

Alle der har A's Pk kan da kryptere en besked med den, og sende den til A, der så vil bruge Sk til at dekryptere beskeden. Altså: `m = Dsk(Epk(m))`. 

Sikkerheden her er at, selvom en fremmed har adgang til Pk; så vil man ved at have en _c_ ikke kunne dekryptere til _m_.

Det er ved Public-key systemer også vigtigt at kryptering indeholder noget tilfældigt; da en fremmed vil kunne modtage en _c_ og blot selv prøve at kryptere en _c'_ indtil `c = c'`; hvorfor han ved hvad _c_ er - hvilken han ikke må.

Ligeså er Public Key systemer også modtagelige over for exhaustive search ved søgning efter _sk_; og eftersom der findes algoritme der er "hurtige" til at regne _sk_ fra _pk_, bruges der normal meget større keys i publik-key systemer.

#### RSA
I RSA består pk af to numre: _n_ og _e_, og sk består af _n_ og _d_. Nummeret _n_ kaldes for modulet, og består af produktet af to prim nummere _p_ og _q_. Numrene _e_ og _d_ er valgt til at tilfredstille en relation.

Kryptering under RSA består simpelt af: `c = m^e % n`.
Dekryptering under RSA består da af: `m = c^d mod n`.

Forholdet her gælder da at: `c^d mod n = (m^e mod n)^d mod n = m`.

Den egenlige trussel mod dette, er at "factoring" _n_; som er hårdere des højere _n_ er. Derfor er keys i RSA i dag 2000-3000 bits lange.

##### OAEP
Hvis en fremmed kan "eksperimentere" med dekrypteringsalgoritmen og derved se hvordan den reagere; kan det muligvis afsløre detaljer om secret keyen. 

Derfor bruger man OAEP der "padder" OAEP(m, R) hvor R er random.

Dette kunne f.eks bruges til at bruge RSA+OAEP til at sende nøgler rund til et secret key system. Så først opsæt en public-key system; for derefter at udnytte det til at køre en hurtigere secret-key system.

## 2. Authentication

Når vi snakker om authentication; er det ideen om, at vi skal kunne bekræfte - at den person der forsøger at fuldføre en handling - ligeså skal have tilladelse til at udføre den handling.

### Unconditional security, the table-based solution

Hvis vi siger, der er en endelig mængde af beskeder; så kunne sender og modtager på forhånd aftale en tabel der matcher alle beskeder med en tilfældig udvalgt t-bit lang MAC uafhængeig fra beskeden. Tabellen fungere nu som en key; hvor man sender beskeden og den tilhørende MAC frem og tilbage. Derfor står det klart, at en fremmed ikke kan gætte sig frem til en MAC for en besked han ikke har set (eller jo - chancen er bare 2^-t). Vigtigt er det her at sige, at en MAC ikke kan genbruges.


### Computational security, secret-key

Overstående var et eksempel på et Secret key system i authentication; hvor vi ikke ønsker at andre skal kende til vores hemmelig key. Jeg nævnte at der bliver brugt en MAC; som er en af tre algoritmer der bruges under et secret key system i authentication.

MAC står for **Message Authentucation Code**, og beskriver fint sig selv. Derudover findes algoritmen G der genere keys, og algoritmen V der bruges til at verificere nøgler.

* G(l) outputter key af længden l
* MACk(m) outter MACen _c_ for _m_ under keyen _k_
* Vk(m, c) verificere om _c_ og _m_ hører sammen under keyen _k_

Som sikkerhed for brug af MACs, siges det at hvis en fremmed ser et antal MACs og de beskeder de repræsentere; vil det uanset antallet ikke være muligt for den fremmede selv at lave en MAC baseret på en besked der ikke er sendt før. For at undgå en fremmed bruger **exhaustive search** til at gennemskue en key; kræves det for moderne computere at de har en længde af minimum 128 bits.

#### Eksempel: CBC-MAC
Også kaldet Cipher Block Chaining Mac.

Er en MAC algoritme, der virker som CBC med IV = 0. Outputtet er da outputtet fra den sidste block.

**Cipher Block Chaining** lægger i navnet op til dets funktion; blokke bliver chainet sammet. Givet en besked bestående af 128 bit blocke: M1, ..., Mt, hvor den sidste bliver _padded_ såvidt den ikke er 128 bit lang; så vil ciphertexten være t+1 blokke lang; hvor `C0 = IV` og for `i = 1,..., t:` vil `Ci = AESk(Mi ⊕ Ci-1)`.


### Computational Security, public-key signatures

Når vi snakker public-key authentication systemer; bruges der ligesåvel tre algoritmer:

* **G** outputter et key-pair sk _(signing key)_ og pk _(verification key)_, med en speciel relation
* **S** bruges til at signe en besked. `c = Ssk(m)` (hvor man så sender m, c)
* **V** bruges til at verificere en nesked. `bool = Vpk(m, c)`.

Sikkerhedenfundamentet her, ligner meget hvad der var i secret-key systemet; en fremmed må se alle de beskeder og signature han vil; men uanset antal må ikke være i stand til selv at producere en valid signatur til en uset besked.

Man bruger en stor key; da der findes algoritmer til at finde Sk fra Pk.

#### Forskel mellem secret og public key authentication systemer

Når man bruger en MAC, kan B ved modtagelse af _m_ fra A; se om den virkelig var fra A (givet at kun A og B kender MAC); men B kan ikke bevise for nogle andre at det var A der sende beskeden, den kunne ligeså godt have været generet af B.

Her er formålet ved at bruge public key systemet; for alle kan have A's pk; men da kun A har SKa; er det kun A der kan signere en besked _m_. Så sender A en signeret besked til B, kan C der har SKa verificere at A lavede _m_ og ikke B, selvom det er B som nu er i besidelse af den.

#### Basis RSA signature er usikre

Basere man signature på RSA, vil S og V virke som følgende:

* Ssk(m) = s = `m^d % n`
* Vpk(m, s) = m' = `s^e % n`; tjek `m' == m`

Men et problem kommer på tale her: Hvis en fremmed blot vælger en _s_ og kører V derpå; vil han få en _m_. Nu kan den fremmede blot sige, at _s_ er en signatur for _m_. Det er ikke sikkert at _m_ betyder noget i det store hele; men det er en mulighed.

### Hashfunctions

Hashfunktioner kan løse førnævnte problem ved brug af RSA til secret key signatur.

En hashfunktion _h_, skal have følgende egenskaber:

* Tage imod en besked uanset længde
* Producere ouput af en fikseret længde
* Skal kunne regne lige så hurtigt som de beste secret key systemer.
* Skal være et svært beregningsmæssigt problem til at producere en _kollision_. Altså skal en _x_ og _y_ være svær at følgende der efterlever hvor `x != y` men `h(x) = h(y)`.

Sidsnævnte kan doig bruteforces, men hvis længden af _h_ er k-bits, vil det tage `2^k/2` forsøge.

Af hash-funktioner kan blandt andet nævnes:

* MD5, som aldrig bør bruges, da den er total brudt
* SHA-1, er tildels brudt
* SHA-256

### Combining RSA and hash and why it solves the problems.

Ved at bruge RSA med hashing, til at lave et signatur system - kan man ikke bare gøre det hurtigere (da hashen typisk er kortere) men også forhindre førnævnte angreb.

* S2sk(m) = s = `Ssk(h(m))`
* V2pk(m, s) = h `Vpk(h(m), s)`

Fordi at man skal sende `(m, s)` og den fremmede frit kan en _s_, men da _s_ afhænger af en hashet udgave af en _m_, skal den fremmede du fra _s_ lave en _m_ der ved `S2(m) == s`. Dette er nærmest umuligt; da det for en hashfunktion der er svær at finde kollisioner for; også er svær at invertere.

Derfor forbedre hashing sikkerheden for RSA.

### Replay attack
At bruge signature beviser bare, at vi på _et tidspunkt_ godkendte en besked; men en fremmed kna i teorien kapre beskeden og signaturen og sende den igen og igen. Dette hedder et replay attack og kunne f.eks bruges mod en bank.

Der er flere måder at forhindre dem på

* Man kan huske på hver besked; men det er dumt
* Man kunne bruge counters, men det kræver der bliver husket på dem; og sæt nu en besked bliver tabt? og man venter på _n_ men modtager _n+1_?
* Man kunne bruge timestamp; men det kræver igen at man husker på et stadie; og hvornår er en besked for gammel? hvad hvis forbindelse bare var langsom? Det kræver desuden også en synkronisering.

En ordenlig metode er, at en modtager vælger et _nounce_ og sender til senderen. Dertil sender senderen sin besked plus en MAC udregnet over det nonce og beskeden. Det forhindre replays, da nonces kun bliver brugt en gang.

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

### Passwords: issues/attacks when a password is:
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

