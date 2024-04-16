import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.Random;

public class Main {

    public static void main(String[] args) {
        DataGenerator dataGenerator = new DataGenerator();
        dataGenerator.generateFiles();
        dataGenerator.generateSalesReport();
    }
}

class DataGenerator {

    private static final int MAX_PRODUCT_ID = 10;
    private static final int MAX_QUANTITY = 10;
    private static final double MIN_PRICE = 10.0;
    private static final double MAX_PRICE = 100.0;
    private static final int RANDOM_SALES_COUNT = 10;
    private static final int PRODUCTS_COUNT = 5;
    private static final int SALESMAN_COUNT = 3;

    public void generateFiles() {
        boolean success = createSalesMenFiles(RANDOM_SALES_COUNT, SALESMAN_COUNT);
        if (success) {
            success = createProductsFile(PRODUCTS_COUNT);
            if (!success) {
                System.err.println("Error al generar el archivo de productos.");
            }
        } else {
            System.err.println("Error al generar los archivos de ventas de vendedores.");
        }
    }

    public void generateSalesReport() {
        Map<String, Integer> salesBySalesman = getSalesBySalesman();
        System.out.println("Reporte de ventas por vendedor:");
        for (Map.Entry<String, Integer> entry : salesBySalesman.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue() + " productos vendidos");
        }

        Map<String, Integer> salesByProduct = getSalesByProduct();
        System.out.println("\nReporte de productos vendidos por cantidad:");
        for (Map.Entry<String, Integer> entry : salesByProduct.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue() + " unidades vendidas");
        }
    }

    private boolean createSalesMenFiles(int randomSalesCount, int salesmanCount) {
        try {
            Random random = new Random();
            for (int i = 1; i <= salesmanCount; i++) {
                String fileName = "Vendedor_" + i + ".txt";
                try (BufferedWriter writer = new BufferedWriter(new FileWriter(fileName))) {
                    for (int j = 1; j <= randomSalesCount; j++) {
                        String saleData = generateRandomSaleData(random);
                        writer.write(saleData);
                        writer.newLine();
                    }
                }
            }
            return true;
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
    }

    private String generateRandomSaleData(Random random) {
        String[] documentTypes = {"DNI", "CI", "PASSPORT"};
        String documentType = documentTypes[random.nextInt(documentTypes.length)];
        int documentNumber = 10000000 + random.nextInt(90000000);
        int productCount = 1 + random.nextInt(5);
        StringBuilder saleData = new StringBuilder(documentType + ";" + documentNumber);
        for (int i = 1; i <= productCount; i++) {
            int productId = 1 + random.nextInt(MAX_PRODUCT_ID);
            int quantity = 1 + random.nextInt(MAX_QUANTITY);
            saleData.append("\n").append("IDProducto").append(productId).append(";").append(quantity);
        }
        return saleData.toString();
    }

    private boolean createProductsFile(int productsCount) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("Productos.txt"))) {
            for (int i = 1; i <= productsCount; i++) {
                String productName = "Producto_" + i;
                double pricePerUnit = MIN_PRICE + (Math.random() * (MAX_PRICE - MIN_PRICE)); 
                writer.write("IDProducto" + i + ";" + productName + ";" + pricePerUnit);
                writer.newLine();
            }
            return true;
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }
    }

    private Map<String, Integer> getSalesBySalesman() {
        Map<String, Integer> salesBySalesman = new HashMap<>();
        try {
            for (int i = 1; i <= SALESMAN_COUNT; i++) {
                String fileName = "Vendedor_" + i + ".txt";
                // Leer el archivo del vendedor
                // Procesar las ventas y sumar las cantidades vendidas por vendedor
                // Aquí se simula la lectura del archivo y el procesamiento de datos
                int totalSales = 0;
                for (int j = 0; j < RANDOM_SALES_COUNT; j++) {
                    totalSales += (1 + (int) (Math.random() * MAX_QUANTITY)); // Sumar una cantidad aleatoria
                }
                salesBySalesman.put("Vendedor_" + i, totalSales);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return salesBySalesman;
    }

    private Map<String, Integer> getSalesByProduct() {
        Map<String, Integer> salesByProduct = new HashMap<>();
        try {
            // Leer el archivo de ventas de todos los vendedores
            // Procesar las ventas y sumar las cantidades vendidas por producto
            // Aquí se simula la lectura del archivo y el procesamiento de datos
            for (int i = 1; i <= PRODUCTS_COUNT; i++) {
                salesByProduct.put("Producto_" + i, (1 + (int) (Math.random() * MAX_QUANTITY)) * RANDOM_SALES_COUNT); // Cantidad aleatoria por producto
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return salesByProduct;
    }
}
