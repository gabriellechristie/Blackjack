import java.util.Scanner;
/**
 * This class creates a Blackjack card game.
 */
public class Blackjack {
    private static int CARD_COUNT_21 = 21;
    private static int generatedNumber = 0;
    private static int totalGenNumSum = 0; //The variable totalGenNumSun is assigned to the player's total sum.
    private static String userChoice; // The variable userChoice is assigned to the inputted values of the player.

    public static void main(String[] args) {
        P1Random randomGen = new P1Random();
        int gameCount = 0;
        int playerWins = 0;
        int dealerWins = 0;
        int tieWins = 0;
        float percentPlayerWins;
        int gameCountCompleted=0;
        Scanner scr = new Scanner(System.in);
        startGame(randomGen, gameCount); // This method starts a new game and gives a new random number.

        /**
         * The purpose of this is to have the player running the program be continually playing the game until they
         * input a specific value ("4") to end the program. Here the program will print "You exceeded 21! You lose." if
         * the player's total sum is over 21. It also tracks game count, games completed, and dealer wins.
         * Then it checks if the player's value is equal to 21 and if so, the program prints "BLACKJACK! You win!" and tracks
         * games completed, game count, and player wins.
         */
        while (true) {
            if (totalGenNumSum > CARD_COUNT_21) {
                System.out.println("You exceeded 21! You lose.");
                System.out.println("");
                gameCountCompleted++;
                gameCount++;
                startGame(randomGen, gameCount);
                dealerWins++;
            }
            else if (totalGenNumSum == CARD_COUNT_21) {
                System.out.println("BLACKJACK! You win!");
                System.out.println("");
                gameCountCompleted++;
                gameCount++;
                startGame(randomGen,gameCount);
                playerWins++;
            }
            displayMenu(scr);// This method prompts the numbers 1-4 as input options and allows for the player to input one of them.

            if (userChoice.equals("1")) {
                pickCard(randomGen);
            }

            /**
             * The purpose of this is to have a way to compare the dealer's hand to the players when the player chooses to "hold" (2).
             * The program then can compare the dealer's cards to the players and follow the instructions based on which statement is true.
             */
            else if (userChoice.equals("2")) {
                int generatedNumberDealer = 16 + randomGen.nextInt(11);
                System.out.println("");
                System.out.println("Dealer's hand: " + generatedNumberDealer);
                System.out.println("Your hand is: " + totalGenNumSum);
                System.out.println("");
                if (CARD_COUNT_21 < generatedNumberDealer) {
                    System.out.println("You win!");
                    System.out.println("");
                    gameCountCompleted++;
                    gameCount++;
                    startGame(randomGen, gameCount);
                    playerWins++;
                }

                /**
                 * The program will check to see if the player's hand is equal to the dealer's hand. If it is it will print "It's a tie! No one wins!".
                 * Then it will count games completed, game count, and tie wins.
                 */
                else if (totalGenNumSum == generatedNumberDealer) {
                    System.out.println("It's a tie! No one wins!");
                    System.out.println("");
                    totalGenNumSum = 0;
                    gameCountCompleted++;
                    gameCount++;
                    startGame(randomGen,gameCount);
                    tieWins++;
                }
                else if (generatedNumberDealer > totalGenNumSum) {
                    System.out.println("Dealer wins!");
                    System.out.println("");
                    totalGenNumSum = 0;
                    gameCountCompleted++;
                    gameCount++;
                    startGame(randomGen,gameCount);
                    dealerWins++;
                }

                /**
                 * The program will be checking to see if the dealer's hand is less than the player's hand. If it is
                 * then it will print out "You win!" and count games completed, game count, and player wins.
                 */
                else if (generatedNumberDealer < totalGenNumSum) {
                    System.out.println("You win!");
                    System.out.println("");
                    totalGenNumSum = 0;
                    gameCountCompleted++;
                    gameCount++;
                    startGame(randomGen,gameCount);
                    playerWins++;
                }
            }
            else if (userChoice.equals("3")) {
                percentPlayerWins = ((playerWins * 100) / (gameCountCompleted));
                System.out.println("");
                System.out.println("Number of Player wins: " + playerWins);
                System.out.println("Number of Dealer wins: " + dealerWins);
                System.out.println("Number of tie games: " + tieWins);
                System.out.println("Total # of games played is: " + gameCountCompleted);
                System.out.println("Percentage of Player wins: " + percentPlayerWins + "%");
                System.out.println("");
            }

            /**
             * The purpose of this is to allow the player to exit the game. If the player chooses to input the number
             * 4 then the game will end.
             */
            else if (userChoice.equals("4")){
                break;
            }
            else {
                System.out.println("");
                System.out.println("Invalid input!");
                System.out.println("Please enter an integer value between 1 and 4.");
                System.out.println("");
            }
        }
    }
    private static void displayMenu(Scanner scr) {
        System.out.println("1. Get another card");
        System.out.println("2. Hold hand");
        System.out.println("3. Print statistics");
        System.out.println("4. Exit");
        System.out.println("");
        System.out.print("Choose an option: ");
        userChoice = scr.next();
    }
    private static void startGame(P1Random randomGen, int gameCount) {
        totalGenNumSum = 0;
        gameCount++;
        System.out.println("START GAME #" + gameCount);
        pickCard(randomGen);
    }

    /**
     * Here the program knows how to pick a new card (random number or randomGen) and then add it to the hand (totalGenNumSum)
     * and print out "Your card is a (randomGen)!" and "Your hand is (totalGenNumSum).
     */
    private static void pickCard(P1Random randomGen) {
        generatedNumber = 1 + randomGen.nextInt(13);
        totalGenNumSum += getCardValue(generatedNumber);
        System.out.println("");
        System.out.println("Your card is a " + getCardName(generatedNumber) + "!");
        System.out.println("Your hand is: " + totalGenNumSum);
        System.out.println("");
    }

    /**
     *This allows for the program to change the generatedNumber's names of the values 1, 11, 12, and 13
     * to the words ACE, Jack, Queen, and King.
     */
    private static String getCardName(int generatedNumber) {
        switch (generatedNumber) {

            case 1: {
                return ("ACE");
            }
            case 11: {
                return ("JACK");
            }
            case 12: {
                return ("QUEEN");
            }
            case 13: {
                return ("KING");
            }
            default: {
                return Integer.toString(generatedNumber);
            }
        }
    }
    private static int getCardValue(int cardNum) {
        if (cardNum == 11 || cardNum == 12 || cardNum == 13) {
            return 10;
        }
        return cardNum;
    }
}

