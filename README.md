orafile
=======

Java for dealing with Oracle SQL*Net .ora files

Example usage
-------------

```java
import org.codeswarm.orafile.*;
import java.util.*;

public class OrafileDemo {

    public static void main(String[] args) throws Exception {

        // The sort of thing you might find in tnsnames.ora
        String tnsFileContent = "TheApplication =\n" +
            "  (ADDRESS = (PROTOCOL = TCP)(HOST = app-server)(PORT = 1521))\n" +
            "  (ADDRESS = (PROTOCOL = TCP)(HOST = app-server)(PORT = 1522))\n" +
            "  (CONNECT_DATA =(SID = banana)(SERVER = dedicated)))";

        // Parse the string.
        OrafileDict tns = Orafile.parse(tnsFileContent);

        System.out.println(tns); /* Output:

        THEAPPLICATION
          ADDRESS
            PROTOCOL: TCP
            HOST: app-server
            PORT: 1521
          ADDRESS
            PROTOCOL: TCP
            HOST: app-server
            PORT: 1522
          CONNECT_DATA
            SID: banana
            SERVER: dedicated
        */

        // Get the entry for "theapplication".
        OrafileVal theapplication = tns.get("theapplication").get(0);

        // Find ADDRESS entries and their HOST, PORT, and SID attributes.
        List<Map<String, String>> values = theapplication
            .findParamAttrs("address", Arrays.asList("host", "port", "sid"));

        System.out.println(values); /* Output:

        [{port=1521, sid=banana, host=app-server}, {port=1522, sid=banana, host=app-server}]
        */

        System.out.println(new OrafileRenderer().renderFile(tns)); /* Output:

        TheApplication =
          (ADDRESS =
            (PROTOCOL = TCP)
            (HOST = app-server)
            (PORT = 1521)
          )
          (ADDRESS =
            (PROTOCOL = TCP)
            (HOST = app-server)
            (PORT = 1522)
          )
          (CONNECT_DATA =
            (SID = banana)
            (SERVER = dedicated)
          )
        */
    }

}
```

Download
--------

Orafile is available from Maven Central.

```xml
<dependency>
  <groupId>org.codeswarm</groupId>
  <artifactId>orafile</artifactId>
  <version>1.1.1</version>
</dependency>
```

Release history
---------------

* 1.0 - Initial release
* 1.1 - [Add rendering feature](https://github.com/codeswarm/orafile/issues/1)
* 1.1.1 - [Java 6 compatibility](https://github.com/codeswarm/orafile/issues/2),
          [quote empty strings](https://github.com/codeswarm/orafile/issues/4),
          [fix string escaping](https://github.com/codeswarm/orafile/issues/9)
