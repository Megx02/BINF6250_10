# Introduction
In this project we implement the Baum-Welch algorithm to estimate the parameters of a Hidden Markov Model with two states: High GC content and Low GC Content. It estimates initial probabilities, transition probabilities, and emission probabilities. Our code divides this algorithm into two steps:

1. Expectation - where we use our previous forward-backward algorithm implementation to estimate soft counts for set of probabilities in each part of the model based on a set of sequences.
2. Maximization - where we iterate, normalize the counts after each iteration, and compare log-likelihood of the previous iterations model with the current iterations model.

The examples provides in this project are just small demonstrations of how our implementation works. To get realistic biological models, the algorithm should be trained on a larger set of sequences from an appropriate biological context.

# Pseudocode

```
Expectation:

  Initilaize lists to store transition_counts, emission_counts, initial_counts

  For each sequence in our data set:
    calculate forward_matrix, sequence_prob
    calculate backwad_matrix

    # Get our initial state soft counts
    for each state in our model:
      log_initial = forward_matrix[state][first position in sequence] + backward_matrix[state][first position in sequence] - sequence_prob
      initial_counts = np.logaddexp(initial_counts[state], log_initial)

    # Get our emission and transition counts
    for each nucleotide in our sequence except the last one:
      for each state in our model:
        # emission counts
        log_forward = forward_matrix[state][nucleotide]
        log_backward = backward_matrix[state][nucleotide]
        log_emission = log_forward + log_backward - sequence_prob
        emission_counts[state][nucleotide] = np.logaddexp(emission_counts[state][nucleotide], log_emission)

        # tranisiton counts
        for each next_state in our model:
          log_state2state = log(transition[state][next_state])
          log_emission = log(emission[next_state][next nucleotide in the sequence])
          log_finish = backward_matrix[next_state][next nucleotide in the sequence]
          log_transition = log_forward + log_state2state + log_emission + log_finish - sequence_prob
          transition_counts[state][next_state] = np.logaddexp(transition_counts[state][next_state], log_transition)

    return transition_counts, emission_counts, initial_counts

Maximization:

  for every iteration in our algorithm:
    make a copy of each part of our model
    perform expectation step

    convert log soft counts for each set of counts to log probabilites by subtracting the
    logsumexp of each set of counts associated with a state (a set of emissions for a state, a set of transitions for a state, and all initial counts for all states)
    from a specific count of a state (count of emission "G" for state "H", count of transiton from state "H" to state "L", initial count of state "L", etc.)

    update our copies with the new log_probabilities
    compute the log likelihood of our old model given our sequences
    update our model with the new log_probabilities
    compute the log likelihood of our new model given our sequences
    compare the ll of our new model to our old model, if the change is smaller than the given threshold we've reached convergence

    return the trained model

    

      
```

# Successes
Description of the team's learning points

# Struggles
Description of the stumbling blocks the team experienced

# Personal Reflections
## Group Leader
Group leader's reflection on the project

## Other member
Meghana Ravi - This project was more challenging for me to understand compared to the previous HMM projects, especially in terms of the underlying logic and understanding the mathematical equations. Aside from the mathematical aspect of this algorithm, what the iterations were supposed to do also confused me a lot. Discussing the process with my group and attending office hours helped clarify many of my confusions. Going through the extra material in the module also helped me visualize the process better. Once I understood the logic, it became easier to work on. 

# Generative AI Appendix
As per the syllabus
