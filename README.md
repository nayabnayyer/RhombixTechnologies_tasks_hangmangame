# RhombixTechnologies_tasks

This isn’t just any Hangman—it’s running right in your browser thanks to Streamlit! 🖥️✨ No boring setups, no fuss—just you, some fun words, and a stick figure who’s counting on you to guess right!

The rules are simple:

🔤 You’ll guess one letter at a time to uncover the hidden word.

💡 Be careful! You only have a limited number of attempts before the poor stick figure meets its fate. 

🏆 Guess the word before the attempts run out, and you’ll emerge victorious!

```python
import random
import streamlit as st

# List of possible words
word_list = ['python', 'hangman', 'programming', 'developer', 'coding']

# Select the word only once and store it in session state
if 'word' not in st.session_state:
    st.session_state.word = random.choice(word_list)
    st.session_state.word_length = len(st.session_state.word)
    st.session_state.blanks = ['_'] * st.session_state.word_length
    st.session_state.wrong_guesses = []
    st.session_state.attempts = 6

# Hangman stages (simplified)
hangman_stages = [
    '''
     -----
     |   |
         |
         |
         |
         |
    ''',
    '''
     -----
     |   |
     O   |
         |
         |
         |
    ''',
    '''
     -----
     |   |
     O   |
     |   |
         |
         |
    ''',
    '''
     -----
     |   |
     O   |
    /|   |
         |
         |
    ''',
    '''
     -----
     |   |
     O   |
    /|\\  |
         |
         |
    ''',
    '''
     -----
     |   |
     O   |
    /|\\  |
    /    |
         |
    ''',
    '''
     -----
     |   |
     O   |
    /|\\  |
    / \\  |
         |
    '''
]

# Set up the page layout
st.title("Hangman Game")
st.write("Guess the word! You have 6 wrong guesses.")
st.text(hangman_stages[6 - st.session_state.attempts])

# Display the current state of the word and wrong guesses
st.write(f"Current word: {' '.join(st.session_state.blanks)}")
st.write(f"Wrong guesses: {', '.join(st.session_state.wrong_guesses)}")
st.write(f"Attempts left: {st.session_state.attempts}")

# Input for the player's guess
guess = st.text_input("Guess a letter:", max_chars=1).lower()

if guess:
    if guess in st.session_state.wrong_guesses or guess in st.session_state.blanks:
        st.warning("You already guessed that letter! Try again.")
    else:
        if guess in st.session_state.word:
            # Reveal the correct letter in the blanks
            for i in range(st.session_state.word_length):
                if st.session_state.word[i] == guess:
                    st.session_state.blanks[i] = guess
            st.success(f"Good guess! {guess} is in the word.")
        else:
            st.session_state.wrong_guesses.append(guess)
            st.session_state.attempts -= 1
            st.error(f"Oops! {guess} is not in the word.")

        # Check for win or lose conditions
        if '_' not in st.session_state.blanks:
            st.success(f"Congrats! You've guessed the word: {st.session_state.word}")
            st.button("Play Again", on_click=lambda: st.session_state.clear())

        if st.session_state.attempts == 0:
            st.error(f"Game Over! The word was: {st.session_state.word}")
            st.button("Play Again", on_click=lambda: st.session_state.clear())


