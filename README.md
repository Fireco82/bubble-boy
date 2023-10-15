# bubble-boy
makes and sorts numbers
import java.io.*;
import java.util.*;

public class SimpleSorter {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(System.in)) {
            while (true) {
                System.out.println("Choose an option:");
                System.out.println("1. Generate and save a random array to a file");
                System.out.println("2. Read and sort an array from a file");
                System.out.println("0. Exit");
                System.out.print("Enter your choice: ");

                int choice = scanner.nextInt();

                if (choice == 0) {
                    System.out.println("You have exited the program.");
                    break;
                }

                if (choice == 1) {
                    System.out.print("Enter the length of the random array: ");
                    int arrayLength = scanner.nextInt();
                    System.out.print("Enter the minimum value (e.g., 0): ");
                    int min = scanner.nextInt();
                    System.out.print("Enter the maximum value (e.g., 100): ");
                    int max = scanner.nextInt();

                    int[] randomArray = createRandomArray(arrayLength, min, max);
                    System.out.println("Random array generated:");
                    printArray(randomArray);

                    System.out.print("Enter a filename to save the array: ");
                    String filename = scanner.next();
                    writeArrayToFile(randomArray, filename);
                    System.out.println("Random array saved to '" + filename + "'");
                } else if (choice == 2) {
                    System.out.print("Enter the filename to read the array: ");
                    String filename = scanner.next();
                    int[] loadedArray = readFileToArray(filename);
                    bubbleSort(loadedArray);
                    System.out.println("Loaded and sorted array from '" + filename + "':");
                    printArray(loadedArray);

                    System.out.print("Enter a filename to save the sorted array: ");
                    String sortedFilename = scanner.next();
                    writeArrayToFile(loadedArray, sortedFilename);
                    System.out.println("Sorted array saved to '" + sortedFilename + "'");
                } else {
                    System.out.println("Invalid choice. Please choose a valid option.");
                }
            }
        }
    }

    public static int[] createRandomArray(int arrayLength, int min, int max) {
        int[] randomArray = new int[arrayLength];
        Random random = new Random();

        for (int i = 0; i < arrayLength; i++) {
            randomArray[i] = random.nextInt((max - min) + 1) + min;
        }

        return randomArray;
    }

    public static void writeArrayToFile(int[] array, String filename) {
        try (PrintWriter writer = new PrintWriter(filename)) {
            for (int element : array) {
                writer.println(element);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static int[] readFileToArray(String filename) {
        List<Integer> integers = new ArrayList<>();
        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                integers.add(Integer.parseInt(line));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        return integers.stream().mapToInt(Integer::intValue).toArray();
    }

    public static void bubbleSort(int[] array) {
        int n = array.length;
        boolean swapped;
        do {
            swapped = false;
            for (int i = 1; i < n; i++) {
                if (array[i - 1] > array[i]) {
                    int temp = array[i - 1];
                    array[i - 1] = array[i];
                    array[i] = temp;
                    swapped = true;
                }
            }
            n--;
        } while (swapped);
    }

    public static void printArray(int[] array) {
        for (int element : array) {
            System.out.println(element);
        }
    }
}
