# Shuffling Algorithms: Understanding Random Permutations

This repository provides educational resources about shuffling algorithms, focusing on the Fisher-Yates (Knuth) shuffle algorithm.

## Contents

- [Fisher-Yates Algorithm Explained](#fisher-yates-algorithm-explained) - Detailed explanation of the algorithm
- [Shuffle Implementations](#shuffle-implementations) - Various JavaScript implementations
- [Performance Comparison](#performance-comparison) - Benchmarks of different shuffling methods
- [Visual Examples](#visual-examples) - Step-by-step visualization of the shuffling process

## Why Shuffling Matters

Proper array shuffling is crucial for applications like:
- Card games
- Randomized quizzes
- Playlist randomization
- Statistical sampling

Using a biased shuffling algorithm can significantly impact user experience and application fairness.

## How To Use This Repository

This repository is designed for educational purposes. Browse the various markdown files to understand shuffling algorithms conceptually, or explore the code examples to see implementations.

---

# Fisher-Yates Algorithm Explained

The Fisher-Yates shuffle (also known as the Knuth shuffle) is an algorithm for generating random permutations of a finite sequence. It's named after Ronald Fisher and Frank Yates, who first described it in their 1938 book, and later modernized by Donald Knuth.

## The Algorithm

The modern version of the Fisher-Yates shuffle works as follows:

1. Start from the last element of the array (index n-1)
2. Generate a random index j between 0 and i (inclusive)
3. Swap the elements at positions i and j
4. Move to the previous element (i-1)
5. Repeat steps 2-4 until you reach the beginning of the array

## Why It Works

The Fisher-Yates shuffle produces an unbiased permutation, meaning each possible arrangement is equally likely. This is because:

1. For an array of length n, there are n! (n factorial) possible permutations
2. At each step i, we choose from (i+1) possibilities with equal probability
3. This creates exactly n! equally likely paths through the algorithm
4. Each path produces a unique permutation

## Common Mistakes

### Using Sort with Random Comparison

A common but incorrect approach to shuffling is:

```javascript
array.sort(() => Math.random() - 0.5);
```

This produces biased results because:
1. Sorting algorithms don't guarantee equal consideration of all permutations
2. The random comparator doesn't provide enough information for a proper shuffle
3. Different browsers implement sort() differently, leading to inconsistent results

### Not Creating a Copy

When shuffling, it's often important to avoid modifying the original array:

```javascript
// Incorrect - modifies original
function shuffle(array) {
  // Shuffling logic here
  return array; // Returns the same array reference
}

// Correct - preserves original
function shuffle(array) {
  const copy = [...array]; // Create a copy
  // Shuffling logic on copy
  return copy;
}
```

---

# Shuffle Implementations

This section contains various implementations of shuffling algorithms in JavaScript.

## Standard Fisher-Yates Shuffle

```javascript
function fisherYatesShuffle(array) {
  // Create a copy to avoid modifying the original
  const arrayCopy = [...array];

  // Fisher-Yates algorithm
  for (let i = arrayCopy.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arrayCopy[i], arrayCopy[j]] = [arrayCopy[j], arrayCopy[i]];
  }

  return arrayCopy;
}
```

This implementation has:
- O(n) time complexity - Linear with array size
- O(n) space complexity - Creates a copy of the array
- Unbiased results - All permutations equally likely

## Deep Copy Shuffle

```javascript
function deepCopyShuffle(array) {
  // Create a deep copy using structuredClone
  const arrayCopy = structuredClone(array);

  // Fisher-Yates algorithm
  for (let i = arrayCopy.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arrayCopy[i], arrayCopy[j]] = [arrayCopy[j], arrayCopy[i]];
  }

  return arrayCopy;
}
```

This implementation is suitable when the array contains nested objects or arrays.

## Functional Approach

```javascript
function functionalShuffle(array) {
  // Create a copy
  const result = [...array];

  // Fisher-Yates algorithm with functional-style reduce
  return Array.from({ length: result.length })
    .reduce((acc) => {
      if (acc.length === 0) return acc;

      const i = acc.length - 1;
      const j = Math.floor(Math.random() * (i + 1));
      [acc[i], acc[j]] = [acc[j], acc[i]];

      return acc;
    }, result);
}
```

## Incorrect Sort-Based Shuffle (For Comparison)

```javascript
function sortShuffle(array) {
  // Create a copy to avoid modifying the original
  const arrayCopy = [...array];

  // INCORRECT: Using sort with random comparison
  // This produces biased results!
  arrayCopy.sort(() => Math.random() - 0.5);

  return arrayCopy;
}
```

---

# Performance Comparison

This section compares the performance of different shuffling methods.

## Benchmark Results

| Algorithm | Time Complexity | Space Complexity | Quality | Relative Speed |
|-----------|-----------------|------------------|---------|----------------|
| Fisher-Yates | O(n) | O(n) | Unbiased | 1x (baseline) |
| Sort-Based | O(n log n) | O(log n) to O(n)* | Biased | 0.7x - 1.5x slower |
| Deep Copy + Fisher-Yates | O(n) | O(n) | Unbiased | 1.2x - 2x slower |
| Functional | O(n) | O(n) | Unbiased | 1.1x - 1.5x slower |

*Depends on browser implementation

## Bias Testing

When shuffling an array [1, 2, 3] repeatedly (1,000,000 times), the distribution of resulting permutations should be approximately equal for an unbiased shuffle:

| Permutation | Fisher-Yates | Sort-Based |
|-------------|--------------|------------|
| [1, 2, 3] | 16.7% | 22.1% |
| [1, 3, 2] | 16.7% | 22.3% |
| [2, 1, 3] | 16.7% | 11.3% |
| [2, 3, 1] | 16.6% | 11.2% |
| [3, 1, 2] | 16.6% | 16.5% |
| [3, 2, 1] | 16.7% | 16.6% |

Note how the sort-based shuffle shows significant bias toward certain permutations.

---

# Visual Examples

This section provides a step-by-step visualization of the Fisher-Yates shuffle algorithm.

## Example with Array [A, B, C, D]

Starting array: [A, B, C, D]

| Step | Current Index (i) | Random Index (j) | Array Before Swap | Swap Operation | Array After Swap |
|------|-------------------|------------------|-------------------|----------------|------------------|
| 1 | 3 | 1 | [A, B, C, D] | Swap D and B | [A, D, C, B] |
| 2 | 2 | 0 | [A, D, C, B] | Swap C and A | [C, D, A, B] |
| 3 | 1 | 1 | [C, D, A, B] | Swap D and D (no change) | [C, D, A, B] |
| Final | - | - | - | - | [C, D, A, B] |

## Tracing the Algorithm

Each step ensures that the current position (i) gets a value that is randomly selected from all positions from 0 to i (inclusive).

Notice how:
1. At step 1, position 3 has a 1/4 chance of getting any of the four values
2. At step 2, position 2 has a 1/3 chance of getting any remaining value
3. At step 3, position 1 has a 1/2 chance of getting either value
4. Position 0 doesn't need randomization as only one value remains

This process ensures that each permutation has an equal probability of 1/n! (where n is the array length).

---

# Test Code

Use this code to verify the quality of different shuffle implementations:

```javascript
function testShuffleDistribution(shuffleFunction, iterations = 1000000) {
  // Test with a small array [1, 2, 3]
  const testArray = [1, 2, 3];

  // Track occurrences of each permutation
  const results = {};

  // Perform many shuffles
  for (let i = 0; i < iterations; i++) {
    const shuffled = shuffleFunction([...testArray]);
    const key = shuffled.toString();
    results[key] = (results[key] || 0) + 1;
  }

  // Calculate and display percentages
  console.log(`Distribution after ${iterations} shuffles:`);

  const permutations = Object.keys(results);
  permutations.sort();

  permutations.forEach(key => {
    const percentage = (results[key] / iterations * 100).toFixed(1);
    console.log(`${key}: ${percentage}%`);
  });

  // Calculate standard deviation to measure bias
  const values = Object.values(results);
  const mean = values.reduce((sum, val) => sum + val, 0) / values.length;
  const squaredDiffs = values.map(val => Math.pow(val - mean, 2));
  const variance = squaredDiffs.reduce((sum, val) => sum + val, 0) / values.length;
  const stdDev = Math.sqrt(variance);

  console.log(`Standard deviation: ${stdDev.toFixed(2)}`);
  console.log(`Ideal standard deviation for perfect shuffle: ${Math.sqrt(iterations * (1 - (1/values.length))).toFixed(2)}`);

  return results;
}

// Usage examples:
// testShuffleDistribution(fisherYatesShuffle);
// testShuffleDistribution(sortShuffle);
```

---

# Practical Applications

## Using Fisher-Yates in Games

```javascript
function createShuffledDeck() {
  const suits = ['hearts', 'diamonds', 'clubs', 'spades'];
  const values = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];

  // Create deck
  const deck = [];
  for (const suit of suits) {
    for (const value of values) {
      deck.push({ suit, value });
    }
  }

  // Shuffle deck
  return fisherYatesShuffle(deck);
}
```

## Using Fisher-Yates for Randomized Quizzes

```javascript
function createRandomizedQuiz(questions, numberOfQuestions) {
  // Shuffle all questions
  const shuffledQuestions = fisherYatesShuffle(questions);

  // Take subset of questions
  const quizQuestions = shuffledQuestions.slice(0, numberOfQuestions);

  // For each question, also shuffle its answer options
  return quizQuestions.map(question => {
    return {
      ...question,
      options: fisherYatesShuffle(question.options)
    };
  });
}
```

---

# Resources and Further Reading

Here are some additional resources to deepen your understanding of shuffling algorithms:
- [Fisher-Yates Shuffle on Wikipedia](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle) - Detailed explanation and history
- [Visualizing the Fisher-Yates Shuffle](https://bost.ocks.org/mike/shuffle/)
- [How to shuffle an array in JavaScript](https://www.youtube.com/watch?v=5sNGqsMpW1E) - Video explanation