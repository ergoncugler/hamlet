# [Paper Materials] ‘To be, or not to be, that is the Question’: Exploring the ‘pseudorandom’ generation of texts to write Hamlet from the perspective of the ‘Infinite Monkey Theorem’

### Dataset generated in *"‘To be, or not to be, that is the Question’: Exploring the ‘pseudorandom’ generation of texts to write Hamlet from the perspective of the ‘Infinite Monkey Theorem’"*, available at [arXiv:2402.16253](https://arxiv.org/abs/2402.16253) and at [math.paperswithcode](https://math.paperswithcode.com/paper/to-be-or-not-to-be-that-is-the-question).

**Abstract:** This article explores the theoretical and computational aspects of the ‘Infinite Monkey Theorem’, investigating the number of attempts and the time required for a set of pseudorandom characters to assemble and recite Hamlet's iconic phrase, ‘To be, or not to be, that is the Question’. Drawing inspiration from Émile Borel's original concept (1913), the study delves into the practical implications of pseudorandomness using Python. Employing Python simulations to generate excerpts from Hamlet, the research navigates historical perspectives and bridges early theoretical foundations with contemporary computational approaches. A set of tests reveals the attempts and time required to generate incremental parts of the target phrase. Utilizing these results, growth factors are calculated, projecting estimated attempts and time for each text part. The findings indicate an astronomical challenge to generate the entire phrase, requiring approximately 2.68×10e69 attempts and 2.95×10e66 seconds — equivalent to 8.18×10e62 hours or 9.32×10e55 years. This temporal scale, exceeding the age of the universe by 6.75×10e45 times, underscores the immense complexity and improbability of random literary creation. The article concludes with reflections on the mathematical intricacies and statistical feasibility within the context of the Infinite Monkey Theorem, emphasizing the theoretical musings surrounding infinite time and the profound limitations inherent in such endeavors. And that only infinity could write Hamlet randomly.

## About the code behind

**This Python code simulates the Infinite Monkey Theorem:** The function generate_partial_text creates partially random texts of specific sizes. The main function, test_monkey_theorem, takes a target text and iterates over its sizes. It generates random texts until a match with the target is found, recording attempts and elapsed time. The usage section runs the function ten times for the target text ‘To be’, printing generated texts, attempts, and elapsed time. The code mimics the idea of a monkey typing randomly to have a specific text.

```python
def generate_partial_text(current_size):
    return ''.join(random.choice(string.ascii_letters + ' ') for _ in range(current_size))


def test_monkey_theorem(target_text):
    generated_text = ''


    for current_size in range(1, len(target_text) + 1):
        start_time = time.time()
        attempts = 0


        while generated_text != target_text[:current_size]:
            attempt = generate_partial_text(current_size)
            elapsed_time = time.time() - start_time
            attempts += 1


            if attempt == target_text[:current_size]:
                generated_text = attempt
                print(f"'{generated_text}' | {attempts + 1:,} attempts | {elapsed_time:.3f} seconds")


# Usage
for i in range(10):
    target_text = "To be"
    attempts = test_monkey_theorem(target_text)
    print("\n")
```

**Dataset:** Here the means estimated by ten previous tests are presented as parameters for geometric progression estimates.

```python
# Provided Data
attempts_per_part = [60, 3101, 159174, 8096722, 345380940]
times_per_part = [0.0001, 0.0060, 0.3600, 22.3550, 1097.5000]
target_text = "To be, or not to be, that is the Question"
```

**Growth Factors:** This code calculates growth factors for attempts and time based on the provided attempts_per_part and times_per_part lists. It then estimates the number of attempts and time for each part of the target text using a geometric progression. The resulting estimates are stored in lists (text_parts, estimated_attempts_f, estimated_times, and estimated_times_hours). Finally, a Pandas DataFrame is created to display the estimated data, including text parts, estimated attempts, and estimated time in both seconds and hours. The growth factors are calculated by summing the ratio of each value to its preceding value in the respective lists and dividing by the length of the lists minus one. These growth factors are then used to project the estimated attempts and time for each part of the target text.

```python
attempts_growth_factor = sum(attempts_per_part[i] / attempts_per_part[i - 1] for i in range(1, len(attempts_per_part))) / (len(attempts_per_part) - 1)
time_growth_factor = sum(times_per_part[i] / times_per_part[i - 1] for i in range(1, len(times_per_part))) / (len(times_per_part) - 1)
text_parts = []
estimated_attempts_f = []
estimated_times = []
estimated_times_hours = []


for i in range(1, len(target_text) + 1):
    text_part = target_text[:i]


    if i <= len(attempts_per_part):
        estimated_attempts = attempts_per_part[i - 1]
        estimated_time = times_per_part[i - 1]
    else:
        estimated_attempts = estimated_attempts * attempts_growth_factor
        estimated_time = estimated_time * time_growth_factor
    text_parts.append(text_part)
    estimated_attempts_f.append(estimated_attempts)
    estimated_times.append(estimated_time)
    estimated_times_hours.append(estimated_time / 3600)


df = pd.DataFrame({
    'Text Part': text_parts,
    'Estimated Attempts': estimated_attempts_f,
    'Estimated Time (seconds)': estimated_times,
    'Estimated Time (hours)': estimated_times_hours})
display(df)
```

## Exploring the results

Utilizing these metrics, growth factors are calculated, enabling the projection of estimated attempts and time for each text part. The results are showcased in a Pandas DataFrame, providing insights into the intriguing process of simulating the Infinite Monkey Theorem and allowing for further exploration and analysis of the generated data. Table, below, presents the results obtained from the geometric progressions, estimating the number of attempts and the estimated time to generate the phrase 'To be, or not to be, that is the Question' from characters in calculations pseudorandoms in Python.

| Text Part | Estimated Attempts | Est. Time (seconds) | Est. Time (hours) |
|-----------|---------------------|----------------------|-------------------|
| T | 6.00E+01 | 1.00E-04 | 2.78E-08 |
| To | 3.10E+03 | 6.00E-03 | 1.67E-06 |
| To | 1.59E+05 | 3.60E-01 | 1.00E-04 |
| To b | 8.10E+06 | 2.24E+01 | 6.21E-03 |
| To be | 3.45E+08 | 1.10E+03 | 3.05E-01 |
| To be, | 1.70E+10 | 6.34E+04 | 1.76E+01 |
| To be, | 8.34E+11 | 3.67E+06 | 1.02E+03 |
| To be, o | 4.10E+13 | 2.12E+08 | 5.89E+04 |
| To be, or | 2.01E+15 | 1.22E+10 | 3.40E+06 |
| To be, or | 9.89E+16 | 7.08E+11 | 1.97E+08 |
| To be, or n | 4.86E+18 | 4.09E+13 | 1.14E+10 |
| To be, or no | 2.39E+20 | 2.36E+15 | 6.57E+11 |
| To be, or not | 1.17E+22 | 1.37E+17 | 3.80E+13 |
| To be, or not | 5.76E+23 | 7.90E+18 | 2.19E+15 |
| To be, or not t | 2.83E+25 | 4.57E+20 | 1.27E+17 |
| To be, or not to | 1.39E+27 | 2.64E+22 | 7.33E+18 |
| To be, or not to | 6.84E+28 | 1.53E+24 | 4.24E+20 |
| To be, or not to b | 3.36E+30 | 8.82E+25 | 2.45E+22 |
| To be, or not to be | 1.65E+32 | 5.10E+27 | 1.42E+24 |
| To be, or not to be, | 8.11E+33 | 2.94E+29 | 8.18E+25 |
| To be, or not to be, | 3.99E+35 | 1.70E+31 | 4.73E+27 |
| To be, or not to be, t | 1.96E+37 | 9.84E+32 | 2.73E+29 |
| To be, or not to be, th | 9.62E+38 | 5.69E+34 | 1.58E+31 |
| To be, or not to be, tha | 4.73E+40 | 3.29E+36 | 9.13E+32 |
| To be, or not to be, that | 2.32E+42 | 1.90E+38 | 5.28E+34 |
| To be, or not to be, that | 1.14E+44 | 1.10E+40 | 3.05E+36 |
| To be, or not to be, that i | 5.61E+45 | 6.35E+41 | 1.76E+38 |
| To be, or not to be, that is | 2.76E+47 | 3.67E+43 | 1.02E+40 |
| To be, or not to be, that is | 1.35E+49 | 2.12E+45 | 5.89E+41 |
| To be, or not to be, that is t | 6.65E+50 | 1.23E+47 | 3.40E+43 |
| To be, or not to be, that is th | 3.27E+52 | 7.08E+48 | 1.97E+45 |
| To be, or not to be, that is the | 1.61E+54 | 4.09E+50 | 1.14E+47 |
| To be, or not to be, that is the | 7.89E+55 | 2.37E+52 | 6.57E+48 |
| To be, or not to be, that is the Q | 3.88E+57 | 1.37E+54 | 3.80E+50 |
| To be, or not to be, that is the Qu | 1.90E+59 | 7.90E+55 | 2.20E+52 |
| To be, or not to be, that is the Que | 9.36E+60 | 4.57E+57 | 1.27E+54 |
| To be, or not to be, that is the Ques | 4.60E+62 | 2.64E+59 | 7.33E+55 |
| To be, or not to be, that is the Quest | 2.26E+64 | 1.53E+61 | 4.24E+57 |
| To be, or not to be, that is the Questi | 1.11E+66 | 8.82E+62 | 2.45E+59 |
| To be, or not to be, that is the Questio | 5.45E+67 | 5.10E+64 | 1.42E+61 |
| To be, or not to be, that is the Question | 2.68E+69 | 2.95E+66 | 8.18E+62 |

To generate the entire phrase “To be, or not to be, that is the Question”, it would be necessary around 2,68×10e69 attempts (2,680,000,000,000,000,000,000,000,000,000,000, 000,000,000,000,000,000,000,000,000,000,000,000); 2,95×10e66 seconds, 8,18×10e62 hours, or 9.32×10e55 years. It is important to highlight that 9.32×10e55 years is approximately 6.75×10e45 times greater than the estimated age of the universe (which is 1,38×10e10 years). Therefore, to write “To be, or not to be, that is the Question”, it would take 6.75×10e45 times the age of the universe.

___

## More About:

Its use is highly encouraged and recommended for academic and scientific research, content analysis, sentiment and speech. It is free and open, and academic use is encouraged. Its responsible use is the sole responsibility of those who adapt and manipulate the data.

___

## Author Info:

Ergon Cugler de Moraes Silva, from Brazil, mailto: <a href="contato@ergoncugler.com">contato@ergoncugler.com</a> / Master's in Public Administration and Government, Getulio Vargas Foundation (FGV).

### How to Cite it:

**SILVA, Ergon Cugler de Moraes. ‘To be, or not to be, that is the Question’: Exploring the ‘pseudorandom’ generation of texts to write Hamlet from the perspective of the ‘Infinite Monkey Theorem’. (feb) 2024. Avaliable at: <a>https://github.com/ergoncugler/hamlet/<a>.**
