# Oasis_Task3
The ATM Interface
import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Scanner;

class User {
    private int userId;
    private int pin;
    private double balance;

    public User(int userId, int pin, double balance) {
        this.userId = userId;
        this.pin = pin;
        this.balance = balance;
    }

    public int getUserId() {
        return userId;
    }

    public int getPin() {
        return pin;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        balance = balance+amount;
    }

    public boolean withdraw(double amount) {
        if (balance >= amount) {
            balance =balance- amount;
            return true;
        }
        return false;
    }

    public boolean transfer(User recipient, double amount) {
        if (withdraw(amount)) {
            recipient.deposit(amount);
            return true;
        }
        return false;
    }
}

class Transaction {
    private String type;
    private double amount;
    private Date timestamp;

    public Transaction(String type, double amount) {
        this.type = type;
        this.amount = amount;
        this.timestamp = new Date();
    }

    public String getType() {
        return type;
    }

    public double getAmount() {
        return amount;
    }

    public Date getTimestamp() {
        return timestamp;
    }
}

class TransactionHistory {
    private List<Transaction> transactions;

    public TransactionHistory() {
        transactions = new ArrayList<>();
    }

    public void addTransaction(Transaction transaction) {
        transactions.add(transaction);
    }

    public List<Transaction> getTransactions() {
        return transactions;
    }
}

public class ATM {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Initialize users and transaction history
        User user = new User(12345, 1234, 1000.0);
        TransactionHistory transactionHistory = new TransactionHistory();

        // ATM login
        System.out.print("Enter User ID: ");
        int userId = scanner.nextInt();
        System.out.print("Enter PIN: ");
        int pin = scanner.nextInt();

        if (userId == user.getUserId() && pin == user.getPin()) {
            System.out.println("Login Successful!");

            while (true) {
                System.out.println("\nATM Menu:");
                System.out.println("1. View Transactions History");
                System.out.println("2. Withdraw");
                System.out.println("3. Deposit");
                System.out.println("4. Transfer");
                System.out.println("5. Quit");

                System.out.print("Enter your choice: ");
                int choice = scanner.nextInt();

                switch (choice) {
                    case 1:
                        
                        List<Transaction> transactions = transactionHistory.getTransactions();
                        System.out.println("Transaction History:");
                        for (Transaction transaction : transactions) {
                            System.out.println(transaction.getType() + " - Amount: " + transaction.getAmount() +
                                    " - Timestamp: " + transaction.getTimestamp());
                        }
                        break;
                    case 2:
                        
                        System.out.print("Enter withdrawal amount: ");
                        double withdrawAmount = scanner.nextDouble();
                        if (user.withdraw(withdrawAmount)) {
                            Transaction withdrawTransaction = new Transaction("Withdrawal", withdrawAmount);
                            transactionHistory.addTransaction(withdrawTransaction);
                            System.out.println("Withdrawal successful.");
                        } else {
                            System.out.println("Insufficient balanace in the account.");
                        }
                        break;
                    case 3:
                        
                        System.out.print("Enter deposit amount: ");
                        double depositAmount = scanner.nextDouble();
                        user.deposit(depositAmount);
                        Transaction depositTransaction = new Transaction("Deposit", depositAmount);
                        transactionHistory.addTransaction(depositTransaction);
                        System.out.println("Deposit successful.");
                        break;
                    case 4:
                        
                        System.out.print("Enter recipient's User ID: ");
                        int recipientUserId = scanner.nextInt();
                        System.out.print("Enter amount to be transfered: ");
                        double transferAmount = scanner.nextDouble();

                        // Transferring to another user 
                        if (recipientUserId == user.getUserId()) {
                            System.out.println("Cannot transfer to yourself.");
                        } else {
                            User recipient = new User(67890, 5678, 0.0); // Another user credentials
                            if (user.transfer(recipient, transferAmount)) {
                                Transaction transferTransaction = new Transaction("Transfer", transferAmount);
                                transactionHistory.addTransaction(transferTransaction);
                                System.out.println("Transfer successful.");
                            } else {
                                System.out.println("Insufficient balance in the account.");
                            }
                        }
                        break;
                    case 5:
                        
                        System.out.println("Thank you for using the ATM.");
                        System.exit(0);
                    default:
                        System.out.println("Invalid choice. Please try again.");
                }
            }
        } else {
            System.out.println("Login Failed. Please check your User ID and PIN.");
        }
    }
}
