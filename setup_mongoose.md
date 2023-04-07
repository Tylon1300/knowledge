# Vorbermerkung:
  unter Ubuntu müssen wir erst **Curl** und anschließend **NVM** installieren um die Aktuelle node version installieren zu können. standartmäßig wird über apt-get nur noder version 12 installiert welche nicht mit mongoose und anderen modulen funktioniert.

# Mongodb Connection erstellen:

- Wir verwenden dotenv welches wir über **npm install --save-dev dotenv** installieren. Anschließen legen wir 
  eine datei **.env** an in der wir unseren MongoDB Connection String speichern.

- als nächstes erstellen wir eine Javascript datei welche die Verbindung zur Datenbank etabliert,
  in unserem Fall zbsp. **databaseConnection.js**.

- wir importieren mongoose und .env.config() und erstellen über einen try catch block in dem wir errors abfangen
  eine databseconnection mit unserem connectionstring aus der .env datei. die connection wird in einer als modul exportiereten asyncronen funktion erstellt.

  <span color="blue">
  ```
  require('dotenv').config();
  const mongoose = require('mongoose');

  module.exports.databaseConnection = async() =>
  {
      try
      {
          await mongoose.connect(process.env.CONNECTON_STRING,
              {
                  dbName: 'testing'
              });
          console.log('Databaseconnection established...');
      }
      catch(error)
      {
          console.log(`Databaseconnection coul'd not be established. Exited with error code ${error}.`);
      };
  };
  ```
  </span> 

  # Model erstellen:

  - als nächstes erstellen wir ein Model in unserem fall haben wir uns ja über den connection string mit der database 
    'testing' verbunden. Wir möchten uns in ein bestehendes Model(Collection) einklinken, dies funktioniert am besten 
    wenn wir im Model definieren welche collection wir verwenden möchten.

  - als erstes erstellen wir einen Ordner 'Models' in welchem ich das model mit dem namen **dbOperators.js** speichere.

  - darin importieren wir **mongoose** und erstellen ein neues **mongoose.schema()** ind dem wir die Spalten des Models 
    definieren und deren Datentypen. Diese müssen, da wir ja an eine bestehende Collection anknüpfen wollen, genau den Spalten und Datentypen der Collection entsprechen. anschließend wähle ich noch die Collection verknüfen. Dies geschieht in einem Zweiten Javascript Object. Zum Schluss müssen wir noch das model als Modul exportieren.
    Dies geht über **module.exports = mongoose.model('bezeichner', {modelname})** 

    <span color="blue">
    ```
    const mongoose = require('mongoose');

    const dbOperatorsModel = new mongoose.Schema
    (
        {
            name: String,
            level: String,
            experience: Number
        },
        { 
            collection: 'dboperators' 
        }
    );

    module.exports = mongoose.model('dboperators', dbOperatorsModel);
    ```
    </span>

    - Zum schluss müssen wir das model noch in unser app.js mainfile importieren und anschließend können wir auf die Datenbankeinträge zum beispiel mit **{databasename}.find({})** abrufen oder mit 
    **
    const data = new {databasename({objects})}
    **
    {databasename}.save(data);
    
    neue erstellen.