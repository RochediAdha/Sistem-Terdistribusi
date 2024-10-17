# Remote Method Invocation (RMI)

Berikut adalah script Java yang menggambarkan penerapan praktis Remote Method Invocation (RMI) dalam sistem perbankan terdistribusi untuk penanganan transaksi sederhana, seperti pengecekan saldo dan transfer antar akun.

## Langkah-langkah Penerapan Praktis RMI - Sistem Perbankan Terdistribusi

### 1. Interface Remote (`Bank.java`)

Interface ini mendefinisikan metode remote yang bisa dipanggil oleh client, seperti pengecekan saldo dan transfer uang antar akun.

```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface Bank extends Remote {
    double checkBalance(String accountNumber) throws RemoteException;
    String transferFunds(String fromAccount, String toAccount, double amount) throws RemoteException;
}

```

### 2. Implementasi Remote (`BankImpl.java`)

Kelas ini mengimplementasikan interface Bank. Metode checkBalance() dan transferFunds() diimplementasikan di sini untuk simulasi fungsi bank.

```java
import java.rmi.server.UnicastRemoteObject;
import java.rmi.RemoteException;
import java.util.HashMap;

public class BankImpl extends UnicastRemoteObject implements Bank {

    // Simulasi data akun dan saldo
    private HashMap<String, Double> accounts;

    public BankImpl() throws RemoteException {
        super();
        accounts = new HashMap<>();
        // Menambahkan beberapa akun untuk simulasi
        accounts.put("ACC123", 5000.00);
        accounts.put("ACC456", 3000.00);
        accounts.put("ACC789", 7000.00);
    }

    // Mengecek saldo
    public double checkBalance(String accountNumber) throws RemoteException {
        if (accounts.containsKey(accountNumber)) {
            return accounts.get(accountNumber);
        } else {
            throw new RemoteException("Account not found.");
        }
    }

    // Transfer dana antar akun
    public String transferFunds(String fromAccount, String toAccount, double amount) throws RemoteException {
        if (!accounts.containsKey(fromAccount) || !accounts.containsKey(toAccount)) {
            return "One or both accounts not found.";
        }

        if (accounts.get(fromAccount) < amount) {
            return "Insufficient funds in the account.";
        }

        // Proses transfer
        accounts.put(fromAccount, accounts.get(fromAccount) - amount);
        accounts.put(toAccount, accounts.get(toAccount) + amount);
        return "Transfer of $" + amount + " from " + fromAccount + " to " + toAccount + " successful.";
    }
}

```

### 3. Server RMI (`BankServer.java`)

Server RMI ini mendaftarkan objek remote BankImpl ke registry agar bisa diakses oleh client.

```java
import java.rmi.Naming;
import java.rmi.registry.LocateRegistry;

public class BankServer {
    public static void main(String[] args) {
        try {
            // Membuat instance BankImpl
            BankImpl bank = new BankImpl();

            // Membuat registry pada port default 1099
            LocateRegistry.createRegistry(1099);

            // Mendaftarkan objek remote ke RMI registry
            Naming.rebind("rmi://localhost/BankService", bank);

            System.out.println("Bank Server is ready.");
        } catch (Exception e) {
            System.out.println("Bank Server failed: " + e);
        }
    }
}

```

### 4. Client RMI (`BankClient.java`)

Client ini mengakses server RMI untuk melakukan pengecekan saldo dan transfer dana antar akun.

```java
import java.rmi.Naming;

public class BankClient {
    public static void main(String[] args) {
        try {
            // Mengakses layanan bank remote
            Bank bank = (Bank) Naming.lookup("rmi://localhost/BankService");

            // Mengecek saldo akun
            System.out.println("Checking balance for ACC123:");
            double balance = bank.checkBalance("ACC123");
            System.out.println("Balance: $" + balance);

            // Melakukan transfer dana
            System.out.println("\nTransferring $1000 from ACC123 to ACC456:");
            String transferResult = bank.transferFunds("ACC123", "ACC456", 1000.00);
            System.out.println(transferResult);

            // Mengecek saldo setelah transfer
            System.out.println("\nChecking balance for ACC123 after transfer:");
            balance = bank.checkBalance("ACC123");
            System.out.println("Balance: $" + balance);

            System.out.println("\nChecking balance for ACC456 after receiving transfer:");
            balance = bank.checkBalance("ACC456");
            System.out.println("Balance: $" + balance);

        } catch (Exception e) {
            System.out.println("Bank Client failed: " + e);
        }
    }
}

```

### Cara Menjalankan Program:

#### 1. Compile semua file Java:

```bash
javac Bank.java BankImpl.java BankServer.java BankClient.java
```

#### 2. Jalankan server RMI terlebih dahulu:

```bash
java BankServer
```

#### 3. Di jendela terminal lain, jalankan client RMI:

```bash
java BankClient
```
