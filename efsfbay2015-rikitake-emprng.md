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

# PRNGs matter

* This is my second talk on pseudo random number generators in Erlang Factory events
* First in 2011
* People are *still using* the good-old `random` module, designed in 1980s, and already *fully exploited*
* So I proposed to do this again with new algorithms, and accepted

---

# Executive summary: do not try inventing your own random number generators!

---

# PRNGs are everywhere

* Rolling dice (for games)
* (Property) testing
* Variation analysis of electronic circuits
* Network congestion and delay tolerance analysis
* Risk analysis of project schedules

---

# Why random module is bad and archaic

* Too old (in 1980s)

---




