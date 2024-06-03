import java.util.Scanner;

public class Calculator {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("Введите арифметическое выражение (например, 1 + 2):");
        String input = scanner.nextLine();

        String[] parts = input.split(" ");
        
        if(parts.length != 3) {
            throw new IllegalArgumentException("Неправильный формат ввода!");
        }

        String num1 = parts[0];
        String operator = parts[1];
        String num2 = parts[2];

        int number1, number2;
        boolean isRoman = Character.isLetter(num1.charAt(0)) || Character.isLetter(num2.charAt(0));

        int maxAllowedNumber = isRoman ? 10 : 10;
        int minAllowedNumber = 1;

        if (isRoman) {
            number1 = RomanNumber.romanToArabic(num1);
            number2 = RomanNumber.romanToArabic(num2);
        } else {
            number1 = Integer.parseInt(num1);
            number2 = Integer.parseInt(num2);

            if (number1 < minAllowedNumber || number1 > maxAllowedNumber || number2 < minAllowedNumber || number2 > maxAllowedNumber) {
                throw new IllegalArgumentException("Числа должны быть в диапазоне от 1 до 10 включительно!");
            }
        }

        int result;
        switch (operator) {
            case "+":
                result = number1 + number2;
                break;
            case "-":
                result = number1 - number2;
                break;
            case "*":
                result = number1 * number2;
                break;
            case "/":
                if(number2 == 0) {
                    throw new IllegalArgumentException("Деление на ноль!");
                }
                result = number1 / number2;
                break;
            default:
                throw new IllegalArgumentException("Недопустимая операция!");
        }

        if (isRoman) {
            System.out.println("Результат: " + RomanNumber.arabicToRoman(result));
        } else {
            System.out.println("Результат: " + result);
        }
    }
}
\\Далее класс с римскими цифрами

import java.util.HashMap;

public class RomanNumber {
    private static final HashMap<Character, Integer> romanMap = new HashMap<>();

    static {
        romanMap.put('I', 1);
        romanMap.put('V', 5);
        romanMap.put('X', 10);
        romanMap.put('L', 50);
        romanMap.put('C', 100);
        romanMap.put('D', 500);
        romanMap.put('M', 1000);
    }

    public static int romanToArabic(String romanNumber) {
        int result = 0;
        int prevValue = 0;

        for (int i = romanNumber.length() - 1; i >= 0; i--) {
            int value = romanMap.get(romanNumber.charAt(i));

            if (value < prevValue) {
                result -= value;
            } else {
                result += value;
            }

            prevValue = value;
        }

        if (!arabicToRoman(result).equals(romanNumber)) {
            throw new IllegalArgumentException("Результат отрицательный!");
        }

        return result;
    }

    public static String arabicToRoman(int number) {
        if (number < 1) {
            throw new IllegalArgumentException("Римские числа меньше 1 недопустимы!");
        }

        StringBuilder result = new StringBuilder();

        int[] numbers = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};

        int i = 0;
        while (number > 0) {
            if (number - numbers[i] >= 0) {
                result.append(symbols[i]);
                number -= numbers[i];
            } else {
                i++;
            }
        }

        return result.toString();
    }
}
