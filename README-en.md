# Feroles coding principles and SOLID cheat note
What's following is my cheat note regarding SOLID principles and general QoL tips for software development. It has been developed as support tool for my code evening sessions, where I work with junior developers and help them understand the world of software development. **NOTE: I'm in no way the greatest software developer in the world or a absolute authority** so if you have suggestions, please create a PR so I can learn something new! ❤️

# 1. General philosophy
## 1. Good code let developers choose their engagement with the code
Bas code forces you to meticulously read every row and work your way through large and convoluted methods, classes and files to understand the genereal flow, logic and output. Good code is structured in such a way that you easily can choose your level of engagement with the code. You can preferably achieve this by breaking up your code in relevant components with good naming practices.

## 2. Good code enables development without deep domain knowledge
Bad code forces you to learn a lot about a solution, organization and their history to understand how to use existing functionality or create new functionality. E.g. when there exists a great number of ways to do one thing, such as fetching orders, products or patient information, without clearly marking what's outdated, depricated and duplicates. Good code is living, well documented and distinct.

## 3. Good code follows the general paradigm for the current solution, E.g. object oriented programming
Bad code appears like it's follwing a paradigm but in reality it's not. The most common example I've encountered during my 7+ years of software development is that developers use object oriented programming like it would be the same thing, or almost the same thing, as procedurial programming. This often makes the choice of paradigm meaningless and leads to code with lesser quality. When a paradigm is chosen it's important that you really understand the paradigm and how to use it in a practical setting.

## 4. Good code makes it possitble to add functionality without retesting unchanged functionality
Bad code has massive methods or functions with great amount of logic and responsabilities. To change something in such component can create unexpected consequenses since the component isn't comprehensible. This forces you to retest existing as well as new or modified functionality. Good code is written in such a way that existing functionality never or rarely changes and new functionality can be added as a extension, i.e. Open/Closed Principle.

## 5. Good code is often written in collaboration with others
Most of what I've learned has been in collaboration with other. Even if you are some form of genious, you can learn and grown by working with other developers. Since the software industri is in a state of continuous radical change, it's important that we learn how to work together and learn from each other.

## 6. Good code lives, changes and is refactored continuously and when need arises
Don't write unneccecary abstractions or break down the code if it's short and efficent. But always know that good code needs to live, to be refactored and to change when the need arise. To change code, make abstractions or even partially/fully change the architecture must be a part of the development process. It's as vital as regularly changing oil and filter in your car.

## 7. Good code has a liturgy, and it's called "unit tests"
Just like liturgy (rituals of worship) in religious settings let people remember important lessons to free capacity for creating new things, good code has automated tests as it's liturgy. It let us continue our development with the certainty that there's a safty net that catches our mistakes when we inevitably makes them. This helps us to focus on the road ahead and to create new functionality and better solutions.

**The fundational idea behind these principles is that we want to be able to finish our workday without headaches and not leave behind code that will be someone elses headache**


# 2. General DO's and DON'ts
## 2.1 Naming classes, methods and properties
Give the components good, descriptive names and follow [Microsofts coding style guide.](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/identifier-names)

## 2.2 IF-statments
**Feroles main rule regarding If-statements:**
*Good If-statements let the developer make the choice beteween focusing on the flow or the logic.*
Bad if-statements forces us to meticously examine code regardless if we want to understand the flow or the logic. If-statements must be constructed in such manner that a developer don't have to exert oneself in order to understand their task. The following is therefor a **bad example** since you have to really study the code to understand the function of the if-statement:
```csharp
if(Attachment != null && data != null && _user.UserPermissions.Contains("W") && AllowedFileTypes.Contains(Attachment.FileType))
{
    Attachment.Upload(data);
}
```

And the following is a **better example**:
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

You could even expand the readability of the new method slighly by:
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

Raw if-statements should handle primitive datatypes och contain a lesser amount of conditions. The following code is therefor a **bad example**:
```csharp
if(IsUserRegistered && ShouldReverifySession && HasTimedOffers && IsInCheckout)
{
   ...
}
```
And this is a **better example**:
```csharp
if(IsUserRegistered && IsUserVerified)
{
	...
}
```

Nested if-statements is almost always a headache inducing mistake so avoid them alltogether. Based on this principle the following code is a **bad example**:
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
And this is a **better example**:
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

## 2.3 Method parameters
A method should not contain more then two in-parameters, three at most. If you find  yourself needing more then that it's probably time to create som form of settings object with standard values that catches most of the method calls, or break out the whole method into a separate class. Methods with ten, fifteen, twenty parameters is a abomination that must never be allowed to exist.

## 2.4 Controllers in MVC
Controllers in project that follows the MVC pattern is **mainly for routing** - not logic, validation or anything else. If you got a endpoint and it isn't obvious what is happening or which view (or similar) that will be served, then it's time to concider some refactoring. Use a middleware for data validation and services for building models, converting DTO:s etc.







# 3. SOLID and you



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


