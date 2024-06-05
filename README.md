import java.util.Scanner;
import java.util.Collections;
import java.util.List;
import java.util.ArrayList;

public class MemoryGame {
    private static final int SIZE = 4;
    private static final String HIDDEN = "*";
    private static String[][] board = new String[SIZE][SIZE];
    private static boolean[][] revealed = new boolean[SIZE][SIZE];

    public static void main(String[] args) {
        // Initialize the board with pairs of numbers
        initializeBoard();
        
        // Scanner to read player input
        Scanner scanner = new Scanner(System.in);

        while (!isGameWon()) {
            // Display the current state of the board
            displayBoard();

            // Get first coordinate input from player
            System.out.println("Enter the coordinates of the first cell (row and column): ");
            int row1 = scanner.nextInt();
            int col1 = scanner.nextInt();

            // Get second coordinate input from player
            System.out.println("Enter the coordinates of the second cell (row and column): ");
            int row2 = scanner.nextInt();
            int col2 = scanner.nextInt();

            // Reveal the chosen cells
            revealed[row1][col1] = true;
            revealed[row2][col2] = true;

            // Display the board with the revealed cells
            displayBoard();

            // Check if the revealed cells match
            if (!board[row1][col1].equals(board[row2][col2])) {
                // If they do not match, hide them again
                System.out.println("No match! Try again.");
                revealed[row1][col1] = false;
                revealed[row2][col2] = false;
            } else {
                // If they match, keep them revealed
                System.out.println("Match found!");
            }
        }

        // When all pairs are found, the player wins
        System.out.println("Congratulations! You've won the game!");
    }

    private static void initializeBoard() {
        // Create a list of pairs of numbers from 1 to 8
        List<String> numbers = new ArrayList<>();
        for (int i = 1; i <= SIZE * SIZE / 2; i++) {
            numbers.add(String.valueOf(i));
            numbers.add(String.valueOf(i));
        }

        // Shuffle the numbers to randomize their positions
        Collections.shuffle(numbers);

        // Fill the board with the shuffled numbers
        int index = 0;
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                board[i][j] = numbers.get(index++);
                revealed[i][j] = false;
            }
        }
    }

    private static void displayBoard() {
        // Print the board with hidden or revealed numbers
        System.out.println("Board:");
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                if (revealed[i][j]) {
                    System.out.print(board[i][j] + " ");
                } else {
                    System.out.print(HIDDEN + " ");
                }
            }
            System.out.println();
        }
    }

    private static boolean isGameWon() {
        // Check if all pairs have been found
        for (int i = 0; i < SIZE; i++) {
            for (int j = 0; j < SIZE; j++) {
                if (!revealed[i][j]) {
                    return false;
                }
            }
        }
        return true;
    }
}
