# utilitySuite.java-
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.Optional;
import java.util.Scanner;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.TimeUnit;

public class UtilitySuite {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        boolean running = true;

        while (running) {
            System.out.println("\n--- Comprehensive Utility Suite ---");
            System.out.println("1. String Processor (Demonstrates Streams and Lambdas)");
            System.out.println("2. List Sorter (Demonstrates Streams, Comparators, and Generics)");
            System.out.println("3. Concurrent Task Runner (Demonstrates Concurrency with ExecutorService)");
            System.out.println("4. Optional Demo (Demonstrates Optional for Null Handling)");
            System.out.println("5. Exit");
            System.out.print("Select a tool (1-5): ");

            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    stringProcessor(scanner);
                    break;
                case 2:
                    listSorter(scanner);
                    break;
                case 3:
                    concurrentTaskRunner();
                    break;
                case 4:
                    optionalDemo(scanner);
                    break;
                case 5:
                    running = false;
                    System.out.println("Exiting the Utility Suite. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
        scanner.close();
    }

    // Tool 1: String Processor - Demonstrates Streams, Lambdas, and Method References
    private static void stringProcessor(Scanner scanner) {
        System.out.println("Enter a list of strings (comma-separated): ");
        String input = scanner.nextLine();
        List<String> strings = Arrays.asList(input.split(","));

        // Use streams to process strings: Convert to uppercase, filter by length, and sort
        strings.stream()
               .map(String::toUpperCase)  // Lambda equivalent: s -> s.toUpperCase()
               .filter(s -> s.length() > 3)  // Filter strings longer than 3 characters
               .sorted()  // Natural sorting
               .forEach(System.out::println);  // Method reference for printing
    }

    // Tool 2: List Sorter - Demonstrates Streams, Comparators, and Generics
    private static void listSorter(Scanner scanner) {
        System.out.println("Enter a list of numbers (comma-separated, e.g., 5,3,8,1): ");
        String input = scanner.nextLine();
        List<Integer> numbers = Arrays.stream(input.split(","))
                                      .map(String::trim)  // Clean up input
                                      .map(Integer::parseInt)  // Convert to Integer
                                      .toList();  // Collect to an immutable list

        // Use streams and a custom comparator (lambda) for sorting
        List<Integer> sortedNumbers = numbers.stream()
                                             .sorted(Comparator.reverseOrder())  // Reverse order sorting
                                             .toList();  // Collect results

        System.out.println("Sorted numbers (descending): " + sortedNumbers);
    }

    // Tool 3: Concurrent Task Runner - Demonstrates Concurrency with ExecutorService
    private static void concurrentTaskRunner() {
        System.out.println("Running concurrent tasks...");

        ExecutorService executor = Executors.newFixedThreadPool(4);  // Thread pool with 4 threads

        for (int i = 1; i <= 10; i++) {
            final int taskId = i;
            executor.submit(() -> {  // Lambda for runnable task
                try {
                    Thread.sleep(1000);  // Simulate work
                    System.out.println("Task " + taskId + " completed by thread: " + Thread.currentThread().getName());
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    System.out.println("Task interrupted.");
                }
            });
        }

        executor.shutdown();  // Graceful shutdown
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();  // Force shutdown if not completed
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }

    // Tool 4: Optional Demo - Demonstrates Optional for Null Handling
    private static void optionalDemo(Scanner scanner) {
        System.out.println("Enter a string (or leave empty for null demo): ");
        String input = scanner.nextLine();
        String value = input.isEmpty() ? null : input;  // Simulate potential null

        // Use Optional to safely process the string
        Optional<String> optionalValue = Optional.ofNullable(value);
        String result = optionalValue
                .map(String::toUpperCase)  // If present, convert to uppercase
                .orElse("No value provided");  // Default if absent

        System.out.println("Processed result: " + result);
    }
}
