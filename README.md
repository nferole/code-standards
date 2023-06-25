# Feroles programmeringsprinciper och SOLID-fusklapp
Det som följer är min fusklapp vad gäller SOLID-principer och generella QoL tips när det kommer till programmering. Det har utvecklats som hjälpmedel för mina programmeringskvällar där jag sitter med mer juniora utvecklare och knackar kod. **OBS: Jag är på inget sätt världens bästa programmerare eller en absolut auktoritet!** så om du känner att du har förbättringsförslag så skapa gärna en PR så lär jag mig något nytt! ❤️

# 1. Generell filosofi
## 1. Bra kod låter utvecklaren välja sin nivå av engagemang med koden
Dålig kod kommer tvinga dig att lusläsa varje rad och ta dig igenom många snåriga och stora metoder, klasser och filer för att förstå det generella flödet, logiken och output. Bra kod är strukturerad på ett sådant sätt att man lätt kan välja vilken nivå av engagemang man vill ha med koden. Detta åstadkommer du förslagsvis genom att stycka upp din kod i relevanta komponenter med bra namngivning.

## 2. Bra kod möjliggör utveckling utan att man besitter djup domänkunskap
Dålig kod tvingar dig att lära dig mycket om en lösning, organisation och deras historia för att förstå hur man ska använda existerande funktionalitet eller skapa ny funktionalitet. T.ex. genom att det finns en uppsjö av sätt att göra en och samma sak så som att hämta ut ordrar, produkter eller patientinformation, utan att tydligt markera vad som är utdaterat, deprecierat och dublerat. Bra kod är levande, dokumenterad och tydlig.

## 3. Bra kod följer det generella paradigm som lösningen har, t.ex. objektorientering
Dålig kod ger sken av att följa ett paradigm men följer egentligen ett helt annat paradigm. Det vanligaste exemplet som jag stött på under de senaste 7+ åren av programmering är att folk använder objektorienterad programmering som om det vore samma sak eller nästan samma sak som procedurisk programmering, vilket ofta gör valet av paradigm meningslöst och skapar kod med sämre kvalité. När ett paradigm har valts är det därför viktigt att du försäkrar dig om vad det paradigmet innefattar och hur man använder sig av det.

## 4. Bra kod möjliggör att du kan lägga till funktionalitet utan att testa om oförändrad funktionalitet
Dålig kod har massiva metoder eller funktioner med en stor mängd logik och många ansvarsområden. Att ändra något i en sådan komponent kan skapa oförutsedda konsekvenser eftersom komponenten inte är överskådlig. Detta kräver att man testar om existerande  såväl som ny eller förändrad funktionalitet. Bra kod kommer runt detta genom att vara skriven på ett sådant sätt att existerande produktionssatt funktionalitet sällan eller aldrig ändras och ny funktionalitet läggs till som en utökning, dvs. Open/Closed principle.

## 5. Bra kod är oftast skriven i sammarbete med andra
Det mesta jag har lärt mig har jag gjort genom att sammarbeta med andra. Vissa kanske är genier men även dem kan växa och lära sig genom att arbeta med andra. Eftersom mjukvaruindustrin ändras radikalt kontinuerligt är det viktigt att vi lär oss att jobba tillsammans och lära från varandra.

## 6. Bra kod lever, förändras och refaktoreras kontinuerligt och vid behov
Skriv inte onödiga abstraktioner och hugg sönder koden om den är kort. Men ha alltid med dig att bra kod måste få leva, refaktoreras om och förändras när behoven uppstår. Att ändra kod, abstraktera kod och till och med ändra arkitektur helt eller delvis måste få vara del av utvecklingen. Det är lika väsentligt som att byta olja och filter i bilen med viss regelbundenhet.

## 7. Bra kod har en liturgi och den stavas "enhetstester"
Precis som liturgi i religiösa sammanhang låter folk åminnas viktiga lärdomar för att frigöra kapacitet till att skapa nytt så har bra kod automatiserade tester som sin egna liturgi. Det låter oss vidareutveckla kod förvissade om att det finns ett skyddsnät som fångar upp våra misstag vilket i sin tur hjälper oss att fokusera blicken framåt och skapa nya funktioner och bättre lösningar.

**Den grundläggande idén bakom dessa principer är att vi vill avsluta våra arbetsdagar utan huvudvärk och lämna efter oss kod som inte blir någon annans huvudvärk.**

# 2. Generella DO's and DON'ts
## 2.1 Namn på klasser, metoder och properties
Ge komponenter bra, deskriptiva namn och följ [Microsofts guide för kodstil.](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/identifier-names)

## 2.2 IF-satser 

**Feroles huvudregel för If-satser är:** 
*Bra If-satser låter utvecklaren välja att fokusera på antingen flödet eller logiken.*

Dåliga if-satser tvingar oss helt enkelt att lusläsa kod oavsett om vi vill förstå flöde eller logik. If-satser måste konstrueras på ett sådant vis att en programmerare inte behöver anstränga sig för att förstå vad deras uppgift är. Följande är alltså ett **dåligt exempel** eftersom man måste lusläsa koden för att förstå if-satsens uppgift:
```csharp
if(Attachment != null && data != null && _user.UserPermissions.Contains("W") && AllowedFileTypes.Contains(Attachment.FileType))
{
    Attachment.Upload(data);
}
```
Och följande är ett **bättre exempel**:
```csharp
if (IsDataAndPermissionsValid(data))
{
    Attachment.Upload(data);
}
```
```csharp
private bool IsDataAndPermissionsValid(byte[] data)
{
    return Attachment != null && data != null && _user.UserPermissions.Contains("W") && AllowedFileTypes.Contains(Attachment.FileType);
}
```
Man kanske till och med vill utöka läsbarheten i den nya metoden något:
```csharp
private bool IsDataAndPermissionsValid(byte[] data)
{
    if (!_user.UserPermissions.Contains("W"))
        return false;

    if (Attachment == null || data == null)
        return false;

    if (!AllowedFileTypes.Contains(Attachment.FileType))
        return false;

    return true;
}
```
"Råa" if-satser bör hantera grundläggande datatyper och innefatta en mindre mängd konditioner. Följande är alltså ett **dåligt exempel**:

```csharp
if(IsUserRegistered && ShouldReverifySession && HasTimedOffers && IsInCheckout)
{
   ...
}
```
och följande är ett **bra exempel**:
```csharp
if(IsUserRegistered && IsUserVerified)
{
	...
}
```

Nestade if-satser blir i stort sett alltid en huvudvärk så undvik dessa. Baserat på den principen innebär det att följande är ett **dåligt exempel**:
```csharp
private Note ViewHiddenNote(Guid noteId)
{
	if(noteId != null)
	{
	    if(note.IsHidden)
	    {
	        if(user.CanViewHiddenNote(noteId))
	        {
	            ..
	        }
	    }
	}
}
```
och följande är ett **bättre exempel**:
```csharp
private Note ViewHiddenNote(Guid noteId)
{
   if (noteId == null)
       return null;

   if (note.IsHidden && !user.CanViewHiddenNote(noteId))
       return null;
	
	return _noteService.GetHiddenNote(noteId);
}
```

## 2.3 Try-catch
Att använda try-catch vid tillfällen i koden där det kan uppstå fel kan förhindra att applikationen kraschar samt hjälper till vid felsökning och förtydligande vad som blivit fel. 
Tänk på att:
1. Om det är troligt att ett fel kan uppstå, använd guard statements istället för try-catch. Tänk på att skriva effektiv kod.
**Dåligt exempel**:
```csharp

   try
   {
        dbConnection.Close();
   }
   catch (InvalidOperationsException ex)
   {
        log.WriteToLog(ex);
   }
   catch (exception ex)
   {
        log.WriteToLog(ex);
   }
```

**Bättre exempel**:
```csharp
    
    if (dbConnection.State != ConnectionState.Closed)
        dbconnection.Close;
```

2. Skapa egna exception-klasser enbart när fördefinierade klasser inte räcker.

3. Använd grammatiskt korrekta felmeddelanden.

4. Hantera vanligare Exceptions före ovanligare.


## 2.3.1 Fördefinierade exceptions
Använd fördefinierade Exceptions för att underlätta hanterandet av vanligt förekommande problem.
**Bättre exempel**:
```csharp

    private User? GetUserFromDb(string userId)
    {
        if (string.IsNullOrEmpty(userId)
            throw new ArgumentNullException()

        try
        {
            db.GetUserById(userId);
        }
        catch (ArgumentException ex)
        {
            log.Write("Argument exception thrown in method GetUserFromDb", ex.InnerMessage, ex) 
            throw ex; 
        }
        catch (Exception ex)
        {
            log.Write("Exception thrown in method GetUserFromDb", ex.InnerMessage, ex);
            throw;
        }
    }
```

## 2.3.2 Definiera egen exceptions vid behov
1. Avsluta alltid namnet på Exception-klassen med "Exception".
2. Använd tre konstruktorer i Exception-klassen.

3. Överväg att skapa lokaliserade strängar för exception för applikationer som används över flera länder. 


## 2.4 Metodparametrar
En metod ska ha två, max tre in-parametrar. Ifall du behöver fler parametrar än det så är det antingen dags för att skapa någon form av settings-objekt med standardvärden som fångar de flesta anropen, eller bryta ut hela metoden till en separat klass. Metoder med tio, femton, tjugo parametrar får aldrig förekomma.

## 2.5 Controllers i MVC
Controllers i projekt som följer MVC-mönstret är **framför allt till för routing**. Inte logik, validering och annat. Har du en endpoint där det inte omedelbart blir klart vad det är som händer och vilken vy eller dylikt som servas då är det också dags att överväga att refaktorera om den. Använd middleware för att validera data och services/vad helst du vill för att bygga upp modeller, konvertera DTO:s osv.


# 3. SOLID och du
SOLID är en samling principer som introducerades av Robert Martin i hans artikel "Design Principles and Design Patterns" år 2000. Sedan dess har de blivit ett slags budord som ofta följs inom objektorienterad design. De är inte absoluta regler men dem löser många vanligt förekommande problem som brukar uppstå i projekt med tiden. Med andra ord så kommer du alldeles troligen att bryta mot SOLID. Många väletablerade tekniker gör det pga. begränsningar i tekniken eller ramverket och det kan vara helt okej!

**Observera att kodexemplena som ges främst ska ses som exempel på principer, inte på fantastiskt genomtänkt kod.**


## 1. Single Responsibility Principle
En bra princip är att en komponent ska ha ett ansvar och endast ett ansvar. På det sättet finns det bara en enda anledning att ändra en komponent och i den bästa av världar underlättar det för utvecklaren att hitta i koden, skriva tester och garantera kvalitativ kod.

Baserat på ovan är följande ett **dåligt exempel**:
```csharp
...
public void CreateOrder(Order order)
{
    var orderCreated = _orderService.Create(order);
    
    if(orderCreated) { 
        var smtpClient = new SmtpClient("smtp.gmail.com")
        {
            Port = 587,
            Credentials = new NetworkCredential("email", "password"),
            EnableSsl = true,
        };

        smtpClient.Send(order.Email, order.Recipient.FullName, "New order created", "Your order is on its way!");
    }
}
```
I koden ovan ser vi att CreateOrder metoden har två ansvar: 1. Skapa upp en order 2. Maila en bekräftelse om ordern blir skapad. I fallet ovan bryts SRP genom att en metod har flera ansvarsområden, men det samma skulle gälla om vi bröt ut mail-biten till en egen metod. I det fallet skulle klassen bryta mot SRP.

Ett **bättre exempel** är som följande:
```csharp
...
public void CreateOrder(Order order)
{
    var orderCreated = _orderService.Create(order);
    
    if(orderCreated)
    {
        Emailer.SendOrderConfirmation(order);
    }
}
```
```csharp
public static class Emailer
{
    public static void SendOrderConfirmation(Order order)
    {
        var smtpClient = new SmtpClient("smtp.gmail.com")
        {
            Port = 587,
            Credentials = new NetworkCredential("email", "password"),
            EnableSsl = true,
        };

        smtpClient.Send(order.Email, order.Recipient.FullName, "New order created", "Your order is on its way!");
    }
}
```

Vanliga exempel på när SRP bryts:

 1. Ett produkt-objekt hanterar också bilduppladdning
 2. En checkout (e-handel) hanterar också varukorg, e-post och betalning
 3. Ett användarobjekt hanterar e-postutskick
 4. Verifiering av indata hanteras i en controller

Den generella principen är att en klass bara ska ha **en anledning till att förändras**. Detta kan så klart dras till absurdum och det finns inga klara regler för exakt hur mycket man ska stycka upp en klass utan får avvägas i fall till fall.





## 2. Open/Closed Principle
OCP menar att kod ska vara öppen för utökning men stängd för modifikation. Det enklaste sättet att förklara detta på är ungefär som så att kod som ligger i produktion och är testad inte ska behöva testas om när man lägger till ny funktionalitet. Det vanligaste exemplen som brukar ges på detta innebär i korthet att man ska undvika en massa if-satser och istället använda sig av interfaces. Men detta får ses som ett mer kliniskt exempel. 

**Dåligt** exempel:
```csharp
var field = new DataField()
{
    Id = Guid.NewGuid(),
    Modified = DateTime.Now,
    DataType = DataFieldType.Integer
};
field.SetValue(int.MaxValue);
```
```csharp
public class DataField 
{
    public Guid Id { get; set; }
    public DateTime Modified { get; set; }
    public DataFieldType DataType { get; set; }
    private dynamic _value { get; set; } 
    
    public dynamic GetValue() { 
        
        switch(DataType)
        {
            case DataFieldType.PlainText:
            case DataFieldType.HTML:
                return Convert.ChangeType(_value, TypeCode.String);

            case DataFieldType.Integer:
                return Convert.ChangeType(_value, TypeCode.Int64);
            case DataFieldType.Image:
                return _value as byte[];
        }
        return null;
    }
    public void SetValue(dynamic value)
    {
        _value = value;
    }
}
```
```csharp
public enum DataFieldType
{
    PlainText = 1,
    HTML = 2,
    Image = 3,
    Integer = 4
}
```
Varför är detta ett dåligt exempel enligt OCP? Framför allt för att vi med denna design måste ändra i kod som potentiellt berör alla datatyper i GetValue oavsett vad vi vill ändra. Om vi lägger till en datatyp måste vi alltså modifiera samma switch-statement som berör alla andra datatyper, vilket kan skapa buggar och andra fel. Ifall vi ska iterera över en lista av datafält kan också ett flertal checkar behövas för att bestämma datatyp vilket återupprepar problematiken för framtida kod.

Ett **bättre alternativ** är att använda ett interface för datafälten:

```csharp
var fieldList = new List<IDataField>() {
	new PlainTextDataField()
	{
	    Id = Guid.NewGuid(),
	    Modified = DateTime.Now,
	    Value = "Hello!"
	},
	new IntegerDataField()
	{
	    Id = Guid.NewGuid(),
	    Modified = DateTime.Now,
	    Value = int.MaxValue
	}
};

foreach(IDataField field in fieldList)
{
	Console.Write(field.Value);
}
```

```csharp
public interface IDataField
{
    Guid Id { get; set; }
    DateTime Modified { get; set; }
    dynamic Value { get; set; }
}
```
```csharp
public class PlainTextDataField : IDataField
{
    public Guid Id { get; set; }
    public DateTime Modified { get; set; }
    public dynamic _value { get; set; }
    public dynamic Value { get { return _value as string; } set { _value = value; } }
}
```
```csharp
public class IntegerDataField : IDataField
    {
        public Guid Id { get; set; }
        public DateTime Modified { get; set; }
        public dynamic _value { get; set; }
        public dynamic Value { get { return Convert.ChangeType(_value, TypeCode.Int64); } set { _value = value; } }
    }
```
Med det bättre designmönstret kan vi lägga till hur många nya datafältstyper som helst utan att röra koden som används för de gamla fälten. 

Även OCP kan dras till absurdum där mängder av interfaces skapas helt i onödan. Det är därför viktigt att vara medveten om att interfaces skiner när det används av flera klasser, inte en enstaka klass. I t.ex. Dependency injection används trots detta interfaces men då mer för att komma runt tekniska begränsningar. Så slösa inte tid på att skriva interfaces som du aldrig kommer använda, men ha alltid med dig princip #*6. Bra kod lever, förändras och refaktoreras kontinuerligt och vid behov.*

## 3. Liskov Substitution Principle
Den något svårförståliga officiella förklaringen för LSP är som följande:

> Let ϕ ( x ) be a property provable about objects x of type T.  
> Then ϕ ( y ) should be true for objects y of type S where S is a subtype of T.

Detta refereras ofta till som "Kära någon, vad betyder detta?-principen" 😉
Men oroa dig inte, vi kommer bryta ner LSP och ge exempel. 

Omskrivet till mer läsbara termer kan man säga att LSP handlar om att ärvda objekt ska ha ett mått av hållbarhet. 
Detta mått utgörs av en princip som säger att om objektet S är en undertyp av objektet T så ska man kunna ersätta objekt av typen T med objekt av typen S utan att förstöra något. Något omskrivet blir detta:

> Möjligheten att ersätta en instans av en föräldraklass med en instans av en klass som ärver den utan negativa effekter

För att exemplifiera när LSP bryts brukar man ofta bli bjuden på kod som handlar om geometri i stil med cirklar och elipser eller kvadrater och rektanglar:

```csharp
public class Rectangle
{
    protected int _width;
    protected int _height;

    public int Width { get => _width; }
    public int Height { get => _height; }

    public virtual void SetWidth(int width) => _width = width;
    public virtual void SetHeight(int height) => _height = height;

    public int Area { get => _width * _height; }
}
```
```csharp
public class Square : Rectangle
{
    public override void SetWidth(int width)
    {
        _width = width;
        _height = width;
    }
    public override void SetHeight(int width)
    {
        _width = width;
        _height = width;
    }
}
```
Enligt LSP ska vi kunna använda Rectangle och Square utbytbart utan att resultatet förändras, men i fallet nedan blir detta helt omöjligt:
```csharp
var shape = new Rectangle();
//var shape = new Square();

shape.SetWidth(120);
shape.SetHeight(25);

Console.WriteLine("Area is: " + shape.Area);
```
Ifall vi använder oss av Rectangle kommer Area att bli 3000 men använder vi oss av Square kommer Area bli 625. Alltså innebär det att subklassens beteende har förändrats i jämförelse med föräldern. T kan alltså inte ersättas av S och vi kan inte heller fixa det i abstraktionen på något självklart sätt.

## 4. Interface Segregation Principle
ISP säger att klienter ska inte tvingas att implementera ett interface som dem aldrig kommer använda eller som är irrelevanta till dem. ISP är inne på samma spår som SRP. Håll dina interfaces korta och ha hellre flera interfaces än ett stort. Det gäller alltså att skriva kärnfulla interfaces och hellre implementera flera små än ett massivt då det är mer troligt att kod som använder stora interfaces kommer stöta på patrull då dess beroenden ökar helt i onödan.

Nedan följer ett **dåligt exempel** från en anteckningsapp.

I början hade en anteckning en fåtal metoder som behövde implementeras:
```csharp
public interface INote
{
    bool SaveNote(Note note);
    string GetText(Note note);

}
```
Men med tiden utökades funktionaliteten. Bilder och citat lades till samt en funktion som kunde hämta en URL-källa och göra en backup av URL:en för referenssyften. Detta lades in i interfacet:
```csharp
    public interface INote  
    {
        bool SaveText(string text);
        string GetText();
        List<Images> GetImages();
        void SaveImages(List<Images> images);
        List<Quotation> GetQuotes();
        void SaveQuotes(List<Quotation> quotes);
        List<UrlBackup> GetUrlBackups();
        void SaveUrlBackups(List<UrlBackup> backups);
    }
```
Det pikära med den här praktiken är att alla anteckningar inte är av en typ som har bilder, citat eller url-backuper. Nu tvingas dock alla anteckningar att implementera dessa metoder. **En bättre praktik** hade varit att begrunda Single Responsibility Principle och ISR för att stycka upp interfacet i flera mindre interfaces:

```csharp
public interface ITextNote  
{
     bool SaveText(string text);
     string GetText();
}
```

```csharp
public interface IImageNote
{
     List<Images> GetImages();
     void SaveImages(List<Images> images);
}
```

```csharp
public interface IAcademicNote
{
     List<Quotation> GetQuotes();
     void SaveQuotes(List<Quotation> quotes);
     List<UrlBackup> GetUrlBackups();
     void SaveUrlBackups(List<UrlBackup> backups);
}
```
Nu kan man skapa ett antal olika typer av anteckningar som har endast text, endast bilder eller, om man vill, allt.
I små projekt som exemplet ovan kanske det inte känns som hela världen men kom ihåg: alla projekt är små i början!

## 5. Dependency Inversion Principle
DIP handlar i korthet om att man inte låter högnivå moduler, t.ex. en main-klass diktera vilka dependencies som lågnivåmoduler har. En stereotypisk mental bild från PHP infaller sig: En config eller settings-fil som läser in alla dependencies som hela programmet ska ha och sedan inkluderas i toppen av alla filer. DIP strävar efter att vända på denna ordning. Ett av de vanligaste sätten att astadkomma DIP med är s.k: *dependency injection* som är ett sätt att injicera dependencies i en modul genom interfaces som kan användas på flera nivåer. Dependency injection är dock *inte* samma sak som DIP, det är ett sätt att implementera DIP på.

Men varför vill man åstadkomma detta? Kort och gott handlar det om att beroenden alltid innebär en risk för mjukvara. Om du har hårda beroenden som dikteras uppifrån och ned ökar helt enkelt risken att din mjukvara ska gå sönder. DIP vill undvika tighta sammankopplingar mellan klasser i stil med **foljande dåliga** exempel:
```csharp
public class Note
{
    private Attachment _attachment;
    public Note()
    {
        _attachment = new Attachment();
    }

    public void SetPDFAttachment(byte[] data)
    {
        _attachment.SetData();
    }
}

public class Attachment
{
    public void SetData(byte[] data)
    {
		...
    }
}
```
Exemplet ovan skapar en tight sammanlänkning mellan de två specifika klasserna. Note, som är en modul med högre nivå än Attachment bestämmer beroendet, vilket bryter mot DIP. Genom att skapa en abstraktion kan vi dock lösa detta:

```csharp
public class Note
{
    private IAttachment _attachment;
    public Note()
    {
        _attachment = new PDFAttachment();
        //_attachment = new ZIPAttachment();
    }

    public void SetPDFAttachment(byte[] data)
    {
        _attachment.SetData(data);
    }
}
```
```csharp
public class PDFAttachment : IAttachment
{
    public void SetData(byte[] data)
    {
         ...
    }
}
```
```csharp
public class ZIPAttachment : IAttachment
{
    public void SetData(byte[] data)
    {
         ...
    }
}
```
I och med abstraktionen så behöver Note-klassen veta mycket mindre om Attachment-klassen. Således har vi minskat sammankopplingen mellan de båda klasserna och följer tydligare DIP. Dock så kommer vi inte undan att Note-klassen alltid kommer ha den angivna attachment-klassen. Vi kan ytterligare minska deras sammankoppling genom att använda oss av Dependency Injection (DI), vilket kommer i några olika smaker. Det vanligaste sättet man brukar se i det vilda är att beroenden **injicieras i konstruktorn** för en klass:
```csharp
 public class Note
 {
     private IAttachment _attachment;
     public Note(IAttachment attachment)
     {
         _attachment = attachment;
     }

     public void SetPDFAttachment(byte[] data)
     {
         _attachment.SetData(data);
     }
 }
```
Detta innebär att varje Note som instansieras kan ha sin alldeles egna typ av attachment.
I vissa fall kanske man behöver variera detta mycket oftare än per instansiering av objektet, t.ex. varje gång en metod anropas. I sådana fall kan man **injicera ett beroende direkt i en metod**:

```csharp
public class Note
{
    public void SetAttachment(IAttachment attachment, byte[] data)
    {
        attachment.SetData(data);
    }
}
```

Man kan även injicera beroenden direkt i en property även om detta ibland kan leda till *temporal sammankoppling* där en metod eller property inte fungerar *före* en annan har anropats. Det är därför viktigt att man endast använder sig av property injection när beroendet är helt och hållet frivilligt. Överlag är det ganska ovanligt men ser ut ungefär som följande:

```csharp
public class Note
{
    public IAttachment Attachment { get; set; }
    public void SetAttachment(byte[] data)
    {
        if (Attachment == null)
            return;

        Attachment.SetData(data);
    }
}
```


