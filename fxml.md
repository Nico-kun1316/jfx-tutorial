# Tworzenie interfejsu użytkownika za pomocą FXMLa
Na potrzeby tego dokumentu, wszystkie fragmenty kodu są tworzone na nowej gałęzi. Jeśli nie chcecie tworzyć nowego projektu, też polecam wam pracować na nowej gałęzi testowej (pamiętajcie, aby commitować zmiany przed zmianą gałęzi).

## Tworzenie okna
Jak już wiemy, pierwsze okno aplikacji zostanie dla nas utworzone automatycznie przez JavaFX. Nic nie stoi na przeszkodzie, abyśmy stworzyli nowe okna ręcznie. Przypomnijmy sobie, że okno w JavaFX to `Stage` - nowe okno tworzymy tak samo, jak jakikolwiek inny obiekt w Javie.
```java
Stage win = new Stage()
```
Teraz do okna możemy odnieść się za pomocą zmiennej `win`. Okno nie pokazuje się automatycznie - żeby się pokazało, wywołujemy instrukcję `win.show()`. Takie okno jest niestety ciągle bardzo puste. W poprzednim dokumencie, mówiliśmy, że każde okno ma swoją scenę, która gości interfejs użytkownika. Stwórzmy więc taką scenę.
```java
VBox root = new VBox();
Scene scene = new Scene(root);
Stage win = new Stage();
win.setScene(scene);
win.show();
```
Utworzyliśmy teraz scenę, której rootem (korzeniem) jest VBox. Root sceny to jest po prostu element początkowy interfejsu użytkownika, musi nim być jakiś layout. Layouty to takie elementy, które same nie mają wizualnego komponentu, ale układają swoje dzieci w jakiś konkretny sposób. Wszystko, co wrzucimy do VBoxa, będzie układane pionowo. Gdybyśmy użyli HBoxa, to by było układane poziomo. Jest w JavaFX mnóstwo różnych layoutów. Dodajmy teraz jakieś dzieci do naszego VBoxa, aby w końcu pokazać coś konkretnego.
```java
Button button = new Button("Click me! I'm a button!");
VBox root = new VBox();
root.getChildren().addAll(button);
Scene scene = new Scene(root);
Stage win = new Stage();
win.setScene(scene);
win.show();
```
Sprawa jest dość prosta. `root.getChildren()` to lista dzieci naszego VBoxa. `add(button)` dodaje do niej nasz przycisk. To wszystko, co jest wymagane, aby utworzyć nowe okno z jakimś interfejsem użytkownika.

## FXML
Takie rozwiązanie jest bardzo nieefektywne. Miesza kod z layoutem, oraz wymaga tworzenia layoutu w bardzo nienaturalny sposób, okropny w pisaniu i czytaniu. Do layoutów dużo bardziej pasuje drzewiasta struktura, i tutaj powstał oparty na XMLu FXML. FXML pozwala nam tworzyć layout w sposób deklaratywny, dużo bardziej naturalny. Odpowiednikiem poprzedniego kodu z przyciskiem jest w FXML (w skrócie):
```XML
<VBox>
  <Button>Click me! I'm a button!</Button>
</VBox>
```
Już teraz możemy zauważyć znaczną poprawę. Teraz nasze elementy widzimy w środku ich rodziców, tam gdzie logicznie powinny być, i tam gdzie będziemy je widzieć w okienku. Rozwińmy trochę ten przykład.
```XML
<BorderPane>
  <left>
    <VBox>
      <Button>Home</Button>
      <Button>Settings</Button>
      <Button>Help</Button>
    </VBox>
  </left>
  <center>
    <VBox>
      <HBox>
        <Label>Name</Label>
        <TextField />
      </HBox>
      <HBox>
        <Label>Password</Label>
        <PasswordField />
      </HBox>
    </VBox>
  </center>
</BorderPane>
```
BorderPane to layout, który dzieci układa w 5 segmentach - na lewo, na prawo, na górę, na dół, oraz w środku. Tutaj mamy pionową listę z przyciskami po lewej stronie interfejsu, a w środku interfejsu mamy pionowy układ par inputów i ich opisek (które same leżą obok siebie). Przykład ten ma na celu zademonstrować możliwości zagnieżdżania FXMLa. Jego zwięzłość jest możliwa tylko dzięki deklaratywnemu stylowi. Ten sam kod w Javie byłby tak żmudny do napisania, że napisanie go pozostawiam jako ćwiczenie dla czytelnika. 

Jak podłączyć plik FXML pod nasz kod? Używając `FXMLLoader.load()`, o którym wcześniej mówiliśmy. Czyli jeśli nasz plik to *Layout.fxml*, to podpięcie go będzie wyglądało następująco:
```java
Parent root = FXMLLoader.load(getClass().getResource("Layout.fxml"));
Scene scene = new Scene(root);
Stage win = new Stage();
win.setScene(scene);
win.show();
```

Aby utworzyć nowy plik FXML, w IntelliJ klikamy prawym przyciskiem na *Resources* i wybieramy *New -> FXML File*

![New FXML File](images/create_fml.png)

W wyskakującym okienku nadajemy nazwę naszemu plikowi i klikamy *OK*. 

![Create FXML File](images/create_fml_dialog.png)

Plik zostanie utworzony. IntelliJ odrazu zaznaczy nazwę kontrolera do edycji. Możemy zatwierdzić domyślną nazwę klikając enter, lub wpisać wybraną nazwę i zatwierdzić klikając enter. Kontroler jest czymś, co obsługuje nasze ruchy myszką, kliknięcia w przycisk, czy też wpisywanie tekstu klawiaturą. Innymi słowy, kontroler obsługuje *wydarzenia* (eventy), które dzieją się w naszym interfejsie.

![Controller Name](images/fml_controller_name.png)

Nazwa naszego kontrolera jest czerwona. Oznacza to, że on nie istnieje. Aby to naprawić, przesuwamy kursor (klawiaturą lub myszką) na nazwę kontrolera, i klikamy *ALT + ENTER* lub klikamy myszką na żarówkę. Wybieramy *Create Class*.

![Create Controller Class](images/fml_controller_create.png)

W oknie wyboru, gdzie utworzyć tę klasę, wybieramy *src/main/java*.

![Create Controller Dir](images/fml_controller_create_dir.png)


