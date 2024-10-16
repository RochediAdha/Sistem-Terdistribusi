# Remote Method Invocation (RMI)

Berikut adalah script Java yang menggambarkan penerapan praktis Remote Method Invocation (RMI) dalam sistem perbankan terdistribusi untuk penanganan transaksi sederhana, seperti pengecekan saldo dan transfer antar akun.

## Langkah-langkah Penerapan Praktis RMI - Sistem Perbankan Terdistribusi

### Interface Remote (`Bank.java`)

Interface ini mendefinisikan metode remote yang bisa dipanggil oleh client, seperti pengecekan saldo dan transfer uang antar akun.

```java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface Bank extends Remote {
    double checkBalance(String accountNumber) throws RemoteException;
    String transferFunds(String fromAccount, String toAccount, double amount) throws RemoteException;
}

```
