# Cogito v2 & v2.1

<div align="center">
  <picture>
      <img src="figures/deep-cogito-logo.png" width="40%" alt="DeepCogito: General SuperIntelligence">
  </picture>
</div>

* Chat: [chat.deepcogito.com](https://chat.deepcogito.com)
* Technical Blog: [deepcogito.com/research/cogito-v2-1](https://www.deepcogito.com/research/cogito-v2-1)
* Website: [deepcogito.com](https://www.deepcogito.com)

<hr>

## Model Introduction

### Cogito v2.1

Cogito v2.1 is a state-of-the-art mixture-of-experts (MoE) language model with 671 billion total parameters and 37 billion activated parameters. It achieves exceptional performance across frontier knowledge, reasoning, and coding and math tasks.

Key Features:
* **Large-Scale Post-Training**: Built a 671B parameter model with extensive post-training, creating one of the most capable open-source foundation model worldwide.
* **Hybrid Reasoning**: Model can answer directly (standard LLM), or self-reflect before answering (like reasoning models). Optimized for coding, STEM, instruction following and general helpfulness, and have significantly higher multilingual, coding and tool calling capabilities than size equivalent counterparts.
* **Token-Efficient Reasoning:**: Model achieves superior performance with significantly fewer tokens than comparable models.
* This model is trained in over 30 languages and supports a context length of 128k.

<div align="center">
  <img src="figures/v2-1-benchmark-1.png" width="80%" alt="v2-1-benchmark-1">
  <img src="figures/v2-1-benchmark-2.png" width="80%" alt="v2-1-benchmark-2">
  <img src="figures/v2-1-benchmark-3.png" width="80%" alt="v2-1-benchmark-3">
</div>

### Cogito v2

The Cogito v2 series introduces four foundation models designed to advance reasoning capabilities in large language models. The series includes two mid-sized models (70B dense and 109B MoE) and two large-scale models (405B dense and 671B MoE), each offering both standard response generation and self-reflective reasoning modes.

Cogito v2 70B, 109B, and 405B have been optimized to enhance their non-reasoning mode performance. These models represent substantial improvements over their base Llama architecture counterparts, delivering performance that approaches frontier intelligence within their respective size classes.

The flagship 671B MoE model implements a novel training approach that simultaneously enhances both reasoning and non-reasoning capabilities. Unlike conventional methods that rely solely on final-output signals, our approach provides guidance throughout the reasoning process itself. This results in more efficient reasoning chains that are approximately 60% shorter than comparable models like DeepSeek R1 0528, while maintaining or exceeding performance benchmarks.

<div align="center">
  <img src="figures/V2-671B-Benchmarks-Reasoning.webp" width="49%" alt="V2 671B Reasoning">
  <img src="figures/V2-671B-Benchmarks-Non-Reasoning.webp" width="49%" alt="V2 671B Non-Reasoning">
  <br>  
  <img src="figures/V2-405B-Benchmarks.webp" width="32%" alt="405B">
  <img src="figures/V2-109B-Benchmarks.webp" width="32%" alt="109B">
  <img src="figures/V2-70B-Benchmarks.webp" width="32%" alt="70B">
</div>

For more eval results, show cases, and technical details, please visit our [technical blog](https://www.deepcogito.com/research/cogito-v2-preview).

## Model Availability

| Model            | Hugging Face     | Inference        |
|------------------|------------------|------------------|
| Cogito v2.1 671B | [BF16](https://huggingface.co/deepcogito/cogito-671b-v2.1), [FP8](https://huggingface.co/deepcogito/cogito-671b-v2.1-FP8) |  OpenRouter, [TogetherAI](https://api.together.ai/models/deepcogito/cogito-v2-1-671b)|
| Cogito v2 671B   | [BF16](https://huggingface.co/deepcogito/cogito-v2-preview-deepseek-671B-MoE), [FP8](https://huggingface.co/deepcogito/cogito-v2-preview-deepseek-671B-MoE-FP8) | [OpenRouter](https://openrouter.ai/deepcogito/cogito-v2-preview-deepseek-671b), [TogetherAI](https://api.together.ai/models/deepcogito/cogito-v2-preview-deepseek-671b) |
| Cogito v2 405B   | [BF16](https://huggingface.co/deepcogito/cogito-v2-preview-llama-405B) | [OpenRouter](https://openrouter.ai/deepcogito/cogito-v2-preview-llama-405b), [TogetherAI](https://api.together.ai/models/deepcogito/cogito-v2-preview-llama-405B) |
| Cogito v2 109B   | [BF16](https://huggingface.co/deepcogito/cogito-v2-preview-llama-109B-MoE) | [OpenRouter](https://openrouter.ai/deepcogito/cogito-v2-preview-llama-109b-moe), [TogetherAI](https://api.together.ai/models/deepcogito/cogito-v2-preview-llama-109B-MoE) |
| Cogito v2 70B    | [BF16](https://huggingface.co/deepcogito/cogito-v2-preview-llama-70B) | [OpenRouter](https://openrouter.ai/deepcogito/cogito-v2-preview-llama-70b), [TogetherAI](https://api.together.ai/models/deepcogito/cogito-v2-preview-llama-70B) |

*The FP8 variant is quantized from the bfloat16 checkpoint to optimize deployment efficiency, requiring only ~671GB of memory and deployable on 8× H200 GPUs. For comparison, the full bfloat16 version requires 8× B200 or 16× H200 GPUs.*

## Examples

The Cogito v2 & v2.1 series models are accessible through OpenAI-compatible API endpoints.

```python
import os
from openai import OpenAI

client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key=os.environ.get("OPENROUTER_API_KEY"),
)
# If you want to use TogetherAI, specify `base_url="https://api.together.xyz/v1"` and `api_key=os.environ.get("TOGETHER_API_KEY")`.

def show_generation(prompt, system=None, enable_thinking=True):
    global client
    if system is not None:
        messages = [
            {"role": "system", "content": system},
            {"role": "user", "content": prompt},
        ]
    else:
        messages = [{"role": "user", "content": prompt}]
    response = client.chat.completions.create(
        model="deepcogito/cogito-v2-preview-deepseek-671b",
        messages=messages,
        extra_body={"chat_template_kwargs": {"enable_thinking": enable_thinking}},
        max_tokens=8192,
        temperature=0.6,
        top_p=0.95,
    )
    print(response.choices[0].message.content)
```

> The recommended temperature for Cogito v2 is `temperature = 0.6`.
> If no special instructions are required, the system prompt is a good default.

### Math capabilities Examples

Let’s test the mathematical capability of the model in this section.

```python
show_generation("Solve for x: 3x + 10 = 5(x - 2)")
```

<details>
<summary>Click to see response from model</summary>
<think>
Okay, let's solve the equation 3x + 10 = 5(x - 2) step by step. First, I need to simplify both sides of the equation. On the right side, there's a distributive property to handle: 5 multiplied by (x - 2). So, 5 times x is 5x, and 5 times -2 is -10. So the equation becomes 3x + 10 = 5x - 10.

Now, I want to get all the x terms on one side and the constants on the other. Let me subtract 3x from both sides to move the x terms to the right. That gives me 10 = 2x - 10.

Next, I'll add 10 to both sides to isolate the term with x. So, 10 + 10 = 2x, which simplifies to 20 = 2x.

Finally, to solve for x, I divide both sides by 2. So, x = 10.

Let me check my work. Plugging x = 10 back into the original equation: 3(10) + 10 = 30 + 10 = 40. And 5(10 - 2) = 5(8) = 40. Both sides equal 40, so the solution is correct.
</think>
Let's solve the equation step by step:

1) Start with the original equation:
   3x + 10 = 5(x - 2)

2) Distribute the 5 on the right side:
   3x + 10 = 5x - 10

3) Subtract 3x from both sides to get x terms on one side:
   10 = 2x - 10

4) Add 10 to both sides to isolate the x term:
   20 = 2x

5) Divide both sides by 2:
   x = 10

**Verification:**
- Left side: 3(10) + 10 = 30 + 10 = 40
- Right side: 5(10 - 2) = 5(8) = 40

Both sides are equal, so x = 10 is the correct solution.

**Answer:** x = 10

---
</details>

Let’s test the model on an IMO-level problem (source: page 174 of the [linked book](https://hengsokha.wordpress.com/wp-content/uploads/2011/06/imo-problem-solution-1959-2009.pdf))

```python
show_generation("Determine all pairs (x, y) of integers such that 1 + 2^x + 2^(2x+1) = y^2.")
```

<details>
<summary>Click to see response from model</summary>
<think>
I need to find all integer pairs (x, y) such that 1 + 2^x + 2^{2x+1} = y^2.

First, let's simplify the left side a bit. Notice that 2^{2x+1} = 2 \cdot 2^{2x} = 2 \cdot (2^x)^2. So, let me set a = 2^x, which is a positive integer since x is an integer (and 2^x > 0 for all integer x). Then the equation becomes:

1 + a + 2a^2 = y^2

So, y^2 = 2a^2 + a + 1

Now, I need to find integer solutions (a, y) to y^2 = 2a^2 + a + 1, where a is a power of 2 (since a = 2^x).

This is a Diophantine equation. Let me see if I can solve it.

I can rearrange it as: y^2 - 2a^2 = a + 1

This looks a bit like a Pell-like equation, but with an extra linear term in a.

Alternatively, maybe I can complete the square or something.

Let me write it as: y^2 = 2a^2 + a + 1

Multiply both sides by 8 to eliminate the fraction when completing the square:

8y^2 = 16a^2 + 8a + 8

Now, 16a^2 + 8a + 1 = (4a + 1)^2, so:

8y^2 = (4a + 1)^2 + 7

So, 8y^2 - (4a + 1)^2 = 7

Let me set u = 4a + 1, then:

8y^2 - u^2 = 7

This is a Pell-type equation: u^2 - 8y^2 = -7

I need to find integer solutions to u^2 - 8y^2 = -7.

This is a negative Pell equation. Let me recall that the fundamental solution to x^2 - 8y^2 = 1 is (3,1) since 3^2 - 8*1^2 = 9 - 8 = 1.

Now, I need to find solutions to u^2 - 8y^2 = -7.

Let me try small values of y:

For y = 0: u^2 = -7, no solution.
For y = 1: u^2 = 8 - 7 = 1, so u = ±1.
For y = 2: u^2 = 32 - 7 = 25, so u = ±5.
For y = 3: u^2 = 72 - 7 = 65, not a perfect square.
For y = 4: u^2 = 128 - 7 = 121, so u = ±11.
For y = 5: u^2 = 200 - 7 = 193, not a perfect square.
For y = 6: u^2 = 288 - 7 = 281, not a perfect square.
For y = 7: u^2 = 392 - 7 = 385, not a perfect square.
For y = 8: u^2 = 512 - 7 = 505, not a perfect square.
For y = 9: u^2 = 648 - 7 = 641, not a perfect square.
For y = 10: u^2 = 800 - 7 = 793, not a perfect square.

I see some solutions: (u,y) = (1,1), (-1,1), (5,2), (-5,2), (11,4), (-11,4), etc.

Now, recall that u = 4a + 1, so a = (u - 1)/4.

Since a must be an integer, (u - 1) must be divisible by 4.

Let's check the solutions we found:

For (u,y) = (1,1): a = (1-1)/4 = 0, but a = 2^x > 0, so not valid.
For (u,y) = (-1,1): a = (-1-1)/4 = -0.5, not integer.
For (u,y) = (5,2): a = (5-1)/4 = 1, which is 2^0, so x = 0.
For (u,y) = (-5,2): a = (-5-1)/4 = -1.5, not integer.
For (u,y) = (11,4): a = (11-1)/4 = 2.5, not integer.
For (u,y) = (-11,4): a = (-11-1)/4 = -3, negative, not valid.

Hmm, so far only (u,y) = (5,2) gives a valid a=1, which corresponds to x=0.

But maybe there are more solutions. The equation u^2 - 8y^2 = -7 can have more solutions generated by the fundamental solution of the Pell equation x^2 - 8y^2 = 1, which is (3,1).

The general solution can be found by multiplying by the fundamental solution of the Pell equation.

Given a solution (u_n, y_n), the next solution can be generated by:
u_{n+1} + y_{n+1}√8 = (u_n + y_n√8)(3 + √8)

So, starting with (u_1, y_1) = (1,1):
u_1 + y_1√8 = 1 + √8

Next solution:
(1 + √8)(3 + √8) = 3 + √8 + 3√8 + 8 = 11 + 4√8
So, (u_2, y_2) = (11,4)

Next:
(11 + 4√8)(3 + √8) = 33 + 11√8 + 12√8 + 32 = 65 + 23√8
So, (u_3, y_3) = (65,23)

Next:
(65 + 23√8)(3 + √8) = 195 + 65√8 + 69√8 + 184 = 379 + 134√8
So, (u_4, y_4) = (379,134)

And so on.

We also have the solution (u,y) = (5,2). Let's generate more solutions from this:

(5 + 2√8)(3 + √8) = 15 + 5√8 + 6√8 + 16 = 31 + 11√8
So, (u,y) = (31,11)

Next:
(31 + 11√8)(3 + √8) = 93 + 31√8 + 33√8 + 88 = 181 + 64√8
So, (u,y) = (181,64)

Next:
(181 + 64√8)(3 + √8) = 543 + 181√8 + 192√8 + 512 = 1055 + 373√8
So, (u,y) = (1055,373)

And so on.

Now, we need to check which of these give integer a = (u - 1)/4, and then check if a is a power of 2.

Let's check the solutions we have:

From the first sequence (starting with (1,1)):
- (1,1): a = (1-1)/4 = 0, not positive.
- (11,4): a = (11-1)/4 = 10/4 = 2.5, not integer.
- (65,23): a = (65-1)/4 = 64/4 = 16, which is 2^4, so x=4.
- (379,134): a = (379-1)/4 = 378/4 = 94.5, not integer.

From the second sequence (starting with (5,2)):
- (5,2): a = (5-1)/4 = 1, which is 2^0, so x=0.
- (31,11): a = (31-1)/4 = 30/4 = 7.5, not integer.
- (181,64): a = (181-1)/4 = 180/4 = 45, not a power of 2.
- (1055,373): a = (1055-1)/4 = 1054/4 = 263.5, not integer.

We also have negative u values, but those would give negative a, which is not possible since a = 2^x > 0.

So far, we have two solutions:
1. (u,y) = (5,2) → a=1 → x=0, y=2
2. (u,y) = (65,23) → a=16 → x=4, y=23

Let me verify these:

For x=0:
1 + 2^0 + 2^{2*0+1} = 1 + 1 + 2^1 = 1 + 1 + 2 = 4 = 2^2, so y=2 or y=-2. But since we're looking for y^2, both are valid, but typically we consider positive y.

For x=4:
1 + 2^4 + 2^{8+1} = 1 + 16 + 2^9 = 1 + 16 + 512 = 529 = 23^2, so y=23 or y=-23.

Are there more solutions? Let's generate the next term in the first sequence:

From (65,23), the next term is:
(65 + 23√8)(3 + √8) = 65*3 + 65√8 + 69√8 + 23*8 = 195 + 134√8 + 184 = 379 + 134√8, so (379,134)

a = (379-1)/4 = 378/4 = 94.5, not integer.

Next term:
(379 + 134√8)(3 + √8) = 379*3 + 379√8 + 402√8 + 134*8 = 1137 + 781√8 + 1072 = 2209 + 781√8, so (2209,781)

a = (2209-1)/4 = 2208/4 = 552, which is not a power of 2 (since 512=2^9, 1024=2^10).

In the second sequence, the next term after (1055,373) is:
(1055 + 373√8)(3 + √8) = 1055*3 + 1055√8 + 1119√8 + 373*8 = 3165 + 2174√8 + 2984 = 6149 + 2174√8, so (6149,2174)

a = (6149-1)/4 = 6148/4 = 1537, not a power of 2.

It seems that as we go further, a gets larger and is unlikely to be a power of 2. But to be thorough, let's check if there are any more small solutions we might have missed.

We can also check negative x values. If x is negative, say x = -k where k > 0, then 2^x = 1/2^k, which is not an integer, but y^2 is an integer, so the left side must be an integer. However, 1 + 2^{-k} + 2^{-2k+1} is not an integer for k > 0, because 2^{-k} is a fraction with denominator 2^k, and the other terms are integers. So, x must be non-negative.

We already checked x=0 and x=4. Let's check x=1,2,3,5, etc.

For x=0: we have y^2 = 1 + 1 + 2 = 4, so y=±2.
For x=1: y^2 = 1 + 2 + 2^{3} = 1 + 2 + 8 = 11, not a perfect square.
For x=2: y^2 = 1 + 4 + 2^{5} = 1 + 4 + 32 = 37, not a perfect square.
For x=3: y^2 = 1 + 8 + 2^{7} = 1 + 8 + 128 = 137, not a perfect square.
For x=4: y^2 = 1 + 16 + 512 = 529 = 23^2, so y=±23.
For x=5: y^2 = 1 + 32 + 2^{11} = 1 + 32 + 2048 = 2081, which is not a perfect square (45^2=2025, 46^2=2116).
For x=6: y^2 = 1 + 64 + 2^{13} = 1 + 64 + 8192 = 8257, not a perfect square (90^2=8100, 91^2=8281).
For x=7: y^2 = 1 + 128 + 2^{15} = 1 + 128 + 32768 = 32897, not a perfect square (181^2=32761, 182^2=33124).
For x=8: y^2 = 1 + 256 + 2^{17} = 1 + 256 + 131072 = 131329, not a perfect square (362^2=131044, 363^2=131769).

It seems that only x=0 and x=4 work for small values. Given that the next solutions in the Pell equation give non-powers of 2 for a, it's likely that these are the only solutions.

Therefore, the integer pairs (x, y) are (0, 2), (0, -2), (4, 23), and (4, -23).

But let me confirm if there are any other solutions from the Pell equation approach that we might have missed. We considered the sequences starting from (1,1) and (5,2). Are there any other fundamental solutions to u^2 - 8y^2 = -7?

The equation u^2 - 8y^2 = -7. Let's see if there are solutions with y=3: u^2 = 8*9 -7 = 72-7=65, not a square. y=4: u^2=128-7=121, u=±11, which we had. y=5: u^2=200-7=193, not a square. y=6: u^2=288-7=281, not a square. y=7: u^2=392-7=385, not a square. y=8: u^2=512-7=505, not a square. y=9: u^2=648-7=641, not a square. y=10: u^2=800-7=793, not a square. So no other small solutions.

Moreover, all solutions are generated by the fundamental solutions (1,1) and (5,2) multiplied by powers of the fundamental unit (3+√8). We've checked the first few and only got a=1 and a=16 as powers of 2. The next terms give larger a that are not powers of 2, and this pattern is likely to continue.

Therefore, the only integer solutions are x=0 and x=4, with y=±2 and y=±23 respectively.
</think>
Let's solve the equation \(1 + 2^x + 2^{2x+1} = y^2\) for integer pairs \((x, y)\).

1. **Simplify the equation:**
   Let \(a = 2^x\) (so \(a > 0\) is an integer power of 2). The equation becomes:
   \[
   y^2 = 1 + a + 2a^2
   \]

2. **Rearrange and complete the square:**
   Multiply both sides by 8:
   \[
   8y^2 = 16a^2 + 8a + 8
   \]
   \[
   8y^2 = (4a + 1)^2 + 7
   \]
   \[
   (4a + 1)^2 - 8y^2 = -7
   \]
   Let \(u = 4a + 1\), then:
   \[
   u^2 - 8y^2 = -7
   \]

3. **Solve the Pell-type equation:**
   We find integer solutions \((u, y)\) to \(u^2 - 8y^2 = -7\):
   - Small solutions: \((u, y) = (\pm1, \pm1), (\pm5, \pm2), (\pm11, \pm4), \ldots\)
   - General solutions are generated from the fundamental solutions \((1,1)\) and \((5,2)\) using the fundamental unit \(3 + \sqrt{8}\).

4. **Find valid \(a = 2^x\):**
   Since \(u = 4a + 1\), we have \(a = \frac{u - 1}{4}\).
   - For \((u, y) = (5, 2)\): \(a = \frac{5 - 1}{4} = 1 = 2^0\), so \(x = 0\).
   - For \((u, y) = (65, 23)\): \(a = \frac{65 - 1}{4} = 16 = 2^4\), so \(x = 4\).
   - Other solutions yield non-integer or non-power-of-2 values for \(a\).

5. **Verify solutions:**
   - For \(x = 0\): \(1 + 2^0 + 2^{1} = 1 + 1 + 2 = 4 = (\pm2)^2\), so \(y = \pm2\).
   - For \(x = 4\): \(1 + 2^4 + 2^{9} = 1 + 16 + 512 = 529 = (\pm23)^2\), so \(y = \pm23\).

6. **Check other values:**
   Testing small negative and positive \(x\) values confirms no other solutions exist, as the left side either becomes non-integer (for \(x < 0\)) or grows too quickly to be a perfect square (for \(x > 4\)).

**Final Answer:**
The integer pairs \((x, y)\) that satisfy the equation are:
\[
\boxed{(0, 2)}, \boxed{(0, -2)}, \boxed{(4, 23)}, \boxed{(4, -23)}
\]

---
</details>

Great — the model handles high-level olympiad problems impressively.

### Coding Examples

Let’s start by checking how well the model explains and constructs a Python function.

```python
show_generation("Write a Python function to find the nth Fibonacci number")
```

<details>
<summary>Click to see response from model</summary>
<think>
The user is asking for a Python function to find the nth Fibonacci number. The Fibonacci sequence is a series of numbers where each number is the sum of the two preceding ones, usually starting with 0 and 1. So the sequence goes: 0, 1, 1, 2, 3, 5, 8, 13, 21, and so on.

There are several ways to compute the nth Fibonacci number in Python:

1. Recursive approach: This is straightforward but inefficient for large n due to repeated calculations.
2. Iterative approach: More efficient, uses a loop to compute the sequence up to n.
3. Dynamic programming: Similar to iterative but stores all previous values (memoization).
4. Using mathematical formula: Binet's formula, which uses the golden ratio. This is fast but can have floating-point inaccuracies for large n.
5. Using matrix exponentiation: Efficient for very large n.

For this response, I'll provide the iterative approach as it's efficient and easy to understand. I'll also mention the recursive approach and its limitations.

The function should:
- Take an integer n as input.
- Return the nth Fibonacci number.
- Handle edge cases like n=0 or n=1.
- Consider negative n (though Fibonacci sequence is typically defined for non-negative integers).

I'll write a function that uses iteration to compute the Fibonacci number efficiently.
</think>
Here's a Python function to find the nth Fibonacci number using an iterative approach, which is efficient and easy to understand:

```python
def fibonacci(n):
    """
    Return the nth Fibonacci number.
    
    Args:
        n (int): The position in the Fibonacci sequence (non-negative integer)
        
    Returns:
        int: The nth Fibonacci number
        
    Raises:
        ValueError: If n is negative
    """
    if n < 0:
        raise ValueError("n must be a non-negative integer")
    if n == 0:
        return 0
    elif n == 1:
        return 1
        
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b

# Example usage:
# print(fibonacci(0))  # Output: 0
# print(fibonacci(1))  # Output: 1
# print(fibonacci(5))  # Output: 5
# print(fibonacci(10)) # Output: 55
```

This implementation:
1. Handles the base cases (n=0 and n=1) directly
2. Uses iteration to calculate Fibonacci numbers efficiently in O(n) time and O(1) space
3. Includes input validation for negative numbers

For reference, here's what the recursive version would look like (not recommended for large n due to exponential time complexity):

```python
def fibonacci_recursive(n):
    if n < 0:
        raise ValueError("n must be a non-negative integer")
    if n <= 1:
        return n
    return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)
```

The iterative version is generally preferred because it's much more efficient for larger values of n.

---
</details>

Great — the model provides a clear, fully reasoned solution with correct, executable code. Now let’s ask the model to return only clean, ready-to-run Python code without extra explanation.

```python
show_generation("Write a Python function to find the nth Fibonacci number.", system="Give the executable code directly which doesn't need any kinda parsing.")
```

<details>
<summary>Click to see response from model</summary>
<think>
To write a Python function that finds the nth Fibonacci number, I need to recall that the Fibonacci sequence is a series of numbers where each number is the sum of the two preceding ones, usually starting with 0 and 1. So the sequence goes: 0, 1, 1, 2, 3, 5, 8, 13, 21, and so on.

The function should take an integer n as input and return the nth Fibonacci number. I should consider different approaches:

1. **Recursive Approach**: This is straightforward but inefficient for large n due to repeated calculations.
2. **Iterative Approach**: This is efficient and uses O(n) time and O(1) space.
3. **Dynamic Programming**: This can be efficient with memoization, but the iterative approach is simpler for this case.
4. **Using Binet's Formula**: This is a mathematical approach that provides an O(1) time solution, but it might have floating-point precision issues for large n.

For the purpose of this task, I'll go with the iterative approach because it's efficient and easy to understand. I'll handle edge cases like n = 0 or n = 1 directly.

The iterative approach:
- Initialize the first two Fibonacci numbers: a = 0, b = 1.
- If n is 0, return a.
- If n is 1, return b.
- For i from 2 to n:
  - c = a + b
  - a = b
  - b = c
- Return b after the loop.

I'll also consider that n should be a non-negative integer. If n is negative, I'll raise a ValueError.

Let me write the code accordingly.
</think>
```python
def fibonacci(n):
    """
    Calculate the nth Fibonacci number.
    
    Args:
        n (int): The position in the Fibonacci sequence (non-negative integer).
        
    Returns:
        int: The nth Fibonacci number.
        
    Raises:
        ValueError: If n is a negative integer.
    """
    if not isinstance(n, int) or n < 0:
        raise ValueError("n must be a non-negative integer")
    
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a
```

---
</details>

Perfect — the model outputs neatly formatted, directly usable code. Next, let’s test whether the model can solve hard problem from LeetCode.

```python
show_generation("The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value, and the median is the mean of the two middle values.\n\nFor example, for arr = [2,3,4], the median is 3.\nFor example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.\nImplement the MedianFinder class:\n\nMedianFinder() initializes the MedianFinder object.\nvoid addNum(int num) adds the integer num from the data stream to the data structure.\ndouble findMedian() returns the median of all elements so far. Answers within 10-5 of the actual answer will be accepted.\nConstraints:\n\n-105 <= num <= 105\nThere will be at least one element in the data structure before calling findMedian.\nAt most 5 * 104 calls will be made to addNum and findMedian.")
```

<details>
<summary>Click to see response from model</summary>
<think>
Okay, so I need to design a data structure that can efficiently add numbers and find the median. The median is the middle value when the numbers are sorted. If there's an even number of elements, it's the average of the two middle ones.

First thought: every time I add a number, I could keep the list sorted. Then finding the median would be straightforward. But inserting into a sorted list takes O(n) time because I might have to shift elements. Since there could be up to 50,000 calls, O(n) per insertion might be too slow because 50,000^2 is 2.5e9 operations, which is too much.

So I need a more efficient way. The problem is all about maintaining the middle elements efficiently. I remember that heaps can be useful for finding max or min quickly. Specifically, using two heaps: a max-heap for the lower half and a min-heap for the upper half. The max-heap will store the smaller half of the numbers, and the min-heap will store the larger half. Then the top of the max-heap is the largest in the lower half, and the top of the min-heap is the smallest in the upper half. The median would then be either the top of the max-heap (if the total count is odd) or the average of the tops of both heaps (if even).

But how do I maintain the balance between the two heaps? The idea is to ensure that the size of the max-heap is either equal to the min-heap or one more. That way, the median is easy to get.

Let me outline the steps:

1. Initialize two heaps:
   - A max-heap for the lower half. In Python, the heapq module only implements min-heap, so to simulate a max-heap, we can store negative values.
   - A min-heap for the upper half.

2. For each number added:
   - If the max-heap is empty or the number is less than or equal to the top of the max-heap, add it to the max-heap. Otherwise, add it to the min-heap.
   - Then, we need to balance the heaps. The max-heap should have at most one more element than the min-heap. Similarly, the min-heap should not have more elements than the max-heap.
   - So after adding, if the max-heap has more than one element than the min-heap, pop the top of the max-heap and push it to the min-heap.
   - Or if the min-heap has more elements than the max-heap, pop from the min-heap and push to the max-heap.

Wait, but when we push to the min-heap, we need to make sure that the min-heap's top is not smaller than the max-heap's top. Actually, the invariant should be that all elements in the max-heap are less than or equal to all elements in the min-heap. So when we add a number, we can first add it to one of the heaps, then if necessary, move the top to the other heap to maintain the size condition and the value condition.

Alternative approach:

- Always push the new number into the max-heap (but as a negative since it's a min-heap in Python).
- Then, pop the top of the max-heap and push it to the min-heap. This ensures that the max-heap's top is now in the min-heap.
- But then, if the min-heap has more elements than the max-heap, pop the top of the min-heap and push it to the max-heap. This step ensures that the max-heap is always at least as big as the min-heap.

This way, after these steps, the max-heap will contain the smaller half and the min-heap the larger half, and the max-heap can have at most one more element than the min-heap.

Let me test this with an example:

Adding 1:
- Push to max-heap: max_heap = [-1]
- Pop from max-heap (which is -1, so 1) and push to min-heap: min_heap = [1]
- Now, since min_heap has 1 element and max_heap has 0, we need to balance: pop min_heap (1) and push to max_heap: max_heap = [-1], min_heap = [].

Now, adding 2:
- Push to max-heap: max_heap = [-2, -1] (but heap structure, so it's [-2,-1] but after heapify, it's [-2,-1]? Wait, no: when you push -2, the heap becomes [-2,-1], but the top is -2 which represents 2. So the max-heap now has the larger element on top? Wait, no: the max-heap is implemented with negatives, so the smallest negative is the largest number. So the top of the max-heap is the maximum of the lower half.

Wait, let's redo:

After adding 1 and balancing:
max_heap = [-1], min_heap = [].

Now add 2:
- Push to max_heap: heappush(max_heap, -2) → max_heap = [-2, -1] (but heap order is maintained, so the smallest negative is first, which is -2, representing 2, which is larger than 1. But that's not correct because the max-heap should have the smaller half. So this approach might be flawed.

Wait, maybe I should reverse the order. Let me think again.

Standard approach:

We maintain two heaps:
- A max-heap for the lower half (so we can get the largest in the lower half quickly)
- A min-heap for the upper half (so we can get the smallest in the upper half quickly)

We want to ensure that:
1. The size of the max-heap is either equal to the min-heap or one more.
2. Every element in the max-heap is less than or equal to every element in the min-heap.

So when adding a number:
1. Add the number to the max-heap. Since Python's heapq is a min-heap, we can store negatives to simulate a max-heap.
2. Then, pop the top of the max-heap and push it to the min-heap. This step ensures that the smallest element in the min-heap is the largest from the max-heap.
3. Now, if the min-heap has more elements than the max-heap, pop the top of the min-heap and push it to the max-heap (remembering to negate it again for the max-heap).

This way, the max-heap will always have the same number of elements as the min-heap or one more.

Let's test with adding 1 and 2.

Initialize:
max_heap = [] (will store negatives for max-heap)
min_heap = []

Add 1:
1. Push to max_heap: max_heap = [-1]
2. Pop max_heap top (-1), push to min_heap: min_heap = [1]
3. Now, check sizes: len(max_heap) = 0, len(min_heap)=1. So min_heap has more, so pop min_heap (1) and push to max_heap: max_heap = [-1], min_heap = [].

Now add 2:
1. Push to max_heap: max_heap = [-2, -1] (heap order is maintained, so the smallest negative is first: -2 is the top, which represents 2)
2. Pop max_heap top (-2), push to min_heap: min_heap = [2], and max_heap is now [-1]
3. Check sizes: len(max_heap)=1, len(min_heap)=1. So no need to balance.

Now, findMedian: since sizes are equal, median is (max_heap[0] * -1 + min_heap[0]) / 2 = (1 + 2)/2 = 1.5. But wait, the numbers are 1 and 2, so median should be 1.5. Correct.

Add 3:
1. Push to max_heap: max_heap = [-3, -1] (after heapify, the order might be [-3, -1] or [-1, -3]? Actually, heappush maintains the heap property. So after pushing -3, the heap is [-3, -1] because -3 < -1, so the top is -3.
2. Pop max_heap top (-3), push to min_heap: min_heap becomes [2,3] (since 3 is added, and heapify places 2 at the top? Wait, min_heap was [2], now push 3: [2,3]. Then, after popping from max_heap, we push 3 to min_heap. So min_heap is [2,3].
3. Now, check sizes: len(max_heap)=1, len(min_heap)=2. So min_heap has more, so pop min_heap top (2) and push to max_heap: max_heap becomes [-2, -1] (after pushing -2). Now, max_heap has [-2, -1], min_heap has [3].

Now, findMedian: since total elements is 3, median is the top of max_heap (which is -2 * -1 = 2). But the numbers are 1,2,3, so median is 2. Correct.

Another test: add 4.
1. Push to max_heap: max_heap = [-4, -2, -1] (heap order: the smallest negative is -4, so top is -4)
2. Pop max_heap top (-4), push to min_heap: min_heap becomes [3,4] (since we push 4, and it's a min-heap, the top is 3)
3. Now, sizes: len(max_heap)=2, len(min_heap)=2. So no need to balance.

findMedian: (max_heap[0]*-1 + min_heap[0]) / 2 = (2 + 3)/2 = 2.5. The numbers are [1,2,3,4], so median is 2.5. Correct.

This seems to work.

So the steps for addNum:
- Push -num to max_heap.
- Pop the top of max_heap, which is the largest in the max_heap (smallest negative), so we get the actual number by multiplying by -1, and push that to min_heap.
- If after this, the min_heap has more elements than max_heap, pop the top of min_heap, negate it, and push to max_heap.

Wait, but in step 2, when we pop from max_heap, we are taking the largest element from the lower half and moving it to the min_heap. Then, if the min_heap has more elements, we move the smallest from the upper half back to the max_heap. This ensures that the max_heap always has the same number of elements as min_heap or one more.

So the code structure:

import heapq

class MedianFinder:
    def __init__(self):
        self.max_heap = []  # for the lower half, stored as negatives
        self.min_heap = []  # for the upper half

    def addNum(self, num: int) -> None:
        # Push to max_heap (as negative)
        heapq.heappush(self.max_heap, -num)
        # Pop the largest from max_heap and push to min_heap
        val = -heapq.heappop(self.max_heap)
        heapq.heappush(self.min_heap, val)
        # If min_heap has more elements, move the smallest back to max_heap
        if len(self.min_heap) > len(self.max_heap):
            val = heapq.heappop(self.min_heap)
            heapq.heappush(self.max_heap, -val)

    def findMedian(self) -> float:
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        else:
            return (-self.max_heap[0] + self.min_heap[0]) / 2

Wait, but in the addNum method, after pushing to max_heap and then popping and pushing to min_heap, we check if min_heap has more elements. But initially, after adding the first element, max_heap becomes empty, then we push to min_heap, then since min_heap has one element and max_heap has zero, we pop from min_heap and push to max_heap. So after first element, max_heap has one element, min_heap is empty. That seems correct.

Let me test with adding 1:

addNum(1):
- max_heap: push -1 → [-1]
- pop from max_heap: -1 → val = 1, push to min_heap: [1]
- now, len(min_heap)=1, len(max_heap)=0 → 1 > 0, so pop from min_heap: 1, push to max_heap: [-1]
- so max_heap = [-1], min_heap = [].

Then findMedian: len(max_heap)=1, len(min_heap)=0 → return -(-1) = 1. Correct.

Add 2:
- push to max_heap: [-2, -1] (but heap order is maintained, so the top is -2)
- pop from max_heap: -2 → val=2, push to min_heap: [2]
- now, len(min_heap)=1, len(max_heap)=1 → no balance needed.
- so max_heap = [-1], min_heap = [2].

findMedian: ( -(-1) + 2 ) / 2 = (1+2)/2=1.5. Correct.

Add 3:
- push to max_heap: [-3, -1] (top is -3)
- pop from max_heap: -3 → val=3, push to min_heap: [2,3] (top is 2)
- now, len(min_heap)=2, len(max_heap)=1 → 2>1, so pop from min_heap: 2, push to max_heap: [-2, -1] (after pushing -2, the heap becomes [-2, -1])
- now, max_heap = [-2, -1], min_heap = [3].

findMedian: since len(max_heap)=2, len(min_heap)=1? Wait, no: after popping from min_heap, min_heap has [3], and max_heap has [-2, -1]. So len(max_heap)=2, len(min_heap)=1. So when we call findMedian, len(max_heap) > len(min_heap) is true only if the total is odd. Here, total elements is 3, so median is the top of max_heap: -(-2) = 2. But wait, the numbers are 1,2,3 → median is 2. Correct.

But in the code, the condition is if len(max_heap) > len(min_heap): return -max_heap[0]. In this case, len(max_heap)=2, len(min_heap)=1, so 2>1, so return 2. Correct.

But wait, in the addNum method, after pushing to max_heap and then moving to min_heap, the check is if len(min_heap) > len(max_heap). After adding 3, after pushing to min_heap, len(min_heap)=2, len(max_heap)=1, so we pop from min_heap and push to max_heap. So after that, len(max_heap)=2, len(min_heap)=1. So the sizes are not equal; max_heap has one more. That's correct for odd total.

So the code seems to handle it.

But let's see the condition in findMedian: if len(max_heap) > len(min_heap), return the top of max_heap. Else, return the average. That works because if the total number is odd, max_heap has one more element, and that's the median. If even, both have the same size, and we average the tops.

This seems correct.

But wait, in the addNum method, after pushing to max_heap and then popping and pushing to min_heap, the next step is to check if min_heap has more elements. But what if after this step, max_heap has more elements? For example, if we have even total, after adding a new number, max_heap might have one more. But in the code, after the first two steps, we only check if min_heap has more and balance by moving one to max_heap. So max_heap can have at most one more than min_heap, which is what we want.

Another test case: add 3, then 2, then 1.

Initialize.

Add 3:
- push to max_heap: [-3]
- pop and push to min_heap: min_heap=[3], max_heap=[]
- since len(min_heap)=1 > len(max_heap)=0, pop min_heap (3) and push to max_heap: max_heap=[-3], min_heap=[]

Add 2:
- push to max_heap: [-3, -2] → heapify to [-3, -2]? Wait, no: when you push -2, the heap becomes [-3, -2], and the heap property is maintained (the smallest negative is at the top). So top is -3.
- pop max_heap: -3, push to min_heap: min_heap=[3], max_heap=[-2]
- now, len(min_heap)=1, len(max_heap)=1 → no balance.

Add 1:
- push to max_heap: [-2, -1] → heap is now [-2, -1] (top is -2)
- pop max_heap: -2, push to min_heap: min_heap becomes [2,3] (since we push 2, and it's a min-heap, the top is 2)
- now, len(min_heap)=2, len(max_heap)=1 → 2>1, so pop min_heap: 2, push to max_heap: max_heap becomes [-2, -1] (after pushing -2, the heap is [-2, -1], but the top is -2 which is 2, but wait, we already have -1 in max_heap? Let me see: after popping from max_heap, max_heap was [-1]. Then we push -2: heappush(-2) → the heap becomes [-2, -1] because -2 < -1. So the top is -2.
- now, max_heap = [-2, -1], min_heap = [3].

findMedian: len(max_heap)=2, len(min_heap)=1 → total 3, median is the top of max_heap: -(-2) = 2. But the numbers are 3,2,1 → sorted [1,2,3], median is 2. Correct.

So the code works.

But wait, in the addNum method, when we push to max_heap, we are pushing -num, which is correct for a max-heap simulation. Then, when we pop from max_heap, we get the largest (most negative), which is the actual maximum in the lower half. Then we push that to min_heap. Then, if min_heap has more elements, we move the smallest from min_heap back to max_heap.

This ensures that the max_heap always has the same number of elements as min_heap or one more.

So the code should be correct.

Now, time complexity: each insertion involves O(log n) operations for heap push and pop. So for n insertions, it's O(n log n), which is acceptable for n=50,000.

Space complexity: O(n) to store all the numbers.

Let me check the constraints: up to 50,000 calls, so O(n log n) is acceptable.

I think this should work.

One more edge case: adding same numbers.

Add 1,1,1.

After first 1:
max_heap = [-1], min_heap = []

After second 1:
- push to max_heap: [-1, -1] (but heap order is maintained, so the top is -1)
- pop from max_heap: -1, push to min_heap: min_heap=[1]
- now, len(min_heap)=1, len(max_heap)=1 → no balance.

After third 1:
- push to max_heap: [-1, -1] (after pushing -1, the heap is [-1, -1, -1]? Wait, no: the heap is a list, and when you push, it maintains the heap property. So after pushing -1, the heap becomes [-1, -1, -1] but the order doesn't matter as long as the heap property holds. The top is -1.
- pop from max_heap: -1, push to min_heap: min_heap becomes [1,1] (since we push 1, and it's a min-heap, the top is 1)
- now, len(min_heap)=2, len(max_heap)=2? Wait, after popping from max_heap, the max_heap was [-1, -1] (since we had [-1, -1, -1], pop removes the top -1, and then the heap becomes [-1, -1] after heapify). Then we push to min_heap, so min_heap was [1], now becomes [1,1]. Then, check if len(min_heap) > len(max_heap): 2 > 2? No, so no balance.

So findMedian: ( -max_heap[0] + min_heap[0] ) / 2 = (1 + 1)/2 = 1. Correct, since the numbers are [1,1,1], median is 1.

Another case: even number of same elements: add 1,1.

After first 1: as before, max_heap = [-1], min_heap = [].

After second 1:
- push to max_heap: [-1, -1] (top is -1)
- pop from max_heap: -1, push to min_heap: min_heap = [1]
- now, len(min_heap)=1, len(max_heap)=1 → no balance.

findMedian: (1 + 1)/2 = 1. Correct.

So the code handles duplicates.

I think this should be correct.

Now, implement in Python.

Note: In Python, the heapq module is a min-heap. So for the max-heap, we store negatives.

The code:

import heapq

class MedianFinder:

    def __init__(self):
        self.max_heap = []  # lower half, stored as negatives
        self.min_heap = []  # upper half

    def addNum(self, num: int) -> None:
        heapq.heappush(self.max_heap, -num)
        val = -heapq.heappop(self.max_heap)
        heapq.heappush(self.min_heap, val)
        if len(self.min_heap) > len(self.max_heap):
            val = heapq.heappop(self.min_heap)
            heapq.heappush(self.max_heap, -val)

    def findMedian(self) -> float:
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        else:
            return (-self.max_heap[0] + self.min_heap[0]) / 2

Wait, but in the addNum method, after pushing to max_heap and then popping, we push to min_heap. Then, if min_heap has more elements, we move one back. This ensures that max_heap is always at least as large as min_heap. So in findMedian, if max_heap has more elements, that's the median. Otherwise, the average.

But in the condition, it's if len(min_heap) > len(max_heap). Since after the first two steps, we have moved one element to min_heap, so min_heap could be larger only if it was equal before and now becomes larger. But actually, after the first two steps, the sizes are: len(max_heap) is decreased by one (because we popped), and len(min_heap) is increased by one. So if they were equal before, now min_heap is one more. Then we check and move back if needed.

But in the code, after pushing to min_heap, we check if len(min_heap) > len(max_heap). If yes, we move one back. So after that, len(max_heap) will be equal to or one more than len(min_heap). So in findMedian, if len(max_heap) > len(min_heap), then the median is the top of max_heap. Otherwise, they are equal, and we average.

This should be correct.

Let me test with the example from the problem:

["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
[[], [1], [2], [], [3], []]

After addNum(1): max_heap = [-1], min_heap = [] → median is 1? But according to the code, after adding 1, in addNum:
- push to max_heap: [-1]
- pop from max_heap: -1 → val=1, push to min_heap: min_heap=[1]
- now, len(min_heap)=1 > len(max_heap)=0 → pop min_heap: 1, push to max_heap: max_heap=[-1], min_heap=[].

So after addNum(1), max_heap has [-1], min_heap has []. Then addNum(2):
- push to max_heap: [-2, -1] (but heap order: the top is -2)
- pop from max_heap: -2 → val=2, push to min_heap: min_heap=[2]
- now, len(min_heap)=1, len(max_heap)=1 → no balance.

Then findMedian: ( -max_heap[0] + min_heap[0] ) / 2 = (1 + 2)/2 = 1.5. Correct.

Then addNum(3):
- push to max_heap: [-3, -1] (top is -3)
- pop from max_heap: -3 → val=3, push to min_heap: min_heap=[2,3] (top is 2)
- now, len(min_heap)=2, len(max_heap)=1 → 2>1 → pop min_heap: 2, push to max_heap: max_heap becomes [-2, -1] (after pushing -2, the heap is [-2, -1], top is -2)
- now, max_heap = [-2, -1], min_heap = [3].

findMedian: since len(max_heap)=2, len(min_heap)=1 → 2>1, so return -max_heap[0] = 2. But the median of [1,2,3] is 2. Correct.

So the code works for this case.

I think this is correct.
</think>
I'll implement a solution using two heaps to efficiently maintain the median of a data stream. The key idea is to use a max-heap for the lower half of the numbers and a min-heap for the upper half, ensuring we can access the middle elements in constant time.

Here's the implementation:

```python
import heapq

class MedianFinder:
    def __init__(self):
        # Max-heap for the lower half (using negative values since heapq is min-heap)
        self.max_heap = []
        # Min-heap for the upper half
        self.min_heap = []

    def addNum(self, num: int) -> None:
        # Add to max_heap (as negative for max-heap behavior)
        heapq.heappush(self.max_heap, -num)
        
        # Move the largest from max_heap to min_heap
        val = -heapq.heappop(self.max_heap)
        heapq.heappush(self.min_heap, val)
        
        # Balance the heaps if necessary
        if len(self.min_heap) > len(self.max_heap):
            val = heapq.heappop(self.min_heap)
            heapq.heappush(self.max_heap, -val)

    def findMedian(self) -> float:
        if len(self.max_heap) > len(self.min_heap):
            # Odd number of elements, return middle value
            return -self.max_heap[0]
        else:
            # Even number of elements, return average of two middle values
            return (-self.max_heap[0] + self.min_heap[0]) / 2
```

### How it works:

1. **Data Structure**:
   - `max_heap`: Stores the lower half of numbers (as negatives to simulate max-heap behavior)
   - `min_heap`: Stores the upper half of numbers

2. **Adding a Number**:
   - First, add the number to `max_heap` (as a negative)
   - Move the largest number from `max_heap` to `min_heap`
   - If `min_heap` ends up with more elements, move the smallest number back to `max_heap`

3. **Finding the Median**:
   - If `max_heap` has more elements, its top is the median
   - If both heaps have equal size, average the tops of both heaps

### Time and Space Complexity:
- **addNum**: O(log n) time per operation
- **findMedian**: O(1) time
- **Space**: O(n) to store all numbers

### Example Usage:
```python
obj = MedianFinder()
obj.addNum(1)
obj.addNum(2)
print(obj.findMedian())  # Output: 1.5
obj.addNum(3)
print(obj.findMedian())  # Output: 2.0
```

This solution efficiently handles the constraints by maintaining the two heaps in a way that always gives us quick access to the median value.

---
</details>

Excellent — the model implements an efficient, fully correct solution that passes all test cases.

### Writing Assistance Examples

Let’s see how well the model can help draft professional, real-world documents.

```python
show_generation("Help me write an email to Professor Johnson asking for an extension on the Machine Learning assignment. The assignment is due tomorrow, but I've been sick with the flu all week. I have a doctor's note. Keep it professional and polite.")
```

<details>
<summary>Click to see reponse from model</summary>
<think>
Okay, I need to help the user draft a professional and polite email to Professor Johnson requesting an extension on the Machine Learning assignment. Let me break down the key elements that should be included:

1. **Subject Line**: Clear and concise, mentioning the course and the purpose of the email. Something like "Request for Extension on Machine Learning Assignment - [Course Name/Number]".

2. **Salutation**: Formal greeting, such as "Dear Professor Johnson,".

3. **Introduction**: State the purpose of the email right away. Mention the assignment and the current due date.

4. **Reason for Request**: Explain that the user has been sick with the flu all week. Mention that they have a doctor's note to provide as documentation. It's important to be honest but not overly detailed about the illness.

5. **Request for Extension**: Clearly ask for an extension. It might be good to suggest a new deadline if possible, but since the user didn't specify, I'll leave it open or offer to discuss a reasonable timeframe.

6. **Apology and Appreciation**: Apologize for any inconvenience and thank the professor for their understanding and consideration.

7. **Closing**: Professional sign-off with the user's full name, student ID (if applicable), and contact information.

I should also ensure the tone is respectful and not entitled. The user should acknowledge that extensions are at the professor's discretion. Additionally, offering to provide the doctor's note is crucial for credibility.

Let me structure the email step by step, making sure each part flows logically into the next. I'll keep the language formal but not overly stiff, and make sure the request is clear and courteous.
</think>

Subject: Request for Extension on Machine Learning Assignment - [Course Name/Number]

Dear Professor Johnson,

I hope this email finds you well. I am writing to respectfully request an extension for the Machine Learning assignment that is due tomorrow. Unfortunately, I have been unwell with the flu for the entire week, which has significantly impacted my ability to complete the assignment to the best of my ability.

I have been under a doctor's care during this time and can provide a doctor's note to support my request. I understand the importance of meeting deadlines and apologize for any inconvenience this may cause.

Would it be possible to grant me a brief extension? I would be happy to submit the completed assignment by [suggest new date] or at your earliest convenience. I am committed to catching up on the work as soon as I am able.

Thank you for your understanding and consideration of my request. Please let me know if you need any additional information or documentation from me.

Best regards,
[Your Full Name]
[Your Student ID]
[Your Contact Information]

---
</details>

The model produces a clear, professional email suitable for academic communication! Now let’s check how the model handles refining an informal or unpolished draft into something appropriate to send.

```python
show_generation("draft: Dear sir, i was sick last week and missed the assignmnet. I need more time to do it. when can i turn it in? also, what was the homework? i lost the syllabus. thanks!")
```

<details>
<summary>Click to see response from model</summary>
<think>
Okay, let's break this down. The user is asking me to draft an email to a professor about missing an assignment due to illness. The email needs to request an extension and ask about the homework since they lost the syllabus. 

First, I should consider the tone. It's a formal email to a professor, so it should be polite and professional. The original draft is a bit too casual and has some grammatical errors. For example, "i" should be capitalized, and "assignmnet" is misspelled. Also, the structure is a bit abrupt. 

The user mentioned being sick last week, so it's good to acknowledge that and perhaps apologize for the inconvenience. They also need to ask for a new deadline and inquire about the missed homework. 

I should make sure the email is clear and concise. It's important to include specific details like the class name and assignment if possible, but since the user doesn't have the syllabus, maybe they can mention the class. 

Also, the subject line should be informative so the professor knows what the email is about at a glance. Something like "Request for Extension and Homework Details - [Class Name]" would work. 

I should also remind the user to fill in placeholders like [Class Name] and their full name. Including contact information might be helpful too. 

Let me structure the email properly: start with a greeting, state the purpose, explain the situation, make the requests (extension and homework details), thank the professor, and sign off. 

I'll also correct any grammar and spelling issues from the original draft. The user might not have mentioned the exact dates, so I'll keep it general but suggest adding specifics if possible. 

Finally, I'll make sure the email is respectful and shows that the student is taking responsibility for catching up. That should cover everything the user needs.
</think>
Here's a more professional version of your email:

Subject: Request for Extension and Missed Assignment Details - [Class Name]

Dear Professor [Last Name],

I hope this email finds you well. I am writing to inform you that I was unable to submit last week's assignment due to illness. I am now working to complete the work and would be very grateful for an extension.

Could you please let me know by when I might submit this assignment? Additionally, I seem to have misplaced my syllabus and would greatly appreciate if you could remind me of the specific assignment details and requirements.

Thank you for your understanding and assistance with this matter. Please let me know if you need any additional information from me.

Best regards,
[Your Full Name]
[Your Student ID, if applicable]
[Your Email/Contact Information]

---
</details>

Again, the model turned a very rough draft into a respectful, well-structured email ready for sending.

### Precision in Following Instructions

Let’s test how precisely the model can follow tightly constrained instructions — even down to the exact number of words.

```python
show_generation("who created you? answer in 1 word.", enable_thinking=False)
# DeepCogito
```

```python
show_generation("who created you? answer in 2 words.", enable_thinking=False)
# Deep Cogito
```

### Scientific knowledge

Now let’s evaluate how clearly the model can explain scientific concepts, even complex ones like quantum entanglement.

```python
show_generation("Explain quantum entanglement in simple terms", enable_thinking=False)
```

<details>
<summary>Click to see response from model</summary>
Quantum entanglement is a strange but fascinating connection between particles where what happens to one particle instantly affects the other, no matter how far apart they are.

Here's a simple way to think about it:

Imagine you have two magic coins that are linked together. When you flip them, they'll always land on opposite sides - if one shows heads, the other will show tails, and vice versa. The weird part is that this happens instantly, even if the coins are on opposite sides of the universe!

In the quantum world, particles like electrons or photons can become "entangled," meaning their properties are connected in this special way. When you measure one, you instantly know the state of the other, no matter the distance between them.

Einstein famously called this "spooky action at a distance" because it seems to break the rule that nothing can travel faster than light. However, we can't actually use this to send information faster than light, which is why it doesn't violate the laws of physics.

Scientists are using quantum entanglement for new technologies like quantum computing and ultra-secure communication systems.

---
</details>

## Running Locally

Our checkpoints are compatible with leading inference frameworks:

### HuggingFace

Examples to run with Transformers library can be found on the [model page](https://huggingface.co/deepcogito/cogito-671b-v2.1#with-huggingface-pipeline).

### vLLM

Running the model locally with vLLM is as simple as calling it through the API shown above. You can reproduce the earlier examples by updating your `show_generation` function to use vLLM in place of the previous inference backend.

```python
from vllm import LLM, SamplingParams

llm = LLM(model="deepcogito/cogito-671b-v2.1-FP8", tensor_parallel_size=8, gpu_memory_utilization=0.95)

def show_generation(prompt, system=None, enable_thinking=True):
    global llm
    tokenizer = llm.get_tokenizer()
    sampling_params = SamplingParams(max_tokens=8192, temperature=0.6, top_p=0.95)
    if system is not None:
        messages = [
            {"role": "system", "content": system},
            {"role": "user", "content": prompt},
        ]
    else:
        messages = [{"role": "user", "content": prompt}]
    prompt = tokenizer.apply_chat_template(
        messages,
        tokenize=False,
        add_generation_prompt=True,
        enable_thinking=enable_thinking,
    )
    response = llm.generate([prompt], sampling_params, use_tqdm=False)[0]
    print(response.outputs[0].text)
```

### SGLang

Running the model locally with SGLang is as simple as calling it through the API shown above. First, start the SGLang server:

```bash
# 8×H200
python3 -m sglang.launch_server --model deepcogito/cogito-671b-v2.1-FP8 --tp 8

# 8×B200
python3 -m sglang.launch_server --model deepcogito/cogito-671b-v2.1-FP8 --tp 8 --quantization compressed-tensors --moe-runner-backend triton
```

After the server is running, update your `show_generation` function to use SGLang deployment:

```python
from openai import Client
client = Client(base_url="http://127.0.0.1:30000/v1", api_key="EMPTY")

def show_generation(prompt, system=None, enable_thinking=True):
    global client
    if system is not None:
        messages = [
            {"role": "system", "content": system},
            {"role": "user", "content": prompt},
        ]
    else:
        messages = [{"role": "user", "content": prompt}]
    response = client.chat.completions.create(
        model="default",
        messages=messages,
        extra_body={"chat_template_kwargs": {"enable_thinking": enable_thinking}},
        max_tokens=8192,
        temperature=0.6,
        top_p=0.95,
    )
    # To enable reasoning, set `enable_thinking=True` above.
    print(response.choices[0].message.content)
```

---

## License

Both the code and the model weights are released under the [MIT License](LICENSE).

---

## Contact Us

If you have any questions or concerns, please reach out to us at [contact@deepcogito.com](mailto:contact@deepcogito.com).
