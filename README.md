import random

# List of predefined words
WORDS = ['python', 'hangman', 'challenge', 'programming', 'interface', 'modular', 'optimize']

# Visual representation of the hangman
HANGMAN_PICS = [
    '''
      +---+
          |
          |
          |
         ===''', '''
      +---+
      O   |
          |
          |
         ===''', '''
      +---+
      O   |
      |   |
          |
         ===''', '''
      +---+
      O   |
     /|   |
          |
         ===''', '''
      +---+
      O   |
     /|\\  |
          |
         ===''', '''
      +---+
      O   |
     /|\\  |
     /    |
         ===''', '''
      +---+
      O   |
     /|\\  |
     / \\  |
         ==='''
]

# Function to get a random word
def get_random_word(word_list):
    return random.choice(word_list)

# Function to display the current game state
def display_game_state(hangman_pics, incorrect_guesses, correct_guesses, secret_word):
    print(hangman_pics[len(incorrect_guesses)])
    print()
    print('Incorrect guesses:', ' '.join(incorrect_guesses))
    print()

    blank_word = ['_' if letter not in correct_guesses else letter for letter in secret_word]
    print(' '.join(blank_word))
    print()

# Function to get the player's guess
def get_player_guess(already_guessed):
    while True:
        guess = input('Guess a letter: ').lower()
        if len(guess) != 1:
            print('Please enter a single letter.')
        elif guess in already_guessed:
            print('You have already guessed that letter. Try again.')
        elif not guess.isalpha():
            print('Please enter a letter.')
        else:
            return guess

# Main game function
def play_hangman():
    print('Welcome to Hangman!')
    secret_word = get_random_word(WORDS)
    incorrect_guesses = []
    correct_guesses = []
    game_is_done = False

    while True:
        display_game_state(HANGMAN_PICS, incorrect_guesses, correct_guesses, secret_word)

        guess = get_player_guess(incorrect_guesses + correct_guesses)

        if guess in secret_word:
            correct_guesses.append(guess)

            found_all_letters = True
            for letter in secret_word:
                if letter not in correct_guesses:
                    found_all_letters = False
                    break
            if found_all_letters:
                print(f'Congratulations! You have guessed the word: {secret_word}')
                game_is_done = True
        else:
            incorrect_guesses.append(guess)

            if len(incorrect_guesses) == len(HANGMAN_PICS) - 1:
                display_game_state(HANGMAN_PICS, incorrect_guesses, correct_guesses, secret_word)
                print(f'Sorry, you have run out of guesses. The word was: {secret_word}')
                game_is_done = True

        if game_is_done:
            if input('Do you want to play again? (yes or no): ').lower().startswith('y'):
                incorrect_guesses = []
                correct_guesses = []
                game_is_done = False
                secret_word = get_random_word(WORDS)
            else:
                break

if __name__ == '__main__':
    play_hangman()
