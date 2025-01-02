import java.math.BigInteger;

public class SecretSharing {

    // Method to decode the base-N encoded string to decimal
    public static BigInteger decodeBaseN(String base, String value) {
        int baseValue = Integer.parseInt(base, 10);
        return new BigInteger(value, baseValue);
    }

    // Method to calculate the constant term 'c' using Lagrange Interpolation
    public static BigInteger calculateConstantTerm(BigInteger[] xValues, BigInteger[] yValues) {
        BigInteger constantTerm = BigInteger.ZERO;
        int n = xValues.length;

        for (int i = 0; i < n; i++) {
            BigInteger term = yValues[i];
            BigInteger numerator = BigInteger.ONE;
            BigInteger denominator = BigInteger.ONE;

            // Lagrange basis polynomial
            for (int j = 0; j < n; j++) {
                if (i != j) {
                    numerator = numerator.multiply(BigInteger.ZERO.subtract(xValues[j]));  // x - x_j
                    denominator = denominator.multiply(xValues[i].subtract(xValues[j])); // x_i - x_j
                }
            }
            // Multiply the term by the appropriate Lagrange basis polynomial
            term = term.multiply(numerator).divide(denominator);
            constantTerm = constantTerm.add(term);
        }

        return constantTerm.mod(BigInteger.valueOf(256));  // Modulo 256 as required in the problem
    }

    public static void main(String[] args) {
        // Test case 2
        String[] xValues = {"1", "2", "3", "4", "5", "6", "7", "8", "9", "10"};
        String[] yValues = {
            "13444211440455345511", "aed7015a346d63", "6aeeb69631c227c", "e1b5e05623d881f",
            "316034514573652620673", "2122212201122002221120200210011020220200",
            "20120221122211000100210021102001201112121", "20220554335330240002224253",
            "45153788322a1255483", "1101613130313526312514143"
        };
        String[] bases = {"6", "15", "15", "16", "8", "3", "3", "6", "12", "7"};

        // Decode the y-values from different bases
        BigInteger[] decodedYValues = new BigInteger[xValues.length];
        BigInteger[] decodedXValues = new BigInteger[xValues.length];

        for (int i = 0; i < xValues.length; i++) {
            decodedXValues[i] = new BigInteger(xValues[i]);
            decodedYValues[i] = decodeBaseN(bases[i], yValues[i]);
        }

        // Calculate the constant term 'c' using Lagrange Interpolation
        BigInteger constantTerm = calculateConstantTerm(decodedXValues, decodedYValues);

        System.out.println("Test case result (constant term c): " + constantTerm);
    }
}
