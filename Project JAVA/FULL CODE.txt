import java.io.*;
import java.util.*;

public class EmployeeAttendanceTracker {
    private static final String DATA_FILE = "attendance_data.txt";

    static class Employee {
        String id;
        String name;
        String status; // "Present" or "Absent"

        public Employee(String id, String name, String status) {
            this.id = id;
            this.name = name;
            this.status = status;
        }

        @Override
        public String toString() {
            return id + "," + name + "," + status;
        }
    }

    private static Map<String, Employee> employees = new HashMap<>();

    public static void main(String[] args) {
        loadData();

        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("\nEmployee Attendance Tracker");
            System.out.println("1. Add Employee");
            System.out.println("2. Mark Attendance");
            System.out.println("3. View All Employees");
            System.out.println("4. Delete Employee");
            System.out.println("5. Save & Exit");
            System.out.print("Choose an option: ");
            int choice = sc.nextInt();
            sc.nextLine();  // consume newline

            switch (choice) {
                case 1:
                    addEmployee(sc);
                    break;
                case 2:
                    markAttendance(sc);
                    break;
                case 3:
                    viewAllEmployees();
                    break;
                case 4:
                    deleteEmployee(sc);
                    break;
                case 5:
                    saveData();
                    System.out.println("Data saved. Exiting...");
                    System.exit(0);
                default:
                    System.out.println("Invalid choice. Try again.");
            }
        }
    }

    private static void addEmployee(Scanner sc) {
        System.out.print("Enter Employee ID: ");
        String id = sc.nextLine();
        if (employees.containsKey(id)) {
            System.out.println("Employee ID already exists.");
            return;
        }
        System.out.print("Enter Employee Name: ");
        String name = sc.nextLine();
        Employee emp = new Employee(id, name, "Absent");
        employees.put(id, emp);
        System.out.println("Employee added successfully.");
    }

    private static void markAttendance(Scanner sc) {
        System.out.print("Enter Employee ID: ");
        String id = sc.nextLine();
        Employee emp = employees.get(id);
        if (emp == null) {
            System.out.println("Employee not found.");
            return;
        }
        System.out.print("Mark as (1) Present or (2) Absent: ");
        int statusChoice = sc.nextInt();
        sc.nextLine(); // consume newline
        if (statusChoice == 1) {
            emp.status = "Present";
        } else if (statusChoice == 2) {
            emp.status = "Absent";
        } else {
            System.out.println("Invalid choice.");
            return;
        }
        System.out.println("Attendance updated.");
    }

    private static void viewAllEmployees() {
        System.out.println("\nEmployee List:");
        for (Employee emp : employees.values()) {
            System.out.println("ID: " + emp.id + ", Name: " + emp.name + ", Status: " + emp.status);
        }
    }

    private static void deleteEmployee(Scanner sc) {
        System.out.print("Enter Employee ID to delete: ");
        String id = sc.nextLine();
        if (employees.containsKey(id)) {
            employees.remove(id);
            System.out.println("Employee deleted successfully.");
        } else {
            System.out.println("Employee not found.");
        }
    }

    private static void loadData() {
        File file = new File(DATA_FILE);
        if (!file.exists()) {
            return;
        }
        try (BufferedReader br = new BufferedReader(new FileReader(file))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                if (parts.length == 3) {
                    Employee emp = new Employee(parts[0], parts[1], parts[2]);
                    employees.put(emp.id, emp);
                }
            }
        } catch (IOException e) {
            System.out.println("Error loading data: " + e.getMessage());
        }
    }

    private static void saveData() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(DATA_FILE))) {
            for (Employee emp : employees.values()) {
                bw.write(emp.toString());
                bw.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error saving data: " + e.getMessage());
        }
    }
}