footer: Kenji Rikitake / Erlang Factory SF Bay 2015
slidenumbers: true

# Xorshift* and Erlang/OTP: Searching for Better PRNGs

<!-- Use Deckset 1.4, Next theme, 4:3 aspect ratio -->

---

## Kenji Rikitake

27-MAR-2015
Erlang Factory SF Bay 2015
San Francisco, CA, USA
@jj1bdx

Professional Internet Engineer

ACM Erlang Workshop 2011 Workshop Chair

Ex Digital Equipment Corporation and Basho Technologies engineer

![right, fit](kenji-standing-20150209-small.jpg)

---

# Executive summary: do not try inventing your own random number generators!

---

# PRNGs matter

* This is my second talk on pseudo random number generators in Erlang Factory events
* First in 2011
* People are *still using* the good-old `random` module, designed in 1980s, and already *fully exploited*
* So I proposed to do this again with new algorithms, and accepted

---

# PRNGs are everywhere

* Rolling dice (for games)
* (Property) testing
* Variation analysis of electronic circuits
* Network congestion and delay tolerance analysis
* Risk analysis of project schedules
* Passwords (*Secure PRNGs* only!)

---

# Variation analysis of a band pass filter

![inline](elsie-circuit.png)

---

# Without variance

![inline](elsie-plot-novariance.png)

---

# With 10% variance

![inline](elsie-plot-10percent.png)

---

# How PRNG works

* Give a seed S1
* `{Result1, S2} = prng(S1)`
* `{Result2, S3} = prng(S2)`
* ... and on and on
* Sequential iterative process
* Need to choose seeds carefully to prevent sequence overlapping on multiple processes

---

# NOT in this talk: Secure PRNGs

* For password and cryptographic key generation with strong security
* Use `crypto:strong_rand_bytes/1`
* Entropy gathering takes time
* This is *cryptography* - use and *only use* proven algorithms! *Do not invent yours!*

---

# In this talk: non-secure PRNGs

* May be *Vulnerable to cryptographic attacks*
* (Uniform) distribution guaranteed
* *Predictive* sequences
* Same seed, same result
* Lots of seed choices
* Long period: no intelligible patterns

---

# Non-secure PRNG failures

* Found from the observable patterns by making a graphical representation
* *Very short period* of showing up the same number sequence again
* Even a fairly long sequence of numbers can be *fully exploited* and made predictable

---

# PHP5 on Windows (2012)

![inline](php5-bad-prng.png)

---

# Other PRNG failures

* [Cryptocat 2013](https://nakedsecurity.sophos.com/2013/07/09/anatomy-of-a-pseudorandom-number-generator-visualising-cryptocats-buggy-prng/) (blue: OK, red: bad)

![inline](colourmap-urandom-4801.png) ![inline](colourmap-mod251-4801.png)

---

# Erlang/OTP's first ever security advisory

* ... was about PRNG! (R14B02, 2011)
* [US CERT VU#178990](http://www.kb.cert.org/vuls/id/178990): Erlang/OTP SSH library uses a weak random number generator ([CVE-2011-0766](http://www.cvedetails.com/cve/CVE-2011-0766/))
* Used `random` non-secure PRNG for the SSH session RNG seed, easily exploitable

---

# Erlang `random`'s problem

* Period: 6953607871644 ~= 2^(42.661), too short for modern computer exploits
* Fully exploited in < 9 hours on Core i5 (single core) ([my C source](https://github.com/jj1bdx/as183-c)) - Richard O'Keefe told me this was *nothing new in either academic and engineering perspectives* (he is right!)
* The algorithm AS183 is too old (designed in 1980s for 16-bit computers)

---

# Other Erlang PRNG implementations

* These are much better than `random`
* [SFMT](https://github.com/jj1bdx/sfmt-erlang) (Period: (2^(19937))-1)
* [TinyMT](https://github.com/jj1bdx/tinymt-erlang) (Period: (2^127)-1, ~2^56 sequences)
* [XorShift64*](https://github.com/jj1bdx/exs64) (Period: (2^64)-1)
* [XorShift128+](https://github.com/jj1bdx/exsplus) (Period: (2^128)-1)
* [XorShift1024*](https://github.com/jj1bdx/exs1024) (Period: (2^1024)-1)

---
