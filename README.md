# Yacht Dice Game

A simplified version of the classic Yacht dice game, built in Python for COMPSCI 101 at the University of Auckland.

## How It Works

- Roll 5 dice each round across 6 rounds
- Choose a scoring category (Ones through Sixes) each round
- Each category can only be used once
- Top 5 high scores are saved to a file

## How to Run
"""
COMPSCI 101 Assignment – Yacht
Username: SRAN567
ID: 404758178
"""

import random

def roll_dice():
    dice_roll = []
    for i in range(5):
        dice_roll.append(random.randrange(1, 7))
    return dice_roll

def print_banner(username):
    banner_text = f"Yacht by {username}"
    stars = "*" * (len(banner_text) + 4)
    print(stars)
    print(f"*  {banner_text}  *")
    print(stars)
    print()

def calculate_roll_score(category, dice_roll):
    return sum(die for die in dice_roll if die == category)

def get_category(available_categories):
    print(f"Available categories: {available_categories}")
    category = int(input("Please choose a category from those available: "))
    while category not in available_categories:
        print(f"{category} is not available!")
        category = int(input("Please choose a category from those available: "))
    available_categories.remove(category)
    return category

def play_game(available_categories, category_names):
    total_score = 0
    for _ in range(6):
        dice_roll = roll_dice()
        dice_roll.sort()
        print(f"You have rolled the following: {dice_roll}")
        category = get_category(available_categories)
        print(f"You have chosen {category_names[category - 1]}")
        round_score = calculate_roll_score(category, dice_roll)
        print(f"You have scored {round_score} this round.")
        total_score += round_score
        if available_categories:
            print(f"Your current total score is {total_score}.")
        print()
    print(f"Congratulations! You have scored {total_score}!")
    return total_score

def read_high_scores(filename):
    with open(filename, "r") as file:
        lines = file.readlines()[1:]  # skip header line
    return [int(line.strip().split(". ")[1]) for line in lines]

def update_high_scores(filename, username, high_scores, new_score):
    if new_score > min(high_scores):
        high_scores.append(new_score)
        high_scores.sort(reverse=True)
        high_scores = high_scores[:5]
    with open(filename, "w") as file:
        file.write(f"High Scores for {username}\n")
        for i, score in enumerate(high_scores, 1):
            file.write(f"{i}. {score}\n")

def handle_high_scores(filename, username, new_score):
    high_scores = read_high_scores(filename)
    update_high_scores(filename, username, high_scores, new_score)

def main():
    available_categories = [1, 2, 3, 4, 5, 6]
    category_names = ["Ones", "Twos", "Threes", "Fours", "Fives", "Sixes"]
    filename = "High_Scores.txt"
    username = "SRAN567"

    print_banner(username)
    total_score = play_game(available_categories, category_names)
    handle_high_scores(filename, username, total_score)

main()

## Skills Demonstrated

- Modular function design (8 distinct functions)
- File I/O for persistent high score tracking
- Input validation and error handling
- Clean code conventions with no global state
