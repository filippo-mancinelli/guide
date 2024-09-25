# Come testare localmente il frontend da telegram
Bisogna creare un bot di test e far puntare l'url della sua miniapp verso il tuo ambiente locale. Visto che telegram necessita di URL sicuri con https, dobbiamo creare un tunnel usando localtunel, ngrok, o localhost.run che esponga la porta del nostro frontend locale e generi l'URL sicuro da mettere nel bot di test. Questa parte del processo finale deve essere ripetuta ogni volta che starti il progetto o ogni volta che il link di localtunnel scade o si rompe (lo vedi dal terminale).

#### 1. BotFather
Apri telegram desktop, non web, e vai dal @BotFather -> https://t.me/BotFather 

#### 2. Crea il tuo bot di test personale
fai ** /newbot** e segui i passaggi per creare il bot di test del telefood

#### 3. Starta il backend
`npm run start:dev
`
#### 4. Starta il frontend
`ionic serve`

#### 5. Apri un tunnel
Usa localtunnel sulla porta 8100 usata dal frontend
`lt --port 8100`
Poi copia l'URL generato

#### 6. Crea una nuova mini-app
 fai **/newapp** e segui le indicazioni per creare la miniapp. Incolla l'url copiato prima quando richiesto 

#### 7. Inserisci/Modifica campo url a DB
colonna mini_app_url, metti il link diretto che ti ha dato il botfather, assicurati che abbia lo short name dell'app alla fine, esempio: t.me....../myapp

#### 8. Prova la miniapp local
Vai sul tuo bot di test e lancia la miniapp dal menu button. puoi aprire l'ispeziona elemento col tasto dentro e verificare nei log che l'app riconosca che stia effettivamente runnando dall'ambiente di telegram e non da un browser qualsiasi.

