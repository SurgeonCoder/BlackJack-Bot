# BlackJack-Bot
 A Black Jack bot made during the process of learning Python for Day 11 Capstone Project


import random
import os
from art import logo
import pyfiglet

def cls():
    os.system('cls' if os.name=='nt' else 'clear')

def game_start():
    def deck_creation(card_list):
        deck = {}
        cards = ["A", '2', '3', '4', '5', '6', '7', '8', '9', '10', "J", "Q", "K"]
        for card in card_list:
            deck[card] = cards

        return deck
    card_list = ["spades", "diamonds", "hearts", "clubs"]
    Royals = ['J', 'Q', 'K']
    deck = deck_creation(card_list)
    

    def dealer_cards(deck):
        dealer_list1 = random.sample((deck[random.choice(card_list)]), 2)
        print(f"\n\nThe dealer cards are: {dealer_list1[0]}, ?\n\n")
        Royals = ['J', 'Q', 'K']
        dealer_sum = 0
        A = [1, 11]
        for x in dealer_list1:
            if x == "A":
                dealer_sum += random.choice(A)

            elif x in Royals:
                dealer_sum += 10

            else:
                dealer_sum += int(x)

        if dealer_sum < 17:
            dealer_list1, dealer_sum = dealer_sec_draw(card_list, deck, sum=dealer_sum, sec_draw_list=dealer_list1)
        return dealer_list1, dealer_sum

    def player_cards(deck):
        player_list1 = random.sample((deck[random.choice(card_list)]), 2)
        sum = 0
        Royals = ['J', 'Q', 'K']
        print(f"\n\nYour cards are: {player_list1}\n\n")
        for x in player_list1:
            if x == "A":
                if input("You have been given an ACE card. What value do you want to choose for the ace:") == "1":
                    sum += 1
                else:
                    sum += 11

            elif x in Royals:
                sum += 10

            else:
                sum += int(x)
        print(f"Your Current Score is:  {sum}")
        return player_list1, sum

    def player_sec_draw(card_list, deck, sum, sec_draw_list):
        sec_draw = str(random.choice(deck[random.choice(card_list)]))
        sec_draw_list += sec_draw
        print(f"\n\nYour cards after the draw are:  {sec_draw_list}\n\n")
        Royals = ['J', 'Q', 'K']
        if sec_draw_list[2] == "A":
            if input("You have been given an ACE card. What value do you want to choose for the ace:") == "1":
                sum += 1
            else:
                sum += 11

        elif sec_draw_list[2] in Royals:
            sum += 10
        else:
            sum += int(sec_draw_list[2])

        return sec_draw_list, sum

    def dealer_sec_draw(card_list, deck, sum, sec_draw_list):
        sec_draw = str(random.choice(deck[random.choice(card_list)]))
        sec_draw_list += sec_draw
        Royals = ['J', 'Q', 'K']
        A = [1, 11]
        if sec_draw_list[2] == "A":
            sum += random.choice(A)
        elif sec_draw_list[2] in Royals:
            sum += 10
        else:
            sum += int(sec_draw_list[2])

        return sec_draw_list, sum

    def result(player_sum, dealer_sum, player_list1, dealer_list1):
        if player_sum > 21 > dealer_sum:
            cls()
            print(pyfiglet.figlet_format("You Lost"))
            print(f"Your Total Score of:  {player_sum} exceeded 21")
        elif player_sum < 21 < dealer_sum:
            cls()
            print(pyfiglet.figlet_format("Dealer Lost"))
            print(f"Dealer had the cards: {dealer_list1} and dealer score is : {dealer_sum} which exceeded 21")
        elif player_sum == dealer_sum:
            cls()
            print (pyfiglet.figlet_format("Its a Draw"))
            print(f" You had the cards: {player_list1} and Total Score is: {player_sum}")
            print(f"Dealer had the cards: {dealer_list1} and dealer also had the score: {dealer_sum}")
        elif player_sum == 21 and len(player_list1) == 2:
            cls()
            print(pyfiglet.figlet_format("BlackJack , You Won"))
            print(f"You won with a BlackJack")
        elif dealer_sum == 21 and len(dealer_list1) == 2:
            cls()
            print(pyfiglet.figlet_format("BlackJack"))
            print(f"Dealer won with a BlackJack and the dealer cards are::  {dealer_list1}")
        elif player_sum > dealer_sum:
            cls()
            print(pyfiglet.figlet_format("You Won"))
            print(f"You had the cards:  {player_list1} and won with Total Score of:  {player_sum}")
            print(f"Dealer had the cards: {dealer_list1}  with Score : {dealer_sum}")
        else:
            cls()
            print(pyfiglet.figlet_format("Dealer Won"))
            print(f"You had the cards: {player_list1} and Total Score is: {player_sum}")
            print(f"Dealer had the cards: {dealer_list1} and Dealer Won with score: {dealer_sum}")
    player_list1, player_sum = player_cards(deck)
    dealer_list1, dealer_sum = dealer_cards(deck)

    if input("Do you want to draw a new card or you want to hit? D for Draw and H for hit:").lower() == "h":
        result(player_sum, dealer_sum, player_list1, dealer_list1)
    else:
        player_list1, player_sum = player_sec_draw(card_list, deck, sum=player_sum, sec_draw_list=player_list1)
        result(player_sum, dealer_sum, player_list1, dealer_list1)


c = True
while c is True:
    if input("Do you want to play Black Jack. Type Y for Yes and N for No::  ").lower() == "y":
        cls()
        print(logo)
        game_start()
    else:
        cls()
        print(pyfiglet.figlet_format("Good Bye"))
        print("\n\n Thank you for Playing the game.\n\n")
        c = False
