# Lab 3
## Klient
Przykład klienta z użyciem `Socket`:
```java
import java.io.IOException;
import java.net.Socket;
import java.nio.charset.StandardCharsets;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) throws IOException {
	    try (Socket s = new Socket("time-a.nist.gov", 13)) {
	        Scanner in = new Scanner(s.getInputStream(), StandardCharsets.UTF_8);
	        while(in.hasNextLine()) {
	            System.out.println( in.nextLine() );
            }
        }
    }
}
```
Przykład klienta z użyciem `URLConnection`:
```java
import java.io.*;
import java.net.URL;
import java.net.URLConnection;
import java.util.Scanner;

public class URLConnectionTest {
    public static void main(String[] args) throws IOException {
        URL url = new URL("https://ibii.up.krakow.pl/");
        URLConnection conn = url.openConnection();
        conn.setRequestProperty("User-Agent", "Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:47.0) Gecko/20100101 Firefox/47.0");
        conn.setRequestProperty("Content-Type", "text/html");
        conn.connect();
        try (Scanner in = new Scanner(conn.getInputStream())) {
            System.out.println("Saving to the file.");
            try(FileWriter fw = new FileWriter("homepage.html")) {
                while(in.hasNextLine()) {
                    fw.write(in.nextLine());
                }
            }
        }
    }
}
```

### Zadania
1. Napisz program który znajdzie adres IP podanego adresu hosta.
2. Napisz klienta który połączy się z dowolną stroną WWW i pokaże jej zawartość. 
3. Napisz klienta który połączy się z API (https://jsonplaceholder.typicode.com/) i wyświetl dostępne posty (GET https://jsonplaceholder.typicode.com/posts). 
4. Napisz klienta który doda nowy POST `{ "title": "foo", "body": "bar" }` i wyświetli informacje zwrotną z serwera.
6. Napisz klienta który wyświetli aktualne informacje pogodowe dla Twojego miasta (lub poziom zanieczyszczeń w powietrzu).
7. Napisz klienta do pobierania plików. Użytkownik powinien móc wprowadzić ścieżkę URL do pliku do pobrania. Pliki zapisz w domyślnej lokalizacji.
8. 🏠 Projekt do domu: Napisz klienta który na podstawie informacji o adresie stworzy mapę. Adres i mapę zapisz w pliku html. Do zadania spróbuj wykorzystać dostępne OpenStreetMap API. (Ewentualnie zamiast adresu mogą być współrzędne).

**Pomocnicze linki:**
* 📖 https://www.baeldung.com/a-guide-to-java-sockets
* 📖 https://www.baeldung.com/java-9-http-client
* 📖 https://docs.oracle.com/javase/7/docs/api/java/net/Socket.html
* 📖 https://docs.oracle.com/javase/8/docs/api/java/net/URLConnection.html

## Server
