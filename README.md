# Design

`
import java.util.Random;
import java.util.Scanner;

public class SnakeAndLadder {
    private static final int WINNING_POSITION = 100;
    private static final int[] SNAKES = {16, 47, 49, 56, 62, 64, 87, 93, 95, 98};
    private static final int[] LADDERS = {1, 4, 9, 21, 28, 36, 51, 71, 80};
    private static final int[] SNAKE_DESTINATIONS = {6, 26, 11, 53, 19, 60, 24, 73, 75, 78};
    private static final int[] LADDER_DESTINATIONS = {38, 14, 31, 42, 84, 44, 67, 91, 100};

    private int player1Position;
    private int player2Position;
    private String player1Name;
    private String player2Name;
    private Random random;
    private Scanner scanner;

    public SnakeAndLadder() {
        player1Position = 0;
        player2Position = 0;
        random = new Random();
        scanner = new Scanner(System.in);
    }

    public void startGame() {
        System.out.println("Welcome to Snake and Ladder Game!");
        System.out.print("Enter Player 1 Name: ");
        player1Name = scanner.nextLine();
        System.out.print("Enter Player 2 Name: ");
        player2Name = scanner.nextLine();

        System.out.println("\nPress Enter to roll the dice...");
        play();
    }

    public void play() {
        boolean player1Turn = true;

        while (player1Position < WINNING_POSITION && player2Position < WINNING_POSITION) {
            System.out.println("\n" + (player1Turn ? player1Name : player2Name) + "'s turn. Press Enter to roll the dice...");
            scanner.nextLine(); // Wait for player to press Enter
            int diceRoll = rollDice();
            System.out.println("You rolled a " + diceRoll);

            if (player1Turn) {
                player1Position = movePlayer(player1Position, diceRoll, player1Name);
            } else {
                player2Position = movePlayer(player2Position, diceRoll, player2Name);
            }

            player1Turn = !player1Turn; // Switch turn
        }

        scanner.close();
    }

    private int rollDice() {
        return random.nextInt(6) + 1; // Roll a dice (1-6)
    }

    private int movePlayer(int position, int diceRoll, String playerName) {
        position += diceRoll;
        if (position > WINNING_POSITION) {
            position -= diceRoll; // Undo the move if it exceeds 100
            System.out.println(playerName + ", you need exactly " + (WINNING_POSITION - position) + " to win!");
        } else {
            System.out.println(playerName + " is now at position " + position);
            position = checkForSnakesAndLadders(position, playerName);
        }

        if (position == WINNING_POSITION) {
            System.out.println("Congratulations, " + playerName + "! You've reached the winning position!");
        }
        return position;
    }

    private int checkForSnakesAndLadders(int position, String playerName) {
        for (int i = 0; i < SNAKES.length; i++) {
            if (position == SNAKES[i]) {
                position = SNAKE_DESTINATIONS[i];
                System.out.println("Oops! " + playerName + " got bitten by a snake. Now at position " + position);
                return position;
            }
        }

        for (int i = 0; i < LADDERS.length; i++) {
            if (position == LADDERS[i]) {
                position = LADDER_DESTINATIONS[i];
                System.out.println("Yay! " + playerName + " climbed a ladder. Now at position " + position);
                return position;
            }
        }
        return position;
    }

    public static void main(String[] args) {
        SnakeAndLadder game = new SnakeAndLadder();
        game.startGame();
    }
}





`
