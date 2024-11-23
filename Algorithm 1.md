# Algorithm 1

## A. Background

- Objective: roll a magical dive to recevive Price 100,000 USD.
- Number of player: 1 with Q(1,2,3) and 4 with Q4.
- Condition: toss “1” three times in a row (1,1,1).

***Cost***:
- cost_fail_1_times = 1,000 (USD)
- cost_fail_3_times = 3 * 1,000 (USD)
- cost_success_1_then_fail = 7,800 (USD)
- cost_success_2_then_fail = 49,500 (USD)

However, you don’t have to pay if you stop playing.

## B. Question & Answer

### Q1: How many dice tosses do you need to get to 95% win confidence?

#### 1. Define key metrics:
- n: numbers of dive roll (toss)
- m = n/3: numbers of sequences dive roll
- P_target confidence: 0.95 (95%)


#### 2. Analyst:

##### a, Probability of success/fail per sequences:
- Probability of rolling "1" : P(win) = 1/6
- Probability of fail rolling "1" : P(fail) = 5/6
==> To win rolling “1” three times in a row (1,1,1)
 $$ P(\text{win in a row}) =  P(win)^3 = (1/6)^3 = 1/216 $$
 $$ P(\text{fail in a row}) =  1- P(win)^3 = 1- 1/216 = 215/216 $$

##### b, Probability of rolling "1" fail per m sequences:
$$ P(\text{fail in m row}) = (215/216)^m $$
##### c, To achieve 95% confidence of winning --> the probability of failing must drop to 5%:
$$ P(\text{fail in m row}) = 0.05 $$
$$\text{<=>}\quad(215/216)^m = 0.05 $$
$$\text{<=>}\quad m \geq \frac{\ln(0.05)}{\ln\left( \frac{215}{216} \right)}\ $$
$$\text{=>}\quad m \approx 647 \, (\text{sequences})\ $$

- With m = n/3: numbers of sequences dive roll
$$\text{=>}\quad n = m \times 3 = 647 \times 3 = 1941 \, (\text{tosses}) \ $$

#### 3. Code python

```python
import math

# Số lần tung cần thiết để đạt 95% xác suất chiến thắng
def tosses_to_95_confidence():
    P_fail = 215 / 216  # Xác suất thất bại trong 1 chuỗi
    P_target = 0.95  # Mục tiêu 95% chiến thắng
    m = math.ceil(math.log(1 - P_target) / math.log(P_fail))  # Số chuỗi cần thiết
    n = m * 3  # Mỗi chuỗi gồm 3 lần tung
    return n

# Kết quả
tosses_needed = tosses_to_95_confidence()
print(f"Số lần tung cần thiết để đạt 95% độ tin cậy chiến thắng: {tosses_needed}")
```

### Q2: Calculate your net gain/loss per toss.

#### 1. Define key metrics:
- n: numbers of dive roll (toss)
- m = n/3: numbers of sequences dive roll
- Expected reward per sequences
- Expected cost per toss
- Net gain/loss per toss = Expected reward per toss−Expected cost per toss

#### 2. Analyst:

##### a, Expected reward per toss:
- The reward for winning is 100,000 USD for one sequences:
   $$ P(\text{win in a row}) = 1/216 $$
$$\text{ => Expected reward per sequence = } P(\text{win in a row}) \times 100,000 = (1/216) \times 100,000 \approx 462.96 \quad(USD) $$
$$\text{ => Expected reward per tosses = } \text{Expected reward per sequence/3} = 462.96/3 \approx 154.32 \quad(USD) $$

##### b, Expected cost per toss:

- If fail 3 times:  $$ P(\text{3 times fail}) = (5/6)^3 = 125/216 $$
& cost_fail_3_times: 3 x 1,000 = 3,000 (USD)

- If roll "1" then fail: $$ P(\text{"1" and fail}) = 1/6 \times 5/6 = 5/36 $$
& cost_success_1_then_fail: 7,800 (USD)

- If roll "1", "1", then fail:$$ P(\text{"1","1" and fail}) = 1/6 \times 1/6 \times 5/6 = 5/216 $$
& Cost: 49,500 (USD)

$$\text{ => Expected cost per sequence =} P(\text{3 times fail}*3,000) + P(\text{"1" and fail}*7,800) + P(\text{"1","1" and fail}*49,500) $$
$$\text{ <=>} \left( \frac{5}{6} \times (1,000) \right) + \left( \frac{5}{36} \times (7,800) \right) + \left( \frac{5}{216} \times (49,500) \right)
$$
$$ \approx 3,965.28 
$$


$$\text{=> Expected cost per toss}  = \text{Expected cost per sequence/3} $$
$$ \text{<=>} \quad 3,965.28 /3 $$
$$ \approx 1,321.76 \quad (USD)$$

##### c, Net gain/loss per toss:
$$\text{ Net gain/loss per toss}  = \text{Expected reward per toss} - \text{Expected cost per toss} $$
$$ \text{<=>} \quad 154.32 - 1,321.76 $$
$$ \approx -1,167.44 \quad (USD) $$

#### 3. Code python

```python
import math

# Step 1: Calculate the expected net gain/loss per toss
def calculate_net_gain_loss():
    # Probabilities
    P_fail_3_times = (5 / 6) ** 3  # Probability of 3 consecutive failures
    P_success_1_then_fail = (1 / 6) * (5 / 6)  # Probability of 1 then fail
    P_success_2_then_fail = (1 / 6) ** 2 * (5 / 6)  # Probability of 1,1 then fail
    P_success_sequence = (1 / 6) ** 3  # Probability of 3 consecutive successes
    
    # Costs
    cost_fail_3_times = 3 * 1000
    cost_success_1_then_fail = 7800
    cost_success_2_then_fail = 49500
    reward_success_sequence = 100000
    
    # Expected cost per sequence
    expected_cost_sequence = (
        P_fail_3_times * cost_fail_3_times +
        P_success_1_then_fail * cost_success_1_then_fail +
        P_success_2_then_fail * cost_success_2_then_fail
    )
    
    # Expected reward per sequence
    expected_reward_sequence = P_success_sequence * reward_success_sequence
    
    # Net expectation per sequence
    net_expectation_per_sequence = expected_reward_sequence - expected_cost_sequence
    
    # Per toss (each sequence has 3 tosses)
    net_expectation_per_toss = net_expectation_per_sequence / 3
    return net_expectation_per_toss

# Step 2: Main calculation
def main():
    
    # Calculate net gain/loss per toss
    net_gain_loss_per_toss = calculate_net_gain_loss()
    
    # Print results
    print(f"Net gain/loss per toss: ${net_gain_loss_per_toss:.2f}")

# Run the program
if __name__ == "__main__":
    main()
```
### Q3: What is the minimum amount of prize money you will consider playing this game for? Calculate your relevant premium in this case.

#### 1. Define key metrics:
- n: numbers of dive roll (toss)
- m = n/3: numbers of sequences dive roll
- Expected reward per sequences (answer Q2) 
- Expected cost per toss (answer Q2)~ 3,965.28
- Relevant premium base on Expected reward per sequences: a% (is the desired profit margin)
- P(win in a row) = 1/216

#### 2. Analyst:
#### a, The minimum amount of prize money
Assumption: if the player wants a 20% profit margin:
$$
\begin{aligned}
\text{Premium per Sequence} &= 20\% \\
\text{=> Relevant premium} &= \text{Premium per Sequence} \times \text{Expected cost per toss} \\
\text{=} &\ 20\% \times \text{Expected cost per toss} \\
\end{aligned}
$$

$$ 
\begin{aligned}
\text{Minimum amount} &= (\text{Expected cost per toss} + \text{Relevant premium})/P(\text{win in a row})
\end{aligned}
$$

$$ 
\begin{aligned}
\text{<=>} (3,965.28 + 3,965.28 \times 0.2) / (1/216)
\end{aligned}
$$

$$ 
\begin{aligned}
\approx 1,028,236.57\quad(USD)
\end{aligned}
$$

#### b, Relevant premium per sequences
$$ 
\begin{aligned}
\text{Relevant premium per sequences} &= (\text{Premium per Sequence} \times \text{Expected cost per toss})
\end{aligned}
$$
$$ 
\begin{aligned}
\text{=} 20\% \times 3,965.28
\end{aligned}
$$
$$ 
\begin{aligned}
\text{=} 793.06\quad(USD)
\end{aligned}
$$

#### c, Relevant premium per tosses
$$ 
\begin{aligned}
\text{Relevant premium per tosses} &= \text{Relevant premium per sequences}/3
\end{aligned}
$$
$$ 
\begin{aligned}
\text{=} 793.06/3
\end{aligned}
$$
$$ 
\begin{aligned}
\approx 264.35\quad(USD)
\end{aligned}
$$

#### 3. Code python
```python
import math

# Step 3: Calculate minimum prize money
def calculate_minimum_prize_money(premium_margin=0.2):
    P_win = 1/216
    expected_cost = 3965.28
    
    # Add premium to expected cost
    premium_per_sequence = premium_margin * expected_cost
    minimum_prize_money = (expected_cost + premium_per_sequence) / P_win
    return minimum_prize_money, premium_per_sequence

# Main function
def main():
    # Calculate expected cost
    expected_cost = 3965.28
    print(f"Expected Cost per Sequence: ${expected_cost:.2f}")
    
    # Calculate minimum prize money
    premium_margin = 0.2  # 20% profit margin
    minimum_prize_money, premium_per_sequence = calculate_minimum_prize_money(premium_margin)
    print(f"Minimum Prize Money (20% profit margin): ${minimum_prize_money:.2f}")
    print(f"Relevant Premium per Sequence: ${premium_per_sequence:.2f}")
    print(f"Relevant Premium per Toss: ${premium_per_sequence / 3:.2f}")

# Run the program
if __name__ == "__main__":
    main()
```
### Q4: Please rationalise the most efficient strategy to maximise gain/minimise loss with 4 players playing this game simultaneously.

#### Background

***Objective***: increase netgain (Expected reward per toss) & degree loss per toss (Expected cost per toss)
- Number of player: 4
- Price: 100,000 USD.
- Condition: toss “1” three times in a row (1,1,1).
- However, you don’t have to pay if you stop playing.
  
***Probability of success/fail per sequences:***
- P(win in a row) = 1/216
- P(fail in a row) = 215/216

***Cost:***
- cost_fail_1_times = 1,000 (USD)
- cost_fail_3_times = 3 * 1,000 (USD)
- cost_success_1_then_fail = 7,800 (USD)
- cost_success_2_then_fail = 49,500 (USD)

#### 1. Define key metrics:
- n: numbers of dive roll (toss)
- m = n/3: numbers of sequences dive roll
- N: number of player
- Expected reward per sequences 
- Expected cost per sequences 3,965.28

#### 2. Analyst:
#### a, Probability of at least one winner

$$\ P(\text{at least 1 win})  = 1 - P(\text{fail})^N $$
$$\ \text{<=>} P(\text{at least 1 win})  = 1 - (215/216)^4 $$
$$ \approx 0.01843 \quad (1.84\%) $$

#### b, Expected cost
- Base on Q2: Expected cost per sequences (for 1 player) = 3,965.28 (USD)
=> With N = 4

$$\ \text{Expected cost per sequences (for 4 player)}  = \text{Expected cost per sequences (for 1 player)} \times 4 $$
$$ = 3,965.28 \times 4 $$
$$ = 15,861 \quad (USD) $$

#### c, Expected profit
$$\ \text{Expected profit}  = P(\text{at least 1 win}) \times \text{Price} - \text{Expected cost per sequences (for 4 player)} $$
$$ \text{<=>} 0.01843 \times 100,000 - 15,861 $$
$$ = -14,018 \quad (USD) $$

#### 3. Optimized Strategies

| Strategy          | Implementation                                                                 | Benefits                                                     |
|-------------------|-------------------------------------------------------------------------------|--------------------------------------------------------------|
| **1. Share Results** | Players share their dice rolls and stop when one player is close to winning.    | Reduces the total number of sequences, lowering overall costs. |
| **2. Risk Distribution** | Players take turns attempting sequences instead of all playing simultaneously. | Prevents any single player from bearing all the costs, spreads risk evenly. |
| **3. Share Rewards**  | The 100,000 USD prize is shared equally or proportionally among the players.  | Reduces losses for those who don't win, ensures collective benefit. |
| **4. Stop Early**     | Each player sets a maximum loss threshold (e.g., 10,000 USD) and stops playing once it’s reached. | Reduces individual and group losses.                         |

#### 4. Simulate Strategies

- By coordinating, the number of required sequences is reduced from: N=4 --> N=2
- $$\ \text{Expected cost per sequences (for 2 player)}  = \text{Expected cost per sequences (for 1 player)} \times 2 $$
$$ = 3,965.28 \times 2 $$
$$ = 7,930 \quad (USD) $$
- $$\ \text{Expected profit}  = P(\text{at least 1 win}) \times \text{Price} - \text{Expected cost per sequences (for 2 player)} $$
$$ \text{<=>} 0.01843 \times 100,000 - 7,930 $$
$$ = −6,087 \quad (USD) $$

#### 5. Comparison Before and After Simulate

| Metric                  | Before Strategies     | After Strategies      |
|-------------------------|-----------------------|-----------------------|
| **Total Expected Cost**  | 15,861.12 USD         | 7,930.56 USD          |
| **Expected Profit**      | -14,018.12 USD        | -6,087.56 USD         |
| **Probability at least 1 win** | 1.843%               | 1.843%                |

#### => Conclusion
##### Optimizing through collaboration:

- **Reduces total costs** by sharing progress and taking turns.
- **Distributes risks** among players.
- **Rewards are shared**, ensuring fairer outcomes for the group.

##### Key benefits of strategies:

- Total expected loss decreases from **−14,018.12 USD** to **−6,087.56 USD**.
- Reduces individual exposure to high costs.

##### Recommendation:

- **Apply collaborative strategies** to significantly reduce losses and improve group outcomes, even though the expected profit remains negative.

### Q5: Will you pay, and why?
#### Background
- Objective: Roll "11", "16", or "66" three times in a row to win 3,000,000 USD
- Cost of Failure: cost of 1,000 USD for each failed toss
- Destroying a Die: On the third successful toss, you can pay 1,000,000 USD  to destroy one die, then only need to roll a "1" on the remaining die to win.
- Reward: Winning earns you US$ 3,000,000.
- Stopping: You can always stop at any point without incurring further costs.

#### 1. Calculate key metrics
##### Probability of rolling “11”, “16”, or “66” with two dice:
- Each die has 6 faces, so there are 36 possible outcomes for two dice. The favorable outcomes are:
- “11” = (1, 1) → 1 outcome
- “16” = (1, 6) and (6, 1) → 2 outcomes
- “66” = (6, 6) → 1 outcome
  
=> Total favorable outcomes = 4.
$$ P(win) = 4/36 = 1/9 ≈ 0.1111 (11.11\%) $$

##### Probability of rolling “1” on one die (if you destroy a die):
$$ P(win) = 1/6 ≈ 0.1667 (16.67\%) $$

##### Expected Value (EV):
$$ EV = P(win) × Prize − Destruction Cost $$
- No destruction (2 dice):
$$ EV = (1/9) × 3,000,000 − 0 = 333,333.33 \quad (USD)$$
- Destruction (1 dice):
$$ EV = (1/6) × 3,000,000 − 1,000,000 = 500,000 − 1,000,000 = - 500,000 \quad (USD) $$ 

#### 2. Summary

| **Scenario**                  | **Win Probability** | **Expected Value (EV)**           | **Destruction Cost**  | **Conclusion**          |
|-------------------------------|---------------------|------------------------------------|-----------------------|--------------------------|
| **No destruction (2 dice)**   | **11.11% (1/9)**    | **333,333.33 USD**                | **0 USD**             | **Better, Recommended** |
| **Destruction (1 dice)**       | **16.67% (1/6)**    | **-500,000 USD**               | **1,000,000 USD**     | **Not Recommended**     |

#### Conclusion
##### No destruction (2 dice):

This option has a higher expected value (333,333.33 USD) and no additional cost, making it the better choice.

##### Destruction (1 dice):

Although the win probability increases from 11.11% to 16.67%, the destruction cost of 1,000,000 USD results in a negative expected value (-500,000 USD), making it not financially viable.

==> ***Should not Pay to destroy one dice***
- ***Positive EV without destruction:***
Keeping 2 dice gives a positive expected value of US$ 333,333.33,
which is better than the negative EV from destroying a die.

- ***High Destruction Cost:***
Paying US$ 1,000,000 to slightly increase the win probability does not justify the cost, as it results in an overall loss.

- ***Financial Efficiency:***
Continuing with 2 dice is more cost-effective and provides a better balance between risks and rewards compared to destroying 1 die.

