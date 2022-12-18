# Lab 4

## Zadanie
Dokończ projekt. Twoim zadaniem jest napisać [WebCrawler](https://pl.wikipedia.org/wiki/Robot_internetowy). 

Nasz mały Crawler powinien definiować następujące klasy pomocnicze:
* `CrawlerThread` - klasę służącą do wykonywania operacji parsowania aktualnie przeglądanej strony.
* `DBConn` - klasę do obsługi bazy danych (SQLite), powinna udostępniać następujące metody: `connect`, `createTables`, `insertRow`, `dropTables`, `disconnect`. 

**Crawler powinien wyszukiwać wszystkie linki na stronie, zapisywać znalezione linki do bazy danych w celu ich późniejszego odwiedzenia. Jeżeli link już znajduje się w bazie danych powinna zostać zwiększona wartość w kolumnie `seen`.**

Crawler powinien korzystać z ograniczonej liczby wątków, w celu ograniczenia liczby wątków użyj klasę `ExecutorService`.

Każdy obiekt klasy `CrawlerThread` powinien pobierać z bazy danych linki które nie zostały jeszcze odwiedzone. Zaproponuj rozwiązanie mające na celu uniknięcie duplikowania odwiedzin linków przez różne wątki.

Do Połączenia z bazą danych wykorzystaj JDBC.

```java
public class WebCrawler {
    public static void main(String ...args) {
        String url = "https://ii.up.krakow.pl/";
        if(args.length == 1) {
            url = args[0];
        }

        startCrawler(url);
    }

    public static void startCrawler(String url) {
        
    }
}
```

Fragment połączenia z bazą danych SQLite i wykonanie operacji:
```java
try(Connection connection = DriverManager.getConnection("jdbc:sqlite:books.db");
    Statement statement = connection.createStatement()) {
    statement.setQueryTimeout(30);  // set timeout to 30 sec.
    statement.executeUpdate("create table book (id integer, title string)");
    statement.executeUpdate("insert into book values(1, 'Java. Podstawy. Wydanie XII')");
    statement.executeUpdate("insert into book values(2, 'Java. Techniki zaawansowane. Wydanie XI')");
    ResultSet rs = statement.executeQuery("select * from book");
    while(rs.next())
    {       
        System.out.println("id: " + rs.getInt("id"));
        System.out.println("title: " + rs.getString("title"));
    }
} catch (SQLException throwables) {
    throwables.printStackTrace();
}
```

Do projektu należy dodać wsparcie dla Frameworku Maven. A następnie dodać do pliku `pom.xml` poniższy fragment pod tagiem `</properties>`.
```xml
    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.xerial/sqlite-jdbc -->
        <dependency>
            <groupId>org.xerial</groupId>
            <artifactId>sqlite-jdbc</artifactId>
            <version>3.40.0.0</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.jsoup/jsoup -->
        <dependency>
            <groupId>org.jsoup</groupId>
            <artifactId>jsoup</artifactId>
            <version>1.15.3</version>
        </dependency>
    </dependencies>
```    

Przykład Parsowania XML przy pomocy biblioteki JSOUP.
```java
import java.io.IOException;
import java.net.URL;
import java.net.URLConnection;
import java.util.Scanner;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.parser.Parser;
import org.jsoup.select.Elements;

public class BankierReaderClient {
    public static void main(String[] args) throws IOException {
        String wiadomosciUrl = "https://www.bankier.pl/rss/wiadomosci.xml";
        URL url = new URL(wiadomosciUrl);
        URLConnection conn = url.openConnection();
        conn.connect();
        Scanner in = new Scanner(conn.getInputStream());
        StringBuilder sb = new StringBuilder();
        while(in.hasNextLine()) {
            sb.append(in.nextLine());
        }
        Document doc = Jsoup.parse(sb.toString(),
                wiadomosciUrl, Parser.xmlParser());
        Elements items = doc.getElementsByTag("item");
        for(Element el : items) {
            String content = el.getElementsByTag("title").first().text();
            System.out.println(content);
        }
    }
}
```

**Przydatne linki:**
* https://github.com/xerial/sqlite-jdbc
* https://www.baeldung.com/java-executor-service-tutorial
* https://www.javatpoint.com/java-executorservice
