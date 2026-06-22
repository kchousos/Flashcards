# -*- mode: org; -*-
#+title: Blockchain foundations flashcards

Course note: [[20260215225759]]

--------------------
* Probabilities

Q: Union bound
A: Έστω τα ενδεχόμενα \(X_1, X_2, \ldots, X_n\). Η πιθανότητα ένα από αυτά να τύχει φράζεται από το άθροισμα των αντίστοιχων πιθανοτήτων, δηλαδή:

\[\Pr\left[X_1 \vee X_2 \vee \ldots \vee X_n\right] \leq \Pr\left[X_1\right]+\Pr\left[X_2\right]+\cdots+\Pr\left[X_n\right].\]

Q: Bernoulli inequality
A: \[(1-p)^r \ge 1-rp \iff 1-(1-p)^r \le rp\]

Q: Chernoff bound
A: Για ανά ζεύγη ανεξάρτητες boolean ΤΜ (Bernoulli trials) \(X_1, \ldots, X_n\), με \(X=\sum X_i\) και \(\mu=pn\), ισχύει ότι για κάθε \(\epsilon\in (0, 1]\):

  \[
  \Pr[X \le (1-\epsilon) \mu] \le e^{-\frac{\epsilon^2 \mu n}{2}}, \quad
  \Pr[X \ge (1+\epsilon) \mu] \le e^{-\frac{\epsilon^2 \mu n}{3}}.
  \]

--------------------
* Signatures

Q: Existential forgery game (άτυπα)
A: Ο αντίπαλος \(\mathcal{A}\) δεν μπορεί να παράξει καινούργια έγκυρη υπογραφή, ακόμα και με signing oracle για άλλα μηνύματα.

--------------------
* Hash functions

C: 2nd pre-image resistance [⇒] pre-image resistance.

C: Pre-image resistance [⇐] 2nd pre-image resistance.

C: Collision resistance [⇒] 2nd preimage resistance [⇒] preimage resistance.

--------------------
* Network

Q: Sybil attack
A: Περικύκλωση ενός honest node από bot nodes του adversary (spamming).

  **Δεν σπάει** το non-eclipsing assumption.

--------------------
* Chains

C: Υπό καθεστώς ορίου στο block size, ένας ορθολογικός miner συμπεριλαμβάνει πρώτα τις συναλλαγές με [την υψηλότερη αναλογία fees/byte].

Q: Common prefix
A: \(\forall P_1, P_2 \in \mathcal{H}, r_1, r_2: r_1 \le r_2 \implies\) \[C^{P_1}_{r_1}[:-k] \preceq C^{P_2}_{r_2}.\]

--------------------
* Proof-of-Work

Q: Πιθανότητα επιτυχίας ενός query για δεδομένα \(\kappa, T\)
A: \[p=\frac{T}{2^{\kappa}}.\]

Q: Πιθανότητα των honest parties να βγάλουν ένα block, για δεδομένα \(n,t,p, q\) (PoW)
A: \[f = 1-(1-p)^{q(n-t)}.\]

Q: Ακριβής πιθανότητα επιτυχίας ενός query \(p\), για δεδομένα \(n,t,f, q\) (PoW)
A: \[p = 1-(1-f)^{\frac{1}{q(n-t)}}.\]

C: Αν διπλασιάσω το target \(T\), το expected growth rate \(f\) του longest chain θα είναι [λιγότερο από] το διπλό.

--------------------
* Merkle trees

Q: Length of proof size for one inclusion
A: log₂(tree-height) · hash bit length

--------------------
* Attacks

Q: Rushing adversary
A: Ελέγχει για κάθε party αν και πότε ακούει το οποιοδήποτε μήνυμα (μέσα στα όρια του network delay/έχοντας non-eclipsing assumption)

--------------------
* Chain virtues / Backbone protocol

Q: Με τι population δουλεύει το environment του backbone protocol;
A: Σταθερό

Q: \(q\)-bounded random oracle
A: Random oracle που κρατάει μετρητή των queries ανά party και τα περιορίζει σε πλήθος \(q\)

Q: Random oracle pseudocode
A:

```
  T ← {}

  def H(x):
      if x ∉ T
          T[x] ←$— {0, 1}𞀹
      return x
```

Q: Safety
A: \(\forall P_1, P_2 \forall r_1, r_2:\)

  \[\mathcal{L}^{P_1}_{r_1} \preceq \mathcal{L}^{P_2}_{r_2}~~ \lor~~ \mathcal{L}^{P_1}_{r_1} \succeq \mathcal{L}^{P_2}_{r_2}.\]

Q: Honest majority assumption (μαθηματικός ορισμός με χρήση honest advantage)
A: \[t< (1-\delta)(n-t).\]

Q: Minimum chain quality \(\mu\)
A: \[\mu \geq 1-\frac{\textrm { max. rate of adversarial blocks }}{\textrm { min. rate of chain growth }} \geq 1-\frac{t}{n-t}.\]

Q: Pairing lemma
A: Αν το \(B\) έγινε mined σε convergence opportunity στον γύρο \(r\), οποιοδήποτε \(B' \neq B\) με το ίδιο ύψος παράχθηκε από τον adversary.

Q: Random variables του backbone protocol
A:

  1. \(X_r \in \{0,1\}\): Αν ο γύρος \(r\) ήταν successful

  2. \(Y_r \in \{0,1\}\): Αν ο γύρος \(r\) ήταν convergence opportunity

  3. \(Z_{rij} \in \{0,1\}\): Αν το \(j\)-οστό query του \(i\)-οστού adversarial node ήταν επιτυχές

     - \(Z_r \in \mathbb{N} = \sum\limits_i^t \sum\limits_j^q Z_{rij}\)

  Για σύνολο γύρων \(S\):

  1. \(X(S) = \sum_{r \in S}X_r\)

  2. \(Y(S) = \sum_{r \in S}Y_r\)

  3. \(Z(S) = \sum_{r \in S}Z_r\)

Q: \(f\)
A: Πιθανότητα επιτυχούς γύρου

  \[f=\mathbb E [X_r].\]

C: \(\mathbb E [Z_r] =\) [\(ptq\)].

C: Upper bound \(\mathbb E[Z_r] <\) [\(\frac{t}{n-t} \frac{f}{1-f}\)].

C: Lower bound \(\mathbb E [Y_r] \ge\) [\(q(n-t)p(1-p)^{q(n-t)-1} \ge f(1-f)\)].

Q: lower + upper bound του \(\mathbb E [X_r]\)
A: \[(1-f)pq(n-t)<f < pq(n-t).\]

Q: Lower + upper bound του \(X(S)\)
A: \[(1-\epsilon)f|S| < X(S) < (1+\epsilon)f|S|\]

Q: Lower bound του \(Y(S)\)
A: \[ \left(1-\frac{\delta}{3}\right)f|S| < (1-\epsilon)f(1-f)|S| < Y(S)\]

Q: Upper bound του \(Z(S)\)
A: \[Z(S) < \frac{t}{n-t}\frac{f}{1-f}|S| + \epsilon f|S| \le \left( 1 - \frac{2\delta}{3} \right)f|S|\]

Q: \(\lambda\)
A: Chernoff interval

Q: \(\epsilon\)
A: Chernoff error

Q: Balancing inequality
A: \[3\epsilon + 3f \le \delta ~\Longrightarrow~ \epsilon=f = \frac{\delta}{6}.\]

C: \(\lambda \ge\) [\(\frac{2}{f}\)].

Q: Typicality (typical executions)
A: Μία εκτέλεση είναι *τυπική* αν για οποιοδήποτε σύνολο \(S\) συνεχόμενων γύρων με \(|S| \ge \lambda\), ισχύουν τα:

  1. \((1-\epsilon)\mathbb E[X(S)] < X(S) < (1+\epsilon) \mathbb E [X(S)]\),

  2. \((1-\epsilon)\mathbb E[Y(S)] < Y(S)\),

  3. \(Z(S) < (1+\epsilon) \mathbb E [Z(S)]\),

  4. Το random oracle είναι *causal*.

Q: Chain growth theorem
A: Σε typical executions, έχουμε chain growth με \(s=\lambda\) και \(\tau=(1-\epsilon)f\).

Q: Patience lemma
A: Σε typical executions, οποιαδήποτε \(k\ge 2\lambda f\) συνεχόμενα blocks παράχθηκαν σε \(> \frac{k}{2f}\) συνεχόμενους γύρους.

C: Common prefix \(k =\) [\(2\lambda f\)].

Q: Common prefix lemma
A: Common prefix για ίδιο \(r\).

Q: Security
A: Safety + liveness

Q: Availability
A: Σε synchronous setting, όταν έχουμε sleepy validators, αν \(t< \beta n\) για τα online parties, το πρωτόκολλο είναι live.

--------------------
* Light clients

Q: Τι κατεβάζει ένα SPV light wallet και γιατί;
A: 1. Header κάθε block για επιβεβαίωση της αλυσίδας και του proof-of-work

2. Merkle proofs για transactions που αφορούν το wallet address

--------------------
* Proof-of-Stake

** Longest chain (Ouroboros Praos)

C: Σε PoS longest-chain puzzle, ένας adversary μπορεί να προτείνει [άπειρα] έγκυρα blocks.

C: Ένα PoS longest chain πρωτόκολλο είναι *safe* και *live* για \(t<\)[\(\frac{n}{2}\)].

Q: Πιθανότητα των honest parties να βγάλουν ένα block, για δεδομένα \(n,t,p\) (PoS)
A: \[f = 1-(1-p)^{n-t}.\]

Q: Ακριβής πιθανότητα επιτυχίας ενός query \(p\), για δεδομένα \(n,t,f\) (PoS)
A: \[p = 1-(1-f)^{\frac{1}{n-t}}.\]

Q: PKI assumption
A: Κάθε party έχει σταθερό public key το οποίο το γνωρίζουν όλοι.

Q: PoS ticket inequality
A: \[H(\rho^i \| pk \| r) < T\cdot\phi(\omega),\]

  όπου \(r\) το slot, \(\rho^i\) το randomness του epoch \(i\), \(H\) μια VRF, \(\omega\) το stake μου και \(\phi: \omega \to [0,1]\) μία συνάρτηση που μετατρέπει το stake σε ποσοστό.

  Π.χ. \(\phi(\omega)= \omega\).

Q: Grinding attack
A: Χρήση computational queries σε PoS πρωτόκολλα.

  Ο adversary πειραματίζεται με το hash input για να κερδίσει το lottery.

  Στο PoW το grinding είναι μέρος του πρωτοκόλλου.

Q: Τι αποτρέπει και τι επιτρέπει το PoS όσον αφορά grinding attacks;
A: Αποτρέπει επιθέσεις στα tickets αλλά επιτρέπει επιθέσεις στα blocks ενός συγκεκριμένου slot.

  ![Blocks grinding attack στο slot 5.](./Attachments/clipboard-20260615T155219.png)

Q: Equivocation
A: Παραγωγή/ψήφος πολλαπλών blocks για το ίδιο slot.

C: Τα fan outs σε PoS longest chain πρωτόκολλα είναι [πάντα adversarial].

Q: PoS block signing
A: \[ \mathrm{Sign}(sk, j \| \rho^j \| r \| x\| s \| pk \| \pi \| y).\]

Q: PoS block header
A: \[ j \| \rho^j \| r \| x\| s \| pk \| \pi \| y \| \sigma,\]

  με \(\sigma \leftarrow \mathrm{Sign}(sk, j \| \rho^j \| r \| x\| s \| pk \| \pi \| y)\).

Q: Τι ελέγχουμε στο PoS block validation για τα slots;
A: Ότι είναι *αύξοντα* και *στο παρελθόν*.

Q: Bounded state shifting assumption
A: Τα χρήματα δεν μετακινούνται πολύ γρήγορα ανάμεσα σε epochs.

Q: Πώς βρίσκω το καινούργιο stake distribution όταν μπαίνω σε νέο epoch;
A: Όταν φτάσω σε slot \(r \equiv 0 \bmod m\), κόβω τα τελευταία \(v\) slots και παίρνω το state του block του προηγούμενου slot ως το stake distribution.

Q: Τύπος \(v\) για εύρεση νέου stake distribution ανάμεσα σε epochs
A: \[v = \max \left( \frac{k+l+1}{\tau},  s+1 \right).\]

Q: Super safety
A: Όλοι συμφωνούν απολύτως με όλους τους άλλους.

  Κυρίως αφορά το safety σε PoS όσον αφορά το stake update ανάμεσα σε epochs.

Q: Τι σχέση έχουν τα coins και τα public keys;
A: Κάθε public key μπορεί να έχει ένα ή κανένα coin.

Q: Παραγωγή epoch randomness \(\rho^j\)
A: \[\rho^j = \bigoplus_{i=1} \rho_i^j \in \{0,1\}^{\kappa},\quad \rho_i^j = G(\rho^{j-1}\| r \| pk),\]

  όπου \(i\) το index του block στο epoch, \(j\) το επόμενο epoch και \(G\) ένα VRF.

Q: Verifiable Random Function (VRF) scheme
A:
  1. \(\mathrm{VRF.Gen}(1^{\kappa}) \rightarrow (sk, pk),\)

  2. \(\mathrm{VRF.Eval}(sk, x) \rightarrow y, \pi,\)

  3. \(\mathrm{VRF.Ver}(pk, x, y, \pi) \rightarrow 0 \lor 1\)

Q: Static adversary
A:
  1. Adversary corrupts chosen parties

  2. Randomness at epoch 0 is generated

Q: Adaptive adversary
A: Corrupts parties dynamically

Q: Πώς αποτρέπονται τα long-range attacks;
A: Μέσω key erasure

C: Τα long-range attacks αφορούν [slowly] adaptive adversaries.

C: Οι fully adaptive adversaries αντιμετωπίζονται χρησιμοποιώντας [VRF για το ticket hash].

Q: Aggregation independent function
A: \[\phi(\omega)= 1-(1-f)^\omega,\]

  με πιθανότητα επιτυχίας ticket ίση με \(1-(1-\phi(\omega))^n\).

--------------------
** Quorum-based (Simplex)

C: Simplex: round [≠] iteration.

Q: Τι honest majority απαιτεί το Simplex;
A: \[t <\frac{n}{3}.\]

Q: Notarized block (Simplex)
A: Block με \(> \frac{2n}{3}\) votes από distinct PKI keys.

Q: Finalized iteration \(h\) (Simplex)
A: Ύψος με \(> \frac{2n}{3}\) `finalize` messages από distinct PKI keys.

Q: Quorum intersection argument
A: Για \(n\) parties, με quorum threshold \(\frac{2n}{3}\), δεν μπορώ να έχω σε δύο blocks quorum. Αν γινόταν, το overlap τους θα είχε μέγεθος \(\frac{n}{3}\) οι οποίοι ως equivocators είναι adversarial. Όμως, από honest majoriy, \(t<\frac{n}{3}\).

Q: Simplex protocol (high-level)
A: Ο παίκτης \(p\) όταν μπαίνει στο iteration \(h\):

  1. **Proposal**: Ελέγχει αν είναι ο slot leader. Αν ναι, τότε προτείνει ένα block \(B=(s_h \| x \| h)\), με \(x\) το mempool, \(h\) το slot number, \(s_h\) οποιοδήποτε notarized block για το slot \(h-1\).

  2. **Timer**: Εκκινεί έναν τοπικό μετρητή \(T_h\) που λήγει μετά από χρόνο \(3\Delta\). Αν λήξει, ο παίκτης ψηφίζει dummy block.

  3. **Voting**: Για το πρώτο proposal που θα δει από τον leader, κάνει validate το block. Αν όλα καλά, το ψηφίζει.

  4. **Finalization**: Όταν έχει ένα notarized block για το \(h\), περνάει στο iteration \(h+1\). Σταματάει το timer και στέλνει μήνυμα finalize.

  5. **Confirmation**: Όταν το \(h\) γίνει finalized, το \(\mathcal{L}\) του chain που τελειώνει στο \(h\) γίνεται confirmed.

Q: Πόσο χρόνο θέλει ένα block για να γίνει από proposed → finalized στο πρωτόκολλο Simplex;
A: \[3\Delta .\]

C: Simplex: Δύο non-⊥ blocks στο ίδιο iteration \(h\) δεν μπορούν ποτέ να [είναι ταυτόχρονα notarized].

  <details>Quorum intersection argument για την απόδειξη.</details>

C: Simplex: Αν σε ένα iteration \(h\) έχουμε notarized bot block \(\bot_h\), το \(h\) δεν μπορεί να [γίνει finalized].

  <details>No honest party votes for both \(\bot_h\) and \(\left\langle \mathrm{finalize}, h \right\rangle\). The result follows from quorum intersection argument.</details>

Q: Simplex: Για να πάω στο επόμενο iteration, χρειάζομαι notarization ή finalization;
A: Notarization

Q: Security in partial synchrony
A: Safety always + liveness after GST

Q: Ποιο είναι το expected growth rate για το notarized chain στο Simplex;
A: Θα μεγαλώνει κάθε \(2\Delta\), άρα \(\frac{1}{2}\) blocks/second.

Q: Ποιο είναι το expected growth rate για το finalized chain στο Simplex;
A: Θα μεγαλώνει κάθε \(2\Delta\), άρα \(\frac{1}{2}\) blocks/second.

Q: Finality
A: Safety under partial synchrony

Q: Ιδιότητες του accountability
A:
  1. **Correctness**: safety violation → \(|J(M)|> \frac{1}{3}n\),
  2. **No framing**: \(J(M) \cap \mathcal{H} = \emptyset\) για οποιοδήποτε \(M\), όπου \(\mathcal{H}\) τα honest parties.

Q: Slashing
A: Καταστροφή των χρημάτων σου σε περίπτωση που το adjudication function σε χαρακτηρίσει adversary/misbehaving.

Q: Economic safety
A: Σε περίπτωση παραβίασης του safety, το 1/3 του stake μπορεί να γίνει slashed.

Q: Πότε είναι ένα πρωτόκολλο accountable;
A: Όταν πέρα από το πρωτόκολλο \(\Pi\) προς εκτέλεση δίνεται και μία adjudication συνάρτηση \(J\).

C: Accountability [⇒] finality.

C: Finality [⇐] accountability.

Q: Χωρισμός finality/availability σε είδη πρωτοκόλλων
A: Το availability ισχύει για longest-chain πρωτόκολλα ενώ το finality σε quorum-based.

Q: Availability/Finality dilemma
A: Κανένα πρωτόκολλο δεν μπορεί να έχει και τα δύο.
