# Worked Solutions to Challenging Probability Problems

This appendix presents mathematically complete, step-by-step algebraic and probabilistic solutions to **eight of the most critical and conceptually demanding problems** from Frederick Mosteller's classic text, **"Fifty Challenging Problems in Probability with Solutions"**. 

These problems are frequently asked during quantitative research and data science interviews because they test your ability to structure complex probability spaces, apply conditional expectations, solve difference equations, and analyze geometric probability paradoxes.

---

## 📂 The Eight Master Probability Challenges

### 1. The Birthday Paradox
#### Problem:
What is the minimum number of people, $n$, in a room such that the probability that at least two people share the same birthday is greater than $50\%$? (Assume $365$ days in a year, all equally likely).

#### Mathematical Solution:
1.  Instead of calculating the probability of at least one collision directly, it is far easier to calculate the probability of the complement event: $P(\text{No two people share a birthday})$.
2.  Let $n$ be the number of people.
    *   Person 1 can have any birthday (probability $\frac{365}{365}$).
    *   Person 2 must have a different birthday than Person 1 (probability $\frac{364}{365}$).
    *   Person $i$ must have a different birthday than all previous $i-1$ people (probability $\frac{365 - (i-1)}{365}$).
3.  By independence, the joint probability of no shared birthdays is the product of these terms:
    $$P(\text{No collision}) = \frac{365}{365} \times \frac{364}{365} \times \dots \times \frac{365 - n + 1}{365} = \frac{365!}{365^n (365 - n)!}$$
4.  The probability of at least one shared birthday is:
    $$P(\text{At least one shared}) = 1 - \frac{365!}{365^n (365 - n)!}$$
5.  **Deriving the Exponential Approximation**:
    We rewrite the product terms:
    $$P(\text{No collision}) = \prod_{i=1}^{n-1} \left( 1 - \frac{i}{365} \right)$$
    Using the Taylor expansion approximation $e^{-x} \approx 1 - x$ for small $x$:
    $$P(\text{No collision}) \approx \prod_{i=1}^{n-1} e^{-\frac{i}{365}} = e^{-\sum_{i=1}^{n-1} \frac{i}{365}} = e^{-\frac{n(n-1)}{2 \times 365}} \approx e^{-\frac{n^2}{730}}$$
6.  We set this probability to $0.50$ to solve for the boundary $n$:
    $$e^{-\frac{n^2}{730}} \le 0.50 \implies -\frac{n^2}{730} \le \log(0.50) \implies \frac{n^2}{730} \ge 0.693 \implies n^2 \ge 505.89 \implies n \ge 22.49$$

#### Conclusion:
The minimum number of people required is **$23$** (which yields an exact probability of $50.73\%$).

---

### 2. The Gambler's Ruin (Random Walk)
#### Problem:
Two gamblers, Player A and Player B, play a series of independent games. In each game, Player A wins $\$1$ with probability $p$ and B wins $\$1$ (A loses $\$1$) with probability $q = 1-p$. Player A starts with $\$a$ and Player B starts with $\$b$ (total capital in the game is $N = a + b$). The game ends when one player goes bankrupt (reaches $\$0$). What is the probability that Player A goes bankrupt?

#### Mathematical Solution:
1.  Let $P_x$ represent the probability that Player A eventually goes bankrupt given that A's current capital is $\$x$.
2.  By the law of total probability, after one transition from state $x$:
    $$P_x = p P_{x+1} + q P_{x-1} \quad \text{for } 0 < x < N$$
3.  **Establish Boundary Conditions**:
    *   If A reaches $\$0$, B has won, so A's ruin is guaranteed: $P_0 = 1$.
    *   If A reaches $\$N$, B has gone bankrupt, so A's ruin is impossible: $P_N = 0$.
4.  We solve this second-order linear homogeneous difference equation. Since $p + q = 1$, we rewrite:
    $$(p + q) P_x = p P_{x+1} + q P_{x-1} \implies p(P_{x+1} - P_x) = q(P_x - P_{x-1}) \implies P_{x+1} - P_x = \frac{q}{p} (P_x - P_{x-1})$$
5.  Let $d_x = P_x - P_{x-1}$. This forms a geometric progression: $d_x = \left(\frac{q}{p}\right)^{x-1} d_1$.
6.  Express $P_x$ as a telescoping sum starting from $P_0 = 1$:
    $$P_x - P_0 = \sum_{i=1}^{x} (P_i - P_{i-1}) \implies P_x - 1 = d_1 \sum_{i=1}^{x} \left(\frac{q}{p}\right)^{i-1}$$
7.  **Case 1: $p \ne q$** (Biased Game):
    $$P_x - 1 = d_1 \left( \frac{1 - (q/p)^x}{1 - q/p} \right)$$
    Use the boundary condition $P_N = 0$ to solve for $d_1$:
    $$0 - 1 = d_1 \left( \frac{1 - (q/p)^N}{1 - q/p} \right) \implies d_1 = -\frac{1 - q/p}{1 - (q/p)^N}$$
    Substitute $d_1$ back into the equation for $P_x$:
    $$P_x = 1 - \frac{1 - (q/p)^x}{1 - (q/p)^N} = \frac{(q/p)^x - (q/p)^N}{1 - (q/p)^N}$$
8.  **Case 2: $p = q = 1/2$** (Fair Game):
    The ratio $\frac{q}{p} = 1$, so the sum simplifies:
    $$P_x - 1 = d_1 \sum_{i=1}^{x} 1 = d_1 x$$
    Use $P_N = 0 \implies 0 - 1 = d_1 N \implies d_1 = -1/N$.
    $$P_x = 1 - \frac{x}{N} = \frac{N - x}{N}$$
    Since $N = a + b$ and starting capital is $x = a$:
    $$P_a = \frac{b}{a + b}$$

#### Conclusion:
The probability of Player A's ruin is **$\frac{(q/p)^a - (q/p)^{a+b}}{1 - (q/p)^{a+b}}$** if $p \ne q$, and **$\frac{b}{a+b}$** if the game is fair.

---

### 3. Bertrand's Box Paradox
#### Problem:
There are three identical boxes:
*   Box 1 contains two gold coins (G, G).
*   Box 2 contains two silver coins (S, S).
*   Box 3 contains one gold and one silver coin (G, S).
You choose a box at random and draw a coin. It is gold. What is the probability that the remaining coin in the chosen box is also gold?

#### Mathematical Solution:
1.  **The Trap**: The remaining coin is either gold or silver, so the probability is $1/2$. This is incorrect because it ignores the conditional probability weight of the first draw.
2.  Let $B_1, B_2, B_3$ be the events of choosing Box 1, 2, and 3 respectively. Each has prior probability:
    $$P(B_1) = P(B_2) = P(B_3) = \frac{1}{3}$$
3.  Let $G$ be the event that the first drawn coin is Gold. The conditional likelihoods are:
    *   $P(G \mid B_1) = 1$ (both are gold)
    *   $P(G \mid B_2) = 0$ (both are silver)
    *   $P(G \mid B_3) = 1/2$ (one gold, one silver)
4.  Apply the Law of Total Probability to find $P(G)$:
    $$P(G) = P(G \mid B_1)P(B_1) + P(G \mid B_2)P(B_2) + P(G \mid B_3)P(B_3) = (1)\left(\frac{1}{3}\right) + (0)\left(\frac{1}{3}\right) + \left(\frac{1}{2}\right)\left(\frac{1}{3}\right) = \frac{1}{3} + \frac{1}{6} = \frac{1}{2}$$
5.  We want to find the probability that the remaining coin is gold, which is equivalent to the probability that we selected Box 1 given that the first drawn coin is Gold ($P(B_1 \mid G)$).
6.  Apply Bayes' Theorem:
    $$P(B_1 \mid G) = \frac{P(G \mid B_1) P(B_1)}{P(G)} = \frac{(1)\left(\frac{1}{3}\right)}{\frac{1}{2}} = \frac{2}{3}$$

#### Conclusion:
The probability that the remaining coin is gold is **$2/3$** (not $1/2$).

---

### 4. Bertrand's Paradox (Random Chords)
#### Problem:
Consider an equilateral triangle inscribed in a circle. If a chord of the circle is chosen "at random," what is the probability that the chord is longer than a side of the inscribed triangle?

---

#### Mathematical Solution:
The length of a side of an inscribed equilateral triangle in a circle of radius $R$ is $L = R\sqrt{3}$. Bertrand showed that the answer depends on how you define the random selection of a chord:

```
        Method 1: Random Endpoints             Method 2: Random Radius
                  *   * *                             *   * *
               *   \     *                         *       |     *
              *     \     *                       * ───[x]─┼───── *
              *      \    *                       *        |      *
               *    /    *                         *       |     *
                  *   * *                             *   * *
```

*   **Method 1: Random Endpoints**
    *   Fix one endpoint of the chord at a vertex of the inscribed triangle. The other endpoint is chosen uniformly along the circumference of the circle.
    *   The triangle divides the circumference into three equal arcs of $120^\circ$ each. 
    *   The chord is longer than $R\sqrt{3}$ if and only if the second endpoint lies on the middle arc opposite the fixed vertex.
    *   $$P = \frac{\text{Arc Length of middle segment}}{\text{Total Circumference}} = \frac{120^\circ}{360^\circ} = \frac{1}{3}$$

*   **Method 2: Random Radius / Distance**
    *   Choose a radius of the circle at random. The chord is perpendicular to this radius. Select a point uniformly along the radius to serve as the midpoint of the chord.
    *   The chord is longer than $R\sqrt{3}$ if its midpoint lies closer to the center of the circle than the midpoint of the triangle's sides, which is at a distance of $R/2$ from the center.
    *   $$P = \frac{\text{Acceptable Distance}}{\text{Total Radius}} = \frac{R/2}{R} = \frac{1}{2}$$

*   **Method 3: Random Midpoint Area**
    *   Choose a point uniformly at random anywhere inside the circle to act as the midpoint of the chord.
    *   The chord is longer than $R\sqrt{3}$ if its midpoint lies within the concentric inner circle of radius $R/2$.
    *   $$P = \frac{\text{Area of Inner Circle}}{\text{Area of Outer Circle}} = \frac{\pi (R/2)^2}{\pi R^2} = \frac{1}{4}$$

#### Conclusion:
Bertrand's paradox demonstrates that "at random" is mathematically undefined without specifying the exact geometric probability measure.

---

### 5. Cliffhanger (Random Walk)
#### Problem:
An intoxicated man stands one step away from the edge of a cliff. In each step, he moves away from the cliff (step forward) with probability $p = 2/3$ and toward the cliff (step backward) with probability $q = 1-p = 1/3$. What is the probability that he eventually falls off the cliff?

#### Mathematical Solution:
1.  Let $P$ represent the probability of eventually falling off the cliff starting from $1$ step away.
2.  By the law of total probability, after his first step:
    *   With probability $q = 1/3$, he steps backward and falls immediately.
    *   With probability $p = 2/3$, he steps forward and is now $2$ steps away from the edge.
    $$P = q(1) + p P_{2}$$
    Where $P_2$ is the probability of eventually falling starting from $2$ steps away.
3.  **The Step Independence Principle**:
    To fall from $2$ steps away, he must first reach the state of being $1$ step away (which carries probability $P$), and then from that state, eventually fall off (carrying another independent probability $P$):
    $$P_2 = P^2$$
4.  Substitute this back into the equation:
    $$P = q + p P^2 \implies P = \frac{1}{3} + \frac{2}{3} P^2$$
5.  Multiply by $3$ to form a quadratic equation:
    $$2P^2 - 3P + 1 = 0 \implies (2P - 1)(P - 1) = 0$$
    This yields two mathematical roots: $P = 1/2$ and $P = 1$.
6.  **Resolve the Ambiguity**:
    Since $p = 2/3 > q = 1/3$, there is a strong drift away from the cliff. The probability of falling must be strictly less than 1. Therefore, we select the root $P = 1/2$.

#### Conclusion:
The probability that he eventually falls off the cliff is **$1/2$**.

---

### 6. The Three Prisoners Problem
#### Problem:
Three prisoners, A, B, and C, are on death row. The governor decides to pardon one of them at random. Prisoner A asks the guard: "Since B or C must be executed, tell me which one. It won't affect my own probability of being pardoned." The guard says: "B is to be executed." Prisoner A is happy, assuming A's probability of being pardoned has jumped from $1/3$ to $1/2$. Is A correct?

#### Mathematical Solution:
1.  Let $A, B, C$ be the events that Prisoner A, B, or C is pardoned. Prior probabilities:
    $$P(A) = P(B) = P(C) = \frac{1}{3}$$
2.  Let $I_B$ represent the event that the guard names Prisoner B as the one to be executed.
3.  The conditional probabilities of the guard's response are:
    *   If A is to be pardoned ($A$), the guard must choose between naming B or C. Assume the guard chooses B at random: $P(I_B \mid A) = 1/2$.
    *   If B is to be pardoned ($B$), the guard cannot name B: $P(I_B \mid B) = 0$.
    *   If C is to be pardoned ($C$), B must be executed, so the guard has no choice but to name B: $P(I_B \mid C) = 1$.
4.  Compute the total probability $P(I_B)$:
    $$P(I_B) = P(I_B \mid A)P(A) + P(I_B \mid B)P(B) + P(I_B \mid C)P(C) = \left(\frac{1}{2}\right)\left(\frac{1}{3}\right) + (0)\left(\frac{1}{3}\right) + (1)\left(\frac{1}{3}\right) = \frac{1}{6} + \frac{1}{3} = \frac{1}{2}$$
5.  Apply Bayes' Theorem to find A's posterior probability of pardon:
    $$P(A \mid I_B) = \frac{P(I_B \mid A)P(A)}{P(I_B)} = \frac{\left(\frac{1}{2}\right)\left(\frac{1}{3}\right)}{\frac{1}{2}} = \frac{1}{3}$$
6.  Compute C's posterior probability of pardon:
    $$P(C \mid I_B) = \frac{P(I_B \mid C)P(C)}{P(I_B)} = \frac{(1)\left(\frac{1}{3}\right)}{\frac{1}{2}} = \frac{2}{3}$$

#### Conclusion:
Prisoner A's probability of being pardoned **remains exactly $1/3$**. Prisoner C's probability has jumped to **$2/3$**.

---

### 7. The Monte Carlo Coin Game (Pattern Matches)
#### Problem:
If you repeatedly toss a fair coin, what is the expected number of tosses required to get the sequence **HTH**?

#### Mathematical Solution:
1.  Let $E$ be the expected number of tosses to get HTH.
2.  We construct a state-transition model based on the prefixes matched:
    *   State 0: No match (default)
    *   State 1: Matched **H**
    *   State 2: Matched **HT**
    *   State 3: Matched **HTH** (End)
3.  Let $E_i$ represent the expected number of *additional* tosses needed to reach State 3 starting from State $i$. We want to find $E_0$.
4.  Set up the transition equations for each state (each transition takes 1 toss):
    *   **From State 0**:
        *   Toss H (prob 1/2) $\to$ Go to State 1.
        *   Toss T (prob 1/2) $\to$ Remain in State 0.
        $$E_0 = 1 + \frac{1}{2} E_1 + \frac{1}{2} E_0 \implies \frac{1}{2} E_0 = 1 + \frac{1}{2} E_1 \implies E_0 = 2 + E_1$$
    *   **From State 1** (Matched H):
        *   Toss T (prob 1/2) $\to$ Go to State 2 (Matched HT).
        *   Toss H (prob 1/2) $\to$ Remain in State 1 (Matched H).
        $$E_1 = 1 + \frac{1}{2} E_2 + \frac{1}{2} E_1 \implies \frac{1}{2} E_1 = 1 + \frac{1}{2} E_2 \implies E_1 = 2 + E_2$$
    *   **From State 2** (Matched HT):
        *   Toss H (prob 1/2) $\to$ Go to State 3 (Matched HTH - Success, 0 remaining).
        *   Toss T (prob 1/2) $\to$ Go to State 0 (Matched T, back to start).
        $$E_2 = 1 + \frac{1}{2} (0) + \frac{1}{2} E_0 \implies E_2 = 1 + \frac{1}{2} E_0$$
5.  Substitute $E_2$ into the equation for $E_1$:
    $$E_1 = 2 + \left( 1 + \frac{1}{2} E_0 \right) = 3 + \frac{1}{2} E_0$$
6.  Substitute $E_1$ into the equation for $E_0$:
    $$E_0 = 2 + \left( 3 + \frac{1}{2} E_0 \right) = 5 + \frac{1}{2} E_0$$
7.  Solve for $E_0$:
    $$\frac{1}{2} E_0 = 5 \implies E_0 = 10$$

#### Conclusion:
The expected number of tosses to get HTH is **$10$**.

---

### 8. The Coconuts Problem
#### Problem:
Five men and a monkey are shipwrecked on an island. They gather coconuts and decide to divide them the next morning. 
During the night, the first man wakes up, divides the coconuts into five equal shares, has one coconut left over, which he gives to the monkey, hides his share, and goes back to sleep.
The second, third, fourth, and fifth men wake up one by one and repeat the exact same procedure (dividing the remaining coconuts into five equal piles, having one left over, giving it to the monkey, and hiding their share).
The next morning, they divide the remaining coconuts into five equal piles, and again there is one left over for the monkey. What is the minimum number of coconuts they could have gathered?

#### Mathematical Solution:
1.  Let $C_0$ be the initial number of coconuts.
2.  Let $C_k$ be the number of coconuts remaining after the $k$-th man has taken his share.
3.  The transition equation is:
    $$C_k = \frac{4}{5} (C_{k-1} - 1)$$
4.  This forms a first-order linear difference equation. We solve it by finding a fixed point (steady-state value) $S$:
    $$S = \frac{4}{5} (S - 1) \implies 5S = 4S - 4 \implies S = -4$$
5.  We rewrite the difference equation in terms of the deviation from the fixed point $C_k + 4$:
    $$C_k + 4 = \frac{4}{5}(C_{k-1} + 4)$$
6.  Apply this recursively from $k=0$ to $k=5$ (after the fifth man):
    $$C_5 + 4 = \left( \frac{4}{5} \right)^5 (C_0 + 4)$$
7.  Isolate the initial number of coconuts $C_0$:
    $$C_0 + 4 = \frac{5^5}{4^5} (C_5 + 4) = \frac{3125}{1024} (C_5 + 4)$$
8.  Since the number of coconuts must be an integer, $C_5 + 4$ must be a multiple of $1024$.
9.  The next morning, the remaining coconuts $C_5$ are divided into five equal piles with one left over, which means:
    $$C_5 = 5m + 1 \implies C_5 + 4 = 5m + 5 = 5(m+1)$$
10. Therefore, $C_5 + 4$ must be a multiple of $1024$ **and** a multiple of $5$. The smallest positive integer that satisfies this is:
    $$C_5 + 4 = 1024 \times 5 = 5120$$
11. Substitute this back to solve for the initial coconuts $C_0$:
    $$C_0 + 4 = \frac{3125}{1024} (5120) = 3125 \times 5 = 15625 \implies C_0 = 15621$$

#### Conclusion:
The minimum number of coconuts they could have gathered is **$15,621$**.
