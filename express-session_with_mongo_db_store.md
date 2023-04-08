# Sessions Integrieren:

## Vorbereitungen:
- als erstes installieren wir das session  Modul über **npm install express-session**
  
- Anschließend benötigen wir connect mongo um den sessionstorage an die mongo datenbank zu legen.
  installiert wird dies über **npm install connect-mongo**.

## Implementation:

- Wir erstellen uns zwei Variablen in denen wir unsere beiden zuvor installieten Module i portieren.
    **const session = require('express-session');**
    **const MongoStore = require(‘connect-mongo’)(session);**

- Als nächstes geben wir unserem Server die benötigten app.use informationen mit allen Optionen welche benötigt werden. unter der Sessionoption store erstellen initialisieren wir einen neuen MongoStore() in dessen Objekt wir unseren connectionstring unsere session ablaufzeit, in sekunden, minuten, stunden, tagen, angeben(ttl). Mit autoRemove wird angegeben das der sessioneintrag in unserer MongodB nach ablauf der sessiongültigkeit(ttl[time to live]) gelöscht wird.
    ```
    app.use(session(
        secret: 'SECRET KEY',
        resave: false,
        saveUninitalized: true,
        store: new MongoStore({
            url: 'mongodb://<username>:<password>@<ip>:<port>'
            ttl: 14*24*60*60
            autoRemove: 'native
        })
    ))
    ```